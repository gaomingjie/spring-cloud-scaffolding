# spring-cloud-scaffolding
spring cloud 脚手架
# 目的：
利用spring cloud生态圈里的各种组件，搭建一套完整的可实用的项目架构脚手架，只需稍加改造，就可以作为开发新项目的脚手架工程。

本脚手架目的：为减少重复架构，统一基础服务，让开发人员把重心放在各微服务的业务逻辑的开发上来。

## 本项目会覆盖的组件有：
* Spring Cloud Netflix Zuul网关服务器 <br>
* Spring Cloud Netflix Eureka发现服务器 <br>
* Spring Cloud Netflix Turbine断路器监控 <br>
* Spring Cloud Sleuth + Zipkin服务调用监控 <br>
* Sping Cloud Stream + RabbitMQ做异步消息 <br>
* Spring Data JPA做数据访问 <br>
* Spring Cloud Security + Oauth2.0做授权与资源保护 <br>
PS：没有集成 spring cloud config 是因为实用性不好，我所了解到部分的开源的配置中心的功能比cloud config好很多，后续我会集成来自携程的apollo来作为配置中心。

## 本项目使用的依赖版本是：
* Spring Cloud - Finchley.RELEASE <br>
* Spring Data - Lovelace-RELEASE <br>
* Spring Cloud Stream - Fishtown.M3 <br>
* Spring Boot - 2.0.3.RELEASE <br>



## 项目模块介绍：
* spring-cloud-eureka-server：eureka注册与发现服务。 <br>
* spring-cloud-zuul-server:[zuul网关服务](https://github.com/yipengcheng001/spring-cloud-scaffolding/blob/master/spring-cloud-zuul-server/README.md),同时作为oauth2的资源服务器，在网关层统一进行资源访问认证处理。 <br>
* spring-cloud-auth-server:认证、授权服务器。 <br>
* spring-cloud-turbine-server:断路器监控，用于汇总Hystrix服务断路器监控流。 <br>
* spring-cloud-admin-server:集成spring-boot-admin，用于对服务的监控，查看配置属性，日志的管理等，详见：[GITHUB:spring-boot-admin](https://github.com/codecentric/spring-boot-admin) <br>
* spring-cloud-common：接口共享方式实现的API项目，API项目不包含任何服务端实现，因此这里只是引入了feign组件。在API接口项目中，我们一般定义，一是服务接口定义，二是传输数据DTO定义,三是公共的基础类。 <br>
* spring-cloud-investservice-server:业务服务模块 <br>
* spring-cloud-projectservice-server:业务服务模块 <br>
* spring-cloud-userservice-server:业务服务模块<br>
* spring-cloud-payservice-server:支付平台服务模块 涉及支付相关逻辑代码放入其中，目前只集成了微信的jsApi方式调用支付的方式。<br>
* spring-cloud-projectservice-listener：业务服务模块，mq监听服务，可以和project-server合并，此处用于方便理解，进行拆分。 <br>
* zipkin，用于做服务调用监控、收集分布式追踪信息，spring cloud 升级到Finchley.RELEASE版本、spring boot升级到2.XX版本后，zipkin官网不建议自行集成，所以提供了下载jar包进行部署的方式。搭建方式：[GITHUB:zipkin](https://github.com/openzipkin/zipkin),jar包下载地址：[zipkinjar包下载](https://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/) 此处使用2.11.6版本。 <br>

## 表结构：
* user_auth表用于oauth2的用户信息记录。<br>
表结构如下：<br>
```

CREATE TABLE `user_auth` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `password` varchar(255) DEFAULT NULL,
  `username` varchar(255) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `UK_sb8bbouer5wak8vyiiy4pf2bx` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';

```

## 总结：
* Eureka服务注册发现<br>
* Feign服务远程调用<br>
* Hystrix服务断路器<br>
* Turbine断路器监控聚合<br>
* Stream做异步处理<br>
* leuth和Zipkin服务调用链路监控<br>
* Zuul服务网关和自定义过滤器<br>
* JPA数据访问和Redisson分布式锁<br>
* security做授权与认证<br>
* swagger做为标准的REST API文档使用 详见:[集成Swagger](https://github.com/yipengcheng001/spring-cloud-scaffolding/blob/master/readme/swagger.md)<br>

后续会进行添加配置中心apollo与分布式事务管理中间件，后续也会集成当当的sharding-jdbc数据库中间件以Druid为数据源做分库分表与读写分离的构建。<br>
