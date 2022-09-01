## 4.1 Spring Cloud础知识

## 4.1.1 为什么学习SpringCloud

不论是商业应用还是用户应用，在业务初期都很简单，我们通常会把它实现为单体结构的应用。但是，随着业务逐渐发展，产品思想会变得越来越复杂，单体结构的应用也会越来越复杂。这就会给应用带来如下的几个问题：

- 代码结构混乱：业务复杂，导致代码量很大，管理会越来越困难。同时，这也会给业务的快速迭代带来巨大挑战；
- 开发效率变低：开发人员同时开发一套代码，很难避免代码冲突。开发过程会伴随着不断解决冲突的过程，这会严重的影响开发效率；
- 排查解决问题成本高：线上业务发现 bug，修复 bug 的过程可能很简单。但是，由于只有一套代码，需要重新编译、打包、上线，成本很高。

由于单体结构的应用随着系统复杂度的增高，会暴露出各种各样的问题。近些年来，微服务架构逐渐取代了单体架构，且这种趋势将会越来越流行。Spring Cloud是目前最常用的微服务开发框架，已经在企业级开发中大量的应用。

## 4.1.2 什么是SpringCloud

Spring Cloud是一系列框架的有序[集合](https://so.csdn.net/so/search?q=集合&spm=1001.2101.3001.7020)。它利用Spring Boot的开发便利性巧妙地简化了分布式系统基础设施的开发，如**服务发现注册、配置中心、智能路由、消息总线、负载均衡、断路器、数据监控等**，都可以用Spring Boot的开发风格做到一键启动和部署。Spring Cloud并没有重复制造轮子，它只是将各家公司开发的比较成熟、经得起实际考验的服务框架组合起来，通过Spring Boot风格进行再封装屏蔽掉了复杂的配置和实现原理，最终给开发者留出了一套简单易懂、易部署和易维护的分布式系统开发工具包。

## 4.1.3 设计目标与优缺点

**设计目标**

协调各个微服务，简化分布式系统开发。

**优缺点**
微服务的框架那么多比如：dubbo、Kubernetes，为什么就要使用Spring Cloud的呢？
**优点：**

- 产出于Spring大家族，Spring在企业级开发框架中无人能敌，来头很大，可以保证后续的更新、完善
- 组件丰富，功能齐全。Spring Cloud 为微服务架构提供了非常完整的支持。例如、配置管理、服务发现、断路器、微服务网关等；
- Spring Cloud 社区活跃度很高，教程很丰富，遇到问题很容易找到解决方案
- 服务拆分粒度更细，耦合度比较低，有利于资源重复利用，有利于提高开发效率
- 可以更精准的制定优化服务方案，提高系统的可维护性
  减轻团队的成本，可以并行开发，不用关注其他人怎么开发，先关注自己的开发
- 微服务可以是跨平台的，可以用任何一种语言开发
  适于互联网时代，产品迭代周期更短
  **缺点：**
- 微服务过多，治理成本高，不利于维护系统
- 分布式系统开发的成本高（容错，分布式事务等）对团队挑战大

总的来说优点大过于缺点，目前看来Spring Cloud是一套非常完善的分布式框架，目前很多企业开始用微服务、Spring Cloud的优势是显而易见的。因此对于想研究微服务架构的同学来说，学习Spring Cloud是一个不错的选择。

## 4.1.4 Spring Cloud发展前景

Spring Cloud对于中小型互联网公司来说是一种福音，因为这类公司往往没有实力或者没有足够的资金投入去开发自己的分布式系统基础设施，使用Spring Cloud一站式解决方案能在从容应对业务发展的同时大大减少开发成本。同时，随着近几年微服务架构和Docker容器概念的火爆，也会让Spring Cloud在未来越来越“云”化的软件开发风格中立有一席之地，尤其是在五花八门的分布式解决方案中提供了标准化的、全站式的技术方案，意义可能会堪比当年Servlet规范的诞生，有效推进服务端软件系统技术水平的进步。

## 4.1.5 整体架构

![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971022.png)

# 2 组成部分简介

- **Spring Cloud Config**
  集中配置管理工具，分布式系统中统一的外部配置管理，默认使用Git来存储配置，可以支持客户端配置的刷新及加密、解密操作。
- **Spring Cloud Netflix**
  Netflix OSS 开源组件集成，包括Eureka、Hystrix、Ribbon、Feign、Zuul等核心组件。
  Eureka：服务治理组件，包括服务端的注册中心和客户端的服务发现机制；
  Ribbon：负载均衡的服务调用组件，具有多种负载均衡调用策略；
  Hystrix：服务容错组件，实现了断路器模式，为依赖服务的出错和延迟提供了容错能力；
  Feign：基于Ribbon和Hystrix的声明式服务调用组件；
  Zuul：API网关组件，对请求提供路由及过滤功能。
- **Spring Cloud Bus**
  用于传播集群状态变化的消息总线，使用轻量级消息代理链接分布式系统中的节点，可以用来动态刷新集群中的服务配置。
- **Spring Cloud Consul**
  基于Hashicorp Consul的服务治理组件。
- **Spring Cloud Security**
  安全工具包，对Zuul代理中的负载均衡OAuth2客户端及登录认证进行支持。
- **Spring Cloud Sleuth**
  Spring Cloud应用程序的分布式请求链路跟踪，支持使用Zipkin、HTrace和基于日志（例如ELK）的跟踪。
- **Spring Cloud Stream**
  轻量级事件驱动微服务框架，可以使用简单的声明式模型来发送及接收消息，主要实现为Apache Kafka及RabbitMQ。
- **Spring Cloud Task**
  用于快速构建短暂、有限数据处理任务的微服务框架，用于向应用中添加功能性和非功能性的特性。
- **Spring Cloud Zookeeper**
  基于Apache Zookeeper的服务治理组件。
- **Spring Cloud Gateway**
  API网关组件，对请求提供路由及过滤功能。
- **Spring Cloud OpenFeign**
  基于Ribbon和Hystrix的声明式服务调用组件，可以动态创建基于Spring MVC注解的接口实现用于服务调用，在Spring Cloud 2.0中已经取代Feign成为了一等公民。

## 2.1 SpringCloud的基础功能

**SpringCloud的基础功能：**

- 服务治理：Spring Cloud Eureka
- 客户端负载均衡：Spring Cloud Ribbon
- 服务容错保护：Spring Cloud Hystrix
- 声明式服务调用：Spring Cloud Feign
- API网关服务：Spring Cloud Zuul
- 分布式配置中心：Spring Cloud Config

**SpringCloud的高级功能(本文不讲)：**

- 消息总线：Spring Cloud Bus
- 消息驱动的微服务：Spring Cloud Stream
- 分布式服务跟踪：Spring Cloud Sleuth

## 2.2 Spring Cloud 的服务发现框架——Eureka

> Eureka是基于REST（代表性状态转移）的服务，主要在AWS云中用于定位服务，以实现负载均衡和中间层服务器的故障转移。我们称此服务为Eureka服务器。Eureka还带有一个基于Java的客户端组件Eureka Client，它使与服务的交互变得更加容易。客户端还具有一个内置的负载平衡器，可以执行基本的循环负载平衡。在Netflix，更复杂的负载均衡器将Eureka包装起来，以基于流量，资源使用，错误条件等多种因素提供加权负载均衡，以提供出色的弹性。
> 总的来说，Eureka 就是一个服务发现框架。何为服务，何又为发现呢？

举一个生活中的例子，就比如我们平时租房子找中介的事情。

在没有中介的时候我们需要一个一个去寻找是否有房屋要出租的房东，这显然会非常的费力，一你找凭一个人的能力是找不到很多房源供你选择，再者你也懒得这么找下去(找了这么久，没有合适的只能将就)。这里的我们就相当于微服务中的Consumer，而那些房东就相当于微服务中的Provider。消费者Consumer需要调用提供者Provider提供的一些服务，就像我们现在需要租他们的房子一样。

但是如果只是租客和房东之间进行寻找的话，他们的效率是很低的，房东找不到租客赚不到钱，租客找不到房东住不了房。所以，后来房东肯定就想到了广播自己的房源信息(比如在街边贴贴小广告)，这样对于房东来说已经完成他的任务(将房源公布出去)，但是有两个问题就出现了。第一、其他不是租客的都能收到这种租房消息，这在现实世界没什么，但是在计算机的世界中就会出现资源消耗的问题了。第二、租客这样还是很难找到你，试想一下我需要租房，我还需要东一个西一个地去找街边小广告，麻不麻烦？

那怎么办呢？我们当然不会那么傻乎乎的，第一时间就是去找中介呀，它为我们提供了统一房源的地方，我们消费者只需要跑到它那里去找就行了。而对于房东来说，他们也只需要把房源在中介那里发布就行了。

那么现在，我们的模式就是这样的了。

![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971023.png)
但是，这个时候还会出现一些问题。

房东注册之后如果不想卖房子了怎么办？我们是不是需要让房东定期续约？如果房东不进行续约是不是要将他们从中介那里的注册列表中移除。

租客是不是也要进行注册呢？不然合同乙方怎么来呢？

中介可不可以做连锁店呢？如果这一个店因为某些不可抗力因素而无法使用，那么我们是否可以换一个连锁店呢？

针对上面的问题我们来重新构建一下上面的模式图
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971024.png)
好了，举完这个例子我们就可以来看关于 Eureka 的一些基础概念了，你会发现这东西理解起来怎么这么简单。👎👎👎

