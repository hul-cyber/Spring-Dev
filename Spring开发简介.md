# Spring开发
什么是Spring？
Spring是一个支持快速开发Java EE应用程序的框架。它提供了一系列底层容器和基础设施，并可以和大量常用的开源框架无缝集成，可以说是开发Java EE应用程序的必备。
Spring最早是由Rod Johnson这哥们在他的《Expert One-on-One J2EE Development without EJB》一书中提出的用来取代EJB的轻量级框架。随后这哥们又开始专心开发这个基础框架，并起名为Spring Framework。
随着Spring越来越受欢迎，在Spring Framework基础上，又诞生了Spring Boot、Spring Cloud、Spring Data、Spring Security等一系列基于Spring Framework的项目。本章我们只介绍Spring Framework，即最核心的Spring框架。后续章节我们还会涉及Spring Boot、Spring Cloud等其他框架。

## Spring Framework
Spring Framework主要包括几个模块：
1. 支持IoC和AOP的容器；
2. 支持JDBC和ORM的数据访问模块；
3. 支持声明式事务的模块；
4. 支持基于Servlet的MVC开发；
5. 支持基于Reactive的Web开发；
6. 以及集成JMS、JavaMail、JMX、缓存等其他模块。

## Spring的优良特性
1. 非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API
2. 控制反转：IOC——Inversion of Control，指的是将对象的创建权交给 Spring 去创建。使用 Spring 之前，对象的创建都是由我们自己在代码中new创建。而使用 Spring 之后。对象的创建都是给了 Spring 框架。
3. 依赖注入：DI——Dependency Injection，是指依赖的对象不需要手动调用 setXX 方法去设置，而是通过配置赋值。
（IOC与DI是一件事情的两面）
4. 面向切面编程：Aspect Oriented Programming——AOP
5. 容器：Spring 是一个容器，因为它包含并且管理应用对象的生命周期
6. 组件化：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。（我们可以使用Spring中的一部分组件）
7. 一站式：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上 Spring 自身也提供了表述层的 SpringMVC 和持久层的 Spring JDBC）

## 使用Spring的好处
1. Spring 在一个单元模式中是有组织的。**即使包和类的数量非常大，你只要担心你需要的，而其它的就可以忽略了**。
2. Spring 不会让你白费力气做重复工作，它真正的利用了一些现有的技术，像 ORM 框架、日志框架、JEE、Quartz 和 JDK 计时器，其他视图技术。(Spring可以集成其它的组件)
3. Spring 的 web 框架是一个设计良好的 web MVC 框架，它为比如 Structs 或者其他工程上的或者不怎么受欢迎的 web 框架提供了一个很好的供替代的选择。**MVC 模式导致应用程序的不同方面(输入逻辑，业务逻辑和UI逻辑)分离，同时提供这些元素之间的松散耦合。模型(Model)封装了应用程序数据，通常它们将由 POJO 类组成。视图(View)负责渲染模型数据，一般来说它生成客户端浏览器可以解释 HTML 输出。控制器(Controller)负责处理用户请求并构建适当的模型，并将其传递给视图进行渲染。**
4. Spring 对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低。
5. 轻量级的 IOC 容器往往是轻量级的

