## 简介

### 网站

gitee：https://gitee.com/dromara/hutool

文档：https://doc.hutool.cn/pages/index/#%F0%9F%93%9A%E7%AE%80%E4%BB%8B

API：https://apidoc.gitee.com/dromara/hutool/

### 介绍

`Hutool`是一个功能丰富且易用的**Java工具库**，通过诸多实用工具类的使用，旨在帮助开发者快速、便捷地完成各类开发任务。 这些封装的工具涵盖了字符串、数字、集合、编码、日期、文件、IO、加密、数据库JDBC、JSON、HTTP客户端等一系列操作， 可以满足各种不同的开发需求。

### 功能

| 模块               | 介绍                                                         |
| ------------------ | ------------------------------------------------------------ |
| hutool-aop         | JDK动态代理封装，提供非IOC下的切面支持                       |
| hutool-bloomFilter | 布隆过滤，提供一些Hash算法的布隆过滤                         |
| hutool-cache       | 简单缓存实现                                                 |
| hutool-core        | 核心，包括Bean操作、日期、各种Util等                         |
| hutool-cron        | 定时任务模块，提供类Crontab表达式的定时任务                  |
| hutool-crypto      | 加密解密模块，提供对称、非对称和摘要算法封装                 |
| hutool-db          | JDBC封装后的数据操作，基于ActiveRecord思想                   |
| hutool-dfa         | 基于DFA模型的多关键字查找                                    |
| hutool-extra       | 扩展模块，对第三方封装（模板引擎、邮件、Servlet、二维码、Emoji、FTP、分词等） |
| hutool-http        | 基于HttpUrlConnection的Http客户端封装                        |
| hutool-log         | 自动识别日志实现的日志门面                                   |
| hutool-script      | 脚本执行封装，例如Javascript                                 |
| hutool-setting     | 功能更强大的Setting配置文件和Properties封装                  |
| hutool-system      | 系统参数调用封装（JVM信息等）                                |
| hutool-json        | JSON实现                                                     |
| hutool-captcha     | 图片验证码实现                                               |
| hutool-poi         | 针对POI中Excel和Word的封装                                   |
| hutool-socket      | 基于Java的NIO和AIO的Socket封装                               |
| hutool-jwt         | JSON Web Token (JWT)封装实现                                 |

## hutool-captcha  图片验证码实现

方法

```
@Service
public class ValidateCodeServiceImpl implements ValidateCodeService {

    @Autowired
    private RedisTemplate<String,String> redisTemplate;

    //生成图片验证码
    @Override
    public ValidateCodeVo generateValidateCode() {
        //1 通过工具生成图片验证码
        //hutool
        //int width, int height, int codeCount, int circleCount
        CircleCaptcha circleCaptcha = CaptchaUtil.createCircleCaptcha(150, 48, 4, 2);
        String codeValue = circleCaptcha.getCode();//4位验证码值
        String imageBase64 = circleCaptcha.getImageBase64(); //返回图片验证码，base64编码

        //2 把验证码存储到redis里面，设置redis的key： uuid   redis的value ：验证码值
        //设置过期时间
        String key = UUID.randomUUID().toString().replaceAll("-","");
        redisTemplate.opsForValue().set("user:validate"+key,
                                        codeValue,
                                        5,
                                        TimeUnit.MINUTES);

        //3 返回ValidateCodeVo对象
        ValidateCodeVo validateCodeVo = new ValidateCodeVo();
        validateCodeVo.setCodeKey(key); //redis存储数据key
        validateCodeVo.setCodeValue("data:image/png;base64," + imageBase64);

        return validateCodeVo;
    }
}
```