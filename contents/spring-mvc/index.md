## Training Outcome

- Know what Spring Framework is
- Able to mention more than one Spring Modules
- Able to create Hello World Webapp in Spring
- Know what MVC pattern is
- Able to implement Web Controller in Spring
    + using Spring Boot, annotation over xml config
    + @Controller, @RequestMapping, @RequestParam, @RequestBody
- Able to write simple JSP view
- Able to write Unit Test for Spring application
    + standalone setup without injecting web context

---

### Training Requirements

- Java SDK 1.8
- Apache Maven 3.x
- spring-boot-starter-web 1.5.2.RELEASE
- spring-boot-starter-test 1.5.2 RELEASE
- junit 4.12
- spring-boot-starter-tomcat 1.5.2.RELEASE
- tomcat-embed-jasper 8.5.6
- jstl 1.2

---

## Spring Framework Overview

What is Spring Framework?

---

### Spring Framework? on docs.spring.io

The Spring Framework is a **Java platform** that provides <em>**comprehensive infrastructure**</em> support for developing Java applications. Spring handles the infrastructure so you can <em>focus on your application</em>.

Spring enables you to build applications from "plain old Java objects" (POJOs) and to apply enterprise services non-invasively to POJOs. This capability applies to the Java SE programming model and to full and partial Java EE.

---

### Benefits of Using the Spring Framework

- Develop enterprise applications using POJOs.
- Modular application, which promote re-usable.
- Wiring existing technologies, e.g.: Hibernate ORM, Logging library, Apache Struts.
- Lightweight IoC containers.
- Testing module easier, let the framework inject dependencies.
- Support web MVC framework and REST.
- Provide scalable transaction management

---

### Spring Modules

![Spring Modules](images/spring-modules.png)

---

### Spring Modules

- Core Container: BeanFactory and Application Context
    + Dependency Injection: aka Inversion of Control
- AOP and Instrumentation: Alliance-compliant appliesect-oriented programming implementation
- Messaging: integration with messaging service
- Data Access: Object-Relational mapping and Repository
- Web: web integration, servlet, REST web service 

---

## Development Environment Setup

- Maven pom.xml
- Spring Initializr
- Spring Tool Suite

---

### Spring Dependencies in Maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>...</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <version>...</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

### Spring Plugin in Maven

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        <plugin>
    </plugins>
</build>
```

---

### Compile and Running the Maven Project

- Compile: `maven compile`
- Run: `maven spring-boot:run`
    + call spring maven plugin and start Spring App / Spring Container

---

### Gradle Setup

```
buildscript {
    ...
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin")
    }
}

apply plugin: 'java'
apply plugin: 'org.springframework.boot'

