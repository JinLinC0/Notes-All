# Protobuf

## Protobuf的基本介绍

远程过程调用协议RPC，如，两台服务器A，B，在服务器A上部署了一个应用，想要调用B服务器上应用提供的函数，可以通过RPC协议调用b.ticket.get()来直接调用。

gRPC是一个高性能、通用的开源RPC框架，由Google主要面向移动应用开发并基于HTTP/2协议标准设计，基于ProtoBuf序列化协议开发，且支持多种开发语言。相较于REST，gRPC传输速率更快，更安全。

protobuf是google旗下的一款平台无关,语言无关,可扩展的序列化结构数据格式，是一种独立和轻量级的数据交换格式。所以很适合用做数据存储和作为不同应用,不同语言之间相互通信的数据交换格式,只要实现相同的协议格式即同一proto文件被编译成不同的语言版本,加入到各自的工程中去，用于RPC系统和持续数据存储系统。这样不同语言就可以解析其他语言通过 protobuf序列化的数据。相比Json，Protobuf有更高的转化效率，时间效率和空间效率。

当想把内存中的对象状态保存到一个文件中或数据库的时候或者想套接字在网络上传输对象的时候需要序列化。

对象的序列化：把对象转化为字节序列的过程，封装到body中。

对象的反序列化：把字节序列恢复为对象的过程。

<center>对象  -->  序列化  -->  传输或存储  -->  反序列化  -->  对象</center>

<center>协议          数据格式         协议</center>

主流序列化协议：xml、json、protobuf

序列化后的两种格式：文本模式（可读的）和二进制模式（不可读）

xml、json以文本方式进行存储；protobuf以二进制方式进行存储



### protobuf中常用的数据类型

proto3不支持自己加默认值，系统定义好的

string 字符串类型，要求是utf-8或7-bit与ascii编码的字符串  默认值为空字符串

bytes 比特类型   

bool 布尔类型   默认值为False

int32 32位整形   默认值为0

int64 64位整形   默认值为0

float 浮点类型   默认值为0.0

repeated 数组（列表）

定义如下：repeated string data = 1；string：当前列表中每一个元素的数据类型 data：变量名

map 字典类型  

定义如下：map<string,string>data = 1;  左边的string对应key的数据类型，右边的string对应value的数据类型，data：变量名



### protobuf中常用的特殊字符

package 包名

syntax  protobuf版本

service 定义服务

rpc 定义服务中的方法

stream  定义的方法传输为流传输

message  定义消息体   message User{}

extend  拓展消息体   extend User{}  

import  导入一些插件

//  注释



### grpc的传输方式

根据不同的业务进行不同的选择

unary单程传输方式：普通的传输方式（和http请求方式差不多）

客户端发起一个http请求到服务端，服务端进行业务处理，在反应给客户端，这一请求就结束了

stream流的传输方式：保持连接的传输方式，数据可以一直不停的来回传输，但是对服务器的压力较大

stream流传输的三种状态：

1. 双向：客户端请求服务器端（通过流的传输方式），服务器端返回客户端（通过流的传输方式）

单向流，服务器端和客户端双方建立了长连接，只是由某一方主动向另一方一直发起流的数据

2. 单向：客户端请求服务器端（通过流的传输方式），服务器端返回客户端（通过普通的方式，单程）

3. 单向：客户端请求服务器端（通过普通的方式），服务器端发送给客户端（通过流的传输方式）



### grpc状态码与返回接收的用法

成功的code码： code:0 -> ok

失败的code码： code:1  Canceled  取消操作，服务端主动去取消连接就显示该码context.cancel()

![image-20231011202436434](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011202436434.png)

失败的code码： code:2  Unknown 未知错误

失败的code码： code:3  InvalidArgument  客户端传入的参数有误

失败的code码： code:4  DeadlineExceeded 超时，超过了业务逻辑最大的时长

失败的code码： code:5  NotFound  找不到资源，一般是通过数据库找不到对应的数据

失败的code码： code:6  AlreadyExists  资源已存在，比如用户名被占用

失败的code码： code:7  PermissionDenied  没有相关的权限

失败的code码： code:8  ResourceExhausted  资源耗尽

失败的code码： code:9  FailedPrecondition  拒绝操作

失败的code码： code:10  Aborted  终止操作

失败的code码： code:11  OutOfRange  超过有效范围

