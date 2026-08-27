## 定义
- `trait`是对未知Self的方法集，比如可以为各种数据类型添加
## 派生
- 通过`#[derive]`
## trait继承
```rust
trait A:B{}
//实现了A的要求是先实现B

trait A<T>:B{}//T为泛型
```
## 关联类型
```rust
//使用泛型
trait Container<A,B> {
    fn contains(&self,a: A,b: B) -> bool;
}

fn difference<A,B,C>(container: &C) -> i32
  where
    C : Container<A,B> {...}
//为什么要AB呢，因为要加限定，就要用到AB，所以显示AB为泛型
---------------------------------
// 使用关联类型
trait Container{
    type A;
    type B;
    fn contains(&self, a: &Self::A, b: &Self::B) -> bool;
}
//当为某个类型实现特征时，可以使用type A = i32等来限定类型
fn difference<C: Container>(container: &C) {}

```
## 用特征作为函数参数
```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}

```

## 调用同名方法
- 当有多个同名的方法，优先调用自身的方法，不调用trait的
- 如果要调用trait的方法使用`特征名::方法(&对象)`
- 如果没有self那么无法使用`特征名::方法`，关联函数还是使用`对象::方法`
- 完全限定语法`<对象as特征名>::方法名(self的三种类型,其他参数)`，这个语法适用于所有情况，比如自身方法<对象>::方法(三种self),关联函数`<对象>::方法(其他参数)`

## 在外部类型上实现外部特征(newtype）
- 解决孤儿规则:对象和特征至少一个在当前的作用域
- 用结构体包裹需要被实现的对象
```rust
use std::fmt;
struct Wrapper(Vec<String>);
impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}
fn main() {
    let w = Wrapper(vec![String::from("hello"), String::from("world")]);
    println!("w = {}", w);
}
```

## dyn返回trait
- 函数需要返回的类型需要知道具体大小
- 如果需要返回一个实现了某种trait的数据类型，可以使用`Box<dyn trait>`
```rust
fn random_animal(random_number: f64) -> Box<dyn Animal> {
    if random_number < 0.5 {
        Box::new(Sheep {})
    } else {
        Box::new(Cow {})
    }
}
```
## 运算符重载
- 默认泛型参数`Bar`
```rust
use std::ops;
impl ops::Add<Bar> for Foo {
    type Output = FooBar;

    fn add(self, _rhs: Bar) -> FooBar {
        println!("> Foo.add(Bar) was called");
	//add 函数来实现右操作数加法 在+号右边的是Bar
        FooBar
    }

}
//add trait
trait add<RHS=Bar>{
//RHS是泛型
}
```
## iterator
- 对集合类型实现迭代器
## impl trait
- 当返回类型是实现了同一个`trait`时，可将返回类型写为`impl trait`
## trait约束
### 在函数中的泛型限制
```rust

```
### impl中的泛型限制
```rust
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

```rust
trait child:father+mother{}//需实现father和mother
```
## 重名
- 默认调用数据的方法`数据.方法()`
- 调用特征的方法`trait::fly(&数据);`
- 统一用法：完全限定语法`<Type as Trait>::function(receiver_if_method, next_arg, ...);`，`as trait`可以省略
