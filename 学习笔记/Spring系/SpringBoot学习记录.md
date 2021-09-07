# 自动配置原理

所有和自动配置相关的内容，都在`spring-boot-autoconfigure-2.5.0.jar`这个jar包下。

## META-INF/spring.factories

![image-20210524173417557](https://gitee.com/mw515031/image/raw/master/image/image-20210524173417557.png)

这个文件中列举出了所有的自动配置类，这些类名都是以`xxxAutoConfiguration`结尾。当框架初始化时，会读取该文件，然后找出所有的自动配置类，并将其自动配置。

这些自动配置类也在这个jar包中，和META-INF文件夹同级。

![image-20210524173734416](https://gitee.com/mw515031/image/raw/master/image/image-20210524173734416.png)

## xxxAutoConfiguration

这些配置类是自动配置的主体。在这个类中实现了相应模块的自动配置。

###@Conditional

虽然这里列举了很多自动配置相关的类，但是并不是所有的类在运行时都会生效，因为有的功能模块并不会在该项目中使用。那框架怎么判断什么时候激活这个自动配置？什么时候忽略这个自动配置？

在自动配置类上，往往有一个`@Conditional系列`的注解，他负责表明这个自动类在什么情况下生效，而在什么情况下无效。当这个注解后边的条件满足时，才把这个类注入到框架中从而生效。

![image-20210524175532603](https://gitee.com/mw515031/image/raw/master/image/image-20210524175532603.png)

==这里的系统中是否存在这个类，就是maven有没有导入这个包==

## @EnableConfigurationProperties(xxxProperties.class)

即使是自动配置，那也得是个配置。既然是配置，那就少不了参数。所以自动配置类上往往还有一个`@EnableConfigurationProperties`注解，他的传入一个`xxxProperties.class`文件，而这个相应的类中，就定义了所有需要自动配置的表，而这些表项的值就是自动配置所需要的参数。一些默认值可以直接以常量的形式写到`xxxProperties`配置类中，而需要程序员配置的内容可以写到yml文件中。

**xxxProperties.java**

这个类可以把它当做配置类的实体类，其主要作用不是完成某些功能，而是承担一种字典功能。类中的属性相当于key，我们在yml文件中的配置相当于value。

每个`xxxProperties.java`文件中必有一个`@ConfigurationProperties(prefix = "xxx")`注解。这个注解中指定了一个前缀。凡是yml文件中以这个前缀开头的配置都会定位到当前的`xxxProperties`类中，并和其中的同名属性一一对应。

# 多环境配置

## .properties

![image-20210524191810701](https://gitee.com/mw515031/image/raw/master/image/image-20210524191810701.png)

## .yaml

![image-20210524191748775](https://gitee.com/mw515031/image/raw/master/image/image-20210524191748775.png)

# SpringBoot静态资源位置和yml文件位置优先级

## yml文件生效顺序

1. `file:./config/`
2. `file:./`
3. `classpath:/config/`
4. `classpath:/`

![image-20210524191542940](https://gitee.com/mw515031/image/raw/master/image/image-20210524191542940.png)

## 静态资源位置

**方式一：适合自己找来的静态资源**

`classpath：/META-INF/resources`

`classpath：/resources`

`classpath：/static`

`classpath：/public`

当有重复时，后三个路径按优先级从上到下的顺序。即上边的会屏蔽下边的。（第一个优先级可能更高）

放在上述所有位置的静态资源都可以通过`/**`访问到

==说人话：上边四个位置放的静态资源（比如jQuery.js），可以直接通过/jQuery.js访问到。==也就是 `localhost:8080/jQuery.js`

**方式二：适合maven引入的静态资源**

所有`classpath:/META-INF/webjars/**`下的静态资源，都可以用`/webjars/**`直接访问到.

**方式三：不推荐**

如果在yml文件中设置了`spring.mvc.static-path-pattern=xxxx`,那么上述的两种方式就会被屏蔽，这时就会按照自己的配置方式生效。

## 首页定制

在上述静态资源位置中的`index.html`会自动被识别为首页，当直接访问服务器时，会直接返回首页

# 配置Spring MVC

> 文档地址：
>
> https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-web-applications.spring-mvc.auto-configuration
>
> 
>
> Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
>
> The auto-configuration adds the following features on top of Spring’s defaults:
>
> - Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
> - Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-web-applications.spring-mvc.static-content)).
> - Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.
> - Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-web-applications.spring-mvc.message-converters)).
> - Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-web-applications.spring-mvc.message-codes)).
> - Static `index.html` support.
> - Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-web-applications.spring-mvc.binding-initializer)).
>
> If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring-framework/docs/5.3.7/reference/html/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.
>
> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.
>
> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.

当我们要想自定义MVC配置从而替代SpringBoot的默认配置时，我们可以这么做：

- 写一个配置类实现`WebMvcConfigurer`接口
- 给这个类标注上`@Configuration`
- 根据需求实现相应的方法（由于接口中都是default修饰的方法，有方法体，所以不必全部实现）
- ==不能标注`@EnableWebMvc`==

