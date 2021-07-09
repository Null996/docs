## @PostConstruct、InitializingBean、initMethod以及@PreDestroy、DisposableBean、destoryMethod的异同和执行顺序。
```java
public class BeanInit implements InitializingBean, DisposableBean {
​
    public BeanInit() {
        System.out.println("BeanInit construct");
    }
​
​
​
    @PostConstruct
    public void postCon(){
        System.out.println("PostConstruct");
    }
​
    @PreDestroy
    public void preDestroy(){
        System.out.println("preDestroy");
    }
​
    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("InitializingBean afterPropertiesSet");
    }
​
    @Override
    public void destroy() throws Exception {
        System.out.println("DisposableBean destroy method");
    }
​
    public void initMethod(){
        System.out.println("initMethod");
    }
​
    public void destroyMethod(){
        System.out.println("destroyMethod");
    }
​
​
}

```

```java
  @Configuration
  @ComponentScan("com.demo.bean.lifecycle")
  public class AppConfig {
  ​
  ​
      @Bean(initMethod = "initMethod",destroyMethod = "destroyMethod")
      public BeanInit beanInit(){
          return new BeanInit();
      }
  }
```
```java
  public class Test {
    
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        BeanInit init = context.getBean(BeanInit.class);
        context.registerShutdownHook();
    }
    
}

```

### 控制台输出如下：
BeanInit construct
PostConstruct
InitializingBean afterPropertiesSet
initMethod
preDestroy
DisposableBean destroy method
destroyMethod

可以看到，当在IOC容器实例化一个bean时，依次执行构造方法 construct——>postConstruct注解——>InitializingBean接口中的afterPropertiesSet方法——>配置中指定的initMethod，销毁bean时依次执行preDestroy注解——>DisposableBean接口中的destroy方法——>以及配置中指定的destroy方法。


