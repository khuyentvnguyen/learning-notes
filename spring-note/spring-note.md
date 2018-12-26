# Spring Note

## Spring

### Basic Concepts

#### Dependency Injection (DI)

Three ways:

- By constructor - **most preferred**
  - If a bean only has one constructor, you can omit the `@Autowired` on the top the constructor.
  - Mark the private property as `final` indicating that it cannot be subsequently changed.
- By setters - area of much debate, may have NullPointerException
- By class properties - least preferred

DI via interfaces rather than concrete classes is highly preferred.

- Allows runtime to decide implementation to inject.
- Follows Interface Segregation Principle of SOLID.
- Makes your code more testable.

---

#### Inversion of Control (IoC)

- Allows dependencies to be injected at runtime.
- Dependencies are not predetermined.

---

### Spring Bean Life Cycle

![spring-bean-life-cycle.png](img/spring-bean-life-cycle.png)

![spring-bean-life-cycle-annotations.png](img/spring-bean-life-cycle-annotations.png)

![callback-interfaces.png](img/callback-interfaces.png)

---

### Spring Configuration

Options:

- XML based configuration
  - Introduced in Spring 2.0.
  - Common in legacy Spring Applications.
  - Still supported in Spring 5.
- Annotation based configuration
  - Introduced in Spring 3.0.
  - Component scans `@Component`, `@Controller`, `@Service`, `@Respository`.
- Java based configuration :white_check_mark:​
  - Introduced in Spring 3.0.
  - Uses Java classes to define Spring Beans.
  - Configuration classes are defined with `@Configuration`.
  - Beans are declared with `@Bean`.
- Groovy Bean definition DSL (Domain-Specific Language) configuration
  - Introduced in Spring 4.0.
  - Allows you to declare beans in Groovy.

Industry trend is using Java based configuration.

![spring-stereotypes.png](img/spring-stereotypes.png)

Q: What is special about the `@Respository` stereotype?

A: Spring will detect platform specific persistence exceptions and re-throw them as Spring exceptions.

---

### Spring Bean Scopes

- Singleton: (**default**) Only one instance of the bean is created in the IoC container.
- Prototype: A new instance is created each time the bean (object) is requested.
- Request: A single instance per http request. Only valid in the context of a web-aware Spring ApplicationContext.
- Session: A single instance per http session. Only valid in the context of a web-aware Spring ApplicationContext.
- Global-session: A single instance per global session. Typically only used in a Portlet context. (Not many people use this.) Only valid in the context of a web-aware Spring ApplicationContext.
- Application: Bean is scoped to the lifecycle of a ServletContext. Only valid in the context of a web-aware Spring ApplicationContext.
- WebSocket: Scopes a single bean definition to the lifecycle of a WebSocket. Only valid in the context of a web-aware Spring ApplicationContext.
- Custom Scope: You can define your own scope by implementing "Scope" interface. (Very rare)

Most commonly used: Singleton and Prototype scope.

---

## Spring MVC

![spring-mvc-architecture.png](img/spring-mvc-architecture.png)

---

## Spring Boot

### Run Applications

Three ways to run a Spring Boot application:

- Run in IDE.
- In terminal, `cd` into project folder. Use Maven to run.`mvn spring-boot:run`.
- In terminal, `cd` into project folder. Use Maven to compile.`mvn install`. Then cd into "target" folder. Use java to execute the .jar file. `java -jar <project_name>.jar`.

---

### application.properties File

Common properties

```properties
server.port=8080
server.servlet.context-path=/<project_name>

spring.datasource.url:jdbc:mysql://127.0.0.1:3306/<table_name>?useSSL=false
spring.datasource.username:<database_username>
spring.datasource.password:<database_password>
spring.datasource.driver:com.mysql.jdbc.Driver

# the way you would like the database to be initialized
spring.jpa.hibernate.ddl-auto=update

# whether or not show sql in console for testing
spring.jpa.hibernate.show-sql=true

spring.jpa.properties.hibernate.dialect:org.hibernate.dialect.MySQL5Dialect
```

How to access properties in properties file? - Use ${ }

```properties
property_name=property_value
property_name2=${property_name}
```

How to access properties in code? - Use annotation.

```java
@Value("${property_name}")
private String property_name;
```
**In real projects, there should be three properties files.**

- application-dev.properties: configure properties for development environment.
- application-prod.properties: configure properties for production environment.
- application.properties: determine which properties file will be used. And common configure properties for both dev and prod environment.
  - For development environment: `spring.profiles.active=dev`.
  - For production environment: `spring.profiles.active=prod`.

When you run the project in terminal, you can specify which one you will use. `java -jar <project_name>.jar --spring.profiles.active=prod`.

---

##  Practical Skills

When testing using **Postman**, if you are testing `PUT` method, you need to select "**x-www-form-urlencoded**" for RequestParam in Body instead of "**form-data**".
