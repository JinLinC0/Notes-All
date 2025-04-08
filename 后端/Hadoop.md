# `Hadoop`

## 基本概念

`Hadoop `是一个开源的分布式计算框架，用于处理和分析海量数据集。它最初由 `Doug Cutting `和` Mike Cafarella` 开发，`Hadoop `广泛应用于：大数据分析、日志处理、推荐系统、数据仓库、机器学习、金融分析和生物信息学等领域等

随着技术的发展，虽然` Hadoop MapReduce `的部分功能已被 `Spark `等更快的引擎取代，但 `HDFS `和` YARN` 仍然是许多大数据架构的基础组件



## 节点虚拟机配置

参考配置网址：[Hadoop-3.3.6 集群的配置教程](https://blog.csdn.net/weixin_38735917/article/details/133521210)

***

### 配置信息

三台虚拟机，一台作为`Master`，两台作为`Slave`

`Master`节点虚拟机：  内存至少8G，磁盘大小至少40G
`ssh root@192.168.0.99`
密码：`jlc`

`Slave1`节点虚拟机：  内存至少4G，磁盘大小至少20G

`ssh root@192.168.0.163`

密码：`a`

`Slave2`节点虚拟机：  内存至少4G，磁盘大小至少20G

`ssh root@192.168.0.164`

密码：`a`

每个虚拟机下都有一个`hadoop`用户，密码为：`jlc`

***

### 用户创建和配置

1. 为虚拟机创建`hadoop`用户：`useradd -m hadoop -s /bin/bash`

   查看是否成功创建用户：`cd /home`    `ls -l`

2. 设置登陆密码：`passwd hadoop`      设置为`jlc`

3. 将`hadoop`加入到`sudo`组里面：`usermod -aG sudo hadoop`，使用之前先查看`cat /etc/sudoers`中是否有 `hadoop`用户

4. 验证用户`hadoop`的权限：`sudo -l -U hadoop`

5. 切换到` hadoop`用户：`su - hadoop`

***

### `SSH`配置

安装`SSH`，配置`SSH`无密码登陆：

1. `ubuntu`系统默认已安装了` SSH client`，此外还需要安装`SSH server`：`sudo apt-get install openssh-server`

2. 执行一次`ssh localhost `需要输入密码，可以进行免密设置：

   ```txt
   exit                           # 退出刚才的 ssh localhost
   cd ~/.ssh/                     # 若没有该目录，请先执行一次ssh localhost
   ssh-keygen -t rsa              # 会有提示，都按回车就可以
   cat ./id_rsa.pub >> ./authorized_keys  # 加入授权
   ```

***

### `Java`安装和配置

`Master`和`Slave`节点虚拟机都需要进行配置

安装`Java`环境：`sudo apt install openjdk-8-jdk`

验证是否安装完成：`java -version`

查看安装路径：`update-alternatives --list java`

安装的路径为：`/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java`

配置环境变量：

```txt
cd ~
vim ~/.bashrc
```

在这个文件的开头位置，添加如下几行内容（配置环境变量）：

```txt
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

使配置文件生效：`source ~/.bashrc`

***

### `hadoop-3.3.6`安装和配置

`Master`节点虚拟机必须安装和配置，`Slave`节点虚拟机可以用`Master`节点配置好的压缩包，通过`scp`传输过去

#### 安装

官网下载`hadoop-3.3.6.tar.gz`压缩包，传输到虚拟机中：

`scp -r ./hadoop-3.3.6.tar.gz root@192.168.0.99:/usr/local`

解压到当前的文件夹下：`sudo tar -zxvf hadoop-3.3.6.tar.gz -C /usr/local/`

将目录 `./hadoop-3.3.6` 及其所有子目录和文件的所有权赋予用户 `hadoop` ：

`sudo chown -R hadoop ./hadoop-3.3.6`

验证是否安装成功：如果 `Hadoop` 安装正确并设置了正确的环境变量，它将显示 `Hadoop `的版本信息

```txt
cd /usr/local/hadoop-3.3.6/
./bin/hadoop version
```

后续出现的 `./bin/...`，`./etc/...` 等包含` ./ `的路径，均为相对路径，以` /usr/local/hadoop `为当前目录

#### 配置

`hadoop`配置`PATH`变量：

为了可以使系统能够在任何目录下都能直接运行`Hadoop`的可执行文件，即`hadoop`、`hdfs`等命令，需要完成`PATH`变量配置

```txt
cd ~
vim ~/.bashrc
```

顶部加入以下的内容：

```txt
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

### 主机名修改

修改主机名，方便后续识别：`sudo vim /etc/hostname`

修改主机名后重启：`sudo reboot`

***

### 配置映射关系

在`Master`节点虚拟机输入：`sudo vim /etc/hosts`

在`Master`节点的`hosts`文件中增加如下两条IP和主机名映射关系，同时将原主机修改成新的主机名

```txt
192.168.0.99 hadoopMaster
192.168.0.163 hadoopSlave1
192.168.0.164 hadoopSlave2
```

修改完后重新启动：`sudo reboot`

验证是否能`ping`通（`ping`主机名和`ip`地址）：`ping 主机名 -c 3`    `ping ip -c 3`

***

### `Master`节点无密码`SSH`登录到`Slave`节点

`Slave`节点`SSH`无密码登录设置：（之前下载的时候设置过了）

在`Master`节点进行设置，使`Master`节点能够无密码`SSH`登录（进行授权，之前下载的时候设置过了）

在`Master`节点将上公匙传输到`Slave`节点：

`scp ~/.ssh/id_rsa.pub hadoop@hadoopSlave1:/home/hadoop/`    输入`yes`和`HadoopSlave1`主机的密码

传输完成后可以在根目录`~`下通过`ls`看到`id_rsa.pub`文件

在`Slave`节点上执行如下命令将`SSH`公匙加入授权：（所有`Slave`节点都需要配置）

```txt
mkdir ~/.ssh       # 如果不存在该文件夹需先创建，若已存在，则忽略本命令
cat ~/id_rsa.pub >> ~/.ssh/authorized_keys
rm ~/id_rsa.pub    # 用完以后就可以删掉
```

这样，在`Master`节点上就可以无密码`SSH`登录到各个`Slave`节点了：

```txt
cd ~
ssh hadoopSlave1
```

***

### 配置集群/分布式环境

在配置集群/分布式模式时，需要修改`“/usr/local/hadoop-3.3.6/etc/hadoop”`目录下的配置文件

这里仅设置正常启动所必须的设置项，包括`workers`、`core-site.xml`、`hdfs-site.xml`、`mapred-site.xml`、`yarn-site.xml`共5个文件

切换到目录：`cd /usr/local/hadoop-3.3.6/etc/hadoop/`

#### 修改`workers`文件

`workers`文件是用于指定作为`DataNode`的主机列表的文件。每行包含一个主机名或`IP`地址，表示要作为`DataNode`的机器。

`vim worker`     将里面的`localhost`修改为`Slave`节点主机的地址，如`hadoopSlave1`

`workers`文件中只包含了`hadoopSlave1`，而没有其他主机，意味着只有`hadoopSlave1`被配置为运行`DataNode`角色。这表示`hadoopSlave1`主机将作为唯一的`DataNode`节点参与`Hadoop`集群

#### 修改`core-site.xml`文件

打开文件：`vim core-site.xml`：修改为如下的内容

```txt
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

- `hadoop.tmp.dir`：用于指定临时文件的存储路径。它被设置为`file:/usr/local/hadoop-3.3.6/tmp`，表示临时文件将存储在指定的路径中

- `fs.defaultFS`：用于指定默认的文件系统和连接的`NameNode`节点。它被设置为`hdfs://hadoopMaster:9000`，表示`Hadoop`将使用`HDFS`作为默认的文件系统，并连接到`hadoopMaster`主机上运行的`NameNode`节点的`RPC`地址和端口（9000）

#### 修改`hdfs-site.xml`文件

对于`Hadoop`的分布式文件系统`HDFS`而言，一般都是采用冗余存储，冗余因子通常为3，也就是说，一份数据保存三份副本，几个`Slave`节点，`dfs.replication`的值就设置为几

```txt
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

- `dfs.namenode.secondary.http-address`：用于配置辅助（`Secondary`）`NameNode` 的 `HTTP `地址，辅助 `NameNode` 的 `HTTP` 地址被设置为`hadoopMaster:50090`，表示辅助` NameNode `可以通过 `hadoopMaster`主机的端口 50090 进行访问。


- `dfs.replication`：用于配置文件块的副本数量。副本数量被设置为 1，表示每个文件块只有一个副本。副本数量的设置对数据的冗余和可靠性有影响。


- `dfs.namenode.name.dir`：用于配置主（`Primary`）`NameNode` 的名称目录路径。名称目录路径被设置为 `file:/usr/local/hadoop-3.3.6/tmp/dfs/name`，表示主` NameNode` 的名称数据将存储在本地文件系统的 `/usr/local/hadoop-3.3.6/tmp/dfs/name` 路径下。


- `dfs.datanode.data.dir`：用于配置数据节点（`Datanode`）的数据目录路径。数据目录路径被设置为 `file:/usr/local/hadoop-3.3.6/tmp/dfs/data`，表示数据节点的数据将存储在本地文件系统的 `/usr/local/hadoop-3.3.6/tmp/dfs/data `路径下。

如果`Hadoop`集群中的`hadoopMaster`主机仅用作`NameNode`，而不是同时兼具`DataNode`角色，那么确实不需要在`hdfs-site.xml`配置文件中包含`dfs.namenode.rpc-address`配置项

`dfs.namenode.rpc-address`配置项用于指定`NameNode`的`RPC`地址和端口，用于与其他`DataNode`节点进行通信。在纯粹的`NameNode`节点上，不需要与其他`DataNode`节点进行通信，因此可以省略这个配置项。

#### 修改`mapred-site.xml`文件

修改为以下的内容：

```txt
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

- `mapreduce.framework.name`：用于配置` MapReduce` 框架的名称。框架名称被设置为 `yarn`，表示使用 `YARN（Yet Another Resource Negotiator）`作为 `MapReduce `的执行框架。


- `mapreduce.jobhistory.address`：用于配置作业历史服务器`（Job History Server）`的地址。作业历史服务器的地址被设置为` hadoopMaster:10020`，表示作业历史服务器可以通过` hadoopMaster`主机的端口 10020 进行访问。


- `mapreduce.jobhistory.webapp.address`：用于配置作业历史服务器的` Web` 应用程序地址。作业历史服务器的 `Web `应用程序地址被设置为` hadoopMaster:19888`，表示可以通过 `hadoopMaster`主机的端口 19888 访问作业历史服务器的` Web `页面。


- `yarn.app.mapreduce.am.env`：用于配置 `MapReduce` 应用程序管理器`（ApplicationMaster）`的环境变量。将 `HADOOP_MAPRED_HOME` 设置为` /usr/local/hadoop-3.3.6`，以指定` MapReduce `应用程序管理器使用的 `Hadoop MapReduce` 的安装路径。


- `mapreduce.map.env` 和 `mapreduce.reduce.env`：用于配置` Map `和` Reduce `任务的环境变量。都将 `HADOOP_MAPRED_HOME `设置为 `/usr/local/hadoop-3.3.6`，以指定` Map `和` Reduce` 任务使用的` Hadoop MapReduce` 的安装路径。

#### 修改`yarn-site.xml`文件

修改为以下的内容

```txt
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

- `yarn.resourcemanager.hostname`：用于配置资源管理器`（ResourceManager）`的主机名。源管理器的主机名被设置为` hadoopMaster`，表示资源管理器运行在名为` hadoopMaster`的主机上。


- `yarn.nodemanager.aux-services`：用于配置节点管理器`（NodeManager）`的辅助服务。辅助服务被设置为 `mapreduce_shuffle`，表示节点管理器将提供 `MapReduce Shuffle `服务。

***

### 打包配置后的`hadoop-3.3.6`文件

先删除`hadoopMaster`主机下的原压缩文件：`sudo rm -r hadoop-3.3.6.tar.gz`

压缩配置好的`hadoop-3.3.6`文件：`tar -zvcf ~/hadoop.master.tar.gz ./hadoop-3.3.6/`，后续用于部署到`Slave`节点主机上

查看是否压缩成功：

```txt
cd ~
ls -l
```

将本地的` hadoop.master.tar.gz `压缩文件复制到远程主机` hadoopSlave1` 的` /home/hadoop`目录下:

`scp ./hadoop.master.tar.gz hadoopSlave1:/home/hadoop`

在主机`hadoopSlave1`中删掉旧的`hadoop-3.3.6`文件：`sudo rm -r /usr/local/hadoop-3.3.6` 

将压缩包进行解压：`sudo tar -zvxf ~/hadoop.master.tar.gz -C /usr/local`

修改目录和其子目录的所有者权限：`sudo chown -R hadoop /usr/local/hadoop-3.3.6`



## 基本使用

### 启动`hadoop`集群

首次启动`Hadoop`集群时，需要先在`Master`节点执行名称节点的格式化（只需要执行这一次，后面再启动`Hadoop`时，不要再次格式化名称节点），命令如下：

```txt
cd /usr/local/hadoop-3.3.6
./bin/hdfs namenode -format
```

启动`Hadoop`，启动需要在`Master`节点上进行，执行如下命令：（后续启动`Master`节点都需要运行）

```txt
cd /usr/local/hadoop-3.3.6
./sbin/start-dfs.sh
./sbin/start-yarn.sh
./sbin/mr-jobhistory-daemon.sh start historyserver
```

通过命令`jps`可以查看各个节点所启动的进程，`Master`节点和`Slave`节点都要看，在`cd /usr/local/hadoop-3.3.6`路径下输入`jps`命令

另外还需要在`Master`节点上通过“”查看数据节点是否正常启动：

```txt
cd /usr/local/hadoop-3.3.6
./bin/hdfs dfsadmin -report
```

如果集群以前能启动，但后来启动不了，特别是数据节点无法启动，不妨试着删除所有节点（包括`Slave`节点）上的`“/usr/local/hadoop-3.3.6/tmp”`文件夹，再重新执行一次`“hdfs namenode -format”`，再次启动即可

***

### 执行分布式实例

执行分布式实例，首先创建`HDFS`上的用户目录，可以在`Master`节点`（hadoopMaster）`上执行如下命令：

```txt
echo $PATH | tr ':' '\n'   #  查看PATH变量是否配置成功
hdfs dfs -mkdir -p /user/hadoop   #  此前已经配置了PATH环境变量，所以不用路径全称
hdfs dfs -ls /user       #  查看是否创建成功
```

然后，在`HDFS`中创建一个`input`目录，并把`“/usr/local/hadoop-3.3.6/etc/hadoop”`目录中的配置文件作为输入文件复制到`input`目录中，命令如下：

```txt
hdfs dfs -mkdir input
hdfs dfs -put /usr/local/hadoop-3.3.6/etc/hadoop/*.xml input
```

接着就可以运行 `MapReduce` 作业了，命令如下：

```txt
hadoop jar /usr/local/hadoop-3.3.6/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'
```

可以在`http://192.168.0.99:8088/cluster`中进行查看进度

查看输出结果：`hdfs dfs -cat output/*`

***

### 关闭`hadoop`集群

最后，关闭`Hadoop`集群，需要在`Master`节点`（hadoopMaster）`执行如下命令：

```txt
stop-yarn.sh
stop-dfs.sh
mr-jobhistory-daemon.sh stop historyserver
```

***

### `Web`总览

访问控制总览`web`：`http://192.168.0.99:9870`
