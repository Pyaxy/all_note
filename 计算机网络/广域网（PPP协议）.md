# PPP协议
- **PPP (Point-to-Point Protocol)**：一种数据链路层协议，广泛应用于**点到点**链路的数据传输
	- 拨号上网、光纤传输、**…**
- **PPP** 协议有三个组成部分
	- 将 **IP** 数据报封装到串行链路的方法
	- 链路控制协议 **LCP (Link Control Protocol)**
	- 网络控制协议 **NCP (Network Control Protocol)**
## PPP协议的帧格式
- **PPP** 是面向字节的，所有的**PPP** 帧的长度都是整数字节
- 标志字段**F**：**=0x7E (**二进制：**01111110)**
- 地址字段**A**：置为**0xFF**，实际上不起作用
- 控制字段**C**：通常置为**0x03**
- 协议字段：**2**字节，用于识别信息字段**(**又称为载荷**,payload)**的类型
	- **0x0021**：**PPP** 帧的信息字段是**IP** 数据报
	- **0xC021**：信息字段是 **PPP** 链路控制数据**(LCP)**
	- **0x8021**：信息字段是网络控制数据**(NCP)**
- 校验字段**FCS**：**2**字节的[[差错控制#==CRC循环冗余校验==|CRC]]校验
- ![PPP帧格式](http://oss.pyaxy.xyz/img/PPP%E5%B8%A7%E6%A0%BC%E5%BC%8F.png)
- **PPP**的透明传输问题**(**帧边界识别**)**
	- 同步传输**(**如**SONET/SDH)**：[[组帧方法#三、比特填充法|零比特填充]]
	- 异步传输：[[组帧方法#二、字节填充法|字符填充]]
- PPP协议的工作状态
- ![PPP协议的工作状态](http://oss.pyaxy.xyz/img/PPP%E5%8D%8F%E8%AE%AE%E7%9A%84%E5%B7%A5%E4%BD%9C%E7%8A%B6%E6%80%81.png)