# 计算机网络

[TOC]

## 一、基础

### 1.1 计算机网络体系结构

计算机网络体系结构，一般有三种：OSI 七层模型、TCP/IP 四层模型、五层结构。

![image-20240810200046421](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408102000499.png)

OSI是一个理论上的网络通信模型，TCP/IP是实际上的网络通信模型，五层 结构就是为了介绍网络原理而折中的网络通信模型。

#### 1.1.1 各层的功能

+ 应用层：通过应用进程之间的交互来完成特定网络应用，应用层协议定义的是应用进程间通信和交互的规则，常见的协议有： HTTP FTP SMTP SNMP DNS . 
+ 表示层：数据的表示、安全、压缩。确保一个系统的应用层所发送的信息可以被另一个系统的应用层读取。 
+ 会话层：建立、管理、终止会话，是用户应用程序和网络之间的接口。 
+ 传输层：提供源端与目的端之间提供可靠的透明数据传输，传输层协议为不同主机上运行的进程提供逻辑通信。 
+ 网络层：将网络地址翻译成对应的物理地址，实现不同网络之间的路径选择, 协议有 ICMP IGMP IP 等 . 
+ 数据链路层：在物理层提供比特流服务的基础上，建立相邻结点之间的数据链路。 
+ 物理层：建立、维护、断开物理连接。



### 1.2 每一层对应的协议

![image-20240810201057786](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408102010900.png)

ping 命令使用的就是 ICMP 协议



## 二、HTTP

### 2.1 常用状态码

HTTP状态码首先应该知道个大概的分类： 

+ 1XX：信息性状态码 
+ 2XX：成功状态码 
+ 3XX：重定向状态码 
+ 4XX：客户端错误状态码 
+ 5XX：服务端错误状态码

![image-20240810202633236](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408102026342.png)

### 2.2 请求方式

![image-20240810204232604](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408102042687.png)



### 2.3 GET 和 POST 的区别

1. 功能：GET 请求只是获取数据，POST请求是提交数据。
2. 携带信息：GET 请求将信息放在 URL，POST 将请求信息放在请求体中。
3. 幂等性：GET 请求不用考虑幂等性，因为只是查看数据。POST 请求需要考虑幂等性。
4. 缓存：GET 请求能够被浏览器和CDN缓存，大大减少了 Web 服务器的负担。

> CDN (Content Delivery Network/Content Distribution Network)，意思是 **内容分发网络** 
>
> - CDN 就是将静态资源分发到多个不同的地方以实现就近访问，进而加快静态资源的访问速度，减轻服务器以及带宽的负担。
> - 基于成本、稳定性和易用性考虑，建议直接选择专业的云厂商（比如阿里云、腾讯云、华为云、青云）或者 CDN 厂商（比如网宿、蓝汛）提供的开箱即用的 CDN 服务。
> - GSLB （Global Server Load Balance，全局负载均衡）是 CDN 的大脑，负责多个 CDN 节点之间相互协作，最常用的是基于 DNS 的 GSLB。CDN 会通过 GSLB 找到最合适的 CDN 节点。
> - 为了防止静态资源被盗用，我们可以利用 **Referer 防盗链** + **时间戳防盗链** 。



### 2.4 http 请求的过程（从浏览器输入 url 到显示主页的过程？）

1. DNS 解析：将域名解析成对应的 IP 地址。 
2. TCP连接：根据IP和端口号，与服务器通过三次握手，建立 TCP 连接 
3. 向服务器发送 HTTP 请求 
4. 服务器处理请求，返回HTTP响应 
5. 浏览器解析并渲染页面 
6. 断开连接：TCP 四次挥手，连接结束



#### 2.4.1 DNS 解析过程

+ 查看浏览器缓存
+ 查看系统缓存
+ 查看路由器缓存
+ 查看本地DNS服务器
+ 若本地DNS服务器也没有，则由本地DNS服务器，进行**迭代查询**：
  + 本地DNS ->  根域名服务器 -> 本地DNS
  + 本地DNS ->  顶级域名服务器 -> 本地DNS
  + 本地DNS ->  权威域名服务器 -> 本地DNS
+ 获取到对应ip后，本地DNS发送给本机



### 2.5 URI和URL

+ URI，统一资源**标识符**(Uniform Resource Identifier， URI)，标识的是Web上每一 种可用的资源，如 HTML文档、图像、视频片段、程序等都是由一个URI进行标 识的。 
+ URL，统一资源**定位符**（Uniform Resource Location)，它是URI的一种子集，主 要作用是提供资源的路径。



