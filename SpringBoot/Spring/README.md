# Spring 基础

> 注解本身是没有功能的,就和xml一样.注解和xml都是一种元数据,元数据即解释数据的数据.

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

## Spring项目的搭建工具 
Spring Tool Suit(STS)是Spring官方推出的基于Eclipse的开发工具,集成了M2E,Spring IDE等插件.

## Spring的基础配置
Spring框架本身的四大原则:
1. 使用POJO进行轻量级和最小侵入式开发
2. 通过依赖注入和基于接口编程实现松耦合
3. 通过AOP和默认习惯进行声明式编程
4. 使用AOP和模板(Template)减少模式化代码

## 依赖注入
> 所谓的依赖注入指的是容器负责承建对象和维护对象间的依赖关系,而不是通过对象本身负责组建的创建和解决组建的依赖.

```
声明Bean的注解
@Component组件,没有明确的角色
@Service在业务逻辑层(service层)使用
@Repository在数据访问层(dao层)使用
@Controller在展现层(MVC->SpringMVC)使用
```

```
注入Bean的注解
@Autowired: Spring提供的注解
@Inject: JSR-330提供的注解
@Resource: JSR-250提供的注解

上列三个注释可以注释在属性或者set方法上
```
```
配置注解
@Configuration 声明当前类是一个配置类
@ComponentScan 自动扫描包名下所以使用声明注解(@Service,@Component,@Repository和@Controller的类,并注册为bean
```

## JAVA配置
> Java配置是Spring4.x推荐的配置方式,可以完全替代xml配置;java配置也会SpringBoot腿甲的配置方式.
```
Java配置是通过@Configuration和@Bean来实现的
@Configuration声明当前的类是一个配置类,相当于一个Spring配置的xml文件.
@Bean注解在方法上,声明当前方法的返回值为一个Bean.
``

## AOP  (@Aspect)
> AOP 面向切面编程,相当于oop面向对象编程
> Spring的AOP存在的目的是为了解耦,AOP可以让一组类共享相同的行
- 用@Aspect声明式一个切面
- 使用@After, @Before, @Around定义建言(Advice),可以直接拦截规则(切点)作为参数




