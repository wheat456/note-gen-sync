linq 就是对`IEnumerable<T>` 的扩展方法

- **`Where`**: 根据布尔条件过滤序列。
```cs
var adults = users.Where(u => u.Age >= 18);
```
- `Select` 把对象转成另一个对象 / 字段
- `SelectMany`  将嵌套的集合的所有结果遍历合并，比如班级中的学生，将所有班级的学生合并到一个集合


# 判断
- `Any` 有没有至少一个满足条件，返回`bool`
```cs
bool hasAdult = list.Any(p => p.Age >= 18);
```
- `All` 是否全部满足条件
```cs
bool allEnabled = list.All(u => u.IsEnabled);
```
- `Contains`  集合里是否包含某元素
```cs
bool hasId = ids.Contains(10);
```

# 获取单个元素

- `First` / `FirstOrDefault`
	- 取第一个匹配项（**满足条件的**）
	- `OrDefault` 找不到返回 `null`/ 默认值，不抛异常
```cs
var first = list.First(); 
var user = list.FirstOrDefault(u => u.Id == 1，默认对象);
```

- `Single` / `SingleOrDefault` 
	- 要求**有且仅有一个**匹配
	- 多了会抛异常，适合查唯一数据
```cs
var user = list.SingleOrDefault(u => u.Id == id);
```

-  `Last` / `LastOrDefault`
	- 取最后一个


# 排序
- `OrderBy` / `OrderByDescending`
	- 正序 / 倒序
	
```cs
var ordered = list.OrderBy(u => u.Name).ThenBy(u => u.Age);
```
- `ThenBy` / `ThenByDescending`
	- 多级排序必备
	- 前一个条件相同时，用thenBy的条件
# 分页 / 截取
- `Skip` / `Take`
```cs
var page = list.Skip(20).Take(10);
```

# 聚合计算
- `Count`
```cs
int count = list.Count(u => u.Age > 18);
```

- `Sum` / `Min` / `Max` / `Average`
```cs
var total = list.Sum(u => u.Score);
var maxAge = list.Max(u => u.Age);
```


# 去重 / 合并
-  **`Distinct`**: 去除重复项
```cs
var unique = list.Distinct();
```

-  `Concat` / `Union`
	- `Concat`：直接拼接（不去重）
	- `Union`：合并并去重
# 分组
- GroupBy
- 按字段分组
- ```cs
  var groups = list.GroupBy(u => u.DepartmentId);
  ```
分组后成，`IEnumerable<IGrouping<string, User>>` ，其中`string`就算分组的类型，

- **`OfType<T>`**: 过滤集合中指定类型的元素（常用于非泛型集合或多态集合）。
```cs
var strings = mixedList.OfType<string>();
```


Linq是延迟执行的，也就是说需要在foreach 等情况下用到数据再去执行linq语句，每一次`foreach`都要去执行一遍`linq`
