# Postgres Module

## Database container class usage example

> TODO

## JDBC URL usage example

> TODO

## Adding this module to your project dependencies

Add the following dependency to your `pom.xml`/`build.gradle` file:

```xml tab='Maven'
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>postgres</artifactId>
    <version>{{latest_version}}</version>
    <scope>test</scope>
</dependency>
```

```groovy tab='Gradle'
testRuntime "org.testcontainers:postgres:{{latest_version}}"
```

!!! hint
    Adding this Testcontainers library JAR will not automatically add a database driver JAR to your project. You should ensure that your project also has a suitable database driver as a dependency.

## Attributions and thanks

> TODO
