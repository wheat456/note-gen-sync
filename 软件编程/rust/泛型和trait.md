- 泛型可以分为trait和其他类型（i32等）

### 泛型非特征
```rust
struct Pair<T> {
    x: T,
    y: T,
}
impl Pair<i32> {
    fn new(x: i32, y: i32) -> Self {
        Self {
            x,
           y,
        }
    }
}
//给泛型添加一个限制
--------------------------------------
还可以使用T=i32给结构体加限制

```
## 泛型是特征
```rust
struct Pair<T> {
    x: T,
    y: T,
}
trait X{

}
impl <T:X>Pair<T> { //T后面只能是trait
    fn new(x: T, y: T) -> Self {
        Self {
            x,
           y,
        }
    }
}
//限定了泛型只能为实现了X特征的元素
```

## 泛型下的函数
```rust
//当函数的参数为泛型时
fn foo<T,U>(arg1:T,arg2:U)->(T,U)
{  
    (arg1,arg2)  
}

```


## 声明泛型
### 结构体
```rust
struct Point<T> { x: T, y: T, } //只有一种类型

struct Point<T,U> { x: T, y: U, }//两种不同类型
```
### 枚举
```rust
enum Option<T> { //一种类型
    Some(T),
    None,
}

enum Result<T, E> {//两种类型
    Ok(T),
    Err(E),
}
```
### trait
```rust
trait Summary<T> {
	
}
```
### 函数
```rust
fn generic_function<T>(value: T) {  }
```

## 泛型约束
### 函数
```rust
pub fn notify<T: Summary>(item: &T) { //实际写法
    println!("Breaking news! {}", item.summarize());
}
pub fn notify<T:Summary+Display>(item: &T) {}
-----------------------------------------------------------------
//语法糖写法
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

pub fn notify(item: &(impl Summary + Display)) {}
-----------------------------------------------------------------
//多重约束
fn some_function<T: Display + Clone, U: Clone + Debug>(t: &T, u: &U) -> i32 {}
//上面卸载T的后面太臃肿了，可以改写一下方法
fn some_function<T,U>(t: &T, u: &U) -> i32
where T:Display + Clone,
U:Clone + Debug
{

	  }
```
### 结构体
```rust
struct Wrapper<T: Display> { } 
impl<T: Display> Wrapper<T> { }
```
### trait
```rust
trait Summarizable<T: Display> { //:后面是约束特征，不能是i32类型，可以使用T=i32,来约定
    fn summarize(&self) -> T;
}

struct NewsArticle {
    headline: String,
    content: String,
}

impl Summarizable<String> for NewsArticle {
    fn summarize(&self) -> String {
        format!("{}: {}", self.headline, self.content)
    }
}

```

### 生命周期
```rust
fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len() {
        s1
    } else {
        s2
    }
}

```
## impl
### 结构体
```rust
use std::fmt::Display;

struct Pair<T> {
    x: T,
    y: T,
}

impl<T> Pair<T> {
    fn new(x: T, y: T) -> Self {
        Self {
            x,
            y,
        }
    }
}

impl<T: Display + PartialOrd> Pair<T> {
    fn cmp_display(&self) {
        if self.x >= self.y {
            println!("The largest member is x = {}", self.x);
        } else {
            println!("The largest member is y = {}", self.y);
        }
    }
}

```


### 总结

```rust
/*     结构体的泛型可以是特征或者i32等类型，当为特征时，T:x特征，写在结构体上，那么这个结构体在实例化的时候必须实现x特征，否则不能实例化。如果是写在impl 上，那么只有实现了x特征从才能使用impl下的方法。        如果为i32类型，则使用T=i32，类似道理。


impl<X,Y,Z> 特征<X> 结构体<Y,Z>{

}
前面的xyz是后面特征和结构体的总结，在这里可以添加限制，也可以在后面用where添加限制，
如果xyz已经指明了某种类型就写在旁边的括号


*/
```