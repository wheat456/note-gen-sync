### **📌 1. 解析完整的 Rust 结构**

适用于 **解析 Rust 代码块或整个文件**。

| 解析目标     | 解析类型              | 例子                                        |
| -------- | ----------------- | ----------------------------------------- |
| Rust 文件  | `syn::File`       | `parse_file("fn main() {}")`              |
| Rust 代码块 | `syn::Item`       | `struct A;`, `fn foo() {}`                |
| 函数       | `syn::ItemFn`     | `fn add(a: i32, b: i32) -> i32 { a + b }` |
| 结构体      | `syn::ItemStruct` | `struct Point { x: i32, y: i32 }`         |
| 枚举       | `syn::ItemEnum`   | `enum Color { Red, Green, Blue }`         |
| impl 块   | `syn::ItemImpl`   | `impl MyStruct { fn new() -> Self {} }`   |
|          |                   |                                           |
### **📌 2. 解析表达式**

适用于 **解析 Rust 代码中的计算或调用**。

| 解析目标    | 解析类型                                                  | 例子                                          |
| ------- | ----------------------------------------------------- | ------------------------------------------- |
| 任意表达式   | `syn::Expr`                                           | `3 + 5`, `"hello"`, `foo(bar)`              |
| 变量赋值    | `syn::ExprAssign`                                     | `x = 42`                                    |
| 函数调用    | `syn::ExprCall`                                       | `foo(10)`                                   |
| 方法调用    | `syn::ExprMethodCall`                                 | `x.len()`                                   |
| 块表达式    | `syn::ExprBlock`                                      | `{ let x = 3; x + 1 }`                      |
| if/else | `syn::ExprIf`                                         | `if x > 0 { y } else { z }`                 |
| 循环      | `syn::ExprLoop`, `syn::ExprForLoop`, `syn::ExprWhile` | `loop {}`, `for x in y {}`, `while true {}` |
|         |                                                       |                                             |
### **📌 3. 解析类型**

适用于 **解析 Rust 类型定义**。

| 解析目标 | 解析类型                 | 例子                                   |
| ---- | -------------------- | ------------------------------------ |
| 任意类型 | `syn::Type`          | `Vec<i32>`, `&str`, `fn(i32) -> i32` |
| 具体类型 | `syn::TypePath`      | `HashMap<String, i32>`               |
| 引用类型 | `syn::TypeReference` | `&mut Vec<T>`                        |
| 指针类型 | `syn::TypePtr`       | `*const i32`                         |
| 函数类型 | `syn::TypeBareFn`    | `fn(i32) -> i32`                     |
### **📌 4. 解析模式**

适用于 **解析模式匹配 (`match` 或 `let` 语句中的模式)**。

| 解析目标  | 解析类型             | 例子                           |
| ----- | ---------------- | ---------------------------- |
| 任意模式  | `syn::Pat`       | `Some(x)`, `[a, b]`, `x @ 5` |
| 变量模式  | `syn::PatIdent`  | `let x = 10;`                |
| 元组模式  | `syn::PatTuple`  | `let (x, y) = (1, 2);`       |
| 结构体模式 | `syn::PatStruct` | `let Point { x, y } = p;`    |
### **📌 5. 解析宏相关**

适用于 **解析宏调用、宏定义等**。

| 解析目标 | 解析类型             | 例子                              |
| ---- | ---------------- | ------------------------------- |
| 宏调用  | `syn::Macro`     | `my_macro!(arg1, arg2)`         |
| 宏规则  | `syn::ItemMacro` | `macro_rules! my_macro { ... }` |
### **📌 6. 解析辅助元素**

适用于 **解析路径、标识符、属性、泛型等**。

|解析目标|解析类型|例子|
|---|---|---|
|标识符|`syn::Ident`|`my_var`, `SomeType`|
|路径|`syn::Path`|`std::fs::File`|
|属性|`syn::Attribute`|`#[derive(Debug)]`|
|字面量|`syn::Lit`|`42`, `"hello"`, `true`|
|泛型|`syn::Generics`|`<T: Clone>`|
|`where` 约束|`syn::WhereClause`|`where T: Clone`|
