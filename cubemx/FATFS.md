# USER_DEFINE
-  **user_define = 自己写存储介质驱动**


## Version
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
### USE_FASTSEEK 
**作用**：快速定位功能
- **Enabled**：支持f_lseek()的快速定位，创建CLMT表
- **Disabled**：普通定位方式
- **优势**：大文件随机访问时显著提高性能
- **代价**：需要额外RAM存储CLMT表
### USE_LABEL 
**作用**：卷标操作功能
- **Enabled**：支持f_getlabel(), f_setlabel()函数
- **Disabled**：无法读取或设置卷标
- **用途**：管理存储设备的卷标信息
### USE_FORWARD (Forward function) 
**作用**：数据转发功能
- **Enabled**：支持f_forward()函数，直接将文件数据传输到输出流
- **Disabled**：不支持转发功能
- **用途**：高效的数据流处理，避免中间缓冲

## 区域和命名空间参数
### CODE_PAGE 
**作用**：设置目标系统的代码页（字符编码） **常用选项**：
- **437**：美国英语（默认）
- **932**：日文Shift-JIS
- **936**：简体中文GBK
- **949**：韩文
- **950**：繁体中文Big5
- **1250-1258**：Windows代码页
- **Multilingual Latin 1 (OEM)**：支持多语言Latin-1字符集
**选择建议**：中文项目建议选择936（GBK）

### USE_LFN
**作用**：长文件名支持配置 **选项说明**：
- **Disabled (0)**：只支持8.3格式短文件名（如：FILE.TXT）
- **Static work buffer (1)**：使用静态缓冲区支持长文件名
- **Dynamic work buffer (2)**：使用动态分配缓冲区
- **Stack work buffer (3)**：使用栈缓冲区
**影响**：
- 禁用：文件名限制为8个字符+3个扩展名字符
- 启用：支持最多255个字符的文件名
### MAX_LFN 
**作用**：设置长文件名的最大字符数
- **范围**：12-255个字符
- **255**：标准最大值，支持完整长文件名
- **较小值**：节省RAM，但限制文件名长度
- **只有USE_LFN启用时才有效**

### LFN_UNICODE 
**作用**：Unicode支持配置 **选项**：
- **ANSI/OEM (0)**：使用ANSI/OEM编码
- **UTF-16 (1)**：使用UTF-16编码
- **UTF-8 (2)**：使用UTF-8编码
**选择建议**：
- 国际化应用选择UTF-8
- 简单应用可使用ANSI/OEM
###  STRF_ENCODE
**作用**：字符串函数的字符编码 **与CODE_PAGE的关系**：
- 决定f_printf()、f_puts()等函数的字符编码方式
- UTF-8：现代推荐编码，支持全球字符
- 需要与系统其他部分编码保持一致
### FS_RPATH (Relative Path)
**作用**：相对路径支持级别 **选项**：
- **0**：禁用相对路径，只能使用绝对路径
- **1**：启用相对路径，支持f_chdir()
- **2**：完整相对路径支持，包括f_getcwd()
**功能对比**：
- 0：只能用"/folder/file.txt"形式
- 1：支持切换当前目录
- 2：还能获取当前工作目录
## 物理驱动器参数
### VOLUMES
**作用**：支持的逻辑驱动器数量
- **1**：只支持一个逻辑驱动器（如SD卡）
- **2-10**：支持多个驱动器（SD卡、USB、内部Flash等）
- **用途**：同时访问多个存储设备时增加此值
### MAX_SS 
**作用**：支持的最大扇区大小（字节）
- **512**：标准扇区大小，适用于大多数SD卡和硬盘
- **1024/2048/4096**：某些特殊存储设备的扇区大小
- **影响**：决定内部缓冲区大小和兼容性
### MIN_SS
**作用**：支持的最小扇区大小（字节）
- **512**：现代存储设备的标准值
- **与MAX_SS相等**：固定扇区大小，节省代码空间
- **不同值**：支持可变扇区大小的设备

### MULTI_PARTITION 
**作用**：多分区支持
- **Disabled**：只能访问主分区
- **Enabled**：支持访问存储设备上的多个分区
- **用途**：大容量存储设备的分区管理

### USE_TRIM (Erase feature) 
**作用**：TRIM命令支持（擦除优化）
- **Enabled**：支持SSD的TRIM命令，提高性能和寿命
- **Disabled**：不使用TRIM功能
- **适用**：主要用于SSD和支持擦除优化的Flash存储

