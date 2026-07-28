- `cmake_minimum_required(VERSION 3.10)` 指定 **CMake** 运行所需的最低版本
- `project(Tutorial VERSION 1.0)` 设置项目的名称
- `add_executable(Tutorial main.cpp)` 构建一个名为 'Tutorial' 的可执行文件
- `cmake ..` CMake 在父目录 (`..`) 中找到 `CMakeLists.txt`，并在当前目录生成构建系统文件。
- `cmake --build .` 构建

# 语法
```
set(变量名 变量值1 变量值2 ... CACHE 缓存说明 [FORCE])
```
### 例子
- 单值变量：`set(NAME "hello")` 存单个字符串 / 数字
- 多值列表：`set(SRCS a.c b.c main.c)` 空格分割 = 数组
- `CMAKE_*` 系列是 **CMake** 预定义全局控制开关
	- `set(CMAKE_C_STANDARD_REQUIRED ON)`
- `[Force]` 强制修改值
	- `set(MAX_THREAD 4 CACHE INTEGER "最大线程数量" FORCE)`
# 目标
```
# 它将由 src/main.cpp 文件编译而成
 add_executable(MyProgram src/main.cpp)
```
# 判断
```cmake 
if (ENABLE\_C17)
    set(CMAKE\_CXX\_STANDARD 17)
else()
    set(CMAKE\_CXX\_STANDARD 11)
endif()

```