- **服务发现**：其实就是一个“中介”，整个过程中有三个角色：服务提供者(出租房子的)、服务消费者(租客)、服务中介(房屋中介)。
- **服务提供者**：就是提供一些自己能够执行的一些服务给外界。
- **服务消费者**：就是需要使用一些服务的“用户”。
- **服务中介**：其实就是服务提供者和服务消费者之间的“桥梁”，服务提供者可以把自己注册到服务中介那里，而服务消费者如需要消费一些服务(使用一些功能)就可以在服务中介中寻找注册在服务中介的服务提供者。
- **服务注册** Register：

官方解释：当 Eureka 客户端向[Eureka] Server注册时，它提供自身的元数据，比如IP地址、端口，运行状况指示符URL，主页等。

结合中介理解：房东 (提供者[Eureka] Client Provider)在中介 (服务器[Eureka] Server) 那里登记房屋的信息，比如面积，价格，地段等等(元数据metaData)。

- **服务续约 Renew**：

官方解释：Eureka 客户会每隔30秒(默认情况下)发送一次心跳来续约。通过续约来告知[Eureka] Server该 Eureka 客户仍然存在，没有出现问题。正常情况下，如果[Eureka] Server在90秒没有收到 Eureka 客户的续约，它会将实例从其注册表中删除。

结合中介理解：房东 (提供者[Eureka] Client Provider) 定期告诉中介 (服务器[Eureka] Server) 我的房子还租(续约) ，中介 (服务器[Eureka] Server) 收到之后继续保留房屋的信息。

- **获取注册列表信息 Fetch Registries：**