失败的code码： code:12  Unimplemented  未执行或未支持的操作，比如服务器版本未支持

失败的code码： code:13  Internal  发生了一些不好的意外

失败的code码： code:14  Unavailable  服务不可用

失败的code码： code:15  DataLoss  数据丢失，不可恢复

失败的code码： code:16  Unauthenticated  无有效的认证



服务端设置code码代码：

```py
context.set_details('bug')  #添加错误描述
context.set_code(grpc.StatusCode.DATA_LOSS)  #设置错误码
raise context  #抛出异常

#客户端代码：
try:
  .......
except Exception as e:
  print(e.code().name, e.code().value)
  print(e.details())
```



## protobuf的基本使用

### 开发一个python的服务器端和客户端调用grpc服务器端  普通rpc协议传输方式

grpc：安装相关的包：

`pip install grpcio`

`pip install grpcio-tools`

`pip install protobuf `



编写.proto代码

test.proto代码如下：

```protobuf
syntax = "proto3";   //使用哪种protobuf
package test;   //给包起一个名称

//服务器中需要的服务，服务框架 demo：服务名 hellojlc：服务对应的函数名  函数对应的hellojlcReq和hellojlcReply
service demo {
    rpc hellojlc(hellojlcReq) returns (hellojlcReply){}
}

//定义数据结构   hellojlcReq和hellojlcReply对应需要传的参数和参数类型
message hellojlcReq {
    string name = 1;
    int32 age = 2;
}
```

消息定义中的每个字段都有一个唯一的编号。这些字段编号用于以二进制格式标识您的字段，一旦消息类型被使用，就不应该被更改。Tag的取值范围最小是1,最大是229229,但是19000~19999 是 protobuf 预留的，用户不能使用。使用频率高的变量最好设置为1~15，这样可以减少编码后的数据大小，但由于编号一旦指定不能修改，所以为了以后扩展，也记得为未来保留一些 1~15 的编号。

map方式定义数据类型的说明：

```protobuf
message hellojlcReq {
    string name = 1;
    int32 age = 2;
message hellojlcReqNumberValue {
string name = 1;
    	    int32 age = 2;
Bool is_active = 3;
}
map<string,hellojlcReqNumberValue > number = 4;
}
```

对应客户端的输入方式`{“a”:pb2.hellojlcReqNumberValue {name : jlc age 23 : is_active :true}}`  

```protobuf
message hellojlcReply {
  string result = 1;
}
```



编译.proto文件

在.proto文件中定义了相关的数据结构，这些数据结构是面向开发者和业务程序的，并不面向存储和传输。当需要把这些数据进行存储或传输时，就需要将这些结构数据进行序列化、反序列化以及读写。ProtoBuf 提供相应的接口代码，可以通过 protoc 这个编译器来生成相应的接口代码，把.proto文件转换成.py文件，在Terminal中输入：

`python -m grpc_tools.protoc -I./ --python_out=. --grpc_python_out=. test.proto`

编译完生成_pb2.py文件和_pb2_grpc.py文件，如下所示：

![image-20231012153753870](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231012153753870.png)

_pb2.py文件

每一个message对应的信息存储，request和response消息体在这里被定义extension

包括message中数据类型及其默认值的信息



_pb2_grpc.py文件

用来存储每一个服务的server与客户端以及注册server的工具

客户端名为：server_name + Stub

服务器名为：server_name + Servicer

注册服务函数为：add_服务端名_to_server

相关代码介绍：

```py
class demoStub(object):   #客户端
    def __init__(self, channel):
        self.hellojlc = channel.unary_unary(   #unary_unary普通的传输类型
                '/test.demo/hellojlc',
                request_serializer=test__pb2.hellojlcReq.SerializeToString,    #序列化
                response_deserializer=test__pb2.hellojlcReply.FromString,    #反序列化
                )
class demoServicer(object):  #服务端
    def hellojlc(self, request, context):
        context.set_code(grpc.StatusCode.UNIMPLEMENTED)   #定义了一些code码和状态码
        context.set_details('Method not implemented!')     #定义了一些错误信息
        raise NotImplementedError('Method not implemented!')
def add_demoServicer_to_server(servicer, server):  #服务注册函数
    rpc_method_handlers = {    #rpc方法的控制器
            'hellojlc': grpc.unary_unary_rpc_method_handler(
                    servicer.hellojlc,
                    request_deserializer=test__pb2.hellojlcReq.FromString,
                    response_serializer=test__pb2.hellojlcReply.SerializeToString,
            ),
    }
    generic_handler = grpc.method_handlers_generic_handler(
            'test.demo', rpc_method_handlers)
    server.add_generic_rpc_handlers((generic_handler,))
```



