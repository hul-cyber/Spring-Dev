## Spring的BeanFactory容器
这是一个最简单的容器，它主要的功能是为依赖注入(DI)提供支持，这个容器接口在`org.springframework.beans.factory.BeanFactory`中被定义。
在 Spring 中，有大量对 BeanFactory 接口的实现。其中，最常被使用的是 **XmlBeanFactory** 类。这个容器从一个 XML 文件中读取配置元数据，由这些元数据来生成一个被配置化的系统或者应用。
在资源宝贵的移动设备或者基于 applet 的应用当中， BeanFactory 会被优先选择。否则，一般使用的是 ApplicationContext，除非你有更好的理由选择 BeanFactory。

## Spring的ApplicationContext容器
Application Context是BeanFactory的子接口，也被称为Spring上下文。
Application Context是spring中较高级的容器。和BeanFactory类似，它可以加载配置文件中定义的 bean，将所有的bean集中在一起，当有请求的时候分配bean。 另外，它增加了企业所需要的功能，比如，从属性文件中解析文本信息和将事件传递给所指定的监听器。这个容器在 org.springframework.context.ApplicationContext interface 接口中定义。
ApplicationContext 包含 BeanFactory 所有的功能，一般情况下，相对于 BeanFactory，ApplicationContext 会更加优秀。当然，BeanFactory 仍可以在轻量级应用中使用，比如移动设备或者基于 applet 的应用程序。
最常被使用的 ApplicationContext 接口实现：
`FileSystemXmlApplicationContext`：该容器从 XML 文件中加载已被定义的 bean。在这里，你需要提供给构造器 XML 文件的完整路径。
`ClassPathXmlApplicationContext`：该容器从 XML 文件中加载已被定义的 bean。在这里，你不需要提供 XML 文件的完整路径，只需正确配置 CLASSPATH 环境变量即可，因为，容器会从 CLASSPATH 中搜索 bean 配置文件。关于IDEA中的CLASSPATH,IDEA中的CLASSPATH即`target-classes`目录。关于CLASSPATH,可以参考这篇文章[eclipse与intellij idea中的classpath分析](https://blog.csdn.net/SkyeBeFreeman/article/details/56495637).
`WebXmlApplicationContext`：该容器会在一个 web 应用程序的范围内加载在 XML 文件中已被定义的bean。
`AnnotationConfigApplicationContext`：从注解中加载已被定义的bean.