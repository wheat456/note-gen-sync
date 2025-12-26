**12V_C**：FM
**12V_MIC**：


- [x] IO_1: mic2 switch detect  数据给arm
- [x] IO_3: 送话器插入检测  给arm
IO_4： 发送==红外==arm到mcu上
- [x] SDA1 和SCL1 控制FM
MIC1N和MIC1P 由arm控制**级联输出**的某些东西
- [x] I2S 由arm到dsp

~~TX RX 给面板上的数据，比如旋钮等通用数据给ARM~~
- [x] RX1 TX1 蓝牙
- [x] TX2 RX2 连接dsp arm
- [x] RX3 and Tx3 : connect dsp with wireless board

MIC2_in Con2的输入和喊话器