编写服务端service.py文件

```py
import time
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc
from concurrent import futures    #创建线程数量

class demo(pb2_grpc.demoServicer):   #编写服务
    def hellojlc(self, request, context):   #编写大类里面的服务函数
        name = request.name    #获取参数
        age = request.age
        result = f'my name is {name}, i am {age} years old'   #返回参数
        return pb2.hellojlcReply(result=result)
def run():    #编写启动服务
    grpc_server = grpc.server(
        futures.ThreadPoolExecutor(max_workers=4)   #定义最大线程数量
    )
    pb2_grpc.add_demoServicer_to_server(demo(),grpc_server)  #注册服务，把demo()注册到grpc_server
    grpc_server.add_insecure_port('127.0.0.1:5000')   #绑定ip和端口号
    print('server will start at 127.0.0.1:5000')
    grpc_server.start()    #启动服务

    try:     #一直连续启动服务
        while 1:
            time.sleep(3600)
    except KeyboardInterrupt:
        grpc_server.stop(0)   #安全退出
if __name__ == '__main__':
    run()
```

![image-20231011203345373](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203345373.png)



编写客户端client.py文件

```py
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc
def run():
    conn = grpc.insecure_channel('127.0.0.1:5000')   #定义频道
    client = pb2_grpc.demoStub(channel=conn)    #生成客户端，指定了conn频道
    resposne = client.hellojlc(pb2.hellojlcReq(      #传输数据
        name='jlc',
        age=23
    ))
    print(resposne.result)
if __name__ == '__main__':
    run()
```

![image-20231011203422181](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203422181.png)



### 开发一个python的服务器端和客户端调用grpc服务器端  单向服务器端发送给客户端流传输

编写.proto代码

test.proto代码如下：

```protobuf
syntax = "proto3";
package test;
service demo {
    rpc TestClientRecvStream(TestClientRecvStreamRequest) returns(stream TestClientRecvStreamResponse){}  //单向服务器端发送给客户端流传输

}
message TestClientRecvStreamRequest {
    string data = 1;
}
message TestClientRecvStreamResponse {
    string result = 1;
}
```



编写服务端service.py文件

```py
import time
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc
from concurrent import futures

class demo(pb2_grpc.demoServicer):
    def TestClientRecvStream(self, request, context):  #客户端请求服务器端是一个普通的方式
        index = 0
        while context.is_active():   #监听客户端是不是一个活跃的状态，只有客户端一直活跃，服务器才会一直发数据
            data = request.data
if data == 'close':    #收到close数据，服务器端停止运行
               print('data is close, request will cancel')
               context.cancek()
            time.sleep(1)
            index += 1
result = 'send %d %s' % (index,data)
print(result)
            yield pb2.TestClientRecvStreamResponse(    #服务器端要给客户端返回一种流的形式
                result = result
            )
def run():
    grpc_server = grpc.server(
        futures.ThreadPoolExecutor(max_workers=4)
    )
    pb2_grpc.add_demoServicer_to_server(demo(),grpc_server)
    grpc_server.add_insecure_port('127.0.0.1:5000')
    print('server will start at 127.0.0.1:5000')
    grpc_server.start()

    try:
        while 1:
            time.sleep(3600)
    except KeyboardInterrupt:
        grpc_server.stop(0)

if __name__ == '__main__':
    run()
```

![image-20231011203600441](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203600441.png)



编写客户端client.py文件

```py
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc

def run():
    conn = grpc.insecure_channel('127.0.0.1:5000')
    client = pb2_grpc.demoStub(channel=conn)
    response = client.TestClientRecvStream(pb2.TestClientRecvStreamRequest(   #客户端不停接收流的形式
        data = 'jlc'
    ))
    for item in response:
        print(item.result)

if __name__ == '__main__':
    run()
```

客户端在不停的收到服务器端的数据流：

![image-20231011203637456](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203637456.png)



### 开发一个python的服务器端和客户端调用grpc服务器端  单向客户端发送给服务器端流传输

服务器端不停接收信息，当达到一定状态时，断开这一次的请求，并且服务器端返回给客户端一个结果

