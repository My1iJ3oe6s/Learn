## Mybatis

### # $ 的区别
#{}速度快，能防止sql注入，是占位符方式，先预编译，然后填充参数，字符串格式，相当于填空题 用户名=（___），参数只是下划线上的内容
${}是直接拼接到语句上，执行语句，对于上面那道填空题 ，这种方式需要自己拼括号和参数，但是也可以拼接想执行的任何语句，也就是传说中的sql注入

# 预先编译  只是占位符   防止sql注入
$ 直接拼接在语句上

## 原理
