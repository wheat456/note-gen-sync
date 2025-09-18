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

# 设备描述符
## Device Descriptor (设备描述符)

**VID (Vendor Identifier)**:
- **值**: 1155 (十六进制: 0x0483)
- **作用**: 厂商标识符，这是ST官方的VID
- **用途**: 电脑通过这个识别设备制造商
**LANGID_STRING**:
- **值**: English(United States)
- **作用**: 设备支持的语言，影响字符串描述符显示
**MANUFACTURER_STRING**:
- **值**: STMicroelectronics
- **作用**: 厂商名称，会在设备管理器中显示
## Device Descriptor FS (全速设备描述符)
**PID (Product Identifier)**:
- **值**: 22314 (十六进制: 0x5720)
- **作用**: 产品标识符，区分不同产品型号
**PRODUCT_STRING**:
- **值**: STM32 Mass Storage
- **作用**: 产品名称，电脑中显示的设备名
**CONFIGURATION_STRING**:
- **值**: MSC Config
- **作用**: 配置描述，说明当前USB配置
**INTERFACE_STRING**:
- **值**: MSC Interface
- **作用**: 接口描述，说明USB接口功能