### 2.6 HTTP 协议1.0、1.1、2.0和3.0

HTTP1.0 默认短连接，HTTP 1.1 默认长连接，HTTP 2.0 采用多路复用。

**1.0**

- 无状态协议：HTTP 1.0 是无状态的，每个请求之间相互独立，服务器不保存任何请求的状态信息。
- 非持久连接：默认情况下，每个 HTTP 请求/响应对之后，连接会被关闭，属于短连接。这意味着对于同一个网站的每个资源请求，如 HTML 页面上的图片和脚本，都需要建立一个新的 TCP 连接。可以设置`Connection: keep-alive` 强制开启长连接。

**1.1**

- 持久连接：HTTP 1.1 引入了持久连接（也称为 HTTP keep-alive），默认情况下不会立即关闭连接，可以在一个连接上发送多个请求和响应。极大减轻了 TCP 连接的开销。
- 流水线处理：HTTP 1.1 支持客户端在前一个请求的响应到达之前发送下一个请求，以提高传输效率。

**2.0**

- 二进制协议：HTTP 2.0 使用二进制而不是文本格式来传输数据，解析更加高效。
- 多路复用：一个 TCP 连接上可以同时进行多个 HTTP 请求/响应，解决了 HTTP 1.x 的队头阻塞问题。
- 头部压缩：HTTP 协议不带状态，所以每次请求都必须附上所有信息。HTTP 2.0 引入了头部压缩机制，可以使用 gzip 或 compress 压缩后再发送，减少了冗余头部信息的带宽消耗。
- 服务端推送：服务器可以主动向客户端推送资源，而不需要客户端明确请求。

**3.0**

 HTTP/3.0 则基于 QUIC 协议，Quick UDP Internet  Connections，直译为快速 UDP 网络连接。传输层基于UDP、使用QUIC保证UDP可靠性。

![image-20240811103558250](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111035356.png)



### 2.7 HTTP 和 HTTPS

1. HTTP 是超文本传输协议，信息是明文传输，存在安全风险的问题。HTTPS 则 解决 HTTP 不安全的缺陷，在TCP 和 HTTP 网络层之间加入了 SSL/TLS 安全协议，使得报文能够加密传输。 
2. HTTP 连接建立相对简单， TCP 三次握手之后便可进行 HTTP 的报文传输。而 HTTPS 在 TCP 三次握手之后，还需进行 SSL/TLS 的握手过程，才可进入加密报文传输。 
3. HTTP 的端口号是 80，HTTPS 的端口号是 443。 
4. HTTPS 协议需要向 CA（证书权威机构）申请数字证书，来保证服务器的身份是可信的。



### 2.8 HTTPS 工作流程

1. 首先客户端请求非对称加密的公钥，并且发送一些 SSL 的版本信息这类数据。
2. 服务器端返回 数字证书 还有 公钥。
3. 客户端 验证数字证书，生成 随机密钥，用于对称加密，通过非对称加密的公钥加密后传输给服务器端。
4. 服务器端 用私钥解密 对称加密的密钥，并且告知客户端后面的通信都使用对称加密。

![image-20240811104154190](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111041408.png)



![image-20240828165908340](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408281659523.png)

### 2.9 Session 和 Cookie

+ Cookie 是保存在客户端的一小块文本串的数据。客户端向服务器发起请求时， 服务端会向客户端发送一个 Cookie，客户端就把 Cookie 保存起来。在客户端下 次向同一服务器再发起请求时，Cookie 被携带发送到服务器。服务端可以根据这 个Cookie判断用户的身份和状态。 

+ Session 指的就是服务器和客户端一次会话的过程。它是另一种记录客户状态的 机制。不同的是cookie保存在客户端浏览器中，而session保存在服务器上。客户 端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上， 这就是session。客户端浏览器再次访问时只需要从该session中查找用户的状态。

其实就是用 cookie 来存储 sessionID，以此来达到记录会话状态的效果，不过现在都是用JWT了。



**分布式环境下**

使用 redis 存储 JWT。



## 三、TCP

### 3.1 三次握手

TCP提供面向连接的服务，在传送数据前必须建立连接，TCP连接是通过三次握手建 立的。

**一次握手**:客户端发送带有 SYN（SEQ=x） 标志的数据包 -> 服务端，然后客户端进入 **SYN_SEND** 状态，等待服务端的确认；

**二次握手**:服务端发送带有 SYN+ACK(SEQ=y,ACK=x+1) 标志的数据包 –> 客户端,然后服务端进入 **SYN_RECV** 状态；

