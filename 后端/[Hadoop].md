# Hadoop

## 基本信息

Master节点虚拟机：  内存至少8G，磁盘大小至少40G
ssh root@192.168.0.89
密码：lkt

Slave1节点虚拟机：  内存至少4G，磁盘大小至少20G

ssh root@192.168.0.133

密码：a

Slave2节点虚拟机：  内存至少4G，磁盘大小至少20G

ssh root@192.168.0.134

密码：a

每个虚拟机下都有一个hadoop用户，密码为：lkt



## 基本配置

参考配置网址：[Hadoop-3.3.6 集群的配置教程](https://blog.csdn.net/weixin_38735917/article/details/133521210)

### 节点虚拟机的配置

#### 用户配置

1. 创建hadoop用户：`useradd -m hadoop -s /bin/bash`

   查看是否成功创建用户：`cd /home`    `ls -l`

2. 设置登陆密码：`passwd hadoop`      设置为lkt

3. 将hadoop加入到sudo组里面：`usermod -aG sudo hadoop`，使用之前先查看cat /etc/sudoers中是否有 hadoop：`cat /etc/sudoers`

4. 验证用户hadoop的权限：`sudo -l -U hadoop`

5. 切换到用户 hadoop：`su - hadoop`

***

#### SSH配置

安装SSH，配置SSH无密码登陆：

1. 默认已安装了 SSH client，此外还需要安装 SSH server：`sudo apt-get install openssh-server`

2. 执行一次ssh localhost 需要输入密码，可以进行免密设置：

   ```ssh
   exit                           # 退出刚才的 ssh localhost
   cd ~/.ssh/                     # 若没有该目录，请先执行一次ssh localhost
   ssh-keygen -t rsa              # 会有提示，都按回车就可以
   cat ./id_rsa.pub >> ./authorized_keys  # 加入授权
   ```

***

#### Java安装和配置

Master和Slave节点虚拟机都需要进行配置

安装Java环境：`sudo apt install openjdk-8-jdk`

验证是否安装完成：`java -version`

查看安装路径：`update-alternatives --list java`

安装的路径为：`/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java`

设置环境变量：

```ssh
cd ~
vim ~/.bashrc
```

在这个文件的开头位置，添加如下几行内容：

```ssh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

使配置文件生效：`source ~/.bashrc`

***

#### hadoop-3.3.6安装和配置

Master节点虚拟机必须安装和配置，Slave节点虚拟机可以使用Master节点配置好的压缩包，通过`scp`传输过去

##### 安装

官网下载hadoop-3.3.6.tar.gz压缩包，传输到虚拟机中：

`scp -r ./hadoop-3.3.6.tar.gz root@192.168.0.89:/usr/local`

解压到当前的文件夹下：`sudo tar -zxvf hadoop-3.3.6.tar.gz -C /usr/local/`

将目录 `./hadoop-3.3.6` 及其所有子目录和文件的所有权赋予用户 `hadoop` ：

`sudo chown -R hadoop ./hadoop-3.3.6`

验证是否安装成功：如果 Hadoop 安装正确并设置了正确的环境变量，它将显示 Hadoop 的版本信息

```ssh
cd /usr/local/hadoop-3.3.6/
./bin/hadoop version
```

后续出现的 `./bin/...`，`./etc/...` 等包含 ./ 的路径，均为相对路径，以 /usr/local/hadoop 为当前目录

##### 配置

hadoop配置PATH变量：

为了可以使系统能够在任何目录下都能直接运行Hadoop的可执行文件，即hadoop、hdfs等命令，需要完成PATH变量配置

```ssh
cd ~
vim ~/.bashrc
```

顶部加入以下的内容：

```ssh
export HADOOP_HOME=/usr/local/hadoop-3.3.6
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```

运行`source ~/.bashrc`使配置生效

***

#### 主机名修改

修改主机名，方便后续识别：`sudo vim /etc/hostname`

修改主机名后重启：`sudo reboot`

***

#### 配置映射关系

在Master节点虚拟机输入：`sudo vim /etc/hosts`

在Master节点的hosts文件中增加如下两条IP和主机名映射关系，同时将原主机修改成新的主机名

```ssh
192.168.0.89 hadoopMaster
192.168.0.133 hadoopSlave1
192.168.0.134 hadoopSlave2
```

修改完后重新启动：`sudo reboot`

验证是否能ping通（ping主机名和ip地址）：`ping 主机名 -c 3`    `ping ip -c 3`

***

#### Master节点无密码SSH登录到Slave节点

Slave节点SSH无密码登录设置：（之前下载的时候设置过了）

在Master节点进行设置，使Master节点能够无密码SSH登录（进行授权，之前下载的时候设置过了）

在Master节点将上公匙传输到Slave节点：

`scp ~/.ssh/id_rsa.pub hadoop@hadoopSlave1:/home/hadoop/`    输入yes和HadoopSlave1主机的密码

传输完成后可以在根目录`~`下通过ls看到id_rsa.pub文件

在Slave节点上执行如下命令将SSH公匙加入授权：（所有Slave节点都需要配置）

```ssh
mkdir ~/.ssh       # 如果不存在该文件夹需先创建，若已存在，则忽略本命令
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
rm ~/id_rsa.pub    # 用完以后就可以删掉
```

这样，在Master节点上就可以无密码SSH登录到各个Slave节点了：

```ssh
cd ~
ssh hadoopSlave1
```

***

#### 配置集群/分布式环境

在配置集群/分布式模式时，需要修改“/usr/local/hadoop-3.3.6/etc/hadoop”目录下的配置文件

这里仅设置正常启动所必须的设置项，包括workers、core-site.xml、hdfs-site.xml、mapred-site.xml、yarn-site.xml共5个文件

切换到目录：`cd /usr/local/hadoop-3.3.6/etc/hadoop/`

##### 修改workers文件

`workers`文件是用于指定作为DataNode的主机列表的文件。每行包含一个主机名或IP地址，表示要作为DataNode的机器。

`vim worker`     将里面的localhost修改为Slave节点主机的地址，如hadoopSlave1

`workers`文件中只包含了`hadoopSlave1`，而没有其他主机，意味着只有`hadoopSlave1`被配置为运行DataNode角色。这表示`hadoopSlave1`主机将作为唯一的DataNode节点参与Hadoop集群。

##### 修改core-site.xml文件

打开文件：`vim core-site.xml`：修改为如下的内容

```ssh
<configuration>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>file:/usr/local/hadoop-3.3.6/tmp</value>
                <description>Abase for other temporary directories.</description>
        </property>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://hadoopMaster:9000</value>
        </property>
</configuration>
```

参数说明：

- hadoop.tmp.dir：用于指定临时文件的存储路径。它被设置为file:/usr/local/hadoop-3.3.6/tmp，表示临时文件将存储在指定的路径中

- fs.defaultFS：用于指定默认的文件系统和连接的NameNode节点。它被设置为hdfs://hadoopMaster:9000，表示Hadoop将使用HDFS作为默认的文件系统，并连接到hadoopMaster主机上运行的NameNode节点的RPC地址和端口（9000）

##### 修改hdfs-site.xml文件

对于Hadoop的分布式文件系统HDFS而言，一般都是采用冗余存储，冗余因子通常为3，也就是说，一份数据保存三份副本，几个Slave节点，dfs.replication的值就设置为几

```ssh
<configuration>
        <property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>hadoopMaster:50090</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:/usr/local/hadoop-3.3.6/tmp/dfs/name</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:/usr/local/hadoop-3.3.6/tmp/dfs/data</value>
        </property>
</configuration>
```

参数说明：

- dfs.namenode.secondary.http-address：用于配置辅助（Secondary）NameNode 的 HTTP 地址，辅助 NameNode 的 HTTP 地址被设置为 hadoopMaster:50090，表示辅助 NameNode 可以通过 hadoopMaster主机的端口 50090 进行访问。


- dfs.replication：用于配置文件块的副本数量。副本数量被设置为 1，表示每个文件块只有一个副本。副本数量的设置对数据的冗余和可靠性有影响。


- dfs.namenode.name.dir：用于配置主（Primary）NameNode 的名称目录路径。名称目录路径被设置为 file:/usr/local/hadoop-3.3.6/tmp/dfs/name，表示主 NameNode 的名称数据将存储在本地文件系统的 /usr/local/hadoop-3.3.6/tmp/dfs/name 路径下。


- dfs.datanode.data.dir：用于配置数据节点（Datanode）的数据目录路径。数据目录路径被设置为 file:/usr/local/hadoop-3.3.6/tmp/dfs/data，表示数据节点的数据将存储在本地文件系统的 /usr/local/hadoop-3.3.6/tmp/dfs/data 路径下。

如果Hadoop集群中的`hadoopMaster`主机仅用作NameNode，而不是同时兼具DataNode角色，那么确实不需要在`hdfs-site.xml`配置文件中包含`dfs.namenode.rpc-address`配置项

`dfs.namenode.rpc-address`配置项用于指定NameNode的RPC地址和端口，用于与其他DataNode节点进行通信。在纯粹的NameNode节点上，不需要与其他DataNode节点进行通信，因此可以省略这个配置项。

##### 修改mapred-site.xml文件

修改为以下的内容：

```ssh
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.address</name>
                <value>hadoopMaster:10020</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.webapp.address</name>
                <value>hadoopMaster:19888</value>
        </property>
        <property>
                <name>yarn.app.mapreduce.am.env</name>
                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.3.6</value>
        </property>
        <property>
                <name>mapreduce.map.env</name>
                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.3.6</value>
        </property>
        <property>
                <name>mapreduce.reduce.env</name>
                <value>HADOOP_MAPRED_HOME=/usr/local/hadoop-3.3.6</value>
        </property> 
</configuration>
```

参数说明：

- mapreduce.framework.name：用于配置 MapReduce 框架的名称。框架名称被设置为 yarn，表示使用 YARN（Yet Another Resource Negotiator）作为 MapReduce 的执行框架。


- mapreduce.jobhistory.address：用于配置作业历史服务器（Job History Server）的地址。作业历史服务器的地址被设置为 hadoopMaster:10020，表示作业历史服务器可以通过 hadoopMaster主机的端口 10020 进行访问。


- mapreduce.jobhistory.webapp.address：用于配置作业历史服务器的 Web 应用程序地址。作业历史服务器的 Web 应用程序地址被设置为 hadoopMaster:19888，表示可以通过 hadoopMaster主机的端口 19888 访问作业历史服务器的 Web 页面。


- yarn.app.mapreduce.am.env：用于配置 MapReduce 应用程序管理器（ApplicationMaster）的环境变量。将 HADOOP_MAPRED_HOME 设置为 /usr/local/hadoop-3.3.6，以指定 MapReduce 应用程序管理器使用的 Hadoop MapReduce 的安装路径。


- mapreduce.map.env 和 mapreduce.reduce.env：用于配置 Map 和 Reduce 任务的环境变量。都将 HADOOP_MAPRED_HOME 设置为 /usr/local/hadoop-3.3.6，以指定 Map 和 Reduce 任务使用的 Hadoop MapReduce 的安装路径。

##### 修改yarn-site.xml文件

修改为以下的内容

```ssh
<configuration>
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>hadoopMaster</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>
```

参数说明：

- yarn.resourcemanager.hostname：用于配置资源管理器（ResourceManager）的主机名。源管理器的主机名被设置为 hadoopMaster，表示资源管理器运行在名为 hadoopMaster的主机上。


- yarn.nodemanager.aux-services：用于配置节点管理器（NodeManager）的辅助服务。辅助服务被设置为 mapreduce_shuffle，表示节点管理器将提供 MapReduce Shuffle 服务。

***

#### 打包配置后的hadoop-3.3.6文件

先删除hadoopMaster主机下的原压缩文件：`sudo rm -r hadoop-3.3.6.tar.gz`

压缩配置好的hadoop-3.3.6文件：`tar -zvcf ~/hadoop.master.tar.gz ./hadoop-3.3.6/`，后续用于部署到Slave节点主机上

查看是否压缩成功：

```ssh
cd ~
ls -l
```

将本地的 hadoop.master.tar.gz 压缩文件复制到远程主机 hadoopSlave1 的 /home/hadoop 目录下:

`scp ./hadoop.master.tar.gz hadoopSlave1:/home/hadoop`

在主机hadoopSlave1中删掉旧的hadoop-3.3.6文件：`sudo rm -r /usr/local/hadoop-3.3.6` 

将压缩包进行解压：`sudo tar -zvxf ~/hadoop.master.tar.gz -C /usr/local`

修改目录和其子目录的所有者权限：`sudo chown -R hadoop /usr/local/hadoop-3.3.6`



## 基本使用

### 启动hadoop集群

首次启动Hadoop集群时，需要先在Master节点执行名称节点的格式化（只需要执行这一次，后面再启动Hadoop时，不要再次格式化名称节点），命令如下：

```ssh
cd /usr/local/hadoop-3.3.6
./bin/hdfs namenode -format
```

启动Hadoop，启动需要在Master节点上进行，执行如下命令：（后续启动Master节点都需要运行）

```ssh
cd /usr/local/hadoop-3.3.6
./sbin/start-dfs.sh
./sbin/start-yarn.sh
./sbin/mr-jobhistory-daemon.sh start historyserver
```

通过命令`jps`可以查看各个节点所启动的进程，Master节点和Slave节点都要看，在`cd /usr/local/hadoop-3.3.6`路径下输入`jps`命令

另外还需要在Master节点上通过“”查看数据节点是否正常启动：

```ssh
cd /usr/local/hadoop-3.3.6
./bin/hdfs dfsadmin -report
```

如果集群以前能启动，但后来启动不了，特别是数据节点无法启动，不妨试着删除所有节点（包括Slave节点）上的“/usr/local/hadoop-3.3.6/tmp”文件夹，再重新执行一次“hdfs namenode -format”，再次启动即可

***

### 执行分布式实例

执行分布式实例，首先创建HDFS上的用户目录，可以在Master节点（hadoopMaster）上执行如下命令：

```ssh
echo $PATH | tr ':' '\n'   #  查看PATH变量是否配置成功
hdfs dfs -mkdir -p /user/hadoop   #  此前已经配置了PATH环境变量，所以不用路径全称
hdfs dfs -ls /user       #  查看是否创建成功
```

然后，在HDFS中创建一个input目录，并把“/usr/local/hadoop-3.3.6/etc/hadoop”目录中的配置文件作为输入文件复制到input目录中，命令如下：

```ssh
hdfs dfs -mkdir input
hdfs dfs -put /usr/local/hadoop-3.3.6/etc/hadoop/*.xml input
```

接着就可以运行 MapReduce 作业了，命令如下：

```ssh
hadoop jar /usr/local/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'
```

可以在`http://192.168.0.89:8088/cluster`中进行查看进度

查看输出结果：`hdfs dfs -cat output/*`

***

### 关闭hadoop集群

最后，关闭Hadoop集群，需要在Master节点（hadoopMaster）执行如下命令：

```ssh
stop-yarn.sh
stop-dfs.sh
mr-jobhistory-daemon.sh stop historyserver
```



## web总览

访问控制总览web：`http://192.168.0.89:9870`
