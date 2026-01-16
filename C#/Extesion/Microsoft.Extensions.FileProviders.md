# 物理文件系统
1. 创建provider
```cs
var physicalProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
2. 使用
```cs
   IFileInfo fileInfo = provider.GetFileInfo(fileName);
   ```