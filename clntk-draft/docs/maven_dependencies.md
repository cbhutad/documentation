---
sidebar_label: "maven_dependencies"
sidebar_position: 2
---

# Maven Dependecies

### spring-boot-starter-actuator

In essence, Actuator brings production-ready features to our application.

_Monitoring our app, gathering metrics, and understanding traffic or the state of our database_ becomes trivial with this dependency.

The main benefit of this library is that we can get production-grade tools without actually having to implement these features ourselves. The actuator mainly exposes operational information about the running application — health, metrics, info, dump, env, etc. It uses HTTP endpoints or JMX beans to enable us to interact with it. Once this dependency is on the classpath, several endpoints are available for us out of the box. As with most Spring modules, we can easily configure or extend it in many ways.

All Actuator endpoints are now placed under the `/actuator` path by default. This also serves as a discovery endpoint such making a get request to this endpoint will return actuator links for various endpoints mentioned below.

List of endpoints are as follows:
- /auditevents lists security audit-related events such as user login/logout. Also, we can filter by principal or type among other fields.
- /beans returns all available beans in our BeanFactory. Unlike /auditevents, it doesn’t support filtering.
- /conditions, formerly known as /autoconfig, builds a report of conditions around autoconfiguration.
- /configprops allows us to fetch all @ConfigurationProperties beans.
- /env returns the current environment properties. Additionally, we can retrieve single properties.
- /flyway provides details about our Flyway database migrations.
- /health summarizes the health status of our application.
- /heapdump builds and returns a heap dump from the JVM used by our application.
- /info returns general information. It might be custom data, build information or details about the latest commit.
- /liquibase behaves like /flyway but for Liquibase.
- /logfile returns ordinary application logs.
- /loggers enables us to query and modify the logging level of our application.
- /metrics details metrics of our application. This might include generic metrics as well as custom ones.
- /prometheus returns metrics like the previous one, but formatted to work with a Prometheus server.
- /scheduledtasks provides details about every scheduled task within our application.
- /sessions lists HTTP sessions, given we are using Spring Session.
- /shutdown performs a graceful shutdown of the application.
- /threaddump dumps the thread information of the underlying JVM.

References :
[baldeung article](https://www.baeldung.com/spring-boot-actuators) 

### spring-boot-starter-web

There are two important features of spring-boot-starter-web:

- It is compatible for web development
- Auto configuration

Starter of Spring web uses Spring MVC, REST and Tomcat as a default embedded server. The single spring-boot-starter-web dependency transitively pulls in all dependencies related to web development.

The spring-boot-starter-web auto-configures the following things that are required for the web development:

- Dispatcher Servlet
- Error Page
- Web JARs for managing the static dependencies
- Embedded servlet container

Each Spring Boot application includes an embedded server. Embedded server is embedded as a part of deployable application. The advantage of embedded server is, we do not require pre-installed server in the environment. With Spring Boot, default embedded server is **Tomcat**. Spring Boot also supports another two embedded servers:

- Jetty Server
- Undertow Server

References :
[javatpoint article](https://www.javatpoint.com/spring-boot-starter-web)

### spring-boot-starter-logging

This particular jar is `transistive dependency` which is included when we add spring-boot-starter-web as part of dependencies. This allows log functionality with zero configurations as it comes with opionated defaults. 

When using starters, Logback is used for logging by default and the default logging level of the Logger is preset to INFO, meaning that TRACE and DEBUG messages are not visible.

In order to activate a different level without changing the configuration, we can pass the –debug or –trace arguments on the command line:

``` bash
java -jar target/spring-boot-logging-0.0.1-SNAPSHOT.jar --trace
```

Although there is opionated default configuration provided we can modify the same and set our own configs for logback as well by providing one of the following files in classpath:

1) logback-spring.xml
2) logback.xml
3) logback-spring.groovy
4) logback.groovy

Here is one sample logback-spring.xml file

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <property name="LOGS" value="./logs" />

    <appender name="Console"
        class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <Pattern>
                %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %yellow(%C{1.}): %msg%n%throwable
            </Pattern>
        </layout>
    </appender>

    <appender name="RollingFile"
        class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${LOGS}/spring-boot-logger.log</file>
        <encoder
            class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
            <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
        </encoder>

        <rollingPolicy
            class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- rollover daily and when the file reaches 10 MegaBytes -->
            <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd}.%i.log
            </fileNamePattern>
            <timeBasedFileNamingAndTriggeringPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
                <maxFileSize>10MB</maxFileSize>
            </timeBasedFileNamingAndTriggeringPolicy>
        </rollingPolicy>
    </appender>
    
    <!-- LOG everything at INFO level -->
    <root level="info">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </root>

    <!-- LOG "com.baeldung*" at TRACE level -->
    <logger name="com.baeldung" level="trace" additivity="false">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </logger>

</configuration>

```

References :
[baldeung article](https://www.baeldung.com/spring-boot-logging)

### log4j-slf4j-impl

Simple Logging Facade for Java (abbreviated SLF4J) acts as a facade for different logging frameworks (e.g., java.util.logging, logback, Log4j). It offers a generic API, making the logging independent of the actual implementation.

This allows for different logging frameworks to coexist. And it helps migrate from one framework to another. Finally, apart from standardized API, it also offers some “syntactic sugar.” SLF4J is an API designed to give generic access to many logging frameworks like Logback, Apache commons logging, java.util.logging; Log4j being one of them. When using SLF4j, which logging implementation will be used is decided at deployment time, not when we write the application code. SLF4j acts like a facade or abstraction where other logging libraries can be seen as implementations. We can replace the logging framework without changing the source code.

The Log4j2 SLF4J Binding allows applications coded to the SLF4J API to use Log4j2 as the implementation. 

### spring-boot-starter-tomcat

Spring-boot-starter-tomcat is a dependency that provides an embedded Tomcat server for Spring Boot applications. It is included in the spring-boot-starter-web starter package, which is used for developing web applications. The spring-boot-starter-tomcat package provides a pre-configured Tomcat server with sensible defaults . It is possible to modify the default configuration to meet custom requirements by modifying the `application.properties` file.

References :
[baldeung article](https://www.baeldung.com/spring-boot-configure-tomcat)

### spring-boot-starter-thymeleaf

Thymeleaf is a server-side Java-based template engine for both web and standalone environments, capable of processing HTML, XML, JavaScript, CSS and even plain text. It is more powerful than JPS and responsible for dynamic content rendering on UI. The engine allows a parallel work of the backend and frontend developers on the same view. It can directly access the java object and spring beans and bind them with UI. And it is mostly used with spring MVC when we create any web application.

References :
[baldeung article](https://www.baeldung.com/thymeleaf-in-spring-mvc)