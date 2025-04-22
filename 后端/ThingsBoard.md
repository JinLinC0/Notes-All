# `ThingsBoard`

## 基本概念

`ThingsBoard`是一个开源的物联网平台，可以实现物联网项目的快速开发、管理和拓展，提供了成熟的SaaS服务化架构解决方案，用于设置管理、设备控制、数据收集、可视化，并支持标准的`MQTT`、`CoAP`、`LWM2M`、`SNMP`及`HTTP`协议连接物联网设备



## 环境搭建

### `Ubuntu`系统下的搭建

在`Ubuntu`操作系统中进行搭建：

基础配置

- `sudo apt update`
- `sudo apt install nodejs`
- `sudo apt install npm`
- `sudo apt install openjdk-11-jdk`
- `sudo apt install maven`
- `sudo apt install curl`
- `curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -`
- `echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list`
- `sudo apt install yarn`

下载`thingsboard`源码，进入下载下来的源码目录，编译`thingsboard`源码：

- `mvn clean install -DskipTests`

遇到`Server UI FAILURE`错误的问题，解决方案：

网络问题造成的报错：继续编译：`mvn package -DskipTests`    不要加`clean`，不然之前编译好了的又得重新编译

***

### `Windows`系统中进行搭建

环境安装在操作系统为`windows`平台下进行的

- 数据库安装：`https://www.enterprisedb.com/downloads/postgres-postgresql-downloads#windows`

  安装数据库时设置数据库密码，可以设置`thingsboard`的配置文件中对应的密码：`postgres`

  安装完数据库后，打开数据库，在主界面中点击`Add New Server`，在`Gerneral`选项卡中设置`Name`为：`localhost`，其他不用设置；在`Connection`选项卡进行如下的设置：密码设置为：`postgres`

- `JDK`安装：`https://adoptopenjdk.net/index.html`

  安装`JDK`后添加系统变量：

  在系统变量中找到PATH变量，点击编辑，在其中添加`%JAVA_HOME%\bin`

- `ThingsBoard`安装：`https://github.com/thingsboard/thingsboard/releases/download/v3.4.1/thingsboard-windows-3.4.1.zip`

- `ThingsBoard`目录的相关文件：`conf`是一个配置文件，用于修改主要的配置参数

在服务管理界面设置`postgredql-x64-15`的属性为本地系统账户(可以不设置，创建`server`错误时可尝试更改)

最后在`localhost`服务器中创建一个数据库`Database`为：`thingsboard`

打开管理员`cmd`，进入`thingsboard`文件夹中，输入：`install.bat`

启动服务：`net start thingsboard`

打开浏览器：访问：`http://localhost:8080/`进入本地`thingsboard`管理界面

默认账号：

系统管理员：`sysadmin@thingsboard.org`    密码：`sysadmin`

租户管理员：`tenant@thingsboard.org`   密码：`tenant`

客户：`customer@thingsboard.org`   密码：`customer`



## 基本使用

### 设置邮箱

进入`QQ`邮箱界面，点击设置，找到`POP3/IMAP/SMTP/Exchange/CardDAV/CalDAV`服务

授权码：`pemmftzubgxmdffg`

其中密码就是授权码，修改完后需要对系统管理员的属性进行使用邮箱的更改，并注销后重新登录，再点击发送测试邮件，验证是否绑定成功

***

### 传递数据

传递数据有两种方式

- 通过`http`方式发送数据

  `$HOST_NAME/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"`

  `$HOST_NAME`表示主机名和端口号：`http://localhost:8080`

  `$ACCESS_TOKEN`表示设备的访问令牌：`Gumb1ylEgruSmNXWRyQf`

  具体形式如下所示：前半部分为发送的数据内容，25表示要发送的温度数据

  `curl -v -X POST -d "{\"temperature\": 25}" http://localhost:8080/api/v1/Gumb1ylEgruSmNXWRyQf/telemetry --header "Content-Type:application/json"`

  返回`HTTP/1.1 200`表示数据发送成功，遥感数据可在thingsboard中查看到

- 通过`MQTT`发送数据