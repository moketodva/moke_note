### Maven安装步骤
> 版本问题：maven3.3+（包含）依赖jdk1.7+（包含）

- 一、下载安装Maven
- 二、已经装好jdk且配好环境变量
- 三、配好maven的环境变量，可以使用mvn -v查看版本来校验是否成功
- 四、配置本地仓库路径：修改setting.xml里的配置

```
<localRepository>your local repository path</localRepository>
```

- 五、配置镜像：修改setting.xml里的配置，使用阿里的镜像

```
<!--镜像就是一个拦截的概念，mirrorOf就是拦截那个仓库；这里使用通配符，所有都拦截-->
<mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

---

### Maven 仓库网站
- mvnrepository.com