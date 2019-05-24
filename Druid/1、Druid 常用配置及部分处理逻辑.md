### Druid

##### 配置
```
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    public-key: yourPublicKey
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://127.0.0.1/school?useUnicode=true&characterEncoding=UTF-8&serverTimezone=GMT%2B8
      username: root
      password: yourPassword
      connection-properties: "config.decrypt=true;config.decrypt.key=${spring.datasource.public-key}"
      initial-size: 20
      min-idle: 20
      max-active: 50
      max-wait: 15000
      validation-query: SELECT 1
      test-while-idle: true
      test-on-borrow: false
      test-on-return: false
      keep-alive: true
      time-between-eviction-runs-millis: 10000
      min-evictable-idle-time-millis: 30000
      # 以下部分是过滤器配置以及内置的监控页
      web-stat-filter:
        enabled: false
      filters: stat,wall,slf4j
      filter:
        stat:
          slow-sql-millis: 1000
          log-slow-sql: true
          merge-sql: false
        config:
          # druid对数据源密码加密使用的是config过滤器,这里是启用该过滤器的开关
          enabled: true
      stat-view-servlet:
        enabled: true
        url-pattern: /druid/*
        login-username: admin
        login-password: admin
        reset-enable: false
```

##### 部分处理逻辑
```
> 注意：【】内的名词不是专业术语

0、【空闲连接数】：空闲的连接数
1、【检测连接有效性的请求方式】：ping方式和正常的sql请求方式（ping方式前提是相应的数据库驱动支持ping方式；ping方式性能上优异；正常的sql请求方式可用来观察）；
配置启动参数-Ddruid.mysql.usePingMethod=false可以禁掉ping方式，否则无论如何不会使用正常的sql请求方式
2、【连接池初始连接数】由initial-size决定，使用keep-alive会导致【连接池初始连接数】上升至min-idle和initial-size两值中的最大值
3、【连接池连接数上限】：max-active的结果
4、【连接有效性的检测方式】：有test-on-borrow、test-on-return、test-while-idle和keep-alive（建议1.1.16版本及以上使用），
依次表示取连接时触发检测、归还连接时触发检测、达到【空闲指标】且取连接的时候触发检测、达成【周期性检测指标】时触发；
建议使用test-while-idle和keep-alive方式，其它方式性能消耗较大
5、【连接空闲时间】：空闲连接的空闲时间，使用就会被重置
6、【检测或销毁的线程】：专门有一个线程周期性的进行检测或者销毁连接的处理，执行周期为time-between-eviction-runs-millis
7、【空闲指标】：【连接空闲时间】达到time-between-eviction-runs-millis算达到【空闲指标】
8、【周期性检测指标】：【空闲连接数】 ≤ min-idle时，【连接空闲时间】达到min-evictable-idle-time-millis算【周期性检测指标】
9、【销毁逻辑】：【检测或销毁的线程】发现满足了【销毁指标】的连接就会进行销毁
10、【销毁指标】：【空闲连接数】 > min-idle时，【连接空闲时间】达到min-evictable-idle-time-millis算达到；
【空闲连接数】 ≤ min-idle时,【连接空闲时间】达到max-evictable-idle-time-millis算达到【销毁指标】

>注意：销毁时间点是受 time-between-eviction-runs-millis周期影响，
导致销毁时间延长（【连接空闲时间】为0的时间点 + min-evictable-idle-time-millis ≤ 销毁时间点 ≤ 【连接空闲时间】为0的时间点 +  min-evictable-idle-time-millis + time-between-eviction-runs-millis）；
而检测时间点是不受影响的

# 伪代码
if 【空闲连接数】 > min-idle {
    if 【连接空闲时间】≥ min-evictable-idle-time-millis {
        return 【销毁指标】;
    }
} else {
    if 【连接空闲时间】≥ min-evictable-idle-time-millis {
        return 【周期性检测指标】;
    }
    if 【连接空闲时间】≥ max-evictable-idle-time-millis {
        return 【销毁指标】;
    }
}

```
##### 连接泄漏解决方式
```
# 加入如下配置

# 是否打开连接泄露检查
remove-abandoned: true
# 连接取出多长时间没归还算连接泄露，单位：秒
remove-abandoned-timeout:
# 是否关闭abanded连接时输出错误日志
log-abandoned: true
```
##### MySQL 8小时问题解决方式

```
1、尝试最新版本，如下配置应该能确保检测；max-evictable-idle-time-millis默认为7小时，进一步防止8小时问题

test-while-idle: true
keep-alive: true

2、修改mysql的配置文件，修改如下（治标不治本，最后手段）

wait_timeout = 86400
interactive_timeout = 86400

```