不标注`@EnableWebMvc`，框架只会把自己自定义的配置来替换默认的配置。如果标注了`@EnableWebMvc`，那么这个配置类就会全面接管对MVC的配置，默认的配置将会完全失效。

# 国际化

## 编写国际化配置文件

在`Resources`文件夹下创建一个`i18n`文件夹，在这个文件夹下放我们的国际化配置。

国际化配置文件的命名分为两种，一种是默认语言配置文件，另一种是其他语言配置文件。

- 默认语言配置文件名为：`name.properties`
- 其他语言配置文件名为：`name_语言_地区.properties`

这里的`name`往往是页面名字，其中保存了这个页面需要国际化的内容。idea会自动把name相同的配置文件打一个包。

这样的名字会被idea自动识别为国际化文件，从而方便开发。点开国际化配置文件，下方会有一个Resource Bundle选项卡，点开后就会出现如下界面，在这里可以配置所有语言，并且同步到其他语言的配置文件中。

![image-20210525121611784](https://gitee.com/mw515031/image/raw/master/image/image-20210525121611784.png)

## 指定国际化配置文件位置

在application.properties文件中指定国际化配置文件的位置。

`spring.messages.basename=i18n.name`

## Thymeleaf取值

在该模板引擎中，区国际化的值要使用`#{name.message}`，比如上图的情况，就要使用：`#{login.tip}`

**注意：**

- thymeleaf的标签要加`th:`

- thymeleaf的取值方式有标签内写法和行内写法两种，如：`${value}`、`[[${value}]]`

==到这里，我们就已经能通过切换浏览器的语言来实现国际化了==

# Druid数据源

1. 引入maven

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
```

2. application.yml自定义数据库配置信息

```properties
spring.datasource.druid.db-type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.druid.driverClassName=com.mysql.jdbc.Driver
spring.datasource.druid.url=jdbc:mysql://localhost:3306/springboot_helloword?allowMultiQueries=true&autoReconnect=true&characterEncoding=utf-8
spring.datasource.druid.username=root
spring.datasource.druid.password=root

# 下面为连接池的补充设置，应用到上面所有数据源中
# 初始化大小，最小，最大
spring.datasource.druid.initial-size=5
spring.datasource.druid.min-idle=5
spring.datasource.druid.max-active=20
# 配置获取连接等待超时的时间
spring.datasource.druid.max-wait=60000
# 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
spring.datasource.druid.time-between-eviction-runs-millis=60000
# 配置一个连接在池中最小生存的时间，单位是毫秒
spring.datasource.druid.min-evictable-idle-time-millis=300000
spring.datasource.druid.validation-query=SELECT 1 FROM DUAL
spring.datasource.druid.test-while-idle=true
spring.datasource.druid.test-on-borrow=false
spring.datasource.druid.test-on-return=false
# 打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.druid.pool-prepared-statements=true
spring.datasource.druid.max-pool-prepared-statement-per-connection-size=20
# 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.druid.filter.commons-log.connection-logger-name=stat,wall,log4j
spring.datasource.druid.filter.stat.log-slow-sql=true
spring.datasource.druid.filter.stat.slow-sql-millis=2000
# 通过connectProperties属性来打开mergeSql功能；慢SQL记录
spring.datasource.druid.connect-properties.=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
# 合并多个DruidDataSource的监控数据
spring.datasource.druid.use-global-data-source-stat=true
```

3. 配置监控页面

```java
@Configuration
public class DruidConfig {
    /**
     *  主要实现WEB监控的配置处理
     */
    @Bean
    public ServletRegistrationBean druidServlet() {
        // 现在要进行druid监控的配置处理操作
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(
                new StatViewServlet(), "/druid/*");
        // 白名单,多个用逗号分割， 如果allow没有配置或者为空，则允许所有访问
        servletRegistrationBean.addInitParameter("allow", "127.0.0.1,172.29.32.54");
        // 黑名单,多个用逗号分割 (共同存在时，deny优先于allow)
        servletRegistrationBean.addInitParameter("deny", "192.168.1.110");
        // 控制台管理用户名
        servletRegistrationBean.addInitParameter("loginUsername", "admin");
        // 控制台管理密码
        servletRegistrationBean.addInitParameter("loginPassword", "eju1314");
        // 是否可以重置数据源，禁用HTML页面上的“Reset All”功能
        servletRegistrationBean.addInitParameter("resetEnable", "false");
        return servletRegistrationBean ;
    }
    
    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean() ;
        filterRegistrationBean.setFilter(new WebStatFilter());
        //所有请求进行监控处理
        filterRegistrationBean.addUrlPatterns("/*"); 
        //添加不需要忽略的格式信息
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.css,/druid/*");
        return filterRegistrationBean ;
    }
    
    //绑定配置
    @Bean
    @ConfigurationProperties(prefix = "spring.datasource.druid")
    public DataSource druidDataSource() {
        return new DruidDataSource();
    }
}
```



































