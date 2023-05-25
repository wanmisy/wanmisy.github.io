# SpringlOC之Bean生命周期重点步骤详解

## 什么是 Spring Bean 的生命周期 
对于普通的 Java 对象，当 new 的时候创建对象，然后该对象就能够使用了。一旦该对象不再被使用，则由 Java 自动进行垃圾回收。
而 Spring 中的对象是 bean，bean 和普通的 Java 对象没啥大的区别，只不过 Spring 不再自己去 new 对象了，而是由 IoC 容器去帮助我们实例化对象并且管理它，我们需要哪个对象，去问 IoC 容器要即可。IoC 其实就是解决对象之间的耦合问题，Spring Bean 的生命周期完全由容器控制。
## Bean的生命周期

1. 实例化阶段：Spring 容器在读取 Bean 的配置信息后，实例化 Bean 的对象。
2. 属性设置阶段：实例化之后，Spring 容器调用 Bean 的 setter 方法来设置属性值。
3. BeanNameAware 接口回调阶段：如果 Bean 实现了 BeanNameAware 接口，Spring 容器会调用 Bean 的 setBeanName() 方法，传入 Bean 的 ID。
4. BeanFactoryAware 接口回调阶段：如果 Bean 实现了 BeanFactoryAware 接口，Spring 容器会调用 Bean 的 setBeanFactory() 方法，传入工厂实例对象。
5. BeanPostProcessor 接口回调阶段：Spring 容器在 Bean 的初始化前后调用所有注册的 BeanPostProcessor 接口方法。
6. 初始化阶段：如果 Bean 实现了 InitializingBean 接口，则 Spring 容器会调用 Bean 的 afterPropertiesSet() 方法；如果在配置文件中指定了 init-method 属性，则会调用指定的方法。此阶段表示 Bean 已经准备就绪，随时可以使用。
7. DisposableBean 接口回调阶段：如果 Bean 实现了 DisposableBean 接口，则 Spring 容器关闭前会调用 Bean 的 destroy() 方法；如果在配置文件中指定了 destroy-method 属性，则会调用指定的方法。
8. 销毁阶段：Bean 对象被回收的阶段。
需要注意的是，生命周期中的第 1、2 步是在 BeanFactory 中处理的，剩余步骤是在 ApplicationContext 中处理的。这是因为 ApplicationContext 对 Bean 生命周期进行了更高层次的封装，可以扩展更多的企业功能，例如 AOP、事务和 JavaMail 等。

## Bean的生命周期的扩展点
Bean 生命周期的扩展点主要包括 BeanPostProcessor 和 BeanFactoryPostProcessor 等，在 Spring 框架中还有其他一些扩展点。下面我将详细介绍这些扩展点的含义和作用。

* BeanPostProcessor
> BeanPostProcessor 接口定义了两个方法：postProcessBeforeInitialization 和 postProcessAfterInitialization。这两个方法是在 Bean 初始化前和初始化后进行调用的，开发人员可以编写自定义的逻辑来处理 Bean 实例。例如，可以在 Bean 初始化前对 Bean 进行一些属性处理，或在 Bean 初始化后对 Bean 进行一些校验操作。

* BeanFactoryPostProcessor
> BeanFactoryPostProcessor 接口用于修改 BeanFactory 中的 Bean 配置信息。它可以修改 Bean 的定义（BeanDefinition），以达到修改 Bean 属性或添加自定义逻辑的目的。BeanFactoryPostProcessor 接口的子类有 PropertyPlaceholderConfigurer、AutowiredAnnotationBeanPostProcessor 等。
* ServletContextAware
> ServletContextAware 是一个标记接口，用于标记类需要获取 ServletContext 对象。当一个 Bean 实现了该接口时，Spring 容器会自动调用 setServletContext() 方法，将 ServletContext 对象注入到该 Bean 实例中。该接口的作用是为了让 Spring Bean 访问 Servlet API 相关的资源。
* BeanNameAware
> BeanNameAware 用于为 Bean 设置名字，将 Bean 的名字注入到实现该接口的类中。
* BeanFactoryAware
>BeanFactoryAware 用于注入 BeanFactory 对象，在实现该接口的类中就可以以编程方式动态获取 Bean。
* InitializingBean 和 DisposableBean
> InitializingBean 和 DisposableBean 接口分别定义了 Bean 初始化的初始化回调方法和 Bean 销毁前的销毁回调方法。开发人员可以在这些回调方法中编写自定义的初始化或销毁逻辑。  
除了上述几个扩展点外，Spring 还提供了很多其他的扩展点，如 ApplicationListener、SmartInitializingSingleton 等，通过这些扩展点，开发人员可以更加灵活自定义 Bean 的初始化和销毁过程，达到更好的业务需求。

