# 06 SOMP/IP协议


## 1.1 SOME/IP简介

​	SOME/IP是一种汽车中间件解决方案，可用于控制消息。最早由宝马在2012-2013年开发，并在2014年集成进AUTOSAR 4.2.1中

​	它从一开始就被设计成适合不同尺寸和不同操作系统的设备。这包括小型设备，如相机、AUTOSAR设备，以及头部单元或远程信息处理设备。此外，还确保SOME/IP支持信息娱乐领域以及车辆其他领域的功能，是SOME/IP既可用于MOST替换场景，也可用于更传统的CAN场景。Service-Oriented --> 面向服务

​	当客户端请求或服务器通知特定订阅者时，才在客户端-服务断配置中交换数据，这就确保了永远不会浪费带宽，并且仅在需要的时间和地点进行数据通信/交换。MiddlewarE --> 中间件

​	使用以太网传输，基于TCP/UDP协议 Over IP --> 基于IP

## 1.2 SOME/IP作用

- 序列化：数据转换成二进制串
- 远程过程调用
- 服务发现（SD）
- 订阅/发布

## 1.3 服务接口

1. 方法-Method

   - RR：服务端请求+客户端响应
   - FF：只有服务端请求

2. 属性/字段-Property/Field

   - Seter/Getter

   - Notifier

3. 时间-Event

## 2.1 SOME/IP报文格式

- Message ID（Service ID/Method ID）[32bit]报文的唯一标识符

  - Service ID[16 bit] + 0 + Method ID [15bit]

  - Service ID[16 bit] + 1 + Event ID [15bit]

- Request ID

  - Client ID:16 bit

  - Session ID:16 bit
    - 0x0001~0xFFFF

- Protocol Version：8bit，SOME/IP协议版本，通常为0x01

- Interface Version：8bit，Service Interface的主版本，常用作检测服务的一致性

- Message Type：8bit，报文类型

- Return Code：占8bits，表示请求是否被成功处理

| Number | Value                | Description                                                  |
| ------ | -------------------- | ------------------------------------------------------------ |
| 0x00   | REQUEST              | A request expecting a response (even void)                   |
| 0x01   | REQUEST_NO_RETURN    | A fire & forget request                                      |
| 0x02   | NOTIFICATION         | A request of a nogification/event callback expecting no response |
| 0x80   | RESPONSE             | The response message                                         |
| 0x81   | ERROR                | The response containing an error                             |
| 0x20   | TP_REQUEST           | A TP request expecting a response (even void)                |
| 0x21   | TP_REQUEST_NO_RETURN | A TP fire & forget request                                   |
| 0x22   | TP_NOTIFICATION      | A TP request of a notification/event call-back expecting no response |

## 2.2 序列化

SOME/IP的序列化包括序列化和反序列化

- 序列化：将对象的状态信息转化为可以存储或以二进制的形式在网络中传输
- 反序列化：将在序列化过程中所生成的二进制串转化成数据结构或者对象的过程

基本数据类型的序列化

- Uint32:0x12345678

|      | Byte 0 | Byte 1 | Byte 2 | Byte 3 |
| ---- | ------ | ------ | ------ | ------ |
| 大   | 12     | 34     | 56     | 78     |
| 小   | 78     | 56     | 34     | 12     |

结构体序列化：将结构体的数据元素顺序排列，然后将各个元素依次进行序列化

## 3.1 什么是SOME/IP-SD

SD：Service Discovery

- 一种特殊的服务，实现服务寻址及事件订阅功能

- 服务寻址

  - 发现服务

  - 提供服务
  - 结束提供服务

- 时间订阅

  - 订阅

  - 结束订阅
  - 订阅ACK
  - 订阅NACK

## 3.2 SOME/IP-SD报文格式

- Flags：Reboot Flag[1 bit] + Unicast Flag[1 bit] + Reserved[6 bit]

  - Reboot Flag:重启/上电时置1，Session ID完成1-0xFFFF循环后置0

  - Unicast Flag：置1时表示支持单播