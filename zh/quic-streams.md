# 数据流

数据流为QUIC提供了一个轻量级的、有序的字节流的抽象。

QUIC中有两种基本数据流：

 - 从发起者到对端的单向数据流。

 - 双向均可传输的双向数据流。

连接双方都可以建立这两种数据流，数据流之间可并行、交错地传输，并且可以取消。

QUIC中传输数据需要建立一个或多个数据流。

## 流量控制（Flow control）

每个数据流都有独立的流量控制，端点可以通过此实现内存控制和反压（back pressure）。数据流的创建本身也有流量控制，连接双方可以声明最多愿意创建几个流ID。

## 流ID

流ID是一个无符号的62比特整数。流ID的最低2个比特用于识别流的类型（单向或多向）和流的发起者。

流ID的最低1个比特（0x1）用于识别流的发起者。客户端发起双数（最低位置0）流，服务器端发起单数（最低位置1）流。

倒数第2个比特（0x2）识别单双向流。单向流置1，双向流置0。

## 流并发

QUIC允许任意数目的并发流。端点通过控制最大流ID数来控制并发数。

每个端点指定自己的最大流ID数，并只对对端端点有效。

## 收发数据

端点使用流收发数据，这是流的最终用途。QUIC数据流是有序的字节流抽象。但是，不同的流之间是无序的。

## 流优先度

如果正确设置了各流的优先度，流复用机制可以显著提升应用的效率。像HTTP/2这样的复用协议的使用经验告诉我们，有效的优先度策略对效率有显著的正面影响。

QUIC本身没有提供交换优先度信息的报文。接收优先度信息依赖于使用QUIC的应用层。应用层可以定义所有复合他们语义的优先度方案。

在HTTP/3使用QUIC时，优先度信息是在HTTP层完成的。