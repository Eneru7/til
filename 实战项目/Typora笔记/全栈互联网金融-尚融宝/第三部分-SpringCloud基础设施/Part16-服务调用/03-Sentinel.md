## 1、service-base中引入sentinel依赖 

```xml
<!--服务容错-->
<dependency>
    <groupId>com.alibaba.cloud</groupId> 
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>
```

## 2、开启Sentinel支持 

在service-sms的yml配置文件中开启Feign对Sentinel的支持 

```yaml
#开启Feign对Sentinel的支持
#feign:
  sentinel:
    enabled: true
```

## 3、创建容错类

fallback：当无法校验手机号是否已注册时，直接发送短信 

```java
package com.atguigu.srb.sms.client.fallback;
@Service
@Slf4j
public class CoreUserInfoClientFallback implements CoreUserInfoClient {
    @Override
    public boolean checkMobile(String mobile) {
        log.error("远程调用失败，服务熔断");
        return false;
    }
}
```

## 4、指定熔断类

为OpenFeign远程调用接口添加fallback属性值没指定容错类 

```java
@FeignClient(value = "service-core", fallback = CoreUserInfoClientFallback.class)
public interface CoreUserInfoClient {
```

## 5、测试

停止core微服务测试