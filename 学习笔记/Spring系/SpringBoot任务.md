# 异步任务

1. 在主类上开启异步注解`@EnableAsync`
2. 在要执行异步的方法上注解`@Async`

异步方法被调用时，系统会采用多线程技术，用另一个线程完成该方法，原调用方法继续执行。

# 邮件任务

> 配置依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

> 配置参数

```properties
# 发送方信息
spring.mail.username=644711976@qq.com
# 这里应该填邮箱生成的token，也就是授权码。注意这里不是登录密码
spring.mail.password=jaexgkgigtjmbfba
# 这里指定发送方的邮件服务器主机名
spring.mail.host=smtp.qq.com
# 开启加密验证(qq必须要配置)
spring.mail.properties.mail.smtp.ssl.enable=true
```

> 发送一个简单的邮件

```java
@SpringBootTest
class SwaggerApplicationTests {

    @Autowired
    JavaMailSenderImpl javaMailSender;

    @Test
    void contextLoads() {
        // 创建一个简单邮件的实例
        SimpleMailMessage simpleMailMessage = new SimpleMailMessage();

        // 设置邮件内容
        simpleMailMessage.setSubject("这里是主题");
        simpleMailMessage.setText("这里是正文");
        simpleMailMessage.setTo("2669654472@qq.com");
        simpleMailMessage.setFrom("644711976@qq.com");

        // 发送
        javaMailSender.send(simpleMailMessage);
    }

}
```

> 发送一个复杂邮件

```java
@Autowired
JavaMailSenderImpl javaMailSender;

@Test
void contextLoads1() throws MessagingException {

    // 创建一个复杂邮件的实例
    MimeMessage mimeMailMessage = javaMailSender.createMimeMessage();

    // 这个helper是用来帮助邮件设置一些参数的工具 把这个复杂邮件组装起来
    // 第一个参数指定帮哪个复杂邮件组装，第二个参数代表开启多文件，第三个参数代表邮件编码
    MimeMessageHelper mimeMessageHelper = new MimeMessageHelper(mimeMailMessage,true,"utf-8");

    // 设置邮件内容
    mimeMessageHelper.setSubject("这里是主题");
    mimeMessageHelper.setText("<h1>这里是html正文</h1>",true);   // 这里开启了html解析

    // 添加附件
    mimeMessageHelper.addAttachment("1.jpg",new File("C:\\Users\\hp\\Desktop\\1.jpg"));

    // 设置发送者和接受者
    mimeMessageHelper.setTo("2669654472@qq.com");
    mimeMessageHelper.setFrom("644711976@qq.com");

    // 发送
    javaMailSender.send(mimeMailMessage);
}
```

# 定时任务

我们有时候需要在特定的时间执行某些程序，SpringBoot也内置了任务管理和调度的功能，在特定时间执行某些方法。

**开启定时任务的注解**

在主类中添加`@EnableScheduling`注解

**使用**

在要使用定时任务的方法上使用`@Schedule`注解。这个注解可以指定时间，也就是什么时候该方法执行。该注解的计划是由他的一个`cron`参数指定的，要传给它一个cron表达式。cron表达式比较不容易写，所以一般都用现成的在线cron表达式生成器来生成，这样比较不会出错。



















































