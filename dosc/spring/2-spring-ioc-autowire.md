# SpringlOC之依赖自动注入六层筛选源码剖析
Spring 中的依赖自动注入是在 Bean 的创建和组装过程中进行的，主要通过实现 AutowiredAnnotationBeanPostProcessor、AutowiredAnnotationBeanPostProcessor2、CommonAnnotationBeanPostProcessor、PersistenceAnnotationBeanPostProcessor、RequiredAnnotationBeanPostProcessor、及 QualifierAnnotationAutowireCandidateResolver 这几个类来实现的。
这六层筛选主要是指在自动注入时，Spring 会根据一定的规则进行筛选符合注入要求的 Bean，筛选过程如下：
1. AutowiredAnnotationBeanPostProcessor
> 在 Bean 初始化前的 postProcessProperties() 方法中进行属性注入操作，扫描容器中的所有 Bean，查找出与该属性类型相符且被注入的属性带有 Autowired 标记的 Bean，进行属性注入。如果有多个符合条件的 Bean，会抛出 NoUniqueBeanDefinitionException 异常。
2. AutowiredAnnotationBeanPostProcessor2
> 增加了对 Optional 类型的支持，只要找到唯一的兼容 Bean，就会直接注入，不会抛出 NoUniqueBeanDefinitionException 异常。如果没有找到兼容的 Bean，则注入 Optional.empty。
3. CommonAnnotationBeanPostProcessor
> 在 Bean 初始化后的 postProcessProperties() 方法中进行属性注入，与 AutowiredAnnotationBeanPostProcessor 类似，但是使用的是 javax.inject 包下的注解来识别需要注入的属性。
4. PersistenceAnnotationBeanPostProcessor
> 在 Bean 初始化后的 postProcessAfterInitialization() 方法中进行属性注入，与 AutowiredAnnotationBeanPostProcessor 类似，但是使用的是 JPA 规范下的 @PersistenceContext 和 @PersistenceUnit 注解来识别需要注入的属性。
5. RequiredAnnotationBeanPostProcessor
> 在 Bean 初始化后的 postProcessAfterInitialization() 方法中进行属性注入，用于检查指定 Bean 中的全部 required 属性是否都已经被正确填充。
6. QualifierAnnotationAutowireCandidateResolver
> 在进行 Bean 注入时，当容器中存在多个类型相同的 Bean 时，会进行进一步的筛选，根据 @Qualifier 注解（或 @Value 注解的 value 值）来找到需要注入的 Bean，如果没有找到，则抛出 NoUniqueBeanDefinitionException 异常。  
这六层筛选组成了 Spring 进行依赖自动注入时的核心流程，通过掌握这个流程可以更好地理解依赖注入的实现原理。

