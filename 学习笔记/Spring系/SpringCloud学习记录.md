# 简介

课堂笔记链接：https://www.kuangstudy.com/bbs/1374942542566551554

Spring Cloud为开发人员提供了工具来快速构建分布式系统中的一些常见模式（例如配置管理，服务发现，断路器，智能路由，微代理，控制总线）。 分布式系统的协调导致样板式样，并且使用Spring Cloud开发人员可以快速站起来实现这些样板的服务和应用程序。 它们可以在任何分布式环境中正常工作，包括开发人员自己的笔记本电脑，裸机数据中心以及Cloud Foundry等托管平台。

# 版本对应

SpringBoot与SpringCloud需要版本对应，否则可能会造成很多意料之外的错误，比如eureka注册了结果找不到服务类啊，比如某些jar导入不进来啊，等等这些错误。下面列出来springBoot和spring cloud的版本对应关系，需要配套使用，才不会出现各种奇怪的错误。下边列举一些查看版本对应信息的方法。

**官方提供的对应信息**

https://start.spring.io/actuator/info

上边这个地址返回一系列Json格式的字符串，其中包含了`SpringCloud`和`SpringBoot`的版本对应。如果返回的是没有格式化的Json字符串，可以用下边的在线Json字符串格式化工具。

https://prettier.io/playground/

但是这种方法不会显示所有对应信息，所以可能找不到自己需要的版本

**SpringCloud官网**

在SpringCloud官网上指定版本的文档里，他会标注建议使用的SpringBoot版本：

