# 使用步骤
## 1. 如果要复用就定义Task函数
1. 无返回值用**Task**, 有返回值就用**Task\<Type\>包着
2. 只有使用 **Task.run** 才会将任务放到**后台运行**
```cs 
Task xxx(){

	Task.run(()=>{}) 
}
```
## 2. 如果不要复用
就可以直接 **Task.Run()** 放在后台执行一段逻辑
1. 可以加一个**回调(ContiueWith)**，用来知道这个函数完成了
```cs
Task.Run(() => 
{
    // 后台干活
}).ContinueWith(t => 
{
    if (t.IsFaulted)
    {
        // 记录一下后台报错了啥，仅此而已，不影响前台
	        Console.WriteLine("后台任务悄悄挂了: " + t.Exception.InnerException.Message);
    }
});
```

# 注意
不要用`Thread.Sleep` 会阻塞，想要延时就用`Task.Delay`
# CancellationToken
**假如后台正在跑逻辑，但是你想让他提前停止就可以**
```cs
// 1. 制造一个令牌源（相当于拿着遥控器的人）
CancellationTokenSource cts = new CancellationTokenSource();

// 2. 把令牌（遥控器信号）传给后台任务
Task.Run(() => 
{
    for (int i = 0; i < 100000; i++)
    {
        // 关键代码：每次循环都问一下，遥控器喊停了吗？
        if (cts.Token.IsCancellationRequested)
        {
            Console.WriteLine("收到指令，任务主动退出");
            return; // 体面地结束
        }
        
        // 模拟干活
        Thread.Sleep(100); 
    }
}, cts.Token); // 注意：要把 Token 塞进 Task 里

// 3. 前台喊停（比如点按钮触发）
cts.Cancel(); // 喊停！
```

# 运行多个任务
等待所有任务完成
```cs 
string[] results = await Task.WhenAll(task1, task2);
```

或者
```cs
`Task<int[]> combinedTask = Task.WhenAll(task1, task2);`
```


谁先回来用谁
```cs
Task completedTask = await Task.WhenAny(delayTask, dataTask);
```

# 在同步函数中使用异步

如果父函数是同步的
1. 在非UI程序中就直接等待
```cs
public int GetId()
{
    // 😭 底层只能用异步方法
    Task<string> task = GetDataFromInternetAsync();
    
    // ⚠️ 被迫强行等待（相对安全的写法）
    // 为什么不用 task.Result？因为 task.Result 会把异常包装成 AggregateException，很难调试
    // GetAwaiter().GetResult() 会在死锁时抛出真实的异常，稍微好处理一点
    string result = task.GetAwaiter().GetResult(); 
    
    return int.Parse(result);
}
```
2. 如果在UI程序中就将任务放到后代运行，再等待
```cs
public int GetId()
{
    // 开一个没有 UI 上下文的后台线程去等，等出结果再回到主线程
    return Task.Run(() => GetDataFromInternetAsync()).GetAwaiter().GetResult();
}
```


