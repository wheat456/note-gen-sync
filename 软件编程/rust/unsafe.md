- static 定义的变量说我全局的，生命周期是'static，也就是从程序启动到结束。
- static变量的查看和修改都是不安全的，必须在unsafe中操作
# 不安全的trait
```rust

#![allow(unused)]
fn main() {
unsafe trait Foo {
    // methods go here
}

unsafe impl Foo for i32 {
    // method implementations go here

}

```