_与在运行时运行的函数不同，宏在编译时扩展，生成成为程序一部分的代码。_
宏和函数之间的主要区别在于它们的~={green}运行时间=~。~={blue}函数=~是在~={orange}运行时执行=~的代码单元，而宏~={orange}在编译期间=~生成代码


# 宏的一些概念

|概念|层级|用途|示例|
|---|---|---|---|
|**Token**|词法单元|词法分析的基础单元|`fn`, `foo`, `(`, `)`|
|**TokenTree**|词法分组|宏系统中处理嵌套结构|`(1 + 2)` 作为一个子树|
|**AST**|语法结构|编译器理解代码语义的核心表示|函数、表达式、类型的层级结构|
### **Token（词法单元）**
- 将代码分成一些"单词",例如：
```rust
fn foo() {} //会分成 fn、 foo、 { 、}、
```

### **Token Tree**
- **定义**：Token 的嵌套结构，用于处理宏展开时的分组（如括号、方括号、花括号内的内容）。

### **AST**
-  包含完整的语法结构（如函数、表达式、语句等）。
-  会~={red}丢弃无关的符号=~（如多余的分号、括号），并验证语法正确性。
```rust
fn foo() { 1 + 2 }

FunctionDecl
  name: foo
  body: Block
    expr: BinaryOp(Add, Literal(1), Literal(2))

```

### **卫生性**
- 当宏展开时，~={green}内部的变量=~不会影响~={green}外部的变量=~
	- 宏内定义的变量名不会意外捕获或覆盖宏外部的同名变量。
- 传入宏的句法上下文与在macro内部的句法~={green}上下文不同=~
- 但宏~={red}无法=~自动避免与~={green}外部类型(struct等)=~、~={green}函数=~、~={green}模块=~等名称的冲突（需手动处理）
	- 外部可以使用宏展开的代码
- 解决宏冲突
	1. `MacroFoo` 这样命名
	2. 将宏内容放入模块
```rust
macro_rules! modular_macro {
    () => {
        mod macro_internal {
            pub struct Foo;
            impl Foo {
                pub fn new() -> Self { Foo }
            }
        }
    };
}
```
  3. 使用全局唯一标识符
```rust
let unique_name = format_ident!("__internal_{}", some_suffix);
quote! {
    struct #unique_name;
}
```
- **标签卫生性**：`loop`、`break` 等标签是卫生的：

# 声明宏

## Designators and Repetition
模式 `（$name：expr）` 捕获任何表达式并将其绑定到名称 `$name`。然后，替换代码使用此捕获的表达式。
```rust
macro_rules! say_hello {
    // Pattern => Replacement
    ($name:expr) => {
        println!("Hello, {}!", $name);
    };
}

fn main() {
    say_hello!("Rust developers"); // Expands to: println!("Hello, {}!", "Rust developers");
}
```

