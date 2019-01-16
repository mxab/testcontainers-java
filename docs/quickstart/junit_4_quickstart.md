# JUnit 4 Quickstart

It's easy to add Testcontainers to your project - let's walk through a quick example to see how.

Let's imagine we have a simple program that has a dependency on Redis, and we want to add some tests for it.
 
You can see the implementation code and the corresponding test before we've added Testcontainers below:

!!! example "Scenario code"
    <!--codeinclude-->
    [Implementation](../example/src/main/java/quickstart/RedisBackedCache.java) block:RedisBackedCache
    [Pre-Testcontainers test code](../example/src/test/java/quickstart/RedisBackedCacheIntTestStep0.java) block:RedisBackedCacheIntTestStep0
    <!--/codeinclude-->

Notice that the existing test has a problem - it's relying on a local installation of Redis, which is a red flag for test reliability.
This may work if we were sure that every developer and CI machine had Redis installed, but would fail otherwise.
We might also have problems if we attempted to run tests in parallel, such as state bleeding between tests, or port clashes.

Let's start from here, and see how to improve the test with Testcontainers:  

## 1. Add Testcontainers as a test-scoped dependency

First, add Testcontainers as a dependency as follows:

```xml tab='Maven'
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>{{latest_version}}</version>
    <scope>test</scope>
</dependency>
```

```groovy tab='Gradle'
testRuntime "org.testcontainers:testcontainers:{{latest_version}}"
```

## 2. Get Testcontainers to run a Redis container during our tests

Simply add the following to the body of our test class:

<!--codeinclude-->
[JUnit 4 Rule](../example/src/test/java/quickstart/RedisBackedCacheIntTest.java) inside_block:rule
<!--/codeinclude-->

The `@Rule` annotation tells JUnit to notify this field about various events in the test lifecycle.
In this case, our rule object is a Testcontainers `GenericContainer`, configured to use a recent Redis image from Docker Hub, and configured to expose a port.

If we run our test as-is, then regardless of the actual test outcome, we'll see logs showing us that Testcontainers:

* was activated before our test method ran
* discovered and quickly tested our local Docker setup
* pulled the image if necessary
* started a new container and waited for it to be ready
* shut down and deleted the container after the test

## 3. Make sure our code can talk to the container

Before Testcontainers, we might have hardcoded an address like `localhost:6379` into our tests.

Testcontainers uses *randomized ports* for each container it starts, but makes it easy to obtain the actual port at runtime.
We can do this in our test `setUp` method, to set up our component under test:

<!--codeinclude-->
[Obtaining a mapped port](../example/src/test/java/quickstart/RedisBackedCacheIntTest.java) inside_block:setUp
<!--/codeinclude-->

## 4. Run the tests!

That's it!

Let's look at our complete test class to see how little we had to add to get up and running with Testcontainers:

<!--codeinclude-->
[RedisBackedCacheIntTest](../example/src/test/java/quickstart/RedisBackedCacheIntTest.java) block:RedisBackedCacheIntTest
<!--/codeinclude-->

## Further reading

TODO