![image-20210528165129376](https://gitee.com/mw515031/image/raw/master/image/image-20210528165129376.png)

# 一个简单的例子

就是一个project工程中写多个模块，每个模块独立部署，模块之间用ip+端口号访问服务。服务就是指Controller提供的接口。（说白了每个module都是个常规SpringBoot项目，不过有的负责通过接口提供服务，有的只需要通过接口调用别人的服务）

一个常规的微服务项目有三个模块：

- api：公共的实体类（其他项目可以通过Maven引入该项目，来使用该项目的类）
- provider：服务提供者
- consumer：服务调用者

Spring提供了一个RESTFUL风格的模板，来进行远程服务的调用：`RestTemplate`。通过这个模板，我们可以远程访问别人提供的服务。

常见函数及重载如下：

```java
// GET
public <T> T getForObject(String url, Class<T> responseType, Object... uriVariables);
public <T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables);
public <T> T getForObject(URI url, Class<T> responseType)1;
public <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Object... uriVariables);
public <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Map<String, ?> uriVariables);
public <T> ResponseEntity<T> getForEntity(URI url, Class<T> responseType);

// POST
public <T> T postForObject(String url, @Nullable Object request, Class<T> responseType,Object... uriVariables);
public <T> T postForObject(String url, @Nullable Object request, Class<T> responseType,Map<String, ?> uriVariables);
public <T> T postForObject(URI url, @Nullable Object request, Class<T> responseType);
public <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request,Class<T> responseType, Object... uriVariables);
public <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request,Class<T> responseType, Map<String, ?> uriVariables);
public <T> ResponseEntity<T> postForEntity(URI url, @Nullable Object request, Class<T> responseType);

// PUT
public void put(String url, @Nullable Object request, Object... uriVariables);
public void put(String url, @Nullable Object request, Map<String, ?> uriVariables);
public void put(URI url, @Nullable Object request);

// DELETE
public void delete(String url, Object... uriVariables);
public void delete(String url, Map<String, ?> uriVariables);
public void delete(URI url);
```

==这个模板没有被Spring容器接管，所以不能直接注入，要自行配置Bean。==

# Eureka

上边这个例子表示了点对点的服务调用。但是有可能会有很多服务，事情就变得复杂了。所以我们需要一个集中管理服务的东西，这就是注册中心。如`zookeeper`和`Eureka`。这时，如果有客户端需要服务，只需要从注册中心里拿即可。

**什么是Eureka**

- Netflix在涉及Eureka时，遵循的就是API原则.
- Eureka是Netflix的有个子模块，也是核心模块之一。Eureka是基于REST的服务，用于定位服务，以实现云端中间件层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务注册与发现，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper.

**Eureka基本的架构**

- Springcloud 封装了Netflix公司开发的Eureka模块来实现服务注册与发现 (对比Zookeeper).
- Eureka采用了C-S的架构设计，EurekaServer作为服务注册功能的服务器，他是服务注册中心.
- 而系统中的其他微服务，使用Eureka的客户端连接到EurekaServer并维持心跳连接。这样系统的维护人员就可以通过EurekaServer来监控系统中各个微服务是否正常运行，Springcloud 的一些其他模块 (比如Zuul) 就可以通过EurekaServer来发现系统中的其他微服务，并执行相关的逻辑.

![image-20210530160651147](https://gitee.com/mw515031/image/raw/master/image/image-20210530160651147.png)

**三大角色**

- Eureka Server：提供服务的注册与发现
- Service Provider：服务生产方，将自身服务注册到Eureka中，从而使服务消费方能狗找到
- Service Consumer：服务消费方，从Eureka中获取注册服务列表，从而找到消费服务

==和Dubbo架构对比.==

![image-20210530160718370](https://gitee.com/mw515031/image/raw/master/image/image-20210530160718370.png)

- ureka 包含两个组件：**Eureka Server** 和 **Eureka Client**.
- Eureka Server 提供服务注册，各个节点启动后，回在EurekaServer中进行注册，这样Eureka Server中的服务注册表中将会储存所有课用服务节点的信息，服务节点的信息可以在界面中直观的看到.
- Eureka Client 是一个Java客户端，用于简化EurekaServer的交互，客户端同时也具备一个内置的，使用轮询负载算法的负载均衡器。在应用启动后，将会向EurekaServer发送心跳 (默认周期为30秒) 。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除掉 (默认周期为90s).

## Eureka-server(注册中心)

### 配置

> POM依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```

> Eureka配置

```yml
server:
  port: 7001
eureka:
  instance:
    hostname: localhost           # Eureka服务端的实例名称
  client:
    fetch-registry: false         # false表示自己为注册中心
    register-with-eureka: false   # 是否向注册中心注册自己，因为这里就是注册中心，所以不用注册
    service-url:  # 监控页面
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

> 开启Eureka注册中心功能

为主启动类添加`@EnableEurekaServer`

### 效果

启动成功后访问 http://localhost:7001/ 得到以下页面

![image-20210530160920592](https://gitee.com/mw515031/image/raw/master/image/image-20210530160920592.png)

## Eureka(服务提供者)

### 配置

服务提供者要向注册中心注册自己，让他知道自己提供服务。

> POM依赖

由于Eureka是一种CS结构的模型，所以除了注册中心是Server端，其他的无论是服务提供者还是消费者都是Client端。

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

> Eureka服务提供者配置

```yml
server:
  port: 8001

# Eureka配置：配置服务注册中心地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改Eureka上的默认描述信息，下图右侧的Status栏的内容
```

> 开启Eureka客户端功能

为主启动类添加`@EnableDiscoveryClient`

### 效果

先启动7001服务端后启动8001客户端进行测试，然后访问监控页http://localhost:7001/ 产看结果如图，成功

![image-20210530161155891](https://gitee.com/mw515031/image/raw/master/image/image-20210530161155891.png)

右侧的Status是一个链接，它发出：`/actuator/info`请求，但会跳转到errot页面。因为我们目前还没有开启这个功能。通过一些配置，我们可以使这里显示一些提示信息。

pom.xml中添加依赖

```xml
<!--actuator完善监控信息-->
<dependency>
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```yml
# info配置
info:
	# 项目的名称
	app.name: haust-springcloud
	# 公司的名称
	company.name: 东北大学
```



### 自我保护机制

如果此时停掉springcloud-provider-dept-8001 等**30s**后 监控会开启保护机制：

![image-20210530161319830](https://gitee.com/mw515031/image/raw/master/image/image-20210530161319830.png)

==EureKa自我保护机制：好死不如赖活着==

一句话总结就是：**某时刻某一个微服务不可用，eureka不会立即清理，依旧会对该微服务的信息进行保存！**

- 默认情况下，当eureka server在一定时间内没有收到实例的心跳，便会把该实例从注册表中删除（**默认是90秒**），但是，如果短时间内丢失大量的实例心跳，便会触发eureka server的自我保护机制，比如在开发测试时，需要频繁地重启微服务实例，但是我们很少会把eureka server一起重启（因为在开发过程中不会修改eureka注册中心），**当一分钟内收到的心跳数大量减少时，会触发该保护机制**。可以在eureka管理界面看到Renews threshold和Renews(last min)，当后者（最后一分钟收到的心跳数）小于前者（心跳阈值）的时候，触发保护机制，会出现红色的警告：`EMERGENCY!EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT.RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEGING EXPIRED JUST TO BE SAFE.`从警告中可以看到，eureka认为虽然收不到实例的心跳，但它认为实例还是健康的，eureka会保护这些实例，不会把它们从注册表中删掉。
- 该保护机制的目的是避免网络连接故障，在发生网络故障时，微服务和注册中心之间无法正常通信，但服务本身是健康的，不应该注销该服务，如果eureka因网络故障而把微服务误删了，那即使网络恢复了，该微服务也不会重新注册到eureka server了，因为只有在微服务启动的时候才会发起注册请求，后面只会发送心跳和服务列表请求，这样的话，该实例虽然是运行着，但永远不会被其它服务所感知。所以，eureka server在短时间内丢失过多的客户端心跳时，会进入自我保护模式，该模式下，eureka会保护注册表中的信息，不在注销任何微服务，当网络故障恢复后，eureka会自动退出保护模式。自我保护模式可以让集群更加健壮。
- 但是我们在开发测试阶段，需要频繁地重启发布，如果触发了保护机制，则旧的服务实例没有被删除，这时请求有可能跑到旧的实例中，而该实例已经关闭了，这就导致请求错误，影响开发测试。所以，在开发测试阶段，我们可以把自我保护模式关闭，只需在eureka server配置文件中加上如下配置即可：`eureka.server.enable-self-preservation=false`【不推荐关闭自我保护机制】

详细内容可以参考下这篇博客内容：https://blog.csdn.net/wudiyong22/article/details/80827594











































