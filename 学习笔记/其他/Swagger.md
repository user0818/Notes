# 步骤

> 1. 导入依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>3.0.0</version>
</dependency>
```

> 2. 开启swagger

在主类上指定注解`@EnableOpenApi`即可开启该功能

开启后，通过http://localhost:8080/swagger-ui/index.html请求即可访问到swagger的管理页面.

> 3. 配置swagger

```java
//@Configuration
public class SwaggerConfig {
    // Docket为swagger的配置类，他的配置属性都在其中，需要配置时可以直接点进去查看所有可选项
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.OAS_30)
                .apiInfo(apiInfo())
            	.enable(true)	// 是否开启swagger
                .groupName("mw")

                .select()
                .apis(RequestHandlerSelectors.any())    // RequestHandlerSelectors是请求选择器，选择要被swagger扫描的接口
                .paths(PathSelectors.ant("/abc"))       // 根据路径选择扫描的请求,指定要加入api文档的请求
                .build();
    }

    // 配置文档信息
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Swagger3接口文档")
                .description("文档描述")
                .termsOfServiceUrl("http://www.baid.com")
                .contact(new Contact("mw", "#", "644711976@qq.com"))  //作者信息
                .version("1.0")
                .build();
    }
    
    public Docket Docket1() {
        return new Docket(DocumentationType.OAS_30)
            .groupName("mw");
    }
}
```

`RequestHandlerSelectors`是一个请求选择器，他用来选择那些请求会被`swagger`扫描到。他一共有如下几种方法：

- `none()`：不扫描
- `any()`：扫描全部
- `basePackage()`：扫描该包下的请求
- `withClassAnnotation()`：扫描带有该注解的类，参数为`注解名.class`
- `withMethodAnnotation()`：扫描带有该注解的方法，参数为`注解名.class`

`PathSelectors`是一个路径选择器，也就是直接对请求进行过滤

- `none()`：不扫描
- `any()`：扫描全部
- `ant()`：扫描指定请求，把该请求加入接口文档中
- `regex()`：用正则表达式过滤请求。

==当上边两种选择方式都配置时，筛选出来的请求取他们俩的交集==

# 分组

swagger提供分组功能，可以把不同的请求分成不同的组，从而可以更有结构的管理接口。

一个`docket`可以有一个组名，要想分成多个组，只需要给多个docket注册@Bean，然后给每个docket设置组名即可实现。

> 实际开发中，往往是多个人开发同一个项目，这时可以每个人配置自己的Docket，指定自己的分组，在这个Docket中扫描自己写的接口。这样选择自己的分组就能看到自己的接口，方便调试

# Model

swagger不仅能够扫描接口，还能扫描实体类。

要想扫描到实体类有两种办法：

1. 所有被扫描到的接口，其所返回的实体类都会被扫描到
2. 给实体类加`@ApiModel`

**@ApiModel**

该注解添加在实体类上，该注解的value可以对这个实体类指定一个名字，如果不配置，默认显示类名。

该注解的`description`属性可以指定该实体类的描述

**@ApiModelProperty**

该注解添加在标注了`@ApiModel`的实体类字段上，该注解的value可以添加对这个实体类字段的描述。如果这个字段是`private`修饰的，那么必须要配置了`get`方法才能在swagger中看到该字段

# 其他注释

当接口文档给别人看的时候，别人可能看不懂每个参数或者请求代表什么意思，所以可以给他们添加注释

- `@ApiOperation("给请求添加注释")`
- `@ApiParam("给参数添加注释")`
- `@Api(tags="控制类注释")`





















