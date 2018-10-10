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
        MockTracer tracer = new MockTracer()
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
