# Chapter 1-1: Foundational Spring

Book: Spring in Action

## What is Spring (a bean container / manager)

- Application is composed of many components, each responsible for its own piece of the overall application functionality, coordinating with the other application elements to get the job done. When the application is run, those components need to be created and introduced to each other.
- Spring offers a container (Spring application context) that creates and manages application components.
- Those components (beans) are wired together inside the Spring application context to make a complete application, much like bricks, mortar, timber, plumbing and wiring are bound together to make a house.
- The act of wiring beans together is based on a pattern Dependency Injection.
- Why using DI: Rather than have components create and maintain the lifecycle of other beans that they depend on, a dependency-injected application relies on the container to create and maintain all components and inject those into the beans that need them. This is done typically through constructor arguments or property accessor methods (similar to how Angular use DI).
- Similar to the tool library, if you want to build a house, instead of buying your own tools for each job, you borrow it from the tools library, sharing the tools with other house builders. Another benefit is that, if you want a different hammer for the job, you can swap the tool easily comparing to buying your own tool.

## How beans are connected?

- Historically, you would guide Spring’s application context to wire beans together through XML files that described the components and their relationship to other components, such as

```
<bean id=“inventoryService” class=“com.example.InventoryService”></bean>
<bean id=“productService” class=“com.example.ProductService”>
	<constructor-arg ref=“inventoryService”>
</bean>
```

In recent versions of Spring, a java-based configuration is more common. The following java-based configuration class is equivalent to the XML configuration:

```java
@Configuration
// the @Configuration annotation indicates to Spring that this is a configuration class that will provide beans to the Spring application context, an equivalent to a XML file
public class ServiceConfiguration {
	@Bean // indicate that the objects they return should be added as beans in the application context
	public InventoryService inventoryService() { return new InventoryService(); }

	// by default, the bean IDs will be the same as the names of the methods that define them, in this case, the bean ID is productService
	public ProductService productService() { return new ProductService(); }
}
```

- Question: so bean in Java is an equivalent to an @Injectable service in Angular???
- Explicit bean configuration is only necessary if Spring is unable to automatically configure the beans.
- With Spring, beans can be configured automatically, known as autowiring and component scanning.
- Component scanning: Spring can automatically discover components from an application’s class path and create them as beans in the Spring application context.
- Autowiring: Spring automatically injects the components with the other beans that they depend on.
- Autoconfiguration: Spring Boot can make reasonable guesses of what components need to be configured and wired together, based on entries in the class path, environment variables, and other factors.

## Bootstrapping the application

- A minimal amount of Spring configuration is added to the bootstrap class
- @SpringBootApplication: this annotation signifies that this is a Spring application, and it’s a combination of 3 other annotations, including:
- @SpringBootConfiguration: designates this is a configuration class, a specialized form of @Configuration.
- @EnableAutoConfiguration: enables Spring Boot automatic configuration, tells Spring Boot to automatically configure any components that it thinks you will need
- @ComponentScan: lets you declare other classes with annotations like @Component, @Controller, @Service, and others. To have Spring automatically discover them and register them as component in the Spring Application context.
- The main() method: it will run when the JAR file is executed. It calls a static run() method on the SpringApplication class, which performs the actual bootstrapping of the application, creating the Spring application context.

## Testing the application:

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ExampleApplicationTest {

@Test
public void contextLoads() {}
}
```

- @RunWith: it’s a JUnit annotation, providing a test runner that guides JUit in running a test. Think of it as applying a plugin to Unit to provide custom testing behaviour. In this case, JUnit is given SpringRunner, a Spring-provided test runner that provides for the creation of a Spring application context that the test will run against. (This is similar to adding a testbed in Angular)
- Note: SpringRunner is an alias for SpringJUnit4ClassRunner, it was introduced to remove the association with a specific version of JUnit.
- @SpringBootTest tells Junit to bootstrap the test with Spring Boot capabilities. It’s equivalent of calling SpringApplication.run() in a main() method.
