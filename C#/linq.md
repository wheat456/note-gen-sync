# 处理集合
- **`Where`**: 根据布尔条件过滤序列。
```cs
var adults = users.Where(u => u.Age >= 18);
```

- **`OfType<T>`**: 过滤集合中指定类型的元素（常用于非泛型集合或多态集合）。
```cs
var strings = mixedList.OfType<string>();
```
- **`Distinct`**: 去除重复项。