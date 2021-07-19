---
title: SpringCloud实现邮件发送
date: 2021-06-01 21:48:56
tags: SpringCloud
categories: 笔记
---

由于阿里云短信发送服务审核过严，故将其替换为邮件发送验证码，记录一下整合邮件发送服务过程

<!-- more -->

首先去qq邮箱开通SMTP服务，并生成授权码

![image-20210601215611281](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210601215618.png)

然后去项目中整合邮件发送服务

> 目前只是简单实现了功能，之后若有更多要求，再行更改

pom依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

application.properties配置

```properties
spring.mail.host=smtp.qq.com
spring.mail.protocol=smtp
spring.mail.default-encoding=utf-8
spring.mail.username=775972516@qq.com
# 这个是授权码，不是邮箱密码！！！
spring.mail.password=xxxxxx
```

Controller

```java
@RestController
@RequestMapping("/edumsm/msm")
@CrossOrigin
public class MsmController {
    @Autowired
    private MsmService msmService;

    @GetMapping("/sendmail")
    public R sendMail(){
        String code = null;
        code = RandomUtil.getFourBitRandom();

        Boolean result = msmService.sendMail(code);
        if (result){
            return R.ok();
        }else {
            return R.error().message("发送邮件失败");
        }
    }
}
```

Service

```java
@Service
public class MsmServiceImpl implements MsmService {
    @Autowired
    private JavaMailSender mailSender;

    @Value("${spring.mail.username}")
    private String from;

    @Override
    public Boolean sendMail(String code) {
        SimpleMailMessage message = new SimpleMailMessage();

        //自己给自己发验证码邮件
        message.setFrom(from);
        message.setSubject("your code");
        message.setText(code);
        message.setTo(from);

        mailSender.send(message);

        return true;
    }
}
```

效果

![image-20210601220427821](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210601220427.png)

![image-20210601220338296](https://cdn.jsdelivr.net/gh/Rayucan/imageCloud/data/20210601220338.png)

