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

