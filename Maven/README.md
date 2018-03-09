## Maven
```
<dependency>内部通过groupId,artifactId以及version确定唯一的依赖
  groupId: 组织的唯一标识
  artifactId: 项目的唯一标识
  version: 项目的版本
```

#### Maven跳过测试
```
Maven跳过测试
mvn clean install -Dmaven.test.skip=true
```
