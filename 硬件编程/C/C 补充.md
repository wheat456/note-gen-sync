# 宏函数
`#define 宏名(参数1, 参数2, ...)   替换内容`



# 函数指针
- `*f`：声明一个名为 `f` 的**指针**。
- `(*f)(int, int)`：这个指针指向一个**函数**，该函数接收两个 `int` 参数。
- `int (*f)(int, int)`：这个函数的返回值是 `int` 类型
## 使用typedef
`typedef int (*MathFunc)(int, int);`
就相当于有个`MathFunc` 类型的函数指针

# 变量
## 全局变量
- 在所有函数外部定义
```c
#include <stdint.h>

// 1. 带初始化的全局变量（推荐写明初始值）
uint8_t g_system_speed = 100; 

// 2. 未初始化的全局变量
int g_run_flag;
```
- **已初始化的全局变量**：存放在内存的 `.data` 段。
- **未初始化的全局变量**：存放在内存的 `.bss` 段，程序启动时硬件/编译器会自动将其**清零（初始化为 0）**。
- 在整个项目（所有 `.c` 文件）中，同一个名字的全局变量**有且只能定义一次**！如果在多个 `.c` 文件中重复定义同名的全局变量，链接时就会直接报错（`Duplicate symbol`）。

## 静态变量
### 1. 静态全局变量
在所有函数的外面，用 `static` 定义变量。
```c
// a.c 文件
#include <stdio.h>

static int g_status = 0; // 定义一个静态全局变量

void System_Init(void) {
    g_status = 1;
}
```
这个变量只在当前 `.c` 文件（`a.c`）中有效。
**无法被其他文件用 `extern` 引用**，相当于给这个全局变量加上了“私有访问锁”，有效防止和其他 `.c` 文件里的变量重名。
### 2. 静态局部变量
```c
void Counter_Function(void) {
    static int count = 0; // 定义一个静态局部变量，只会在第一次调用时初始化一次
    count++;
    printf("Count = %d\n", count);
}
```
# 关键字

`volatile` 防止的是编译器的“瞎优化”，确保 CPU 每次都去读真内存
## static
- **限制全局变量/函数的作用域（隐藏功能）**： 在某个 `.c` 文件里用 `static` 修饰全局变量或函数，它们就**只能在这个 `.c` 文件内部使用**。其他文件哪怕 `extern` 也调不到。这能有效防止不同文件里的变量或函数“重名冲突”。
- **延长局部变量的生命周期**： 在函数内部定义的局部变量，一旦函数执行完毕，变量就会被销毁。但如果加了 `static`，这个变量就会一直保存在内存中，**函数退出了值还在**，下次进入函数还能接着用。
## const
**`const` 变量**：会被编译器直接放进容量大得多的 **Flash（程序存储器/Rom）** 里。
## extern
**作用**：当你需要在 `main.c` 里使用在 `usart.c` 中定义的全局变量时，就需要用 `extern` 来声明一下：“这个变量在别的地方定义过了，编译器你先别报错，去其他文件找找。”
```c
// 在 usart.c 中真正定义并分配空间
uint8_t RxBuffer[64]; 

// 在 main.c 或 usart.h 中用 extern 声明，表示“借用”
extern uint8_t RxBuffer[64];
```
**使用方法**：首先，在一个`a.c`文件中定义全局变量，然后再`a.h`中定义`extern xxx`这样其他文件要使用这个变量，就直接包含该头文件
# 设计模式
## 1. 有限状态机
- 对一个任务进行状态描述
```
typedef enum {
    KEY_STATE_RELEASE = 0, // 按键释放/空闲
    KEY_STATE_DEBOUNCE,    // 消抖阶段
    KEY_STATE_PRESS,       // 确认按下（等待抬起或长按）
    KEY_STATE_LONG        // 已经触发长按
} KeyState_t;
```
- 定时查询状态
```
void Key_Process(void) {
    static KeyState_t state = KEY_STATE_RELEASE;
    static uint16_t timer = 0; // 局部静态变量，记录状态持续时间
    
    bool is_pressed = (HAL_GPIO_ReadPin() == 0); // 低电平代表按下

    switch (state) {
        case KEY_STATE_RELEASE:
            if (is_pressed) {
                state = KEY_STATE_DEBOUNCE; // 检测到按下信号，进入消抖状态
            }
            break;

        case KEY_STATE_DEBOUNCE:
            if (is_pressed) {
                state = KEY_STATE_PRESS; // 10ms后依然按下，确认有效，切换到按下状态
                timer = 0;               // 重置计时器
            } else {
                state = KEY_STATE_RELEASE; // 抖动，回归释放状态
            }
            break;

        case KEY_STATE_PRESS:
            if (!is_pressed) {
                // 没到长按时间就抬起了 -> 判定为短按！
                printf("--> 触发：短按事件 (Short Press)\n");
                state = KEY_STATE_RELEASE;
            } else {
                timer += 10; // 累加时间 (假设本函数 10ms 调用一次)
                if (timer >= 1000) { // 按住超过 1000ms
                    printf("--> 触发：长按事件 (Long Press)\n");
                    state = KEY_STATE_LONG;
                }
            }
            break;

        case KEY_STATE_LONG:
            if (!is_pressed) {
                state = KEY_STATE_RELEASE; // 松开后回到初始状态
            }
            break;
    }
}
```
