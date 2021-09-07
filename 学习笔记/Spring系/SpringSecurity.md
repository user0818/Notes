# 简介

Spring Security是一个提供身份验证，授权和保护以防止常见攻击的框架。其核心思想只有两个：==认证==、==授权==。

认证：什么样的人能登录该网站

授权：给用户授予不同的权限。

# 使用

> 1、导入依赖

```xml
<!-- ... other dependency elements ... -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

> 2、配置==授权==和==认证==

- 第一个方法为授权，即给不同角色哪些请求的访问权限
- 第二个方法为认证，即用户能否登录，以什么角色登录

```java
@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

    @Autowired
    private DataSource dataSource;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                // 当用户访问没有权限的请求时，将会访问登陆请求（前提是已经开启登录请求）
                .antMatchers("/").permitAll()               // 默认请求，允许任何人访问
            	.antMatchers("/req").authenticated()		// req请求需要认证
            	
                .antMatchers("/level1/**").hasRole("vip1")  // level1开头的请求，允许角色为vip1的用户访问
                .antMatchers("/level2/**").hasRole("vip2")  // level2开头的请求，允许角色为vip2的用户访问
                .antMatchers("/level3/**").hasRole("vip3")  // level3开头的请求，允许角色为vip3的用户访问
				.anyRequest().authenticated();				// 所有请求需要认证
        // 开启登录请求，默认为/login请求
        http.formLogin()
                .loginPage("/myLogin")	// 自定义登录页面的请求，GET请求
                .loginProcessingUrl("/myLogin")		// 自定义登录页面中form表单的提交请求，POST请求
        		.failureUrl("/login?error")			// 认证失败发送的请求
                .defaultSuccessUrl("/success")		// 认证成功发送的请求
           		.permitAll();
        
        //开启自动配置的注销功能
        http.logout()					// 开启注销功能。默认发送/logout请求
            .logoutSuccessUrl("/");		// 注销成功后发送的请求

        //开启 记住我 功能
        http.rememberMe().rememberMeParameter("remember");	// 表单中rememberme属性的名字
    }
    
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.jdbcAuthentication()
                .dataSource(dataSource)             // 配置数据源
                .usersByUsernameQuery("select username,password,enabled from users WHERE username=?")       // 查询用户
                .authoritiesByUsernameQuery("select username,authority from authorities where username=?")  // 查询权限
                .passwordEncoder(new BCryptPasswordEncoder());      // 设置密码的编码格式
    }

}
```

**授权：**

- 如果不指定登录页面，SpringSecurity会生成一个登录页面，通过`/login`请求访问到该页面
- 如果自定义了登录页面，要告诉框架哪个请求会转到该页面，也就是`loginPage`
- `loginProcessingUrl`指定了登录页面表单的提交请求，框架接管了这一请求，从而完成认证功能
- `defaultSuccessUrl`和`successForwardUrl`的区别
  - `defaultSuccessUrl`如果直接访问登录页面，登录成功后转到参数指定的请求；如果访问其他请求但是被拦截到了登录页面，那么在登录成功后就会回到原来的请求
  - `successForwardUrl`任何情况下，登陆成功后都会指向指定页面
- `failureForwardUrl`和`failureUrl`区别
  - `failureForwardUrl`失败后跳转，这里登陆功能是个post请求，所有转发的也是post请求
  - `failureUrl`失败后重定向，get请求

**认证：**

- 密码的编码格式在`SpringSecurity 5.0+`以后必须配置。
- 数据库中存储的是加密形式，在登陆时，框架会拿登录密码加密，然后跟数据库中的密码的密文作比较，所以这里要指定编码格式
- 查询用户时的`enabled`属性指定该账户是否有效，值为0，1



# 过滤链



# 问题：前后端分离中怎么实现？

https://cloud.tencent.com/developer/article/1612310

