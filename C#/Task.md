等待所有任务完成
```cs 
string[] results = await Task.WhenAll(task1, task2);
```

谁先回来用谁
```cs
Task completedTask = await Task.WhenAny(delayTask, dataTask);
```