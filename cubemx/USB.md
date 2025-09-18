# Mass Storage Class
让设备作为存储设备
## 基本参数
### USBD_MAX_NUM_INTERFACES
- **作用**: USB设备最大接口数量
- **值**: 1 (表示只有一个USB接口)
### USBD_MAX_NUM_CONFIGURATION
- **作用**: USB设备最大配置数量
- **值**: 1 (大多数设备只需要一个配置)
### USBD_MAX_STR_DESC_SIZ
- **作用**: USB字符串描述符的最大长度
- **值**: 512 bytes (用于设备名称、厂商信息等)
### USBD_SELF_POWERED
- **作用**: 设备供电方式
- **值**: Enabled (自供电，设备有独立电源)
- **Alternative**: Bus Powered (总线供电，从USB获取电源)
### USBD_DEBUG_LEVEL
- **作用**: USB调试信息级别
- **值**: 0 - No debug message (不输出调试信息)
## Class Parameters 
**MSC_MEDIA_PACKET**:
- **作用**: 媒体I/O缓冲区大小
- **值**: 512 bytes (标准扇区大小)
- **用途**: 与存储介质(SD卡、SPI Flash等)进行数据交换的缓冲区