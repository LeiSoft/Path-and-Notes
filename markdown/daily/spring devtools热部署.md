# springboot dev-tools 热加载自动重启以及实时刷新页面 



springboot 2.x ,java 8, idea/[eclipse](https://so.csdn.net/so/search?q=eclipse&spm=1001.2101.3001.7020)

### 一，自动重启以及与实现原理

#### 1.引入依赖

```xml
<dependencies>
  <dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <optional>true</optional>
  </dependency>
</dependencies>
1234567
```

#### 2.devtools 实现应用的自动重启

devtools 可以对classpath 下的所有文件进行监控,当classpath下面的文件发现改变时，进行自动重启，

- idea触发自动重启的条件为，

  ***Build -> Build Project***

- eclipse 触发自动重启的条件

  ***保存代码***

> 有没有觉得这里的自动重启没有那么自动，我也这么觉得，因为需要手动触发 Build 才可以，自动重启，原理在后面解释

静态文件修改不需要自动重启的,可以通过如下参数设置，排斥自动加载

spring.devtools.restart.exclude=static/**,public/**

> 如果你使用的是 maven 或者gradle 自动重启， 需要将参数 forking 设置为enabled, devTools依赖于应用程序上下文的shutdown钩子来在重启期间关闭它。如果禁用了shutdown钩子，它就不能正常工作 (SpringApplication.setRegisterShutdownHook(false))

#### 3.dev-tools 热加载实现原理：

Restart 技术（自动重启）采用了两个类加载器实现，对于有的class是不会被改变的，比如第三方依赖jar,这部分class被加载到一个 base 类加载器中，对你自己日常开发的class文件则被加载到另外一个 restart类加载器中。重启的时候，直接销毁掉restart类加载器，重新创建一个新的restart类加载器，从而实现快速重启的功能，因为 base 类加载器中的类没有发生改变，加载一次就好了

> 如果您发现重新启动对您的应用程序来说不够快，或者遇到类加载问题，您可以考虑ZeroTurnaround 公司的重加载技术
> JRebel。这些方法是在加载类时重写它们，使它们更易于重新加载

当你使用java -jar 形式运行一个springboot 项目时 dev-tools 将被自动禁用（dev-tools根据使用的类加载器来判断是否启用，springboot以jar启动时使用的是自定义类加载器）,你也可以通过系统参数进行启用，这时dev-tools 将忽略你所使用的类加载器类型：

```java
-Dspring.devtools.restart.enabled=true
1
```

如上方式，将启动 dev-tools, dev-tools 适合在开发环境下使用，不建议在生产中启用。springboot 很多组件都提供了缓存的功能，比如 template engines, 缓存已经编译好的模板，避免了重复解析模板文件，spring mvc 也缓存了请求头信息，用来提升访问静态文件的性能， 缓存是个很好的设计，但是如果用在开发环境，会比较容易出错，写bug将变得更简单。 在开发环境，可以通过spirng-boot-devtools 配置禁用缓存。

当然，带缓存的一般都会有特定的设置可以进行禁用比如 thymeleaf, 提供了 spring.thymeleaf.cache 缓存开关,spring-boot-devtools模块不需要手动设置这些属性，而是自动应用合理的开发时配置,

更完整的默认值设置在这个文件下面：
[默认值设置](https://github.com/spring-projects/spring-boot/blob/v2.4.5/spring-boot-project/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java)

### 二，页面自动刷新

页面自动刷新需要和浏览器插件配合使用，比如chrome 浏览器需要安装 LiveReload，需要将插件启用，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210524220528291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYV9jaGVuZ18wMDc=,size_16,color_FFFFFF,t_70#pic_center)

浏览器插件装好后，启动springboot 项目，修改完静态文件，然后触发重新编译，页面将自动刷新，触发重新编译，不同的编译器，不同的方式，和上面的自动重启是一样的

- idea触发自动重启的条件为，

  ***Build -> Build Project***

- eclipse 触发自动重启的条件

  ***保存代码***

我们以Idea为例， 引入了dev-tools 插件的 SpringBoot 项目启动时，会同时启动一个 liveReload server 服务（这个服务为后端服务与前端浏览器插件进行交互的 websocket 服务），可以通过以下日志验证

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210524220739494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYV9jaGVuZ18wMDc=,size_16,color_FFFFFF,t_70#pic_center)
修改文件后，重新编译项目，如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210524220806226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYV9jaGVuZ18wMDc=,size_16,color_FFFFFF,t_70#pic_center)
重新编译后，前端页面自动刷新，通过网络监控可以看到，这个是一个websocket协议通知触发的自动刷新

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210524220827830.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppYV9jaGVuZ18wMDc=,size_16,color_FFFFFF,t_70#pic_center)