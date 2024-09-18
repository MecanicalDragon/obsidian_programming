## Spring Context Initialization

**I. Configuration parsing and BeanDefinition creation.**<br>
Spring has 3 types of context:
1. *ClassPathXmlApplicationContext* (context.xml).
   1. *XmlBeanDefinitionReader* reads input stream from the xml file and creates BDs.
2. *AnnotationConfigApplicationContext* (package scanning and `@Configuration` classes).
   1. *AnotatedBeanDefinitionReader* registers all `@Configuration` classes, then *BeanDefinitionRegistryPostProcessor* parses java config and creates BDs.
   2. *ClassPathBeanDefinitionScanner* scans specified packages of the `@Component` annotated classes and creates BDs.
3. *GenericGroovyApplicationContext* (context.groovy).
   1. *GroovyBeanDefinitionReader* creates BDs from the groovy script.

`@Configuration` classes are special types of beans that are created in the first place. Their `@Bean` annotated methods are proxied and invoked only once for singleton beans.

*BeanDefinition* is an interface that contains metadata of future beans. All BDs after the parsing are collected in a special map.

**II. BeanFactoryPostProcessors creation and tuning.**<br>
BFPPs are another special type of beans, they’re created on this step before all other beans creation. BFPPs tune BDs before bean creation, so on this step we can adjust data for bean creation, even replace bean class for future bean. *PropertySourcesPlaceholderConfigurer* injects property values into beans during this step.

**III. Custom FactoryBeans creation.**<br>
*FactoryBean* is an interface that is useful for XML configuration; it can modify beans during creation. It is useless for annotation-based and java config. Btw, XML-based configuration has an advantage over JavaConfig: it can be updated with just a substitution of the XML config file, without recompilation of the code.

**IV. Bean creation.**<br>
*BeanFactory* creates beans using BDs. It also creates *BeanPostProcessors* which are another special type of beans.

**V. BeanPostProcessor tuning.**<br>
BPPs tune created beans in 3 steps:
1. *beforeInit* method is used to modify the bean itself.
2. bean initialization. `@PostConstuct` methods are invoked.
3. *afterInit* method is used to modify the bean’s behavior. All proxies are created during this step.

**Context is ready.**<br>
After all the steps above passed, the *ContextRefreshed* event is emitted, and the application is ready to work. Context-related application events are: started, stopped, refreshed, and closed.
## Spring Bean Scopes

**Singleton** Default. The application creates a single bean instance at the start of the IoC container and uses it everywhere.

**Prototype** (`@Scope(“prototype”)`). The container creates and returns an instance of the bean every time it is requested. *destroy* method can’t be invoked automatically for this type of bean.

The following scopes are only available in a web-aware application context:

**Request** (`@RequestScope`). Bean exists only during an HTTP request. 

**Session** (`@SessionScope`). Bean exists for a distinct HTTP session. HttpSessions are stored in this kind of bean.

**Application** (`@ApplicationScope`). Bean exists throughout a lifecycle of a *ServletContext*. It is different than singleton because the bean instance is shared across multiple servlet-based apps, running in the same *ServletContext*, while singleton exists for a particular app.

**WebSocket** Bean exists for a particular WS session. It is stored in WS session attributes when it is accessed the first time. After that, the same instance of the bean is returned whenever the bean is requested during the entire WS session.
## Spring Hints

If you declare a *BeanFactoryPostProcessor* in JavaConfig, it must be static, otherwise it cannot be loaded before `@Configuration` class itself and doesn’t take effect.

How to inject a prototype bean into a singleton bean? Declare a `@Bean Supplier<Prototype>` and inject it instead of the prototype itself. Furthermore, you can use *Function* or *BiFunction* if the prototype has dependencies.

`@Repository` over the *CrudRepository* classes is redundant because generated repositories have this annotation already.

Since version 2 SpringBoot uses CGLIB to create proxies, that’s why we can operate with proxied beans without interface declaration but must not make beans final. To force SpringBoot to use proxies over the interfaces again property `spring.aop.proxy-target-class` must be set to false.

*BeanDefinitionRegistrar* is an interface that affords to register *BeanDefinitions*. It can be retrieved from context and allows adding beans in context even after the application start. Furthermore, you can `@Import` *BeanDefinitionRegistrar* implementation in `@Configuration` class and use it to describe bean definitions for third-party library classes.

`@Autowired` annotation is allowed above 3 places: constructor, field, and setter. If you place `@Autowired` over the arbitrary method, it will be invoked during the bean initialization phase. Therefore, we can place it over the default interface method, and it will be invoked for all bean implementations.

`@ControllerAdvice(annotations = “CustomAnnotation”)` placed over that class implementing `ResponseBodyAdvice` and overriding `beforeBodyWrite` allows to modify ResponseBody for all classes annotated with `@CustomAnnotation`.

Spring's `spring.lifecycle.timeout-per-shutdown-phase` property determines the timeout for the shutdown of any phase (group of *SmartLifecycle* beans with the same *phase* value), but not the whole shutdown period. The default phase value is 0, webserver beans have 2 phases: `int max` and `int max - 1`. Phase can be determined with interface *Phased*. All singletons are considered as LifeCycle beans; SmartLifeCycle beans implement interface *Phased*. Beans are destroyed in reverse order of their phases and in reverse dependency order (dependents first). Non-obvious dependencies can be specified with `@DependsOn` annotation.