官方解释：Eureka 客户端从服务器获取注册表信息，并将其缓存在本地。客户端会使用该信息查找其他服务，从而进行远程调用。该注册列表信息定期（每30秒钟）更新一次。每次返回注册列表信息可能与 Eureka 客户端的缓存信息不同, Eureka 客户端自动处理。如果由于某种原因导致注册列表信息不能及时匹配，Eureka 客户端则会重新获取整个注册表信息。Eureka 服务器缓存注册列表信息，整个注册表以及每个应用程序的信息进行了压缩，压缩内容和没有压缩的内容完全相同。Eureka 客户端和 Eureka 服务器可以使用JSON / XML格式进行通讯。在默认的情况下 Eureka 客户端使用压缩JSON格式来获取注册列表的信息。

结合中介理解：租客(消费者[Eureka] Client Consumer) 去中介 (服务器[Eureka] Server) 那里获取所有的房屋信息列表 (客户端列表[Eureka] Client List) ，而且租客为了获取最新的信息会定期向中介 (服务器[Eureka] Server) 那里获取并更新本地列表。

- **服务下线 Cancel：**

官方解释：Eureka客户端在程序关闭时向Eureka服务器发送取消请求。发送请求后，该客户端实例信息将从服务器的实例注册表中删除。该下线请求不会自动完成，它需要调用以下内容：DiscoveryManager.getInstance().shutdownComponent();

结合中介理解：房东 (提供者[Eureka] Client Provider) 告诉中介 (服务器[Eureka] Server) 我的房子不租了，中介之后就将注册的房屋信息从列表中剔除。

- **服务剔除 Eviction：**

官方解释：在默认的情况下，当Eureka客户端连续90秒(3个续约周期)没有向Eureka服务器发送服务续约，即心跳，Eureka服务器会将该服务实例从服务注册列表删除，即服务剔除。

结合中介理解：房东(提供者[Eureka] Client Provider) 会定期联系 中介 (服务器[Eureka] Server) 告诉他我的房子还租(续约)，如果中介 (服务器[Eureka] Server) 长时间没收到提供者的信息，那么中介会将他的房屋信息给下架(服务剔除)。

下面就是Netflix官方给出的 Eureka 架构图，你会发现和我们前面画的中介图别无二致。

![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971025.png)

## 2.3 负载均衡之 Ribbon

------

**什么是 RestTemplate?**

------

不是讲Ribbon么？怎么扯到了RestTemplate了？你先别急，听我慢慢道来。更多的关于设计，原理知识点的问题，可以在码匠笔记后台回复实践获取。

RestTemplate是Spring提供的一个访问Http服务的客户端类，怎么说呢？就是微服务之间的调用是使用的RestTemplate。比如这个时候我们 消费者B 需要调用 提供者A 所提供的服务我们就需要这么写。如我下面的伪代码

```java
@Autowired
private RestTemplate restTemplate;
// 这里是提供者A的ip地址，但是如果使用了 Eureka 那么就应该是提供者A的名称
private static final String SERVICE_PROVIDER_A = "http://localhost:8081";

@PostMapping("/judge")
public boolean judge(@RequestBody Request request) {
    String url = SERVICE_PROVIDER_A + "/service1";
    return restTemplate.postForObject(url, request, Boolean.class);
}
12345678910
```

如果你对源码感兴趣的话，你会发现上面我们所讲的 Eureka 框架中的注册、续约等，底层都是使用的RestTemplate。

**为什么需要 Ribbon？**

------

Ribbon 是Netflix公司的一个开源的负载均衡 项目，是一个客户端/进程内负载均衡器，运行在消费者端。

我们再举个例子，比如我们设计了一个秒杀系统，但是为了整个系统的高可用，我们需要将这个系统做一个集群，而这个时候我们消费者就可以拥有多个秒杀系统的调用途径了，如下图。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971026.png)
如果这个时候我们没有进行一些均衡操作，如果我们对秒杀系统1进行大量的调用，而另外两个基本不请求，就会导致秒杀系统1崩溃，而另外两个就变成了傀儡，那么我们为什么还要做集群，我们高可用体现的意义又在哪呢？

所以Ribbon出现了，注意我们上面加粗的几个字——运行在消费者端。指的是，Ribbon是运行在消费者端的负载均衡器，如下图。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971027.png)
其工作原理就是Consumer端获取到了所有的服务列表之后，在其内部使用负载均衡算法，进行对多个系统的调用。

**Nginx 和 Ribbon 的对比**

------

提到负载均衡就不得不提到大名鼎鼎的Nignx了，而和Ribbon不同的是，它是一种集中式的负载均衡器。

何为集中式呢？简单理解就是将所有请求都集中起来，然后再进行负载均衡。如下图。

![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621971028.png)
我们可以看到Nginx是接收了所有的请求进行负载均衡的，而对于Ribbon来说它是在消费者端进行的负载均衡。如下图。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981029.png)
请注意Request的位置，在Nginx中请求是先进入负载均衡器，而在Ribbon中是先在客户端进行负载均衡才进行请求的。

------

**Ribbon 的几种负载均衡算法**

负载均衡，不管Nginx还是Ribbon都需要其算法的支持，如果我没记错的话Nginx使用的是 轮询和加权轮询算法。而在Ribbon中有更多的负载均衡调度算法，其默认是使用的RoundRobinRule轮询策略。

- RoundRobinRule：轮询策略。Ribbon默认采用的策略。若经过一轮轮询没有找到可用的provider，其最多轮询 10 轮。若最终还没有找到，则返回 null。
- RandomRule: 随机策略，从所有可用的 provider 中随机选择一个。
- RetryRule: 重试策略。先按照 RoundRobinRule 策略获取 provider，若获取失败，则在指定的时限内重试。默认的时限为 500 毫秒。

还有很多，这里不一一举了，你最需要知道的是默认轮询算法，并且可以更换默认的负载均衡算法，只需要在配置文件中做出修改就行。

```java
providerName:
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule
123
```

