#使用Spring Cloud Config搭建配置中心

##配置文件的管理
Spring Cloud Config支持在Git, SVN和本地存放配置文件，使用Git或者SVN存储库可以很好地支持版本管理，Spring默认配置是使用Git存储库。在本案例中将使用OSChina提供的Git服务存储配置文件。为了能让Config客户端准确的找到配置文件，在管理配置文件项目my-sample-config中放置配置文件时，需要了解application, profile和label的概念：

- {application}映射到Config客户端的spring.application.name属性
- {profile}映射到Config客户端的spring.profiles.active属性，可以用来区分环境
- {label}映射到Git服务器的commit id, 分支名称或者tag，默认值为master

在天成安信公正项目案例中，由于存在服务端部署到客户现场的场景，因此统一的中心化配置管理非常重要。

1) 增加@EnableConfigServer注解

```
@SpringBootApplication
@EnableConfigServer
public class MyConfigServerApplication {
```
2) 配置application.yml文件，设置服务器端口号和配置文件Git仓库的链接

```
server:
  port: 8888
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github/chentian610/notary_config.git
          searchPaths: xxxxxx（比如hangzhou-xihu）
```