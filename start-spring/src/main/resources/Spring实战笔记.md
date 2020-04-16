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

##第4章 面向切面的Spring

#第2部分 Web中的Spring
##第5章 构建Spring Web应用程序
##第6章 渲染Web视图
##第7章 Spring MVC的高级技术
##第8章 使用Spring Web Flow
##第9章 保护Web应用

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