当然，在Ribbon中你还可以自定义负载均衡算法，你只需要实现IRule接口，然后修改配置文件或者自定义Java Config类。

## 2.4 什么是 Open Feign

有了 Eureka，RestTemplate，Ribbon我们就可以愉快地进行服务间的调用了，但是使用RestTemplate还是不方便，我们每次都要进行这样的调用。

```java
@Autowired
private RestTemplate restTemplate;
// 这里是提供者A的ip地址，但是如果使用了 Eureka 那么就应该是提供者A的名称
private static final String SERVICE_PROVIDER_A = "http://localhost:8081";

@PostMapping("/judge")
public boolean judge(@RequestBody Request request) {
    String url = SERVICE_PROVIDER_A + "/service1";
    // 是不是太麻烦了？？？每次都要 url、请求、返回类型的
    return restTemplate.postForObject(url, request, Boolean.class);
}
1234567891011
```

这样每次都调用RestRemplate的API是否太麻烦，我能不能像调用原来代码一样进行各个服务间的调用呢？

聪明的小朋友肯定想到了，那就用映射呀，就像域名和IP地址的映射。我们可以将被调用的服务代码映射到消费者端，这样我们就可以“无缝开发”啦。

OpenFeign 也是运行在消费者端的，使用 Ribbon 进行负载均衡，所以 OpenFeign 直接内置了 Ribbon。

在导入了Open Feign之后我们就可以进行愉快编写 Consumer端代码了。

```java
// 使用 @FeignClient 注解来指定提供者的名字
@FeignClient(value = "eureka-client-provider")
public interface TestClient {
    // 这里一定要注意需要使用的是提供者那端的请求相对路径，这里就相当于映射了
    @RequestMapping(value = "/provider/xxx",
    method = RequestMethod.POST)
    CommonResponse<List<Plan>> getPlans(@RequestBody planGetRequest request);
}
12345678
```

然后我们在Controller就可以像原来调用Service层代码一样调用它了。

```java
@RestController
public class TestController {
    // 这里就相当于原来自动注入的 Service
    @Autowired
    private TestClient testClient;
    // controller 调用 service 层代码
    @RequestMapping(value = "/test", method = RequestMethod.POST)
    public CommonResponse<List<Plan>> get(@RequestBody planGetRequest request) {
        return testClient.getPlans(request);
    }
}
1234567891011
```

## 2.5 必不可少的 Hystrix

什么是 Hystrix之熔断和降级
在分布式环境中，不可避免地会有许多服务依赖项中的某些失败。Hystrix是一个库，可通过添加等待时间容限和容错逻辑来帮助您控制这些分布式服务之间的交互。Hystrix通过隔离服务之间的访问点，停止服务之间的级联故障并提供后备选项来实现此目的，所有这些都可以提高系统的整体弹性。

总体来说[Hystrix]就是一个能进行熔断和降级的库，通过使用它能提高整个系统的弹性。

那么什么是 熔断和降级 呢？再举个例子，此时我们整个微服务系统是这样的。服务A调用了服务B，服务B再调用了服务C，但是因为某些原因，服务C顶不住了，这个时候大量请求会在服务C阻塞。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981030.png)
服务C阻塞了还好，毕竟只是一个系统崩溃了。但是请注意这个时候因为服务C不能返回响应，那么服务B调用服务C的的请求就会阻塞，同理服务B阻塞了，那么服务A也会阻塞崩溃。

请注意，为什么阻塞会崩溃。因为这些请求会消耗占用系统的线程、IO 等资源，消耗完你这个系统服务器不就崩了么。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981031.png)
这就叫服务雪崩。妈耶，上面两个熔断和降级你都没给我解释清楚，你现在又给我扯什么服务雪崩？

所谓熔断就是服务雪崩的一种有效解决方案。当指定时间窗内的请求失败率达到设定阈值时，系统将通过断路器直接将此请求链路断开。

也就是我们上面服务B调用服务C在指定时间窗内，调用的失败率到达了一定的值，那么[Hystrix]则会自动将 服务B与C 之间的请求都断了，以免导致服务雪崩现象。

其实这里所讲的熔断就是指的[Hystrix]中的断路器模式，你可以使用简单的@[Hystrix]Command注解来标注某个方法，这样[Hystrix]就会使用断路器来“包装”这个方法，每当调用时间超过指定时间时(默认为1000ms)，断路器将会中断对这个方法的调用。

当然你可以对这个注解的很多属性进行设置，比如设置超时时间，像这样。

```java
@HystrixCommand(
    commandProperties = {@HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1200")}
)
public List<Xxx> getXxxx() {
    // ...省略代码逻辑
}
123456
```

但是，我查阅了一些博客，发现他们都将熔断和降级的概念混淆了，以我的理解，降级是为了更好的用户体验，当一个方法调用异常时，通过执行另一种代码逻辑来给用户友好的回复。这也就对应着[Hystrix]的后备处理模式。你可以通过设置fallbackMethod来给一个方法设置备用的代码逻辑。比如这个时候有一个热点新闻出现了，我们会推荐给用户查看详情，然后用户会通过id去查询新闻的详情，但是因为这条新闻太火了(比如最近什么*易对吧)，大量用户同时访问可能会导致系统崩溃，那么我们就进行服务降级，一些请求会做一些降级处理比如当前人数太多请稍后查看等等。

```java
// 指定了后备方法调用
@HystrixCommand(fallbackMethod = "getHystrixNews")
@GetMapping("/get/news")
public News getNews(@PathVariable("id") int id) {
    // 调用新闻系统的获取新闻api 代码逻辑省略
}
//
public News getHystrixNews(@PathVariable("id") int id) {
    // 做服务降级
    // 返回当前人数太多，请稍后查看
}
1234567891011
```

什么是Hystrix之其他

