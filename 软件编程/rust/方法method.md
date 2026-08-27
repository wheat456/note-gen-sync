##  self和&self和&mut self
- `&self`代表`self:&Self`,该类型的实例
- `self`会将变量的所有权转移给函数
- `Self`代表数据类型
## 方法名与字段名重复
- 方法名可以与字段名相同

## 关联函数
- 类似`String::from`
- 方法中没有`self`
## impl
- 可以有多个`impl`


## From和Into trait
- From和Into是trait
```rust
use std::convert::From;
impl From<i32> for the_struct //------ 从i32类型 得到 Number{value:item}
{
    fn from(item:i32)->Self
    {
        Number{value:item}
    }
}
//自定义From特征
let num=the_struct::from(30);
```

```rust
//使用into
let int =5;
let num:Number=int.into();
//将int转换为Number类型
```