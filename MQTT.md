# `MQTT`

## 基本概念

`MQTT`（`Message Queuing Telemetry Transport`）是基于`TCP/IP`协议栈构建的异步通信消息协议，是一种轻量级的发布/订阅型消息传输协议，特别适用于物联网应用。采用简洁的协议设计和低带宽消耗，能够在不可靠的网络环境下可靠地传输消息。

`MQTT`是一个客户端服务端架构的发布/订阅模式的消息传输协议。`MQTT`中的客户端既能发送消息也能接收信息。发送消息的客户端先要把信息发送到服务端（`MQTT`服务器），服务端先把信息进行必要的保存，<提升`MQTT`传输信息的稳定性>。在判断转发给哪些客户端，订阅了相关主题的客户端将收到信息。`MQTT`客户端在发送信息的同时也可能在接收信息。发布者可以订阅自己发布的主题从而收到自己发送的信息

消息结构：主题`Topic`(字符串)  负载`Payload`(字符串)

发布者`Publisher`  服务器`Broker ` 订阅者：`Subscriber`

只要订阅者订阅了发布者发布的主题的消息，任何的订阅者够可以收到消息。

`MQTT`发布/订阅特性：

客户端在发布和订阅信息时，可以相互独立（发布者/订阅者不管平台上有多少客户端订阅了该主题），且在空间上可以分离（客户端的空间位置可以在不同的地方），时间上可以异步（信息在发送接收的时间上可能不同步，受网络因素影响，服务器转发给订阅者的信息可能会有延迟）。

`MQTT`的优点：轻巧高效（客户端小，消息头小以优化网络带宽），双向通讯，可扩展大量连接设备，消息传输可靠，启用安全。

`MQTT`主题

主题是字符串，区分大小写，主题是可以使用空格的（建议不要使用），不建议使用中文字符

主题分级：`MQTT`主题各个级别之间使用/来分隔。如：`Tyler-1/motor/1/speed` 主题分为四个级别

### 主题通配符

- 单级别的通配符：+  （可以替代一个`MQTT`主题级别）如：订阅了主题`Tyler-1/motor/+/speed`  ，则可以收到以下主题的信息`Tyler-1/motor/1/speed  Tyler-1/motor/2/speed  Tyler-1/motor/3/speed`


- 多级别的通配符：#  可以涵盖任意数量的主题级别，多级通配符必须是主题中的最后一个字符，如订阅了该主题`Tyler-1/motor/#`  则可以收到`Tyler-1/motor/1/speed  Tyler-1/motor/2/temperature`的信息


以$开始的主题是`MQTT`服务端系统保留的特殊主题，我们不能随意的订阅或发送消息。



## `MQTT`客户端连接服务端

1. 客户端向服务端发送连接请求：发送请求连接的数据包CONNECT（又叫报文）（必定包含clientId和cleanSession和keepAlive的数据）

clientId：客户端的名称，对于同一个服务端所连接的客户端的名称是不能相同的

cleanSession：布尔型数据（true和false）告知服务端此客户端是否是重要的客户端显示false，重要客户端发送信息时服务端需要发送确认信息来表示收到信息，若没有收到确认信息，服务端会将信息保存在本地，后续不断的发送该信息，确保信息可以被客户端收到。

keepAlive：心跳时间间隔，一般设置60s。

心跳机制：要求每个客户端定时向服务端发送一个心跳信息，告知服务端本客户端还在连接中，其时间间隔就是keepAlive。

2. 服务端向客户端发送连接确认：回复数据包CONNACK（包含两个数据：sessionPresent和returnCode）

sessionPresent：当前会话，服务端告诉客户端有没有保存上次连接发送过去未确认信息（对于重要客户端来讲），true表示有。

returnCode：连接返回码，来说明客户端连接的情况，成功连接返回数字0

![image-20231012153058377](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153058377.png)

客户端发布、订阅和取消订阅

发布信息：PUBLISH  客户端向服务器发布信息

PUBLISH报文内容：

topicName：主题名字，向哪个主题发布信息

payload：发布的具体信息内容

retainFlag：保留标志，当客户端向服务端发送PUBLISH报文中若retainFlag数据设置为true时，服务端保存其信息，其他客户端一旦订阅的某个主题，马上就会收到信息内容。

packetId：为数据包编号，用于服务端管理数据包

qos：服务质量，信息的重要性标准，级别越高，重要性越高

dupFlag：重发标志，重发发送，true为信息会重发

订阅主题：SUBSCRIBE 客户端向服务器订阅主题

SUBSCRIBE报文内容：

一个SUBSCRIBE报文可以包含单个或者多个订阅主题名

订阅确认：SUBACK   服务器向客户端回复订阅确认

SUBACK报文内容：

returnCode：订阅返回码，客户端向服务端发送订阅请求后，服务端会给客户端返回一个订阅返回码

![image-20231012153133255](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153133255.png)

取消订阅：UNSUBSCRIBE  客户端向服务器取消订阅

一个取消订阅数据包可以有多个取消的主题名，可以一次性取消订阅多个主题。

 

QoS服务质量等级

告诉我们哪些信息是重要的，需要准确传输，哪些信息不是特别重要，传输中丢失不会影响系统的运行。

QoS=0  最多发一次，发送端只发一次，不管信息有没有被接收端接收到，不会重发信息

QoS=1  最少发一次，发送端发送信息后，要接收端发送确认信息，若没有收到确认信息，发送端会继续发信息，到得到确认信息为止。

QoS=2  保证收一次，接收端发送两次确认信息。



## MQTT平台的搭建和使用

### MQTT服务端平台的搭建

在Ubuntu下搭建mqtt-broker中心服务器：

