# Spring 基础

## Spring简史
```
第一阶段:xml配置
在Spring1.0的时候开发都是使用xml配置bean ,当项目扩大的时候将xml配置放在不同的配置文件里面
```
```
第二阶段:注解配置
2.0的时候spring提供了bean的注解(@component.@Service),大大减少配置量
```

```
第三阶段:java配置
3.0以及后面的4.0的时候spring都在使用java配置 以及springboot也在使用
```
> Spring提供了IOC容器,AOP,数据访问,web开发,消息,测试等相关的支持

>Spring使用简单的POJO(普通java对象)来进行企业开发,每个被Spring管理的Java对象都称为Bean,而Spring提供的IO
C用来初始化对象,解决对象间的依赖管理和对象的使用


## Spring的生态
1. Spring Boot: 使用默认的开发配置来实现快速的发
2. Spring XD: 用来简化大数据应用开发
3. Spring Cloud: 为分布式系统开发提供工具集
4. Spring Data: 对主流的关系型和NOSQL数据库的支持
5. Spring Integration: 通过消息机制对企业集成模式(EIP)的支持
6. Spring Batch: 简化及优化大量数据的批量处理
7. Spring Security: 通过认证和授权保护应用
8. Spring Web Services : 提供基于协议有限的SOAP/WEB 服务
9. Spring Session : 提供一个API及实现来管理用户的会话信息
