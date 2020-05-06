#第1部分 Spring的核心
##第1章 Spring之旅
四大策略：
* 通过依赖注入和面向接口实现松耦合
* 通过切面和模板减少样板式代码
* 基于切面和惯例进行声明式编程
* 基于POJO的轻量级和最小侵入性编程

##第2章 装配Bean
* 自动化装配bean：使用注解@Component标识一个bean，使用@Autowire装配bean (记得开启组件扫描，可以用类(推荐)或xml)
* JavaConfig装配bean: 使用@Configuration @Bean new 来装配bean（对原来的类没由入侵，适合装配第三方库）
* XML方式：不再是第一选择，除非是维护老的项目
* 混合方式：首先自动化装配不需要关心从bean从哪来的
            其次，JavaConfig引用JavaConfig用@Import   JavaConfig引用XML使用@ImportResource
                  XML引用XML使用<import>              XML引用JavaConfig使用<bean>
* 关于三种方式装配细节的一些疑问？
    * 自动化装配bean：@Autowire装配可以作用于构造器，方法（没用过）（网上还有说属性注入和接口注入？）
    * JavaConfig装配bean：JavaConfig装配可以使用任何你想到的方法装配（我只想到了用new？），限制不在Spring
    * XML方式：装配显得更加被动，调用构造器完成的（也可以属性注入？）
    
##第3章 高级装配
* 环境与配置：这是一种很棒的方法，通过环境激活状态，去动态创建bean
    JavaConfig可以用@Profile，XML可以用profile属性。都可以作用在整个文件（Java或xml）或者方法上
* 条件化的bean: 可能某些复杂场景需要
    可以根据多种条件创建（包含某个环境变量，某个特定的bean也声明了之后...）
* 歧义性：如果有不止一个可以装配的bean就会发生错误
    通过@Primary来指定首选bean，或者做一些限定条件（我觉得耦合太高了，也不灵活，我暂时用不到）

* bean的作用域：Spring默认单例，上下文注入的其实是同一个对象，我们可以通过@Scope来设置不同的作用域
  但是不同的作用域装配时会发生问题，你不可能把一个还没有的bean拿来装配，我们通过作用域代理来实现（CGLib）
    * 单例，整个应用中就一个实例（默认的）
    * 原型，每次注入或从上下文获取的时候，会创建一个新的实例
    * 会话，在Web应用中，每次会话创建一个新的实例
    * 请求，在Web应用中，每次请求创建一个新的实例
* 之前的讨论大多时注入引用，也讨论了常量，但常量都是写死硬编码的。我们需要有些值在运行时再确定怎么处理
    *可以使用占位符（${...}）
    *可以声明属性源，然后用Environment的方法获取
* SpEL表达式，可以帮助在注入的时候灵活的计算（调用方法，属性，运算符，正则，集合），SpEL也应用到了其他
    方面比如Security，Thymeleaf
    
##第4章 面向切面的Spring
* 横切关注点：散落于应用中多处的功能被称为横切关注点。AOP（通过切面编程）就是为了把横切关注点与业务逻辑相分离
* 通知：切面的具体工作被称为通知定义了做什么，用代码实现
    * 通知类型（做的方式）有5种：
    * 前置通知 @Before
    * 后置通知 @After
    * 返回通知 @AfterReturning
    * 环绕通知 @Around
    * 异常通知 @AfterThrowing
* 切点：通知在什么地方执行(@Pointcut)，用AspectJ切点表达式实现
* 切面：通知+切点，就构成了完整的切面(@Aspect)，用@EnableAspectJAutoProxy开启AspectJ自动代理
* AOP框架各有不同，比如AspectJ,JBoss：这里先讨论SpringAOP
    * 基于代理的经典AOP
    * POJO切面
    * @AspectJ注解驱动的切面
    * 注入式AspectJ切面
* 通知的参数，我们也可以通过切点表达式获取被包装方法的参数，然后传递给我们的通知
* 使用@DeclareParents注解我们甚至能不修改源码的情况下为被代理类添加方法

#第2部分 Web中的Spring

##第5章 构建Spring Web应用程序
##第6章 渲染Web视图
##第7章 Spring MVC的高级技术
##第8章 使用Spring Web Flow
##第9章 保护Web应用
* 9.1 Spring Security简介
    * Spring Security提供声明式的安全保护，能够在Web请求级别（基于Filter）和方法级别（基于AOP）处理认证和授权
    * Spring Security3.2分为11个模块，常用的有Core Configuration Web TagLibrary
    * 我们是借助各种Filter来提供安全服务的，借助Spring的小技巧，我们只需要配置一个DelegatingFilterProxy（Servlet上下文）
      就行了，它会把工作委托给Spring的已注入的Filter(Spring上下文)
    * 1.启用Spring Security：先配置DelegatingFilterProxy过滤器，可以使用XML配置。也可以使用Java配置（任何实现了WebSecurityConfigurer接口的
      都可以用来配置SpringSecurity）
    * 2.指定安全细节：重载WebSecurityConfigurerAdapter的configure()方法，我们可以指定安全细节
    
* 9.2 选择查询用户详细信息的服务
    * Spring Security内置了多种存储场景，内存，关系型数据库，LDAP，我们也可以编写并插入自定义的实现
    * 内存存储（开发测试用）：
    ```
        auth.inMemoryAuthentication().withUser("user").password("111111").roles("USER")
    ```
    * 关系型数据库：
    ```
        auth.jdbcAuthentication().dataSource(dataSource)
                        .usersByUsernameQuery("select username,password,true from Spitter where username=?")
                        .authoritiesByUsernameQuery("select username,ROLE_USER from Spitter where username=?")
                        .passwordEncoder(new StandardPasswordEncoder("53cr3t"));
    ```
    * 自定义数据源：
    我们自己定义一个UserDetailsService接口实现，根据用户名查询UserDetails对象
    
* 9.3 拦截请求

* 9.4 任务用户
* 9.5 保护视图
* 9.6 小结
#第3部分 后端中的Spring
##第10章 通过Spring和JDBC征服数据库
##第11章 使用对象-关系映射持久化数据
##第12章 使用NoSQL数据库
##第13章 缓存数据
##第14章 保护方法应用

#第4部分 Spring集成
##第15章 使用远程服务
##第16章 使用Spring MVC创建REST API
##第17章 Spring消息
##第18章 使用WebSocket和STOMP实现消息功能
##第19章 使用Spring发送Email
##第20章 使用JMX管理Spring Bean
##第21章 借助Spring Boot简化Spring开发