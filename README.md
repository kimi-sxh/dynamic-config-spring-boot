# dynamic-config-spring-boot
## 简介:
- 配置监听器

## 功能使用

> 添加依赖

```
<dependency>
  <groupId>com.purge</groupId>
  <artifactId>dynamic-config-spring-boot-starter</artifactId>
  <version>0.1.0.RELEASE</version>
</dependency>
```

### 1. @EnableDynamicConfigEvent

> 简介：开启这个特性注解，具备配置推送更新监听能力。

> 使用：

启动类添加`@EnableDynamicConfigEvent` 注解。
```
@EnableDynamicConfigEvent
@SpringBootApplication(scanBasePackages = {"io.deepblueai.frontdemo.service"})
public class FrontDemoApplication {

  public static void main(String[] args) {
    SpringApplication.run(FrontDemoApplication.class, args);
  }

}
```

编写事件接受器：

创建`NacosListener`实现`ApplicationListener<ActionConfigEvent>`

```
@Slf4j
@Component
public class NacosListener implements ApplicationListener<ActionConfigEvent> {

  @Override
  public void onApplicationEvent(ActionConfigEvent event) {
    log.info("接收事件");
    log.info(event.getPropertyMap().toString());
  }
}
```

在`onApplicationEvent`方法里获取目标值，作相应的逻辑处理。

`ActionConfigEvent` 主要包含 `Map<String, HashMap> propertyMap;`,propertyMap结构如下:

```
{
    `被更新的配置key`:{
        before: `原来的值`，
        after: `更新后的值`
    },
    `被更新的配置key`:{
        before: `原来的值`，
        after: `更新后的值`
    }
}
```