我在阅读 《Spring微服务实战》这本书的时候还接触到了一个舱壁模式的概念。在不使用舱壁模式的情况下，服务A调用服务B，这种调用默认的是使用同一批线程来执行的，而在一个服务出现性能问题的时候，就会出现所有线程被刷爆并等待处理工作，同时阻塞新请求，最终导致程序崩溃。而舱壁模式会将远程资源调用隔离在他们自己的线程池中，以便可以控制单个表现不佳的服务，而不会使该程序崩溃。

具体其原理我推荐大家自己去了解一下，本篇文章中对舱壁模式不做过多解释。当然还有[Hystrix]仪表盘，它是用来实时监控[Hystrix]的各项指标信息的，这里我将这个问题也抛出去，希望有不了解的可以自己去搜索一下。

## 2.6 Zuul

ZUUL 是从设备和 web 站点到 Netflix 流应用后端的所有请求的前门。作为边界服务应用，ZUUL 是为了实现动态路由、监视、弹性和安全性而构建的。它还具有根据情况将请求路由到多个 Amazon Auto Scaling Groups（亚马逊自动缩放组，亚马逊的一种云计算方式） 的能力

在上面我们学习了 Eureka 之后我们知道了服务提供者是消费者通过[Eureka] Server进行访问的，即[Eureka] Server是服务提供者的统一入口。那么整个应用中存在那么多消费者需要用户进行调用，这个时候用户该怎样访问这些消费者工程呢？当然可以像之前那样直接访问这些工程。但这种方式没有统一的消费者工程调用入口，不便于访问与管理，而 Zuul 就是这样的一个对于消费者的统一入口。

如果学过前端的肯定都知道 Router 吧，比如 Flutter 中的路由，Vue，React中的路由，用了 Zuul 你会发现在路由功能方面和前端配置路由基本是一个理。

大家对网关应该很熟吧，简单来讲网关是系统唯一对外的入口，介于客户端与服务器端之间，用于对请求进行鉴权、限流、路由、监控等功能。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981032.png)
没错，网关有的功能，Zuul基本都有。而Zuul中最关键的就是路由和过滤器了，在官方文档中Zuul的标题就是

> Router and Filter : Zuul

**Zuul 的路由功能**

简单配置
本来想给你们复制一些代码，但是想了想，因为各个代码配置比较零散，看起来也比较零散，我决定还是给你们画个图来解释吧。

比如这个时候我们已经向[Eureka] Server注册了两个Consumer、三个Provicer，这个时候我们再加个Zuul网关应该变成这样子了。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981033.png)

首先，Zuul需要向 Eureka 进行注册，注册有啥好处呢？

你傻呀，Consumer都向[Eureka] Server进行注册了，我网关是不是只要注册就能拿到所有Consumer的信息了？

拿到信息有什么好处呢？

我拿到信息我是不是可以获取所有的Consumer的元数据(名称，ip，端口)？

拿到这些元数据有什么好处呢？拿到了我们是不是直接可以做路由映射？比如原来用户调用Consumer1的接口localhost:8001/studentInfo/update这个请求，我们是不是可以这样进行调用了呢？localhost:9000/consumer1/studentInfo/update呢？你这样是不是恍然大悟了？

这里的url为了让更多人看懂所以没有使用 restful 风格。

上面的你理解了，那么就能理解关于Zuul最基本的配置了，看下面。

```java
server:
  port: 9000
eureka:
  client:
    service-url:
      # 这里只要注册 Eureka 就行了
      defaultZone: http://localhost:9997/eureka
1234567
```

然后在启动类上加入@EnableZuulProxy注解就行了。没错，就是那么简单😃。

统一前缀
这个很简单，就是我们可以在前面加一个统一的前缀，比如我们刚刚调用的是localhost:9000/consumer1/studentInfo/update，这个时候我们在yaml配置文件中添加如下。

```java
zuul:
  prefix: /zuul
12
```

这样我们就需要通过localhost:9000/zuul/consumer1/studentInfo/update来进行访问了。

**路由策略配置**
你会发现前面的访问方式(直接使用服务名)，需要将微服务名称暴露给用户，会存在安全性问题。所以，可以自定义路径来替代微服务名称，即自定义路由策略。

```java
zuul:
  routes:
    consumer1: /FrancisQ1/**
    consumer2: /FrancisQ2/**
1234
```

这个时候你就可以使用localhost:9000/zuul/FrancisQ1/studentInfo/update进行访问了。

**服务名屏蔽**

这个时候你别以为你好了，你可以试试，在你配置完路由策略之后使用微服务名称还是可以访问的，这个时候你需要将服务名屏蔽。

```java
zuul:
  ignore-services: "*"
12
```

路径屏蔽
Zuul还可以指定屏蔽掉的路径 URI，即只要用户请求中包含指定的 URI 路径，那么该请求将无法访问到指定的服务。通过该方式可以限制用户的权限。

```java
zuul:
  ignore-patterns: **/auto/**
12
```

这样关于 auto 的请求我们就可以过滤掉了。

> ** 代表匹配多级任意路径
> *代表匹配一级任意路径

敏感请求头屏蔽
默认情况下，像 Cookie、Set-Cookie 等敏感请求头信息会被 zuul 屏蔽掉，我们可以将这些默认屏蔽去掉，当然，也可以添加要屏蔽的请求头。

**Zuul 的过滤功能**

如果说，路由功能是Zuul的基操的话，那么过滤器就是Zuul的利器了。毕竟所有请求都经过网关(Zuul)，那么我们可以进行各种过滤，这样我们就能实现限流，灰度发布，权限控制等等。

**简单实现一个请求时间日志打印**

