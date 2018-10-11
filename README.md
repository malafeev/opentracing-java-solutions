# OpenTracing Java Solutions

## Grails 3

Because Grails 3 is based on Spring Boot we can use 
[OpenTracing Spring Web instrumentation](https://github.com/opentracing-contrib/java-spring-web).

build.gradle
```groovy
compile "io.opentracing.contrib:opentracing-spring-web-starter:0.3.4"
``` 

### Usage
```groovy
// Instantiate tracer
Tracer tracer = ...

// Register tracer with GlobalTracer
GlobalTracer.register(tracer)
```

For example in `main` method:
```groovy
class Application extends GrailsAutoConfiguration {
    static void main(String[] args) {
        Tracer tracer = new MockTracer()
        GlobalTracer.register(tracer)
        GrailsApp.run(Application, args)
    }
}
```

## Microsoft Azure

Azure java sdk is using OkHttp client and it supports adding interceptor.   
[OpenTracing OkHttp Client Instrumentation](https://github.com/opentracing-contrib/java-okhttp)
provides required interceptor. 

pom.xml
```xml
<dependency>
    <groupId>io.opentracing.contrib</groupId>
    <artifactId>opentracing-okhttp3</artifactId>
    <version>VERSION</version>
</dependency>
```

### Usage
```java
// Instantiate tracer
Tracer tracer = ...

// Add TracingInterceptor
Azure azure = Azure.configure()
        .withInterceptor(new TracingInterceptor(tracer))
        .authenticate(new File(System.getenv("AZURE_AUTH_LOCATION")))
        .withDefaultSubscription();
```

## Struts 2

Because Struts 2 is based on Servlet API we can use
[OpenTracing Java Web Servlet Filter Instrumentation](https://github.com/opentracing-contrib/java-web-servlet-filter)

pom.xml
```xml
<dependency>
    <groupId>io.opentracing.contrib</groupId>
    <artifactId>opentracing-web-servlet-filter</artifactId>
    <version>VERSION</version>
</dependency>
```

### Usage
1. Instantiate and register tracer in `ServletContextListener`
    ```java
    public class AppServletContextListener implements ServletContextListener {
      @Override
      public void contextInitialized(ServletContextEvent sce) {
        // Instantiate tracer
        Tracer tracer = new MockTracer();
        
        // Register tracer with GlobalTracer
        GlobalTracer.register(tracer);
      }
    
      @Override
      public void contextDestroyed(ServletContextEvent sce) {
    
      }
    }
    ```
2. Add listener to web.xml
    ```xml
    <listener>
        <listener-class>
          com.AppServletContextListener
        </listener-class>
    </listener>
    ```
3. Add `TracingFilter` to web.xml 
    ```xml
    <filter>
        <filter-name>opentracing</filter-name>
        <filter-class>io.opentracing.contrib.web.servlet.filter.TracingFilter</filter-class>
    </filter>
    
    <filter-mapping>
        <filter-name>opentracing</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    ```