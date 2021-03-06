---
layout:         post
title:          SpringBoot学习笔记
date:           2019-08-18
tags:           [Java]
categories:
comments: false
---

### 记录学习SpringBoot的过程

Spring Boot是一个基于Java的开源框架，用于创建微服务

SpringBoot的好处
1. 避免在Spring中进行复杂的XML配置
2. 以更简单的方式开发生产就绪的Spring应用程序
3. 减少开发时间并独立运行应用程序
4. 提供一种更简单的应用程序入门方式

#### 正式开始SpringBoot

##### 1.依赖SpringBoot

所有Spring Boot启动程序都遵循相同的命名模式`spring-boot-starter-*`


Spring Boot Starter Actuator依赖关系用于监视和管理应用程序。 其代码如下所示 

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Spring Boot Starter Security依赖项用于Spring Security。 其代码如下所示 
```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
Spring Boot Starter Web依赖项用于编写Rest端点。 其代码如下所示 
```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Spring Boot Starter Thyme Leaf依赖项用于创建Web应用程序。 其代码如下所示 
```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
Spring Boot Starter Test依赖项用于编写测试用例。 其代码如下所示 
```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-test<artifactId>
</dependency>
```

##### 2. 代码结构

Java推荐的包声明命名约定是反向域名。 例如 - com.jack.projectName


```
com
    +- jack
        +- projectName
            +- Application.java
            |
            +- Model / Bean
            |    +- Model.java
            +- Dao
            |    +- Mapper.java
            +- Controller
            |    +- Controller.java
            +- Service
            |    +- Service.java
```

##### 3. 构建Restful项目

@RestController注释用于定义RESTful Web服务。它提供JSON，XML和自定义响应。其语法如下所

封装一个公共的返回对象，方便返回
```

public class CommonResult<T> {
    private long code;
    private String message;
    private T data;
    
    // 对接口的返回常用的类型做封装
    public static <T> CommonResult<T> success(T data) {
        return new CommonResult<T>(ResultCode.SUCCESS.getCode(), ResultCode.SUCCESS.getMessage(), data);
    }

    public static <T> CommonResult<T> success(T data, String message) {
        return new CommonResult<T>(ResultCode.SUCCESS.getCode(), message, data);
    }
}
```

SpringBoot的注解整理
```
// 读取环境或应用程序属性值
@Value("${property_key_name}")

// 定义RESTful Web服务。它提供JSON，XML和自定义响应
@RestController

// 注释用于定义访问REST端点的Request URI
@RequestMapping(value = "/apiName")

// 注释用于定义请求正文内容类型
 public RequestResult userRegister(@RequestBody User user) {
    return RequestResult.success("注册成功");
    }
    
// 批注用于定义自定义或动态请求URI
@PathVariable
public ResponseEntity<Object> updateProduct(@PathVariable("id") String id) {

}

// 注释用于从请求URL读取请求参数
@RequestParam


```

开始编写接口
```
@RestController
public class UserController {


    @RequestMapping(value = "/login", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult login(@RequestBody UmsAdminLoginParam umsAdminLoginParam, BindingResult result) {
        String token = adminService.login(umsAdminLoginParam.getUsername(), umsAdminLoginParam.getPassword());
        if (token == null) {
            return CommonResult.validateFailed("用户名或密码错误");
        }
        Map<String, String> tokenMap = new HashMap<>();
        tokenMap.put("token", token);
        tokenMap.put("tokenHead", tokenHead);
        return CommonResult.success(tokenMap);
    }
}
```