**三次握手**:客户端发送带有 ACK(ACK=y+1) 标志的数据包 –> 服务端，然后客户端和服务端都进入**ESTABLISHED** 状态，完成 TCP 三次握手。

#### 3.1.1 为什么不是两次

服务端需要第三次握手确定客户端是否能够接收到它的消息。



### 3.2 四次挥手

**第一次挥手**：客户端发送一个 FIN（SEQ=x） 标志的数据包->服务端，用来关闭客户端到服务端的数据传送。然后客户端进入 **FIN-WAIT-1** 状态。

**第二次挥手**：服务端收到这个 FIN（SEQ=X） 标志的数据包，它发送一个 ACK （ACK=x+1）标志的数据包->客户端 。然后服务端进入 **CLOSE-WAIT** 状态，客户端进入 **FIN-WAIT-2** 状态。

**第三次挥手**：服务端发送一个 FIN (SEQ=y)标志的数据包->客户端，请求关闭连接，然后服务端进入 **LAST-ACK** 状态。

**第四次挥手**：客户端发送 ACK (ACK=y+1)标志的数据包->服务端，然后客户端进入**TIME-WAIT**状态，服务端在收到 ACK (ACK=y+1)标志的数据包后进入 CLOSE 状态。此时如果客户端等待 **2MSL** 后依然没有收到回复，就证明服务端已正常关闭，随后客户端也可以关闭连接了。

#### 3.2.1 为什么是四次挥手

因为结束通信时，需要双方都不再发送信息。相比于三次握手，四次挥手主要就是服务端的ACK和FIN分为两次发送，因为第一次是表示服务端知道客户端没有发送信息的需求了，但是服务端自己还可能要继续发送消息，消息发送完了，然后第二次是表示自己也没有发送信息的需求了。所以四次挥手要比三次握手多一次。

#### 3.2.2 TCP 四次挥手过程中，为什么需要等待 2MSL, 才进入 CLOSED 关闭状态？

为了确定 服务端 收到了客户端的最后一个 ACK，如果服务端没有收到这个 ACK，则会重发FIN，客户端等待 2MSL 时间就是为了确保这种情况下，客户端能收到 重发的FIN。

为什么是2MSL：第四次挥手到达服务器端最长时间MSL，服务器重发第三次挥手到达客户端最长需要MSL，所以客户端收到服务器端重发的第三次挥手的时间最大2MSL。

> MSL(Maximum Segment Lifetime) : 一个片段在网络中最大的存活时间，2MSL 就是一个发送和一个回复所需的最大时间。如果直到 2MSL，Client 都没有再次收到 FIN，那么 Client 推断 ACK 已经被成功接收，则结束 TCP 连接。



### 3.3 TCP如何保证可靠传输

TCP协议主要通过以下七点来保证传输可靠性：连接管理，校验和，序列号，确认应答，超时重传，流量控制，拥塞控制。

1. **连接管理**：即三次握手和四次挥手。连接管理机制能够建立起可靠的连接，这是保证传输可靠性的前提。

2. **校验和**：发送方对发送数据的二进制求和取反，然后将值填充到TCP的校验和字段中，接收方收到数据之后，以相同的方式计算校验和并进行对比。如果结果不符合预期，则将数据包丢弃。

3. **序列号**：TCP将每个字节的数据都进行了编号，这就是序列号。序列号的具体作用如下：能够保证可靠性，既能防止数据丢失，又能避免数据重复。能够保证有序性，按照序列号顺序进行数据包还原。能够提高效率，基于序列号可实现多次发送，一次确认。

4. **确认应答**：接收方接收数据之后，会回传ACK报文，报文中带有此次确认的序列号，用于告知发送方此次接收数据的情况。在指定时间后，若发送端仍未收到确认应答，就会启动超时重传。

5. **超时重传**：具体来说，超时重传主要有两种场景：数据包丢失：在指定时间后，若发送端仍未收到确认应答，就会启动超时重传，向接收端重新发送数据包。确认包丢失：当接收端收到重复数据(通过序列号进行识别)时将其丢弃，并重新回传ACK报文。

6. **流量控制**：接收端处理数据的速度是有限的，如果发送方发送数据的速度过快，就会导致接收端的缓冲区溢出，进而导致丢包……为了避免上述情况的发生，TCP支持根据接收端的处理能力，来决定发送端的发送速度。这就是流量控制。流量控制是通过在TCP报文段首部维护一个滑动窗口来实现的。

7. **拥塞控制**：拥塞控制就是当网络拥堵严重时，发送端减少数据发送。拥塞控制是通过发送端维护一个拥塞窗口来实现的。可以得出，发送端的发送速度，受限于滑动窗口和拥塞窗口中的最小值。
   拥塞控制方法分为：慢开始，拥塞避免快重传和快恢复。