| expr  | 表达式                                                        |
| ----- | ---------------------------------------------------------- |
| ident | 标识符，例如变量名、函数名、结构体/枚举名、**模块名**、**泛型参数名**（T）、**生命周期参数名**(`a) |
| block | 代码块,例如`{}`                                                 |
| item  | 项（函数、结构体等）                                                 |
| ty    | 类型                                                         |
| pat   | 模式                                                         |
| stmt  | 语句                                                         |
| meta  | 元项目（属性）                                                    |
| tt    | 标记树                                                        |
**1. 基本片段分类符**

| 分类符        | 匹配内容                                        | 示例                             |
| ---------- | ------------------------------------------- | ------------------------------ |
| `ident`    | 标识符（变量名、函数名、结构体名等）                          | `x`, `foo`, `MyStruct`         |
| `expr`     | 表达式（求值为一个值的代码）                              | `x + 1`, `foo()`, `if x { y }` |
| `ty`       | 类型（类型表达式）                                   | `i32`, `Vec<String>`, `&[u8]`  |
| `pat`      | 模式（用于 `match`、`let` 等）                      | `Some(x)`, `(a, b)`, `_`       |
| `stmt`     | 语句（单条语句，可能包含分号）                             | `let x = 1;`, `foo();`         |
| `block`    | 代码块（由 `{}` 包围的多个语句）                         | `{ let x = 1; x }`             |
| `item`     | 程序项（函数、结构体、模块等顶层定义）                         | `fn foo() {}`, `struct Bar;`   |
| `meta`     | 元项（属性内部的内容，如 `#[derive(Debug)]` 中的 `Debug`） | `derive(Debug)`, `cfg(test)`   |
| `lifetime` | 生命周期参数（以 `'` 开头的标识符）                        | `'a`, `'static`                |
| `vis`      | 可见性修饰符（如 `pub`, `pub(crate)`）               | `pub`, `pub(super)`            |
| `literal`  | 字面量（数字、字符串、字符等）                             | `42`, `"hello"`, `'c'`         |
 **2. 特殊用途分类符**

| 分类符     | 用途                                   | 示例                  |
| ------- | ------------------------------------ | ------------------- |
| `tt`    | 匹配单个 Token 或 Token 树（最通用的分类符，用于复杂语法） | `+`, `[`, `foo`     |
| `path`  | 路径（如模块路径、类型路径）                       | `std::mem::replace` |
| `param` | 函数参数（仅在某些过程宏中使用）                     | 不常用                 |
|         |                                      |                     |
**3. 重复模式中的元变量**

在宏的重复模式（`$(...)`）中，元变量可以结合重复操作符（`*`, `+`, `?`）使用：
- `*`：匹配 0 次或多次。  
- `+`：匹配 1 次或多次。
- `?`：匹配 0 次或 1 次。

### **重复**
```rust
macro_rules! create_function {
    ($func_name:ident, $($param_name:ident: $param_type:ty),*) => {
        fn $func_name($($param_name: $param_type),*) {
            println!("Called function with parameters: {}", stringify!($($param_name),*));
        }
    };
}
// 其中 $param_name:ident: $param_type:ty 是函数参数
```

- `$(...),*` 匹配此模式的~={yellow}零个或多个=~以逗号分隔的项目
- 如果用 `+`（1 次或多次），则至少需要一个参数。
-  `?`0次或者1次

## $crate元变量
- 当一个宏调用另一个宏时，会出现找不到的情况,调用宏的位置无法找到定义宏的位置
```rust
```rust
//// 在 `helper_macro` crate 里定义 `helped!` 和 `helper!` 宏
#[macro_export]
macro_rules! helped {
    // () => { helper!() } // 这行可能导致 `helper` 不在作用域的错误
    () => { $crate::helper!() }
}

#[macro_export]
macro_rules! helper {
    () => { () }
}

//// 在另外的 crate 中使用这两个宏
// 注意：`helper_macro::helper` 并没有导入进来
use helper_macro::helped;

fn unit() {
   // 这个宏能运行通过，因为 `$crate` 正确地展开成 `helper_macro` crate 的路径（而不是使用者的路径）
   helped!();
}```
## Self and _
- self既可以用`ident`或`tt`来匹配，但是注意句法上下文,也就是传出的与self相关的东西，必须传先传入self
- `_`只能在模式中使用，用`pat`和`tt`匹配
# 作用域
- `#[macro_use]`在mod上使用后，会导出宏，在mod的作用域外可以使用
- 使用`use xxx::xxx_macro`引入**外部**`crate`的`macro`
- 在当前crate中使用`macro`,使用#\[macro_use]在一个`marco`的`rs`文件上，比如`#[macro_use]mod xxx`
- 使用`#[macro_export]`可以无视可视规制直接导出
- `#[macro_use]`是导入宏，`#[macro_export]`是导出宏，也就是方便use引入
### 文本作用域
- 宏在子模块中可见
```rust
macro_rules! X { () => {}; }
mod a {
    X!(); // defined
}
mod b {
    X!(); // defined
}
mod c {
    X!(); // defined
}
fn main() {}
```
- 只有定义了才能使用，也就是要在文本上是有先后顺序的
- 宏之间的调用不需要顺序关系
- 宏可以展示被覆盖，也就是可以拥有同名的宏
- `#[macro_use]`可以导出`mod`或者函数中的`macro`,在定义或者导入时都可以
# 过程宏
- [[SYN解析]]
- 主要区别在于~={green}过程宏=~必须位于~={red}它们自己的 crate=~
```toml
[package]
name = "p_macro"
version = "0.1.0"
edition = "2024"

[dependencies]
syn="*"
quote="*"

[lib]
proc-macro = true
```

## AST
### **TokenStream 可解析的 AST 节点类型总结表**
| **分类**      | **AST 节点类型（`syn`）**    | **用途示例**                 | **代码解析示例**                                        |
| ----------- | ---------------------- | ------------------------ | ------------------------------------------------- |
| **表达式**     | `Expr`                 | 解析任意表达式（如 `1 + 2`）       | `parse_macro_input!(input as Expr)`               |
|             | `ExprLit`              | 字面量（`42`、`"hello"`）      | `if let Expr::Lit(lit) = expr { ... }`            |
|             | `ExprBinary`           | 二元运算（`a + b`）            | `parse_macro_input!(input as ExprBinary)`         |
|             | `ExprCall`             | 函数调用（`foo(bar)`）         |                                                   |
| **语句**      | `Stmt`                 | 单条语句（`let x = 1;`）       | `parse_macro_input!(input as Stmt)`               |
| **类型**      | `Type`                 | 类型标注（`Vec<String>`）      | `parse_macro_input!(input as Type)`               |
| **Item（项）** | `ItemFn`               | 函数定义（`fn foo() {}`）      | `parse_macro_input!(input as ItemFn)`             |
|             | `ItemStruct`           | 结构体定义（`struct S;`）       |                                                   |
|             | `ItemEnum`             | 枚举定义（`enum E { A, B }`）  |                                                   |
| **模式**      | `Pat`                  | 模式匹配（`Some(x)`、`(a, b)`） | `parse_macro_input!(input as Pat)`                |
| **属性**      | `Attribute`            | 属性（`#[derive(Debug)]`）   | `parse_macro_input!(input as Attribute)`          |
| **自定义结构**   | 实现 `syn::parse::Parse` | 解析自定义语法（如 DSL）           | 见下文示例                                             |
| **列表/分隔符**  | `Punctuated<T, P>`     | 逗号分隔的列表（`a, b, c`）       | `Punctuated::<Expr, Token![,]>::parse_terminated` |

---

### **关键解析方法对比**

| **场景**    | **推荐方式**                             | **示例**                                                                |
| --------- | ------------------------------------ | --------------------------------------------------------------------- |
| 解析简单表达式   | `parse_macro_input!(input as Expr)`  | `let expr = parse_macro_input!(input as Expr);`                       |
| 解析带分隔符的列表 | `Punctuated<T, P>::parse_terminated` | `let list = Punctuated::<Expr, Token![,]>::parse_terminated(input)?;` |
| 解析自定义语法   | 实现 `syn::parse::Parse` trait         | 见下方代码示例                                                               |
| 错误处理      | 返回 `syn::Error`                      | `syn::Error::new_spanned(input, "错误消息").to_compile_error().into()`    |


## 卫生性
- `quote!` 宏（来自 `quote` crate）会为局部变量自动添加语法上下文，提供类似声明宏的变量卫生性，但对类型、函数等全局项仍需手动处理。
## 派生宏
- `#[derive(Debug)]`
- 为类型~={red}自动生成 trait 实现=~
- 函数签名：
```rust
#[proc_macro_derive(宏名[, attributes(辅助属性名)])]
pub fn 函数名(input: proc_macro::TokenStream) -> proc_macro::TokenStream { /*...*/ }
```

```rust
#[proc_macro_derive(YourMacroName)] // 宏的名称（用户通过 #[derive(YourMacroName)] 使用）
pub fn your_macro_name(input: proc_macro::TokenStream) -> proc_macro::TokenStream {
    // 实现逻辑...
     let input = parse_macro_input!(input as DeriveInput); //1 step
	//let syn::DeriveInput{ident ,...}=syn::parse_macro_input!{input}


	quote!{
		//生成的代码		
		impl MyTrait for #ident { //#ident 实际拿到ident的值
            fn hello() {
                println!("Hello from {}", stringify!(#ident));
            }
        }
	}
}
```


## 属性宏
- 对类型进行任意转换或添加元数据
```rust
#[proc_macro_attribute]
pub fn your_attribute_name(
    attr: TokenStream,  // 属性本身的参数 示例: 对于 `#[your_attribute(foo, bar)]`，`attr` 包含 `foo, bar`
    item: TokenStream  // 被标记的项（函数、结构体等）
) -> TokenStream {
    // 宏的实现代码
    // 返回修改/生成的 TokenStream
}
```

## 函数宏
- 像~={green}声明宏=~一样使用
```rust
#[proc_macro]
pub fn double(input: TokenStream) -> TokenStream {
    let expr = parse_macro_input!(input as Expr); // 解析为表达式
    //let syn::DeriveInput{expr ,...}=syn::parse_macro_input!{input}
    let output = quote! {
        2 * (#expr) // 包裹括号确保运算优先级
    };
    
    output.into()
}
```
# 常用的宏
- `#[track_caller]` 显示调用者信息
- `#[must_use]` 函数必须使用
