### Maven生命周期
> Maven 有三条各自独立的生命周期

> phase的命令行执行会依次执行该phase之前的其它phase的命令行

##### 完整的生命周期

###### clean的lifecycle
phase | 说明
---|---
pre-clean | 执行清理之前的流程
clean | 清理项目构建的文件（常用）
post-clean | 执行清理之后的流程

###### default的lifecycle
phase | 说明
---|---
validate | 验证项目是否正确的、所有必要的信息是否可用
initialize | 初始化构建状态，例如设置一些初始化属性以及创建目录等
generate-sources | 生成源代码
process-sources | 处理源代码
generate-resources | 生成资源文件
process-resources | 处理资源文件
compile | 编译（常用）
process-classes | 处理.class文件，例如字节码增强
generate-test-sources | 生成测试源代码
process-test-sources | 处理测试源代码
generate-test-resources | 生成测试资源文件
process-test-resources | 处理测试资源文件
test-compile | 测试源代码编译
process-test-classes | 处理测试的.class文件，例如字节码增强
test | 测试（常用）
prepare-package | 打包之前的准备工作
package | 打包（常用）
pre-integration-test | 集成测试前的准备工作，例如准备测试环境
integration-test | 集成测试
post-integration-test | 集成测试后的清理工作，例如清理测试环境
verify | 校验包的合法性
install | 发布到本地仓库（常用）
deploy | 发布到远程仓库（常用）

###### site的lifecycle
phase | 说明
---|---
pre-site | 生成项目的站点文档之前的流程
site | 生成项目的站点文档
post-site | 生成项目的站点文档之后的流程
site-deploy | 发布到远端

> *上面这些lifecycle（生命周期）各phase（阶段）的功能是依赖于maven的plugin（插件）的，生命周期阶段本身是不具备这些功能的

> 一个plugin提供了一个或多个goal（目标），每个goal做特点的事情，功能更加细化；因此，一个phase对应一个或多个goal

> goal的命令行格式mvn [plugin-name]:[goal-name]

> phase的命令行格式mvn [phase-name]

> 这下应该对maven中的生命周期（lifecycle）、阶段（phase）、插件（plugin）、目标（goal）有了大致了解了

##### 内置生命周期的绑定（phase与goal的绑定）

###### 清理
phase | goal
---|---
clean | clean:clean

###### packaging为jar/war时
phase | goal
---|---
process-resources | resources:resources
compile | compiler:compile
process-test-resources | resources:testResources
test-compile | compiler:testCompile
test | surefire:test
package | jar:jar 或 war:war 或 ...
install |  install:install
deploy | deploy:deploy

###### packaging为pom时
phase | goal
---|---
package | site:attach-descriptor
install |  install:install
deploy | deploy:deploy

###### 站点
phase | goal
---|---
site | site:site
deploy |  site:deploy

> 既然有默认绑定，也肯定有指定绑定。在pom.xml里使用Execution标签可以给指定的phase配置指定的goal 

> Spring Boot项目中的spring-boot-maven-plugin已经依赖好maven插件以及一些配置

---

### Pom.xml 内常用标签介绍

##### maven坐标

> 通过坐标标识资源

```
groupId：项目组名（项目名）
artifactId：项目名（模块名）
version：项目的当前版本
packaging：项目的打包方式
```

```
<modelVersion>：POM模型版本
<parent>：继承父类依赖，好处是版本统一、消除冗余
<relativePath>：<parent>里的子标签，指定父pom.xml的相对路径,默认为../pom.xml
<name>：项目名，站点文档使用
<url>：项目主页url，站点文档使用
```

```xml
<!--聚合，用于快速构建项目，若有很多很多模块项目，一个一个构建得做法肯定不明智，就通过这种标签一起构建-->
<modules>
    <module>a</module>
    <module>b</module>
    <module>c</module>
</modules>
```

```xml
<!--属性定义，提供引用。常用来定义版本，方便统一管理-->
<properties>
    <java.version>1.8</java.version>
    <druid-spring-boot-starter.version>1.1.9</druid-spring-boot-starter.version>
    <hutool.version>4.5.7</hutool.version>
</properties>
```

```xml
<!--项目需要的依赖，version标签里${}的写法是引用的意思，引用properties里定义的属性-->
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-typehandlers-jsr310</artifactId>
        <version>${mybatis-typehandlers-jsr310.version}</version>
    </dependency>
</dependencies>

<!--依赖管理，子类使用相同依赖（dependency）时若不使用版本号，则会自动使用父类的版本号-->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.squareup.okhttp3</groupId>
            <artifactId>mockwebserver</artifactId>
            <version>${okhttp3.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```


```xml
<!--构建配置，常用来指定插件-->
<build>
    <!--打包后的名字-->
    <finalName>myProject</finalName> 
    <!--各种插件配置，用法类似依赖dependency-->
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>${mybatis-generator-core.version}</version>
            <!--插件配置信息-->
            <configuration>
                <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
                <verbose>true</verbose>
                <overwrite>true</overwrite>
            </configuration>
        </plugin>
    </plugins>
    <!--类似dependencyManagement-->
    <pluginManagement>
        <plugins>
          <plugin>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
                  <source>1.8</source>
                  <target>1.8</target>
                  <encoding>${project.build.sourceEncoding}</encoding>
              </configuration>
          </plugin>
        </plugins>
    </pluginManagement>
</build>
```


##### 聊聊依赖

###### 依赖传递

```
依赖具有传递性，你的项目依赖A，A又依赖B，B又依赖C。那你的项目依赖了A、B、C
```
###### 依赖冲突

```
比如你的项目依赖A和V，而A的依赖链路又如下A->B->C,V的依赖链路则是V->X->Y->C。此时，都依赖了C，但是C的版本是不一致的。这种现象叫依赖冲突。

默认的依赖冲突解决策略是：选链路短；如果链路一样长，则选先声明的。

如果想要介入，自己选择版本，可以通过exclusion进行依赖排除
```
###### 依赖排除

```xml
<!--例子：-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```