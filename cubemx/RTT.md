- 需要`SEGGER_RTT.h SEGGER_RTT.C SEGGER_RTT_Conf.h` 3个文件
- 使用probe-rs来监控输出
```c
 SEGGER_RTT_WriteString(0, "This is an error message\n"); //0代表输出通道，也就是日志等级
```
