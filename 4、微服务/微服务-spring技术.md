## Maven打包通用项目

### 应用

#### 基本步骤

1、建立模块

2、maven打包：install

3、其他项目调用

## RestTemplate

### 简介

官网：

### 作用

### 应用

#### 基本步骤：

1、注册RestTemplate的bean对象

```
@Configuration
public class RestTemplateConfig
{
    @Bean
    public RestTemplate restTemplate()
    {
        return new RestTemplate();
    }
}
```

2、服务远程调用RestTemplate

#### 常用方法

笔记：https://cloud.tencent.com/developer/article/1898119

### 原理