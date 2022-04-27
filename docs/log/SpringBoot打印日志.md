# SpringBoot打印日志
概括：本教程用于使用springboot自带的日志框架打印日志
## 步骤
1、引入Lombok依赖，idea开启Lombok

2、在application.yml中添加日志配置
```yaml
logging:
  pattern:
    #格式化，只能输出日期和内容
    console: "%d - %msg%n"
  file:
    #文件存放目录，没有就会在当前项目的根目录中自动创建
    path: log/
  level:
    #日志输出级别
    root: info
```
3、在需要打印日志的类上添加注解@Slf4j，即可使用日志框架
```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
class test{
    log.info("");
    log.error("");
}
```
