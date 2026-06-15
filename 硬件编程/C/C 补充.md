# 宏函数
`#define 宏名(参数1, 参数2, ...)   替换内容`



# 函数指针
- `*f`：声明一个名为 `f` 的**指针**。
- `(*f)(int, int)`：这个指针指向一个**函数**，该函数接收两个 `int` 参数。
- `int (*f)(int, int)`：这个函数的返回值是 `int` 类型
## 使用typedef
`typedef int (*MathFunc)(int, int);`
就相当于有个`MathFunc` 类型的函数指针