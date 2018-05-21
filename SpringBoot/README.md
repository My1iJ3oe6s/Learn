# SpringBoot


### SpringBoot解决加载yml文件的问题
```
<dependency> 
  <groupId>org.yaml</groupId> 
  <artifactId>snakeyaml</artifactId> 
  <version>${snakeyaml.version}</version> 
</dependency> 
```


### SpringBoot自动配置注解
|注解|解释|
|-|-|
|@ConditionalOnClass|classpath中存在该类时起效 |
|@ConditionalOnMissingClass|classpath中不存在该类时起效 |
|@ConditionalOnBean|DI容器中存在该类型Bean时起效 |
|@ConditionalOnMissingBean|DI容器中不存在该类型Bean时起效 |
|@ConditionalOnSingleCandidate|DI容器中该类型Bean只有一个或@Primary的只有一个时起效 |
|@ConditionalOnExpression|SpEL表达式结果为true时 |
|@ConditionalOnProperty|参数设置或者值一致时起效 |
|@ConditionalOnResource| 指定的文件存在时起效 |
|@ConditionalOnJndi| 指定的JNDI存在时起效 |
|@ConditionalOnJava| 指定的Java版本存在时起效 |
|@ConditionalOnWebApplication |Web应用环境下起效 |
|@ConditionalOnNotWebApplication| 非Web应用环境下起效|
