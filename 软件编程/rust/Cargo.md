# config.toml
### 构建平台
```rust
[build]
target = "x86_64-unknown-linux-gnu"
```

# build.rs

# 工作区
- 用于组织多个package，通常创建一个顶层项目 就是一个package
- 这是一个crate
```toml
[package]
name = "test1"
version = "0.1.0"
edition = "2021"
[dependencies]
tgbot = "0.34.0"
```
- 工作区是一个或多个包的集合，这些包共享通用依赖项解析（具有 shared `Cargo.lock` ）、输出目录和各种设置，例如配置文件。属于工作区的包称为工作区成员。工作区有两种类型：作为根包或作为虚拟清单。
- 所有包共享一个位于工作区根目录中的通用 `Cargo.lock` 文件。
- 所有包共享一个公共输出目录，该目录默认为工作区根目录中命名 `target` 的目录。
- `[patch]` 和 `[replace]` `[profile.*]` sections 仅在根清单中 `Cargo.toml` 被识别，在成员 crate 的清单中被忽略。
## 根包
- 工作区根是工作区 `Cargo.toml` 所在的 `Cargo.toml` 目录。

# 虚拟清单
- 或者， `Cargo.toml` 可以创建包含 `[workspace]` 部分但不包含 `[package]` 部分的文件。这称为虚拟清单。当没有 “主” 包，或者您希望将所有包组织在单独的目录中时，这通常很有用。
## workspace
**在多个package的目录下创建Cargo.toml文件**
```toml
[workspace]
members = ["member1", "path/to/member2", "crates/*"]
exclude = ["crates/foo", "path/to/other"]

```
- 该 `members` 列表还支持 glob 匹配多个路径，使用典型的文件名 glob 模式（如 `*` 和 `?` ）。

如果某些package需要引入其他package需要在toml文件下引入
```toml 
core1={path="../fdfs"}
```

## 选择package
在工作空间中，`package` 相关的 `Cargo` 命令(例如 `cargo build` )可以使用 `-p` 、 `--package` 或 `--workspace` 命令行参数来指定想要操作的 `package`。
- 若工作目录是虚拟清单类型的工作空间，则该命令将作用在所有成员上
- 而 `default-members` 可以在命令行参数没有被提供时，手动指定操作的成员:
```toml
[workspace]
members = ["path/to/member1", "path/to/member2", "path/to/member3/*"]
default-members = ["path/to/member2", "path/to/member3/foo"]

```

# crate 依赖
```toml
[dev-dependencies] # 用于在开发测试时用的依赖

[features]
default = ["native-tls", "ctrlc_handler", "teloxide-core/default"] # 其中这些都是依赖
webhooks = ["rand"] # 启动webhooks 特性才会导入依赖rand
```
# cargo 工具
- 安装更新工具
```bash
 cargo install cargo-update
```
- 更新所用安装的工具
```
 cargo install-update -a
```
- 查看安装的工具
```
 cargo install-update -a
```


# 设置代理
```toml
[http]  
proxy="127.0.0.1:44550"
```