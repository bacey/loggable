
## The original README.md:

Usage:

```
mvn clean verify
java -jar target/loggable-0.0.1-SNAPSHOT-jar-with-dependencies.jar
```

You can also import the project in Eclipse using m2e and run the Foo class. You just have to build it once because m2e ignores the jcabi-maven-plugin plugin, that is, there isn't a m2e configurator for that.

Based on http://www.yegor256.com/2014/06/01/aop-aspectj-java-method-logging.html

## The description of my issue:

I added a new logger called `performanceLogger` (see the `log4j.properties` file) because in the future I'd like to redirect these performance logs into a different file.
But when I used this new `performanceLogger` (i.e. `@Loggable(name = "performanceLogger")`), then the class name (i.e. `com.jcabi.example.Foo`) was lost from the output log line.

The original output (`master` branch, using `@Loggable`):

```
2016-04-13 15:56:35 [INFO] com.jcabi.aspects.aj.NamedThreads:193 - com.jcabi.log.Logger com.jcabi.log.Logger.info(Logger.java:193) info  Logger.java - jcabi-aspects 0.22.1/58f97a9 started new daemon thread jcabi-loggable for watching of @Loggable annotated methods
2016-04-13 15:56:35 [INFO] com.jcabi.example.Foo:193 - com.jcabi.log.Logger com.jcabi.log.Logger.info(Logger.java:193) info  Logger.java - #power(2, 2): 4.0 in 995.58▒s
4.0
```

The output, using `@Loggable(name = "performanceLogger")` in the `performanceLogger` branch. Notice that the `com.jcabi.example.Foo:193` class name turned into `performanceLogger:193` and I don't see a way to extract/get that class name from anywhere:

```
2016-04-13 15:55:59 [INFO] com.jcabi.aspects.aj.NamedThreads:193 - com.jcabi.log.Logger com.jcabi.log.Logger.info(Logger.java:193) info  Logger.java - jcabi-aspects 0.22.1/58f97a9 started new daemon thread jcabi-loggable for watching of @Loggable annotated methods
2016-04-13 15:55:59 [INFO] performanceLogger:193 - com.jcabi.log.Logger com.jcabi.log.Logger.info(Logger.java:193) info  Logger.java - #power(2, 2): 4.0 in 988.47▒s
4.0
```

Just as a hint, in the (now deceased) perf4j one was able to write something like this:

```
@Profiled(tag = "CalculationService.calculatePi")
```

Would it be possible somehow to have a `@Loggable(name = "performanceLogger")` annotation while preserving somewhere, somehow the original class name?

Thank you in advance,
Bela