要实现自己定义的Filter我们只需要继承ZuulFilter然后将这个过滤器类以@Component注解加入 Spring 容器中就行了。

在给你们看代码之前我先给你们解释一下关于过滤器的一些注意点。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981034.png)
过滤器类型：Pre、Routing、Post。前置Pre就是在请求之前进行过滤，Routing路由过滤器就是我们上面所讲的路由策略，而Post后置过滤器就是在Response之前进行过滤的过滤器。你可以观察上图结合着理解，并且下面我会给出相应的注释。

```java
// 加入Spring容器
@Component
public class PreRequestFilter extends ZuulFilter {
    // 返回过滤器类型 这里是前置过滤器
    @Override
    public String filterType() {
        return FilterConstants.PRE_TYPE;
    }
    // 指定过滤顺序 越小越先执行，这里第一个执行
    // 当然不是只真正第一个 在Zuul内置中有其他过滤器会先执行
    // 那是写死的 比如 SERVLET_DETECTION_FILTER_ORDER = -3
    @Override
    public int filterOrder() {
        return 0;
    }
    // 什么时候该进行过滤
    // 这里我们可以进行一些判断，这样我们就可以过滤掉一些不符合规定的请求等等
    @Override
    public boolean shouldFilter() {
        return true;
    }
    // 如果过滤器允许通过则怎么进行处理
    @Override
    public Object run() throws ZuulException {
        // 这里我设置了全局的RequestContext并记录了请求开始时间
        RequestContext ctx = RequestContext.getCurrentContext();
        ctx.set("startTime", System.currentTimeMillis());
        return null;
    }
}

// lombok的日志
@Slf4j
// 加入 Spring 容器
@Component
public class AccessLogFilter extends ZuulFilter {
    // 指定该过滤器的过滤类型
    // 此时是后置过滤器
    @Override
    public String filterType() {
        return FilterConstants.POST_TYPE;
    }
    // SEND_RESPONSE_FILTER_ORDER 是最后一个过滤器
    // 我们此过滤器在它之前执行
    @Override
    public int filterOrder() {
        return FilterConstants.SEND_RESPONSE_FILTER_ORDER - 1;
    }
    @Override
    public boolean shouldFilter() {
        return true;
    }
    // 过滤时执行的策略
    @Override
    public Object run() throws ZuulException {
        RequestContext context = RequestContext.getCurrentContext();
        HttpServletRequest request = context.getRequest();
        // 从RequestContext获取原先的开始时间 并通过它计算整个时间间隔
        Long startTime = (Long) context.get("startTime");
        // 这里我可以获取HttpServletRequest来获取URI并且打印出来
        String uri = request.getRequestURI();
        long duration = System.currentTimeMillis() - startTime;
        log.info("uri: " + uri + ", duration: " + duration / 100 + "ms");
        return null;
    }
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

上面就简单实现了请求时间日志打印功能，你有没有感受到Zuul过滤功能的强大了呢？

没有？好的、那我们再来。

**令牌桶限流**

当然不仅仅是令牌桶限流方式，Zuul只要是限流的活它都能干，这里我只是简单举个例子。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981035.png)

**我先来解释一下什么是令牌桶限流吧。**

首先我们会有个桶，如果里面没有满那么就会以一定固定的速率会往里面放令牌，一个请求过来首先要从桶中获取令牌，如果没有获取到，那么这个请求就拒绝，如果获取到那么就放行。很简单吧，啊哈哈、

下面我们就通过Zuul的前置过滤器来实现一下令牌桶限流。

```java
@Component
@Slf4j
public class RouteFilter extends ZuulFilter {
    // 定义一个令牌桶，每秒产生2个令牌，即每秒最多处理2个请求
    private static final RateLimiter RATE_LIMITER = RateLimiter.create(2);
    @Override
    public String filterType() {
        return FilterConstants.PRE_TYPE;
    }

    @Override
    public int filterOrder() {
        return -5;
    }

    @Override
    public Object run() throws ZuulException {
        log.info("放行");
        return null;
    }

