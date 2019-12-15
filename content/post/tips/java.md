---
title: java
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-11-12T08:57:46+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Secret in java `keytool`, its default password is `changeit`

## Java on macbook
> `/usr/libexec/java_home -V` to show java homes 
> `export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)` to switch to version 1.8

## How to print envs?

```java
        Map<String, String> enviorntmentVars = System.getenv();
        enviorntmentVars.entrySet().forEach(System.out::println);
```

## [What is a static class in Java?](https://www.tutorialspoint.com/What-is-a-static-class-in-Java)
> You cannot use the static keyword with a class unless it is an inner class. A static inner class is a nested class which is a static member of the outer class. It can be accessed without instantiating the outer class, using other static members. Just like static members, a static nested class does not have access to the instance variables and methods of the outer class.

##  About `Thread.currentThread().join`
> Thread.currentThread().join() blocks the current thread forever. In your example, that prevents the main from exiting, unless the program is killed, e.g. with CTRL+C on Windows.
> An alternative would have been to use Thread.sleep(Long.MAX_VALUE);.

## Fix `Failed to load class "org.slf4j.impl.StaticLoggerBinder".`
> SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
> SLF4J: Defaulting to no-operation (NOP) logger implementation
> SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.

The fix is to add `slf4j-simple` dependency to pom.xml like this:

```xml
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-simple</artifactId>
      <version>1.6.6</version>
    </dependency>
```

## [Java 7中的Try-with-resources](http://ifeve.com/java-7%E4%B8%AD%E7%9A%84try-with-resources/)

## Study [MicroDonuts: An OpenTracing Walkthrough](https://github.com/opentracing-contrib/java-opentracing-walkthrough)
> Use `exec-maven-plugin` to run `mvn package exec:exec` to run java app
> Load Properties from a properties file:
```java
        FileInputStream fs = new FileInputStream(file);
        Properties config = new Properties();
        config.load(fs);
```
> This project demoed how to start a `jetty` server
> Why I can't get that hello-world program work is because it lacks `SenderConfiguration` like this:
```java
            SenderConfiguration senderConfig = new SenderConfiguration()
                .withAgentHost(config.getProperty("jaeger.reporter_host"))
                .withAgentPort(Integer.decode(config.getProperty("jaeger.reporter_port")));
            ReporterConfiguration reporterConfig = new ReporterConfiguration()
                .withLogSpans(true)
                .withFlushInterval(1000)
                .withMaxQueueSize(10000)
                .withSender(senderConfig);
```
> With `GlobalTracer.register(tracer);` done, other code is the same to use tracer.

## How to run app via java and jar?
> Like this: `java -classpath Predit.jar your.package.name.MainClass`

## Use google guava to create an immutable map

```java
Map<String, String> tags = ImmutableMap.<String, String>builder().put("deviceId", "dev101").build();
Map<String, Object> fields = ImmutableMap.<String, Object>builder().put("temperature", 10)
        .put("speed", "10000rpm").build();
```

## Java final and `Autowired`
> Having @Autowired and final on a field are contradictory.
> The latter says: this variable has one and only one value, and it's initialized at construction time.
> The former says: Spring will construct the object, leaving this field as null (its default value). Then Spring will use reflection to initialize this field with a bean of type WorkspaceRepository.
> If you want final fields autowired, use constructor injection, just like you would do if you did the injection by yourself:

```
@Autowired
public WorkspaceController(WorkspaceRepository repository) {
    this.repository = repository;
}
```

## How to load test resource?
```
package com.example;
import org.apache.commons.io.IOUtils;
public class FooTest {
  @Test 
  public void shouldWork() throws Exception {
    String xml = IOUtils.toString(
      this.getClass().getResourceAsStream("abc.xml"),
      "UTF-8"
    );
  }
}
```
The above code will load resource: *src/test/resources/com/example/abc.xml*.  You can load *src/test/resources/foo/test.xml* by `getResourceAsStream("/foo/test.xml")`
Reference: https://stackoverflow.com/questions/3891375/how-to-read-a-text-file-resource-into-java-unit-test
## How to run a simple java program?
Suppose you need to compile and running SimpleJdbc.java:
```
javac SimpleJdbc
java -cp ifxjdbc.jar:. SimpleJdbc
```
NOTE: Add both current directory(_._) and needed jar to class path
## How to read line from stdin?
```
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
br.readLine();
```

## Reference: 
## [Java concurrency guide](http://winterbe.com/posts/2015/04/07/java8-concurrency-tutorial-thread-executor-examples/)
## [Java profiling tools](http://www.infoq.com/articles/java-profiling-with-open-source)
