依赖：
```toml
[dependencies]
env_logger = "0.11.11"
log = "0.4.34"

```

# example
- 控制台默认输出的是error,如果需要输出info需要指定
```rust
use log::info;

fn main() {
   env_logger::init();
   info!("this is a info ");
   error!("this is a error");
}

```

# 在库中
- 不需要初始化
```rust
use log::info;

pub fn my_function() {
    info!("Logging from a library");
}

```

