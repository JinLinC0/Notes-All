# ThingsBoard

ThingsBoard是一个开源的物联网平台，可以实现物联网项目的快速开发、管理和拓展，提供了成熟的SaaS服务化架构解决方案，用于设置管理、设备控制、数据收集、可视化，并支持标准的MQTT、CoAP、LWM2M、SNMP及HTTP协议连接物联网设备

## 环境搭建

环境安装在操作系统为windows平台下进行的

数据库安装：`https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows`

JDK安装：`https://adoptopenjdk.net/index.html`

ThingsBoard安装：`https://github.com/thingsboard/thingsboard/releases/download/v3.4.1/thingsboard-windows-3.4.1.zip`

thingsboard目录的相关文件：conf是一个配置文件，用于修改主要的配置参数

安装JDK后添加系统变量：

![image-20240301205904499](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240301205904499.png)

在系统变量中找到PATH变量，点击编辑，在其中添加`%JAVA_HOME%\bin`

安装数据库时设置数据库密码，可以设置thingsboard的配置文件中对应的密码：`postgres`

安装完数据库后，打开数据库，在主界面中点击Add New Server，在Gerneral选项卡中设置Name为：localhost，其他不用设置；在Connection选项卡进行如下的设置：密码设置为：`postgres`

![image-20240301210425678](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240301210425678.png)

在服务管理界面设置postgredql-x64-15的属性为本地系统账户(可以不设置，创建server错误时可尝试更改)

最后在localhost服务器中创建一个数据库,Database为：`thingsboard`

打开管理员cmd，进入thingsboard文件夹中，输入：`install.bat`

启动服务：`net start thingsboard`

打开浏览器：访问：`http://localhost:8080/`进入本地thingsboard管理界面

默认账号：

系统管理员：`sysadmin@thingsboard.org`    密码：`sysadmin`

租户管理员：`tenant@thingsboard.org`   密码：`tenant`

客户：`customer@thingsboard.org`   密码：`customer`



## 设置邮箱

进入QQ邮箱界面，点击设置，找到POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV服务

授权码：pemmftzubgxmdffg

![image-20240302100811069](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302100811069.png)

其中密码就是授权码，修改完后需要对系统管理员的属性进行使用邮箱的更改，并注销后重新登录，再点击发送测试邮件，验证是否绑定成功



## 传递数据

传递数据有两种方式

#### 通过http方式发送数据

`$HOST_NAME/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"`

$HOST_NAME表示主机名和端口号：`http://localhost:8080`

$ACCESS_TOKEN表示设备的访问令牌：Gumb1ylEgruSmNXWRyQf

具体形式如下所示：前半部分为发送的数据内容，25表示要发送的温度数据

`curl -v -X POST -d "{\"temperature\": 25}" http://localhost:8080/api/v1/Gumb1ylEgruSmNXWRyQf/telemetry --header "Content-Type:application/json"`

返回HTTP/1.1 200表示数据发送成功，遥感数据可在thingsboard中查看到

#### 通过MQTTBox发送数据

其中MQTTBox的相关基础配置如下：

![image-20240302135502612](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302135502612.png)

数据发送界面配置：

![image-20240302135858939](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302135858939.png)

#### 温度异常警告案例

首先在设备配置中添加设备配置包括设备的报警规则

![image-20240302145840970](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145840970.png)

![image-20240302145915985](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145915985.png)

再设置资产和添加设备

![image-20240302150141761](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302150141761.png)

添加的设备配置需要选择之前配置的一个设备配置

![image-20240302150242565](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302150242565.png)

在资产界面将设备的关系分配到创建的资产中

![image-20240302150507322](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302150507322.png)

可视化界面的配置，在仪表板库新建一个仪表板，只需输入标签，其他保持默认即可，再进入创建好仪表板的编辑模式，点击右上角的实体别名按钮，添加一个实体别名，其相关的配置如下：

![image-20240302150904969](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302150904969.png)

再添加一个别名为当前选则的设备

![image-20240302151003443](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302151003443.png)

再进行添加实体列表，点击添加新的部件->Cards->Entities table->添加数据源

![image-20240302151331556](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302151331556.png)

点击温度：temperature后面的铅笔进行修改，其修改为：

![image-20240302151315922](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302151315922.png)

之后就创建了一个实体窗口，里面有一个列表，点击仪表板库左上角的仪表板状态管理，添加一个状态用于显示当前的设备

![image-20240302151759049](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302151759049.png)

点击实体窗口右上角的铅笔编辑部件，再点击动作，添加一个动作

![image-20240302152445703](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302152445703.png)

添加完之后点击该实体就可以跳转到具体的配置部件窗口，在该窗口中就可以创建一些显示仪表板工具

添加数字量的展示控件：点击添加部件->Digital gauges->Digital thermometer

![image-20240302152922066](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302152922066.png)

可以在高级设置中设置其最大值和最小值

添加可视化折线图部件：点击Charts->Timeseries Line Chart

![image-20240302153248736](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302153248736.png)

可以在数据源中的标签中修改显示标签，在设置中修改折线图的标题，在高级选项中设置显示线条光滑

添加告警部件：点击Alarm widgets->Alarms table

![image-20240302153609147](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302153609147.png)

传输数据既可以通过之前的两种方式，还可以通过数据持续生成的规则链的js代码配置如下：

![image-20240302145020960](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145020960.png)

邮箱发送警告数据规则链如下：

![image-20240302145150483](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145150483.png)

其中script数据提取的js代码如下：

![image-20240302145242590](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145242590.png)

to email组装邮件的配置如下：

![image-20240302145356293](C:\Users\Asus\AppData\Roaming\Typora\typora-user-images\image-20240302145356293.png)