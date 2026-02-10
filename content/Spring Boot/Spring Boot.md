# 注解
## @Component
>泛型注解 用以标记一个类为"Spring 组件"
- 目的: 将此类实例化并管理生命周期 作为一个Bean注册到IoC容器中
- 机制: 依赖于组件扫描 需要配置`@ComponentScan`(通常在@Configuration上) 指定Spring应该扫描哪些包路径
- 本质:与@Service @Repository @Controller一致 声明一个自己编写的 需要Spring管理的辅助类
### @Controller
>标记传统的Spring MVC控制层
-  返回HTML
### @RestController
> 标记RESTful Web服务的控制器 是@Controller 和 @ResponseBody的组合注解
- 返回JSON/XML数据
### @Service
>提供业务操作
### @Repository
> 标记访问数据库
### @Mapper
>MyBatis的映射器接口 并非Spring原生注解

## @Bean
>方法级注解
- 目的: 将该方法返回的对象之策为IoC容器中的Bean
- 机制: 需要在配置类(被@Configuration标记的类)内部  并且控制Bean实例的细节
- 本质: 
	- 注册第三方编写的类 注册到IoC容器中
	- 配置复杂需求的Bean
	- 显示定义Bean
### @Configuration
> 类级注解
- 目的: 标记一个配置类 用于定义和组装Bean
- 机制: 
	- 在Spring启动后 该配置类被注册为Bean 并执行其中的`@Bean`方法 将返回对象注册为Bean
	- 当Bean方法中调用了其他Bean方法时 Spring会确保容器中的单例Bean(CGLIB代理) (与**@Component区别**)
	- 主配置类 集中配置各种Bean