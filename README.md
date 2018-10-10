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
