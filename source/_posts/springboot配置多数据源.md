---
title: springboot配置多数据源
date: 2018-12-25 11:58:48
tags: springboot,spring
categories: springboot
---
springboot配置多数据源
```(java)
/**
 * @ClassName TargetDataSource
 * @Description TODO
 * @Author missj
 * @Date 2018/12/6 11:54
 * @Version 1.0
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface TargetDataSource {
    String name() default "";
}
```

<!-- more -->

```
/**
 * @ClassName DataSourceNames
 * @Description TODO
 * @Author missj
 * @Date 2018/12/6 11:57
 * @Version 1.0
 */
public interface DataSourceNames {
    String FIRST = "first";
    String SECOND = "second";
}
```

```
public class DynamicDataSource extends AbstractRoutingDataSource {
    private static final ThreadLocal<String> contextHolder = new ThreadLocal<>();

    public DynamicDataSource(DataSource defaultDatasource, Map<Object, Object> targetDataSources) {
        super.setDefaultTargetDataSource(defaultDatasource);
        super.setTargetDataSources(targetDataSources);
        super.afterPropertiesSet();
    }

    public static String getDatasource(){
        return contextHolder.get();
    }

    @Override
    protected Object determineCurrentLookupKey() {
        return getDatasource();
    }

    public static void setDatasource(String datasource){
        contextHolder.set(datasource);
    }

    public static void clearDatasource(){
        contextHolder.remove();
    }
}
```

```
@Configuration
public class MultiDataSourceConfig {
    @Bean
    @ConfigurationProperties("spring.datasource.druid.one")
    public DataSource firstDataSource(){
        return DruidDataSourceBuilder.create().build();
    }

    @Bean
    @ConfigurationProperties("spring.datasource.druid.two")
    public DataSource secondDataSource(){
        return DruidDataSourceBuilder.create().build();
    }

    @Bean
    @Primary
    public DynamicDataSource dataSource(DataSource firstDataSource, DataSource secondDataSource){
        Map<Object, Object> targetDataSources = new HashMap<>();
        targetDataSources.put(DataSourceNames.FIRST, firstDataSource);
        targetDataSources.put(DataSourceNames.SECOND, secondDataSource);
        return new DynamicDataSource(firstDataSource, targetDataSources);
    }
}
```

```
@Aspect
@Component
public class DatasourceAspect implements Ordered {
    @Pointcut("@annotation(com.missj.config.datasource.annotation.TargetDataSource)")
    public void dataSourcePointCut() {

    }

    @Around("dataSourcePointCut()")
    public Object around(ProceedingJoinPoint point) throws Throwable{
        MethodSignature signature = (MethodSignature)point.getSignature();
        Method method = signature.getMethod();
        TargetDataSource ds = method.getAnnotation(TargetDataSource.class);
        if (ds == null){
            DynamicDataSource.setDatasource(DataSourceNames.FIRST);
        } else {
            DynamicDataSource.setDatasource(ds.name());
        }
        try {
            return point.proceed();
        }finally {
            DynamicDataSource.clearDatasource();
        }
    }

    @Override
    public int getOrder() {
        return 1;
    }
}
```