#### 3.3.1 流量控制

**TCP 利用滑动窗口实现流量控制。流量控制是为了控制发送方发送速率，保证接收方来得及接收。** 通信双方各有一个发送缓冲区与接收缓冲区，两端都各自维护一个发送窗口和一个接收窗口。接收方发送的确认报文中的窗口字段可以用来控制发送方窗口大小，从而影响发送方的发送速率。将窗口字段设置为 0，则发送方不能发送数据。

#### 3.3.2 重传机制

TCP 实现可靠传输的方式之一，是通过序列号与确认应答。

在 TCP 中，当发送端的数据到达接收主机时，接收端主机会返回一个确认应答消息，表示已收到消息。**确认ACK中是下一个数据的序列号**。

![4](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111503959.webp)

**超时重传**

重传机制的其中一个方式，就是在发送数据时，设定一个定时器，当超过指定的时间后，没有收到对方的`ACK` 确认应答报文，就会重发该数据，也就是我们常说的**超时重传**。

**快速重传**

快速重传的工作方式是当收到三个相同的 ACK 报文时，会在定时器过期之前，重传丢失的报文段。

![10](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111507982.webp)

#### 3.3.3 拥塞控制

拥塞控制主要是四个算法：

- 慢启动
- 拥塞避免
- 拥塞发生
- 快速恢复

##### 慢启动

当发送方每收到一个 ACK，拥塞窗口 cwnd 的大小就会加 1，每个RTT内 cwnd 都翻倍，指数增长。

有一个叫慢启动门限 `ssthresh` （slow start threshold）状态变量。

- 当 `cwnd` < `ssthresh` 时，使用慢启动算法。

+ 当 `cwnd` >= `ssthresh` 时，就会使用「拥塞避免算法」

##### 拥塞避免

每当收到一个 ACK 时，cwnd 增加 1/cwnd。每个RTT内 cwnd 加1，线性增长。

##### 拥塞发生

当网络出现拥塞，也就是会发生数据包重传，重传机制主要有两种：

- 超时重传
- 快速重传

>  **发生超时重传的拥塞发生算法：**

当发生了「超时重传」，则就会使用拥塞发生算法。

- `ssthresh` 设为 `cwnd/2`

- `cwnd` 重置为恢复为 cwnd 初始化值

> **发生快速重传的拥塞发生算法：**

- `cwnd = cwnd/2` ，也就是设置为原来的一半;

- `ssthresh = cwnd`;
- 进入快速恢复算法

##### 快速恢复

快速重传和快速恢复算法一般同时使用，正如前面所说，进入快速恢复之前，`cwnd` 和 `ssthresh` 已被更新了：

- `cwnd = cwnd/2` ，也就是设置为原来的一半;
- `ssthresh = cwnd`;

然后，进入快速恢复算法如下：

+ cwnd = sshthresh + 3 
+ 重传重复的那几个 ACK（即丢失的那几个数据包） 
+ 如果再收到重复的 ACK，那么 cwnd = cwnd +1 
+ 如果收到新数据的 ACK 后, cwnd = sshthresh。因为收到新数据的 ACK，表明恢 复过程已经结束，可以再次进入了拥塞避免的算法了。

![拥塞发生-快速重传.drawio](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111540461.webp)



### 3.4 TCP 和 UDP

最根本区别：TCP 是面向连接，而 UDP 是无连接

![image-20240811154506866](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202408111545042.png)

应用场景：TCP追求可靠性，UTP追求时效性。



## 四、网络安全

### 4.1 加密算法

**哈希算法**

对任意长度的数据生成一个固定长度的唯一标识，哈希算法的是不可逆的，你无法通过哈希之后的值再得到原值。哈希值的作用是可以用来验证数据的完整性和一致性。

1. MD5
2. SHA
3. CRC

**对称机密**

指加密和解密使用同一密钥，优点是运算速度较快，缺点是如何安全将 密钥传输给另一方。

1. DES
2. AES

**非对称加密**

指的是加密和解密使用不同的密钥（即公钥和私钥）。公钥与私钥是 成对存在的，如果用公钥对数据进行加密，只有对应的私钥才能解密。

1. RSA
2. DSA

### 4.2 常见网络攻击

**DOS、DDOS攻击**

拒绝服务攻击，常见的就是 SYN Flood 攻击，其实就是流量攻击。

主要防范，其实有专业的防火墙来应对这种攻击。

