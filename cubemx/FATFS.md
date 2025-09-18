# USER_DEFINE
-  **user_define = 自己写存储介质驱动**


## version
显示当前使用的FATFS文件系统库的版本号,这是只读信息，显示集成在CubeMX中的FatFS版本
## 功能参数
### FS_READONLY
**作用**：只读模式配置
- **Enabled**：文件系统只能读取，不能写入、创建、删除文件
- **Disabled**：支持完整的读写操作
### FS_MINIMIZE
**作用**：功能最小化等级，用于减少代码大小
- **0 (Disabled)**：包含所有基本功能
- **1**：移除f_stat(), f_getfree(), f_unlink(), f_mkdir(), f_chmod(), f_utime()
- **2**：移除f_opendir(), f_readdir(), f_closedir()
- **3**：移除f_lseek()
- **选择原则**：根据实际需要的API功能选择合适等级
### USE_STRFUNC
**作用**：字符串函数支持
- **Disabled**：不支持`f_printf(), f_puts(), f_gets()`等字符串函数
- **Enabled without conversion**：支持字符串函数，无换行符转换
- **Enabled with LF -> CRLF conversion**：支持字符串函数，LF自动转换为CRLF
- **用途**：文本文件处理、日志记录
### USE_FIND
**作用**：文件查找功能
- **Enabled**：支持f_findfirst(), f_findnext()函数
- **Disabled**：不支持查找功能
- **用途**：搜索匹配特定模式的文件和目录
### USE_MKFS
**作用**：文件系统格式化功能

- **Enabled**：支持f_mkfs()函数，可格式化存储设备
- **Disabled**：无法格式化，只能使用预格式化的存储设备
- **重要性**：首次使用SD卡或需要重新格式化时必需