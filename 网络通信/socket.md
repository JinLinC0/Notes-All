# socket

socket(套接字)编程主要用于实现Windows与Linux平台间的网络通信，socket是对应TCP/IP协议的最典型的应用开发接口，它提供了不同主机间进程通信的端点

## socket基本原理

### socket通信过程

套接字编程采用客户机/服务器(C/S)模式，连接成功后，双方可以进行通信



### 基本socket函数

#### 创建套接字

`sockfd = socket(domain, type, protocol);`

参数domain指定socket地址簇类型；type为套接字类型；protocol指明socket请求的协议；sockfd为套接字返回的文件描述符

#### 绑定套接字与本地地址信息

` bind(sockfd，(struct sockaddr*)&server_addr,sizeof(struct sockaddr));`

将本地主机地址以及端口号与所创建的套接字绑定起来

#### 监听连接

`listen(sockfd，backlog);`

此函数表示服务器愿意接收连接，backlog指队列中允许的最大排队请求的个数

#### 建立连接

`connect(sockfd，(struct sockaddr*) &server_addr，sizeof(struct sockaddr));`

connect用于建立连接，server_addr是保存着服务器IP地址和端口号的数据结构struct sockaddr

#### 接收连接请求

`accept(sockfd，(struct sockaddr*) &client_addr，sizeof(struct sockaddr));`

用于接收客户机发来的连接请求

#### 发送数据

 `send(sockfd，msg，len，flags);`

将len字节的数据msg发送出去，flags通常为0

#### 接收数据

`recv(sockfd，buf，len，flags);`

从套接字缓冲区buf中读取len字节长度的数据

#### 关闭套接字

`close(sockfd);`

用于关闭套接字连接