下载安装包：`wget https:www.emqx.com/zh/downloads/broker/5.0.11/emqx-5.0.11-ubuntu18.04-amd4.deb`

安装：`sudo apt install ./emqx-5.0.11-ubuntu18.04-amd64.deb`

运行：`sudo systemctl start emqx`



在Windows下搭建mqtt-broker中心服务器：

下载windows版本emqx网址：

[https://www.emqx.com/zh/downloads-and-install?product=broker&version=5.2.1&os=Windows&oslabel=Windows]()

打开cmd进入安装路径D:\emqx\bin运行：`emqx console`



浏览器登陆控制台：

[http://10.234.75.59:18083/](http://10.0.2.15:18083/)      10.234.75.59为本电脑的ip地址

输入默认账号和密码：admin  public       登陆显示界面如下：

![image-20231012153202456](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153202456.png)



### MQTT客户端平台的搭建

下载 MQTT客户端软件MQTT.fx，基本界面如下：

![image-20231012153226276](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153226276.png)

建立新客户端mqtt连接，相关参数进行配置，连接服务端地址10.234.75.59

![image-20231012153248872](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153248872.png)

将客户端和服务端进行连接，连接成功界面如下：

![image-20231012153316622](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153316622.png)

同时在浏览器登陆控制台我们可以看到成功连接了一个MQTT_FX_Client1的客户端：

![image-20231012153339341](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153339341.png)

为了实现客户端间的通信，需要进行订阅主题，在Subscribe界面订阅了test的主题：

![image-20231012153407339](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153407339.png)

在Publish进行发布信息：

![image-20231012153432149](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153432149.png)

订阅了test主题的客户端都可以收到相关信息，结果显示如下：

![image-20231012153458016](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153458016.png)



### 通过python进行客户端的创建和连接的访问

MQTT的python接口实现：

首先需要安装相关的库：`pip install paho-mqtt`

MQTT客户端的发布信息程序范例如下：

```py
import paho.mqtt.client as mqtt
import threading
import random
import time
class Mqtt_Publisher:
    def __init__(self,central_ip='10.234.75.59',port=1883,
                 node_name='bci_',anonymous=True,timeout=60):
        self.broker_ip=central_ip
        self.broker_port=port
        self.timeout=timeout
        self.connected=False
        self.node_name=node_name
        if anonymous:
            self.node_name=self.node_name+str(random.randint(100000,999999))
        self.Start()
#central_ip是服务端的ip地址；端口号port是特定的1883端口

    def Start(self):
        self.client = mqtt.Client(self.node_name)     #创建客户端，客户端可以发送信息，也可以接收
        self.client.on_connect = self.on_connect  # 指定回调函数，判断是否连接成功
        self.client.connect(self.broker_ip, self.broker_port, self.timeout)     #客户端连接服务端
        self.client.loop_start()    #开启一个独立的循环通讯线程

    def Publish(self,topic,payload,qos=0,retain=False):    #发布消息，（topic主题，消息内容）
        if self.connected:      #判断客户端是否连接到服务端上
            return self.client.publish(topic,payload=payload,qos=qos,retain=retain)
        else:
            raise Exception("mqtt server not connected! you may use .Start() function to connect to server firstly.")

    def on_connect(self,client, userdata, flags, rc):      #回调函数，判断是否连接成功
        if rc==0:
            self.connected=True
        else:
            raise Exception("Failed to connect mqtt server.")

if __name__=='__main__':
    p=Mqtt_Publisher()
    while not p.connected:
        pass
    while True:
        p.Publish('test','this is a test message')   
        p.Publish('test_2','this is test 2')
        time.sleep(1)     #延迟一秒
```

执行代码后，浏览器控制台可以看到监控节点不断收到进来的消息，同时检测到了一个客户端在往外发送消息



MQTT客户端的接收信息程序范例如下：客户端连接过程与发送信息客户端同理

```py
import paho.mqtt.client as mqtt
import threading
import random
import time
class Mqtt_Subscriber:
    def __init__(self,central_ip='10.234.75.59',port=1883,
                 topic_name='test',callback_func=None,
                 node_name='bci_',anonymous=True,timeout=60):
               self.topic=topic_name
        self.callback=callback_func
        self.broker_ip=central_ip
        self.broker_port=port
        self.timeout=timeout
        self.connected=False
        self.node_name=node_name
        if anonymous:
            self.node_name=self.node_name+str(random.randint(100000,999999))
        self.Start()

    def Start(self):
        self.client = mqtt.Client(self.node_name)     #创建客户端
        self.client.on_connect = self.on_connect  # 指定回调函数
        self.client.on_message=self.default_on_message    #绑定打印输出回调函数
        self.client.connect(self.broker_ip, self.broker_port, self.timeout)     #开始连接
        self.client.subscribe(self.topic)     #绑定名称相同的topic，接收收到同topic消息
        self.client.loop_start()    #开启一个独立的循环通讯线程。

    def default_on_message(self,client, userdata, msg):   #回调函数
        print(msg.payload.decode('utf-8'))      #将收到的消息以utf-8的格式打印出来

    def on_connect(self,client, userdata, flags, rc):
        if rc==0:
            self.connected=True
        else:
            raise Exception("Failed to connect mqtt server.")

if __name__=='__main__':
    p=Mqtt_Subscriber()
    while not p.connected:
        pass
    while True:
        time.sleep(1)
```

运行接收端程序后，就会时时收到同topic客户端发出的消息，显示如下：

![image-20231012153614480](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153614480.png)

监控台不仅有信息流入，也有信息流出：

![image-20231012153635594](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153635594.png)

出现了两个已连接的客户端，一个发送端，一个接收端：

![image-20231012153658211](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153658211.png)

