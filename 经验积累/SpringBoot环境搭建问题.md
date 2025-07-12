# `SpringBoot`环境搭建问题

## 基本概念

该文档用于存放在`SpringBoot`项目搭建过程中遇到的一些系统环境搭建配置问题



## `Maven`初始化编译报错

[ERROR] Failed to execute goal org.apache.maven.plugins:maven-archetype-plugin:3.4.0:generate (default-cli) on project standalone-pom: The plugin org.apache.maven.plugins:maven-archetype-plugin:3.4.0 requires Maven version 3.6.3 -> [Help 1] [ERROR]

> 解释：在执行Maven项目中的`install`命令时，遇到编译插件版本不匹配的错误。具体报错为：`maven-compiler-plugin:3.13.0`要求`Maven`版本至少为`3.6.3`。解决方案是将`Maven`版本升级到`3.6.3`或降低插件版本。

解决方案：

1. 从官网下载`Maven3.6.3`版本，具体的链接为：[https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip](https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip?spm=a2c6h.12873639.article-detail.5.263b36a1UeDqki&file=apache-maven-3.6.3-bin.zip)

2. 将`Maven`压缩包解压到指定的文件夹，一般放在`D`盘，与老版本的`Maven`并列即可

3. 配置环境变量，将解压后的文件中的`bin`目录配置到`path`环境变量中，注意要在之前老版本的配置之前（这里可以将老版本的`path`删除），在`cmd`中运行`mvn -v`验证是否是最新的版本

4. 在文件夹中创建`mvn_resp`本地仓库（与`bin`同级）（将旧版本的`mvn_resp`文件夹复制过来，如果旧版本没有内容可以不复制）

5. 编辑`conf/settings.xml`

   - 配置自己的本地仓库路径：

     `<localRepository>D:\MySoftware\Developing\apache-maven-3.6.3\mvn_resp</localRepository>`

   - 配置阿里云的`Maven`私服镜像

     ```xml
     <mirrors>
      <mirror>
         <id>alimaven</id>
         <name>aliyun maven</name>
         <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
         <mirrorOf>central</mirrorOf>
      </mirror>
     </mirrors>
     ```

6. 在`IDEA`中配置`Maven 3.6.3`（为新项目配置`Maven`环境）（在设置里面）



## `JDBC`数据库连接失败

对于配置文件：

```pr
db.driver.name=com.mysql.cj.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/easypan?useUnicode=true&characterEncoding=utf8
db.username=root
db.password=552259
```

我们需要注意：检查 `MySQL` 驱动版本是否匹配

- `MySQL 5.x` 使用 `com.mysql.jdbc.Driver`
- `MySQL 8.x` 使用 `com.mysql.cj.jdbc.Driver`

否则就会导致 `ClassNotFoundException` 或连接失败
