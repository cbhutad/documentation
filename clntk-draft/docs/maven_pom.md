---
sidebar_label: "maven_pom"
sidebar_position: 2
---

# Maven_POM

This documentation contains the tags or elements of pom.xml which i did not know before

### Properties `<start-class>` element

A Spring Boot application’s main class is a class that contains a public static void main() method that starts up the Spring ApplicationContext. By default, if the main class isn’t explicitly specified, Spring will search for one in the classpath at compile time and fail to start if none or multiple of them are found.

Spring Boot expects the artifact’s Main-Class metadata property to be set to org.springframework.boot.loader.JarLauncher (or WarLauncher) which means that passing our main class directly to the java command line won’t start our Spring Boot application correctly.

In Maven we can provide this using the start-class element. Note that this property will only be evaluated if we also add the spring-boot-starter-parent as `<parent>` in our pom.xml.

Alternatively, the main class can be defined as the mainClass element of the spring-boot-maven-plugin in the plugin section of our pom.xml:

``` xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>             
            <configuration>    
                <mainClass>com.baeldung.DemoApplication</mainClass>
            </configuration>
        </plugin>
    </plugins>
</build>
```