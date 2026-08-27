依赖：
```toml
[dependencies]
log = "0.4"
pretty_env_logger = "0.5"

```

# example
```rust
use log::info;

fn main() {
    pretty_env_logger::init(); //必须初始化

    info!("This is a pretty log message");
}

```

# 在库中
- 不需要~={green}初始化=~`logger`
```rust
use log::info;

pub fn my_function() {
    info!("Logging from a library");
}

```

