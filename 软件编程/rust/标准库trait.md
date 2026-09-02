### AsRef\<T>

 任何类型只要实现了 `AsRef<T>`，就承诺提供一个从 `&self` 到 `&T` 的只读引用转换。
 ```rust
 pub trait AsRef<T: ?Sized> {
    fn as_ref(&self) -> &T;
}
 ```

### Size
如果一个类型实现了 `Sized`，意味着它的**内存占用在编译期是固定且已知的**（例如 `i32` 固定 4 字节，`String` 固定 24 字节）。
在 Rust 中，为了能安全地在栈上为变量分配空间，**所有泛型类型参数默认都会被隐式加上 `T: Sized` 约束**：
```rust
// 你写的代码：
fn foo<T>(x: &T) {}

// 编译器实际处理的代码：
fn foo<T: Sized>(x: &T) {}
```

### ?Sized
写上 `T: ?Sized` 后，编译器就会撤销默认的 `T: Sized` 要求，允许泛型接收**动态大小类型（DST, Dynamically Sized Types）**。
```rust

```

### Deref
- `ref` 专门为**智能指针**（如 `Box<T>`, `Rc<T>`, `Arc<T>`，以及内存所有权包装类型如 `String`, `Vec<T>`）设计，用来重载 `*` 运算符。
- 当类型不匹配时，编译器会自动插入 `.deref()` 转换。
```rust 
struct MyBox<T>(T);

impl<T> std::ops::Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

let b = MyBox(String::from("hello"));
// 隐式解引用：自动从 MyBox<String> -> &String -> &str
println!("Length: {}", b.len());
```