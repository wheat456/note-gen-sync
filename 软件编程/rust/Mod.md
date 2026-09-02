
> [!NOTE] Tip
> - 一个`mod`就是一个`.rs`文件或者是一个带有`mod.rs`文件的文件夹
> - 如果一个`mod`中含有其他`mod`,就在父文件中使用`mod xxx`导入子`mod`

- `module`中的内容默认是private对外，不可访问
- `module`同级可以直接访问，但同级的子内容需要权限
- `pub mod` 二次导出：如果`test1.rs`中有`pub mod test2.rs;`，其他文件只要`mod test1`，就可以`test1`为路径，来访问test2.rs的内容,比如`use test1::test2`

## 可见性
- `pub(in path)`对`path`进行pub，`path`必须是父级及以上，`pub(self)`在当前`mod`公开，`pub(super)`在父级中公开，`pub(crate)`在当前`crate`可见
- `self`表示当前模块作用域，`super`表示父级`mod`
```rust
// src/network/server.rs

// pub(super) 表示仅对父级模块及同级兄弟模块可见，不对整个 crate 或外部暴露
pub(super) fn get_port() -> u16 {
    8080
}

// 也可以使用 pub 或 pub(crate)
pub fn ping() {
    println!("pong");
}
```


## Use
- `use as`可以给一个路径起别名，例如`use a::b::c as func`
- 使用`use`可以在此作用域中屏蔽其他同名的函数
- 相对使用和绝对使用
```rust
// src/network/client.rs
use super::server;

pub fn start() {
    let port = server::get_port();
    println!("Connecting to port {}", port);
}

// src/network/client.rs
use crate::network::server::ping;

pub fn start() {
    ping();
}
```

## crate 
- 可执行文件的crate 根是main.rs
- 库的crate 根是lib.rs
	- 在main.rs中通过crate 名来引入lib.rs中的公开函数，不用使用mod
	- lib
		- 在二进制项目中，在toml中没有指定编译类型就默认编译到exe中
		- 在lib中，默认编译为rlib

# 新的管理方法

```rust 
text
my_project/
└── src/
    ├── main.rs
    └── my_module/
        ├── sub_module_a.rs
        └── sub_module_b.rs

// 在main.rs中
mod my_module{pub mod sub_module_a.rs}//不用使用mod.rs
```