    @Override
    public boolean shouldFilter() {
        RequestContext context = RequestContext.getCurrentContext();
        if(!RATE_LIMITER.tryAcquire()) {
            log.warn("访问量超载");
            // 指定当前请求未通过过滤
            context.setSendZuulResponse(false);
            // 向客户端返回响应码429，请求数量过多
            context.setResponseStatusCode(429);
            return false;
        }
        return true;
    }
}
1234567891011121314151617181920212223242526272829303132333435
```

这样我们就能将请求数量控制在一秒两个，有没有觉得很酷？

关于 Zuul 的其他
Zuul的过滤器的功能肯定不止上面我所实现的两种，它还可以实现权限校验，包括我上面提到的灰度发布等等。

当然，Zuul作为网关肯定也存在单点问题，如果我们要保证Zuul的高可用，我们就需要进行Zuul的集群配置，这个时候可以借助额外的一些负载均衡器比如Nginx。

## 2.7 Spring Cloud配置管理——Config

为什么要使用进行配置管理？
当我们的微服务系统开始慢慢地庞大起来，那么多Consumer、Provider、[Eureka] Server、Zuul系统都会持有自己的配置，这个时候我们在项目运行的时候可能需要更改某些应用的配置，如果我们不进行配置的统一管理，我们只能去每个应用下一个一个寻找配置文件然后修改配置文件再重启应用。

首先对于分布式系统而言我们就不应该去每个应用下去分别修改配置文件，再者对于重启应用来说，服务无法访问所以直接抛弃了可用性，这是我们更不愿见到的。

那么有没有一种方法既能对配置文件统一地进行管理，又能在项目运行时动态修改配置文件呢？

那就是我今天所要介绍的Spring Cloud Config。

能进行配置管理的框架不止Spring Cloud Config一种，大家可以根据需求自己选择（disconf，阿波罗等等）。而且对于Config来说有些地方实现的不是那么尽人意。

Config 是什么
Spring Cloud Config为分布式系统中的外部化配置提供服务器和客户端支持。使用Config服务器，可以在中心位置管理所有环境中应用程序的外部属性。

简单来说，Spring Cloud Config就是能将各个 应用/系统/模块 的配置文件存放到统一的地方然后进行管理(Git 或者 SVN)。

你想一下，我们的应用是不是只有启动的时候才会进行配置文件的加载，那么我们的Spring Cloud Config就暴露出一个接口给启动应用来获取它所想要的配置文件，应用获取到配置文件然后再进行它的初始化工作。就如下图。
![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981036.png)

当然这里你肯定还会有一个疑问，如果我在应用运行时去更改远程配置仓库(Git)中的对应配置文件，那么依赖于这个配置文件的已启动的应用会不会进行其相应配置的更改呢？

答案是不会的。

那怎么进行动态修改配置文件呢？这不是出现了配置漂移吗？

别急嘛，你可以使用Webhooks，这是 github提供的功能，它能确保远程库的配置文件更新后客户端中的配置信息也得到更新。

噢噢，这还差不多。我去查查怎么用。

慢着，听我说完，Webhooks虽然能解决，但是你了解一下会发现它根本不适合用于生产环境，所以基本不会使用它的。

而一般我们会使用Bus消息总线 +Spring Cloud Config进行配置的动态刷新。

**引出 Spring Cloud Bus**

用于将服务和服务实例与分布式消息系统链接在一起的事件总线。在集群中传播状态更改很有用（例如配置更改事件）。

你可以简单理解为Spring Cloud Bus的作用就是管理和广播分布式系统中的消息，也就是消息引擎系统中的广播模式。当然作为消息总线的Spring Cloud Bus可以做很多事而不仅仅是客户端的配置刷新功能。

而拥有了Spring Cloud Bus之后，我们只需要创建一个简单的请求，并且加上@ResfreshScope注解就能进行配置的动态修改了，下面我画了张图供你理解。

![在这里插入图片描述](assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Y2X5pa55pyJ6ZuoWWE=,size_20,color_FFFFFF,t_70,g_se,x_16-16619982621981037.png)

总结

参考：https://blog.csdn.net/qq_43061290/article/details/106279624

# 3 其他问题

## 3.1 SpringBoot和SpringCloud的区别？

SpringBoot专注于快速方便的开发单个个体微服务。

SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，
为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等集成服务

SpringBoot可以离开SpringCloud独立使用开发项目， 但是SpringCloud离不开SpringBoot ，属于依赖的关系
SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架

## 3.2 使用 Spring Boot 开发分布式微服务时，我们面临以下问题

（1）与分布式系统相关的复杂性-这种开销包括网络问题，延迟开销，带宽问题，安全问题。

（2）服务发现-服务发现工具管理群集中的流程和服务如何查找和互相交谈。它涉及一个服务目录，在该目录中注册服务，然后能够查找并连接到该目录中的服务。

（3）冗余-分布式系统中的冗余问题。

（4）负载平衡 --负载平衡改善跨多个计算资源的工作负荷，诸如计算机，计算机集群，网络链路，中央处理单元，或磁盘驱动器的分布。

（5）性能-问题 由于各种运营开销导致的性能问题。

（6）部署复杂性-Devops 技能的要求。

## 3.3 服务注册和发现是什么意思？Spring Cloud 如何实现？

当我们开始一个项目时，我们通常在属性文件中进行所有的配置。随着越来越多的服务开发和部署，添加和修改这些属性变得更加复杂。有些服务可能会下降，而某些位置可能会发生变化。手动更改属性可能会产生问题。 Eureka 服务注册和发现可以在这种情况下提供帮助。由于所有服务都在 Eureka 服务器上注册并通过调用 Eureka 服务器完成查找，因此无需处理服务地点的任何更改和处理。

## 3.4 Spring Cloud 和dubbo区别?

（1）服务调用方式 dubbo是RPC springcloud Rest Api

（2）注册中心,dubbo 是zookeeper springcloud是eureka，也可以是zookeeper

（3）服务网关,dubbo本身没有实现，只能通过其他第三方技术整合，springcloud有Zuul路由网关，作为路由服务器，进行消费者的请求分发,springcloud支持断路器，与git完美集成配置文件支持版本控制，事物总线实现配置文件的更新与服务自动装配等等一系列的微服务架构要素。

## 3.5 负载平衡的意义什么？

在计算中，负载平衡可以改善跨计算机，计算机集群，网络链接，中央处理单元或磁盘驱动器等多种计算资源的工作负载分布。负载平衡旨在优化资源使用，最大化吞吐量，最小化响应时间并避免任何单一资源的过载。使用多个组件进行负载平衡而不是单个组件可能会通过冗余来提高可靠性和可用性。负载平衡通常涉及专用软件或硬件，例如多层交换机或域名系统服务器进程。

## 3.6 什么是 Hystrix？它如何实现容错？

Hystrix 是一个延迟和容错库，旨在隔离远程系统，服务和第三方库的访问点，当出现故障是不可避免的故障时，停止级联故障并在复杂的分布式系统中实现弹性。
通常对于使用微服务架构开发的系统，涉及到许多微服务。这些微服务彼此协作。
思考以下微服务

假设如果上图中的微服务 9 失败了，那么使用传统方法我们将传播一个异常。但这仍然会导致整个系统崩溃。

随着微服务数量的增加，这个问题变得更加复杂。微服务的数量可以高达 1000.这是 hystrix 出现的地方 我们将使用 Hystrix 在这种情况下的 Fallback 方法功能。我们有两个服务 employee-consumer 使用由 employee-consumer 公开的服务。

简化图如下所示

现在假设由于某种原因，employee-producer 公开的服务会抛出异常。我们在这种情况下使用 Hystrix 定义了一个回退方法。这种后备方法应该具有与公开服务相同的返回类型。如果暴露服务中出现异常，则回退方法将返回一些值。

## 3.7 什么是 Hystrix 断路器？我们需要它吗？

由于某些原因，employee-consumer 公开服务会引发异常。在这种情况下使用Hystrix 我们定义了一个回退方法。如果在公开服务中发生异常，则回退方法返回一些默认值。

如果 firstPage method() 中的异常继续发生，则 Hystrix 电路将中断，并且员工使用者将一起跳过 firtsPage 方法，并直接调用回退方法。 断路器的目的是给第一页方法或第一页方法可能调用的其他方法留出时间，并导致异常恢复。可能发生的情况是，在负载较小的情况下，导致异常的问题有更好的恢复机会 。

## 3.8 什么是 Netflix Feign？它的优点是什么？

Feign 是受到 Retrofit，JAXRS-2.0 和 WebSocket 启发的 java 客户端联编程序。
Feign 的第一个目标是将约束分母的复杂性统一到 http apis，而不考虑其稳定性。
在 employee-consumer 的例子中，我们使用了 employee-producer 使用 REST模板公开的 REST 服务。
但是我们必须编写大量代码才能执行以下步骤

（1）使用功能区进行负载平衡。

（2）获取服务实例，然后获取基本 URL。

（3）利用 REST 模板来使用服务。 前面的代码如下

```java
@Controller
public class ConsumerControllerClient {
@Autowired
private LoadBalancerClient loadBalancer;
public void getEmployee() throws RestClientException, IOException {
	ServiceInstance serviceInstance=loadBalancer.choose("employee-producer");
	System.out.println(serviceInstance.getUri());
	String baseUrl=serviceInstance.getUri().toString();
	baseUrl=baseUrl+"/employee";
	RestTemplate restTemplate = new RestTemplate();
	ResponseEntity<String> response=null;
	try{
		response=restTemplate.exchange(baseUrl,
					HttpMethod.GET, getHeaders(),String.class);
	}
	catch (Exception ex)
		{
		System.out.println(ex);
	}
	System.out.println(response.getBody());
}
123456789101112131415161718192021
```

之前的代码，有像 NullPointer 这样的例外的机会，并不是最优的。我们将看到如何使用 Netflix Feign 使呼叫变得更加轻松和清洁。如果 Netflix Ribbon 依赖关系也在类路径中，那么 Feign 默认也会负责负载平衡。

## 3.9 什么是 Spring Cloud Bus？我们需要它吗？

考虑以下情况：我们有多个应用程序使用 Spring Cloud Config 读取属性，而Spring Cloud Config 从 GIT 读取这些属性。

下面的例子中多个员工生产者模块从 Employee Config Module 获取 Eureka 注册的财产。

如果假设 GIT 中的 Eureka 注册属性更改为指向另一台 Eureka 服务器，会发生什么情况。在这种情况下，我们将不得不重新启动服务以获取更新的属性。

还有另一种使用执行器端点/刷新的方式。但是我们将不得不为每个模块单独调用这个 url。例如，如果 Employee Producer1 部署在端口 8080 上，则调用 http：// localhost：8080 / refresh。同样对于 Employee Producer2 http：//localhost：8081 / refresh 等等。这又很麻烦。这就是 Spring Cloud Bus 发挥作用的地方。

Spring Cloud Bus 提供了跨多个实例刷新配置的功能。因此，在上面的示例中，如果我们刷新 Employee Producer1，则会自动刷新所有其他必需的模块。如果我们有多个微服务启动并运行，这特别有用。这是通过将所有微服务连接到单个消息代理来实现的。无论何时刷新实例，此事件都会订阅到侦听此代理的所有微服务，并且它们也会刷新。可以通过使用端点/总线/刷新来实现对任何单个实例的刷新。

## 3.10 Spring Cloud断路器的作用

当一个服务调用另一个服务由于网络原因或自身原因出现问题，调用者就会等待被调用者的响应 当更多的服务请求到这些资源导致更多的请求等待，发生连锁效应（雪崩效应）

断路器有完全打开状态:一段时间内 达到一定的次数无法调用 并且多次监测没有恢复的迹象 断路器完全打开 那么下次请求就不会请求到该服务

半开:短时间内 有恢复迹象 断路器会将部分请求发给该服务，正常调用时 断路器关闭

关闭：当服务一直处于正常状态 能正常调用

## 3.11 什么是Spring Cloud Config?

在分布式系统中，由于服务数量巨多，为了方便服务配置文件统一管理，实时更新，所以需要分布式配置中心组件。在Spring Cloud中，有分布式配置中心组件spring cloud config ，它支持配置服务放在配置服务的内存中（即本地），也支持放在远程Git仓库中。在spring cloud config 组件中，分两个角色，一是config server，二是config client。
使用：

（1）添加pom依赖

（2）配置文件添加相关配置

（3）启动类添加注解@EnableConfigServer

## 3.12 什么是Spring Cloud Gateway?

Spring Cloud Gateway是Spring Cloud官方推出的第二代网关框架，取代Zuul网关。网关作为流量的，在微服务系统中有着非常作用，网关常见的功能有路由转发、权限校验、限流控制等作用。

使用了一个RouteLocatorBuilder的bean去创建路由，除了创建路由RouteLocatorBuilder可以让你添加各种predicates和filters，predicates断言的意思，顾名思义就是根据具体的请求的规则，由具体的route去处理，filters是各种过滤器，用来对请求做各种判断和修改。

