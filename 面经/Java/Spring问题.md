##### springboot启动流程

​	![alt](/Users/limin/code/mygit/MY-FAQ/image/springboot-start.webp)

1. 第一步 初始化 SpringApplication
   			1. 初始化 spring.factory 应用上下文实例(ApplicationContextInitializer)
   			1. 初始化spring.factory 监听(ApplicationListener)实例

2.  第二步 调用run方法
   1. 初始换环境，包含配置，启动参数等
   2. 发布事件
   3. 创建容器上下文(ConfigurableApplicationContext)
   4. 刷新容器(创建bean过程)

##### springboot原理

1. 本质就是把spring包了一层，其中最终要的三个注解@SpringBootConfiguration ，@EnableAutoConfiguration，@ComponentScan 三个注解，实现了对spring的自动注册依赖的bean信息，bean实例化，以及自动装配。

1. @SpringBootConfiguration 可以简单的理解为spring中的 @Configuration,其实是AnnotationConfigApplicationContext，

   AnnotationConfigApplicationContext 主要作用是加载spring 配置信息和注册几个关键的BeanFactoryPostProcessor,bean定义，bean实例化。

   AutowiredAnnotationBeanPostProcessor（主要处理@Autowired注解，自动装配），CommonAnnotationBeanPostProcessor(@Resource，自动装配)

2. @EnableAutoConfiguration开启自动装配

3. ComponentScan扫描的包路径

##### spring启动流程

以AnnotationConfigApplicationContext为例：

1. ```java
   	public AnnotationConfigApplicationContext(Class<?>... componentClasses) {
       //注册bean定义，AutowiredAnnotationBeanPostProcessor(主要处理@Autowired注解，自动装配),
       // CommonAnnotationBeanPostProcessor(@Resource，自动装配)
   		this();
       //注册启动类
   		register(componentClasses);
       //刷新容器，实例bean
   		refresh();
   	}
   ```

2. 

##### bean生命周期

![alt](/Users/limin/code/mygit/MY-FAQ/image/springbean-life-2.jpg)

##### springmvc流程

![alt](/Users/limin/code/mygit/MY-FAQ/image/mvc.jpg)

##### @Autowired原理

​	实现 `@Autowired` 的原理主要依赖于 `AutowiredAnnotationBeanPostProcessor` 后置处理器。当 Spring 容器启动时，`AutowiredAnnotationBeanPostProcessor` 会扫描容器中所有的 Bean，检查它们是否标记了 `@Autowired` 注解。如果发现了带有 `@Autowired` 注解的字段、构造函数或方法参数，就会尝试为这些属性自动注入合适的 Bean。

##### springAop原理

##### spring事务传播

1. **REQUIRED**：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。这是默认的传播行为。
2. **REQUIRES_NEW**：始终创建一个新的事务，如果当前存在事务，则将当前事务挂起。
3. **NESTED**：如果当前存在事务，则在嵌套事务中执行；如果当前没有事务，则创建一个新的事务。嵌套事务是当前事务的子事务，它有自己的保存点，并且可以独立提交或回滚。如果外部事务回滚，嵌套事务也会回滚，但是嵌套事务的回滚不会影响外部事务。
4. **SUPPORTS**：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式执行。
5. **NOT_SUPPORTED**：以非事务的方式执行操作，如果当前存在事务，则将其挂起。
6. **NEVER**：以非事务的方式执行操作，如果当前存在事务，则抛出异常。
7. **MANDATORY**：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。

**事务隔离级别**：事务隔离级别定义了事务之间的可见性和影响范围。常用的隔离级别包括：

- `READ_UNCOMMITTED`：最低的隔离级别，允许读取未提交的数据，可能会导致脏读。
- `READ_COMMITTED`：确保一个事务不会读取到另一个事务未提交的数据，避免脏读，但是可能会出现不可重复读。
- `REPEATABLE_READ`：（mysql默认级别）确保一个事务不会读取到另一个事务已提交但未结束的数据，避免不可重复读，但是可能会出现幻读。
- `SERIALIZABLE`：最高的隔离级别，确保事务之间的数据读写互不干扰，但是性能较低。

##### 什么情况下事务失效

##### spring一个事务中调用另外一个事务，另一个事务发生异常会怎么样

1. 如何是this.方法()这种情况，无任何影响，因为spring事务是的代理模式，这种第二个都不生效，就没有影响
2. 调用另外一个bean的事务，则需要看spring的事务传播机制了。