编写.proto代码

test.proto代码如下：

```protobuf
syntax = "proto3";
package test;
service demo {
    rpc TestClientSendStream(stream TestClientSendStreamRequest) returns(TestClientSendStreamResponse){}  //单向客户端发送给服务器端流传输

}
message TestClientSendStreamRequest {
    string data = 1;
}
message TestClientSendStreamResponse {
    string result = 1;
}
```



编写服务端service.py文件

```py
import time
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc
from concurrent import futures

class demo(pb2_grpc.demoServicer):
    def TestClientSendStream(self, request_iterator, context):   #request_iterator表示迭代器
        index = 0
        for request in request_iterator:
            print(request.data, ':',index)
            if index == 10:         #index为10，断开连接
                break
            index += 1
        return pb2.TestClientSendStreamResponse(result='ok')   #断开连接后，给客户端返回一个ok

def run():
    grpc_server = grpc.server(
        futures.ThreadPoolExecutor(max_workers=4)
    )
    pb2_grpc.add_demoServicer_to_server(demo(),grpc_server)
    grpc_server.add_insecure_port('127.0.0.1:5000')
    print('server will start at 127.0.0.1:5000')
    grpc_server.start()

    try:
        while 1:
            time.sleep(3600)
    except KeyboardInterrupt:
        grpc_server.stop(0)

if __name__ == '__main__':
    run()
```

![image-20231011203805817](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203805817.png)



编写客户端client.py文件

```py
import time
import random
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc

def test():
    index = 0
    while 1:
        time.sleep(1)
        data = str(random.random())
        if index == 5:   #index为5，断开连接，客户端不向服务端主动发送数据
            break
        print(index)
        index += 1
        yield pb2.TestClientSendStreamRequest(data=data)

def run():
    conn = grpc.insecure_channel('127.0.0.1:5000')
    client = pb2_grpc.demoStub(channel=conn)
    #单向客户端发送给服务器端流传输方式
    response = client.TestClientSendStream(test())
    print(response.result)

if __name__ == '__main__':
    run()
```

![image-20231011203837185](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203837185.png)



### 开发一个python的服务器端和客户端调用grpc服务器端  双向流传输

编写.proto代码

test.proto代码如下：

```protobuf
syntax = "proto3";
package test;
service demo {
        rpc TestTwoWayStream(stream TestTwoWayStreamRequest) returns(stream TestTwoWayStreamResponse) {}  //双向流传输
}
message TestTwoWayStreamRequest {
    string data = 1;
}
message TestTwoWayStreamResponse {
    string result = 1;
}
```



编写服务端service.py文件

```py
import time
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc
from concurrent import futures

class demo(pb2_grpc.demoServicer):
        def TestTwoWayStream(self, request_iterator, context):
        for request in request_iterator:   #从客户端获取数据的方式
            data = request.data
            yield pb2.TestTwoWayStreamResponse(result='service send client %s' % data)

def run():
    grpc_server = grpc.server(
        futures.ThreadPoolExecutor(max_workers=4)
    )
    pb2_grpc.add_demoServicer_to_server(demo(),grpc_server)
    grpc_server.add_insecure_port('127.0.0.1:5000')
    print('server will start at 127.0.0.1:5000')
    grpc_server.start()

    try:
        while 1:
            time.sleep(3600)
    except KeyboardInterrupt:
        grpc_server.stop(0)

if __name__ == '__main__':
    run()
```

![image-20231011203950195](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011203950195.png)



编写客户端client.py文件

```py
import time
import random
import grpc
import test_pb2 as pb2
import test_pb2_grpc as pb2_grpc

def test():
    index = 0
    while 1:
        time.sleep(1)
        data = str(random.random())
        if index == 5:   #index为5，断开连接，客户端不向服务端主动发送数据
            break
        print(index)
        index += 1
        yield pb2.TestClientSendStreamRequest(data=data)

def run():
    conn = grpc.insecure_channel('127.0.0.1:5000')
    client = pb2_grpc.demoStub(channel=conn)
    #双向流的方式传输
    response = client.TestTwoWayStream(test())    #客户端以流的形式不停的发信息给服务器
    for res in response:
        print(res.result)

if __name__ == '__main__':
    run()
```

双向流，客户端一直发送信息，同时一直会受到服务端反馈的流信息：

![image-20231011204027140](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20231011204027140.png)

