# UDP：用户数据报协议

UDP 是一个简单的**面向数据报的**运输层协议：进程的每个输出操作都正好产生一个 UDP 数据报，并组装一份待发送的 IP 数据报。

UDP 不提供数据可靠性：它把应用程序传给 IP 层的数据发送出去，但并不保证它们能到达目的地。

## UDP 首部

端口号表示发送进程和接收进程。

由于 IP层已经把 IP 数据报分配给 TCP 或 UDP（根据 IP 首部中协议字段值），因此 TCP 端口号由 TCP 来查看，而 UDP 端口号由 UDP 来查看。TCP 端口号和 UDP 端口号是相互独立的。

UDP 长度字段指的是 UDP 首部和 UDP 数据的字节长度。该字节的最小值为 8 字节（发送一份 0 字节的 UDP 数据报也是可以的）。IP 数据报长度是指数据报全长，因此 UDP 数据报长度是全长减去 IP 首部的长度。

## UDP 校验和

UDP 校验和覆盖 UDP 首部和 UDP 数据。IP 首部的校验和只覆盖 IP 的首部——并不是覆盖 IP 数据报中的任何数据。

UDP 和 TCP 在首部中都有覆盖它们首部和数据的检验和。**UDP 的检验和是可选的，而 TCP 的检验和是必须的。**

UDP 数据报的长度可以为奇数字节，但是检验和算法是把若干个 16 bit 字相加。解决办法是必要时在最后增加填充字节 0，这只是为了校验和的计算（可能增加的填充字节不被传送）。

UDP 数据报和 TCP 数据报都包含一个 12 字节长的伪首部，它是为了计算检验和而设置的。伪首部包含 IP 首部一些字段。其目的是让 UDP 两次检查数据是否已经正确到达目的地。

如果发送端没有计算检验和而接收端检测到检验和有差别，那么 UDP 数据报就要被悄悄地丢弃。不产生任何差错报文（当 IP 层检测到 IP 首部检验和有差错时也这样做）。

UDP 检验和是一个端到端的检验和。**它由发送端计算，然后由接收端验证**。其目的是为了发现 UDP 首部和数据在发送端和接收端之间发生的任何改动。
