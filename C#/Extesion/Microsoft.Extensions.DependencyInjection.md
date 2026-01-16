依赖注入就是创建好一个东西，不先急着用，而是等到某些东西想用的时候你再传给它
控制反转就是原本需要你自己去创建东西然后给需要的地方，现在变成你告诉别人要哪些东西，别人给你准备好

# 注入的三种方式
## 1.构造函数注入
在构造函数中指定接口
```cs
public class OrderService
{
    private readonly IEmailSender _emailSender;

    // 容器会自动看到这里需要 IEmailSender，并把它传进来
    public OrderService(IEmailSender emailSender)
    {
        _emailSender = emailSender;
    }
}
```

## 2.属性注入


# 生命周期
| **方法**         | **生命周期**           | **行为描述**                          |
| -------------- | ------------------ | --------------------------------- |
| `AddTransient` | **瞬时 (Transient)** | 每次要，管家都给你 **new** 一个全新的。          |
| `AddScoped`    | **范围 (Scoped)**    | 在同一个业务范围内（比如一次 Web 请求），管家只给同一个实例。 |
| `AddSingleton` | **单例 (Singleton)** | 整个程序运行期间，管家只创建一次，所有人共用这一个。        |
在这里，同一个作用域拿到的都是同样的对象
步骤
1. 将对象加入在作用域中，`AddScoped`
2. 创建好provider后，创建作用域`using (IServiceScope scopeA = provider.CreateScope())`
3. 在作用域中拿到作用域provider，就可以获取服务了
```cs
// 1. 注册服务
var services = new ServiceCollection();
services.AddScoped<IdGenerator>(); // 使用 Scoped 注册

var provider = services.BuildServiceProvider();

// --- 模拟第一个任务范围 (Scope A) ---
Console.WriteLine("--- 进入 Scope A ---");
using (IServiceScope scopeA = provider.CreateScope())
{
    var pA = scopeA.ServiceProvider;
    
    var id1 = pA.GetRequiredService<IdGenerator>().Id;
    var id2 = pA.GetRequiredService<IdGenerator>().Id;
    
    Console.WriteLine($"第一次获取: {id1}");
    Console.WriteLine($"第二次获取: {id2}");
    Console.WriteLine($"结论：在同一个 Scope 内，对象是共享的吗？ {id1 == id2}");
}

Console.WriteLine("\n--- 进入 Scope B ---");
// --- 模拟第二个任务范围 (Scope B) ---
using (IServiceScope scopeB = provider.CreateScope())
{
    var pB = scopeB.ServiceProvider;
    
    var id3 = pB.GetRequiredService<IdGenerator>().Id;
    Console.WriteLine($"新 Scope 获取: {id3}");
    Console.WriteLine($"结论：跨 Scope 后，对象是同一个吗？ 结果为否。");
}
```


# 同接口注入
当向容器中添加了2个实现同样接口的类时可以使用下面的方法找到对应需要的类

第一种方法
先拿到所有满足这种服务的对象
```cs
// 获取所有注册的 IMessageService 实现
IEnumerable<IMessageService> services = serviceProvider.GetServices<IMessageService>();

foreach (var service in services)
{
    Console.WriteLine(service.GetType().Name); 
    // 会依次输出 EmailService 和 SmsService
}
```

然后，判断子类中的名字
```cs
public interface IMessageService {
    string ProviderName { get; } // 标识属性
    void Send(string msg);
}

// 在代码中筛选
var service = services.FirstOrDefault(s => s.ProviderName == "SMS");
```

第二种方法
```cs
builder.Services.AddKeyedScoped<IMessageService, EmailService>("email");
builder.Services.AddKeyedScoped<IMessageService, SmsService>("sms");
builder.Services.AddKeyedTransient<IMessageService, SmsService>("sms");
```
获取时
```cs
// 1. 在构造函数中使用属性标签
public class MyController([FromKeyedServices("sms")] IMessageService smsService)
{ ... }

// 2. 或者使用 ServiceProvider 扩展方法

var smsService = serviceProvider.GetRequiredKeyedService<IMessageService>("sms");
```