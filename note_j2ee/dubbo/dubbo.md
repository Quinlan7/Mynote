# Dubbo

> 深入了解 RPC 的实现，为了更好的了解 JSF

## 一、网络编程



### 1. serverSocket

定义：属于 **传输层（TCP）** 的实现，负责监听端口、建立和管理 TCP 连接。

- **ServerSocket 是 HTTP 的底层通道**：提供 TCP 连接管理，确保字节流可靠传输。



### 2. Tomcat

定义：**Tomcat 是 ServerSocket 的高阶封装**，它将底层的 TCP 通信、HTTP 协议解析、Servlet 业务处理等复杂逻辑封装为一个开箱即用的企业级 Web 服务器。



### 3. Jetty

简而言之，如果你需要一个**快速启动、低资源占用且能嵌入代码的服务器**，Jetty 是理想选择；若需要**成熟的企业级功能**，Tomcat 更为合适。两者均基于 `ServerSocket` 实现网络通信，但架构和设计哲学不同，服务于不同的场景需求。

- **Jetty 是轻量级、嵌入式的 Web 服务器**：适合现代云原生和微服务架构。
- **高性能与灵活性**：通过非阻塞 I/O 和模块化设计优化资源利用。
- **与 Tomcat 互补**：Jetty 更侧重轻量和嵌入式，Tomcat 适合传统企业级应用。



### 4. Netty

Netty 不是 TCP 协议，而是一个基于 Java NIO 的网络通信框架。它的定位在于提供一个高效、异步和事件驱动的编程模型，用于构建基于 TCP、UDP 等传输协议的网络应用程序。换句话说，Netty 封装了底层的网络细节，使开发者可以更方便地实现网络通信逻辑，而无需直接处理复杂的底层 API。介于 应用层 和 传输层 之间 ，向上屏蔽 传输层的 复杂操作。

+ Netty 通常会和应用层协议一起使用。

+ Netty 和 BIO、NIO 这些 是并列的网络框架。