...
dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile("org.springframework.boot:spring-boot-starter-test")
}
```

---

### Spring Initializr and STS

- Spring Initializr:
    + please go to **http://start.spring.io**
    + fill in the **group**, **artifact** and **dependencies**
    + then click **generate project**
- alternative link: http://start.spring.io/sts
- Spring Tool Suite is downloadable program please visit
    + http://spring.io/tools/sts

---

## Hello World in Spring

```java
@SpringBootApplication
public class App {
    // ... beans

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Validate it with calling spring maven plugin.

![Spring Boot Run](images/spring-boot-run.png)

---

### Hello World in Spring (contd.)

- `SpringApplication.run` will launch our application
- `@SpringBootApplication` is a convenience annotation that adds:
    + `@Configuration` tags the class as configuration class
    + `@EnableAutoConfiguration` tell Spring Boot to add beans automatically
    + `@EnableWebMvc` flags the application as a webapp
    + `@ComponentScan` look for other components, configurations and services

---

### Create Bean

- Bean that retrieve all created beans

```java
public class App {
    // ...
    @Bean
    public CommandLineRunner printAll(ApplicationContext ctx) {
        return args -> {
            System.out.println("All beans provided by Spring Boot:")
            String[] beanNames = ctx.getBeanDefinitionNames();
            for (String beanName : beanNames)
                System.out.println(beanName);
        };
    }
}
```

- Call the spring maven plugin again, and we should see `app` as bean name.

---

### Very Short Intro Bean

A Spring IoC container manages one or more beans. These beans are created with the configuration metadata that you supply to the container.

---

### Java POJO as Component

```java
@Component
public class HelloWorld {

    private String message;

    public void setMessage(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }

    public void printMessage() {
        System.out.println("Your message: " + message);
    }
}
```

---

### HelloWorld Injection

```java
public class App {
    // ...
    @Bean
    CommandLineRunner printMessage(HelloWorld component) {
        return args -> {
            component.setMessage("hello component");
            component.printMessage();
        }
    }
}
```

- Now we see "hello component", after calling spring maven plugin.

---

## Why Spring Boot?

- Some answer from Mitrais fellows:
    + Spring Boot is now, and vanilla Spring was now.
    + Spring Boot is just Spring with extra magic.
    + Spring Boot is automatic Spring.
- Benefits of Spring Boot:
    + Spring Boot provides the necessary configuration, just focus on the business logic.
    + Spring Boot is flexible too, just override or extend Spring Boot provided configuration to match the requirements.
    + Single point of dependency, e.g.: spring-boot-starter-web, and it will implicitly selecting dozens required dependencies.

---

## Hello World Webapp in Spring

- Create our first Controller.

```java
@Controller
public class HelloController() {

    @RequestMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Greetings from Spring Boot!";
    }
}
```

- Re-run our Spring Application, and open http://localhost:8080/hello

---

## Model-View-Controller Pattern

![MVC Pattern](images/wiki-mvc.svg)

---

### Request Workflow on Spring MVC

![Spring MVC Request Workflow](images/spring-request-flow.png) 

---

### Request Workflow (contd.)

- Front Controller/ Dispatcher Servlet receive HTTP request.
- Dispatcher consult handler mapping to call appropriate controller.
- Dispatcher call the controller, then controller return the model.
- Dispatch resolve appropriate view to render response.
- Finally dispatch render the view on the browser.

---

## Controller in Spring

- Put `@Controller` or `@RestController` on a class
    + Those annotations flag the following class as controller component, that automatically injected into the container.
    + `@RestController` annotates the class with `@Controller` and `@ResponseBody`.
- `@ResponseBody` wraps the method's return value with HTTP response.

---

### Handler Mapping

- Put `@RequestMapping` to map controller/method with URI. 
- `@RequestMapping` has optional element:
    + `value` is URL relative path.
    + `method` the HTTP request method e.g.: GET, POST, etc.
- Other than `@RequestMapping`, Spring provides `@GetMapping`, `@PostMapping`.
- Consult http://docs.spring.io/spring/docs/current/javadoc-api for detail

---

### Request Parameters

- Method mapped to handle request has flexible signature.
    + `hello()`
    + `hello(String message)`
- To inject parameter from GET query string, use @RequestParam
    + URI path: `localhost/hello?message=Hello%20World`
    + Method signature: `hello(@RequestParam String message)`
    + Given aforementioned URI path, message value is `"Hello World"`

---

### Request Parameters (contd.)

- `@RequestParam` has optional element:
    + name: the request parameter to bind to
    + defaultvalue: when value missing, "Hello Spring" is injected.

```java
public String hello(
        @RequestParam(value = "message", defaultValue = "Hello Spring")
        String message
    ) {
        return "Your message: " + message;
}
```

---

### Request Body

- Other than `@RequestParam`, request body is injectable too with `@RequestBody`

```java
public String hello(@RequestBody String message) {

    return "Your message: " + message;
}
```

---

### URI Path Pattern

- Spring also can inject from URI path variable.

```java
@RequestMapping(value = "/{message}")
public String hello(@PathVariable String message) {
    // ...
    return "Your message: " + message;
}
```

- Regular expression on URI path is possible.
- Please consult Spring docs to know more, e.g.: `@RequestHeader`, `@RequestPart`

---

### Hands-on Labs

- Please create controllers that
    + mapping URI `host/hello?message=HELLO`, with GET request method
        * hint: @RequestMapping GET, @RequestParam
    + mapping URI `host/hello`, with POST request method and request body
        * hint: @RequestMapping POST, @RequestBody
    + mapping URI `host/hello/{message}`
        + hint: @RequestMapping PUT, @PathVariable

---

## Unit Testing

- By default, maven put JUnit as dependency.

```xml
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
```

- In test-driven development, starting with a broken test case then doing the coding that pass the test.
- Every class for testing purposes is in test folder.
- Run the test: `mvn test`.
- For this training, please use JUnit 4.12.

---

### Unit Testing (contd.)

- Example for JUnit testing.

```java
public class AppTest {

    @Test
    public void itShouldRun() {
        // ... test then run matcher or make expectation
        bool variable = getValue();
        assertTrue(variable);
    }
}
```

---

### Testing Controller in Spring

- Use MockMvc to mock our controller.

```java
@Test
public class WebAppTest {

    private MockMvc mockMvc;

    @Before
    public void setup() {
        this.mockMvc = MockMvcBuilders
            .standaloneSetup(new HelloController())
            .build();
    }
}
```

---

### Testing Controller in Spring (contd.)

```java
@Test
public void getMessage() throws Exception {
    this.mockMvc.perform(get("/hello"))
        .andExpect(status().isOk());
}
```

- `.perform(...)` mock request to dispatcher servlet, and `get(...)` mock http GET request method.
- `.andExpect(...)` make expectation.

---

### Hands-on Labs

- Use TDD, please create controllers that:
    + mapping URI `'host/employees?gender=male'`
        * give response with all existing employees whose gender is male
    + mapping URI `'host/employees'`, with request method POST and request body
        * give response with created employee
- All employees are stored into java collection (List, etc).

---

### Testing with Web Context

```java
@RunWith(SpringRunner.class)
@WebConfiguration
@ContextConfiguration("servlet-context.xml")
public class WebAppTest{

    @Autowired
    private WebApplicationContext wac;

    private MockMvc mockMvc;

    @Before
    public void setup() {
        this.mockMvc = MockMvcBuilders
            .webAppContextSetup(this.wac)
            .build();
    }
}
```

---

### Testing with Web Context (contd.)

- To inject web context into test method, use @WebAppConfiguration and @ContextConfiguration.
- When building mockMvc, use `.webappContextSetup(...)` instead `.standaloneSetup(...)`.

---

## JSP

- Spring provides JSP support by default, with small configuration.
- Add configuration in `src/main/resources/application.properties`.

```
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
```

- The JSP templates are located on `/WEB-INF/jsp/` and each file has `.jsp` as suffix.

---

### Setup for JSP

- Add dependencies and we're good to go.

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>1.5.2.RELEASE</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.tomcat.embed</groupId>
      <artifactId>tomcat-embed-jasper</artifactId>
      <version>8.5.6</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
```

---

### Create Hello JSP

```html
<%@ page contentType="text/html; charset=UTF-8" %>
<html>
    <head>
        <title>Hello Spring JSP</title>
    </head>
    <body>
        <h2>${message}</h2>
    </body>
</html>
```

- Write the snippet code above to `webapp/WEB-INF/jsp/hello.jsp`.
- Attribute of `${message}` is taken from `ModelMap`.

---

### Controller Return View and ModelMap

```java
public String hello(ModelMap modelMap) {

    modelMap.addAttribute("message", "Hello Spring from JSP");
    return "hello";
}
```

- Later on, spring will resolve the returned `"hello"` to `hello.jsp`.
- Re-run Spring application, and open our hello page.

---

## Handling Exception

- Spring handles all unexpected exceptions that occur during controller execution.
- The exception is associated with HTTP status code.

Exception | Status Code | Status Text
--- | ---: | :---
NoHandlerFoundExcepetion | 404 | Not Found
HttpMessageNotReadableException | 400 | Bad Request
HttpMessageNotWritableException | 500 | Internal Server Error

---

### Business Exception

- Controller can throw business exception and associate it with HTTP status code.
- `@ResponseStatus` associates exception with HTTP status code.
- Business exception must extend `RuntimeException`.

```java
@ResponseStatus(
    value = HttpStatus.BAD_REQUEST,
    reason = "make peace no war"
)
public class NoWarException extends RuntimeException {}
```

---

### Business Exception (contd.)

```java
public String hello(String message) {

    if (message.toLowerCase().indexOf("war") != -1)
        throw new NoWarException();
    // ...
    return "Your message: " + message;
}
```

- Validate it after visiting the hello page, with message containing "war".

---

### Testing Business Exception

- Given incorrect input, make expectation on HTTP status code.
    - Don't need to expect any exception thrown, because exception already associated with HTTP status code.
- Use `is4xxClientError()` or `isBadRequest()` for ResultMatcher.

```java
@Test
public void shouldReturnBadRequestGivenWar() {

    this.mockMvc.perform(get("hello?message=make%20war"))
        .andExpect(status().isOk())
        .andExpect(status().isBadRequest());
}
```

---

### Hands-on Lab

- Please write a controller that accept PUT request on `host/employees/{id}`.
    + given employee exists, it replaces the existing with request body.
    + given not exist, it give 404 response.
- Please use TDD approach.

---

### Reference

- http://spring.io/guides/gs/spring-boot/
- http://www.tutorialspoint.com/spring/spring_hello_world_example.htm
- http://docs.spring.io/spring/docs/current/javadoc-api/
- http://docs.spring.io/spring/docs/current/spring-framework-reference/
- http://junit.org/junit4/
- https://www.mitrais.com/en/spring-boot-not-new-spring
- MLS Java EE, Spring Introduction