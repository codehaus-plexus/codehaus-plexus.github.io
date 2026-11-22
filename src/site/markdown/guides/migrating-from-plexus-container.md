---
title: Migrating from the Plexus container
---

# Migrating from the Plexus container

The Plexus IoC container ([plexus-containers](https://github.com/codehaus-plexus/plexus-containers)) is retired. Maven, and every library on this site, now use [JSR-330](https://jcp.org/en/jsr/detail?id=330) annotations on top of [Eclipse Sisu](https://eclipse.dev/sisu/). The old tutorials and guides keep their addresses because other sites link to them, but they no longer teach the container. This page maps each old concept to its replacement and links the maintained documentation for each step.

## Where the maintained documentation is

The following pages already cover most of the migration, so this page only summarizes them.

- [Converting Plexus components to JSR-330](https://eclipse.dev/sisu/org.eclipse.sisu.plexus/conversion-to-jsr330.html) in the Sisu documentation covers `@Component`, `@Requirement`, `@Configuration` and the lifecycle interfaces, each with a before and after.
- [Maven and JSR-330](https://maven.apache.org/maven-jsr330.html) covers the POM setup and the index for plugins and extensions.
- [Sisu lifecycle support](https://eclipse.dev/sisu/org.eclipse.sisu.inject/lifecycle.html) covers `@PostConstruct` and `@PreDestroy`.
- [Plexus Testing](https://codehaus-plexus.github.io/plexus-testing/) covers JUnit 5 tests of components.
- [Maven Plugin Testing Harness](https://maven.apache.org/plugin-testing/maven-plugin-testing-harness/) and its [migration recipes](https://maven.apache.org/plugin-testing/maven-plugin-testing-harness/migration-3.4.0.html) cover tests of Mojos.

## Components and injection

A Plexus component is a class with a role, an optional role hint and fields marked `@Requirement`. In JSR-330 the class is a `@Named` bean, the hint becomes the name, and the requirements are `@Inject` constructor parameters. Prefer constructor injection: a component can then be created in a unit test without any container.

Before, with the retired container:

```java
@Component( role = Parser.class, hint = "my" )
public class MyParser implements Parser
{
    @Requirement
    private Logger logger;
}
```

After:

```java
@Named( "my" )
@Singleton
public class MyParser implements Parser
{
    private final Helper helper;

    @Inject
    public MyParser( Helper helper )
    {
        this.helper = helper;
    }
}
```

A component that was not a singleton (`instantiationStrategy="per-lookup"`) drops `@Singleton`. An optional requirement becomes a parameter annotated `@Nullable` (`org.eclipse.sisu.Nullable`). Both cases, and the `Default` prefix naming rule, are in the Sisu conversion page.

## Component index

The container read `META-INF/plexus/components.xml`, which you wrote by hand or generated with `plexus-component-metadata` or the Javadoc-tag tools. Sisu reads `META-INF/sisu/javax.inject.Named`, a plain list of class names that the `sisu-maven-plugin` generates from the `@Named` classes at build time. Nothing needs to be written by hand.

```xml
<dependencies>
  <dependency>
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
    <scope>provided</scope>
  </dependency>
</dependencies>

<build>
  <plugins>
    <plugin>
      <groupId>org.eclipse.sisu</groupId>
      <artifactId>sisu-maven-plugin</artifactId>
      <version><!-- latest release --></version>
      <executions>
        <execution>
          <id>index-components</id>
          <goals>
            <goal>main-index</goal>
          </goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

The `main-index` goal runs in the `process-classes` phase; `test-index` does the same for test classes. Maven itself only reads the index and does not scan the class path, so a component without an index is not found.

## Looking up components

`container.lookup( role, hint )` becomes injection. A named bean is requested with `@Named`, and every implementation of an interface is requested with a `List` or a `Map` keyed by name.

```java
@Inject
public MyComponent( @Named( "my" ) Parser parser, Map<String, Parser> allParsers )
{
    ...
}
```

Code that cannot be injected, such as a plugin that must pick an implementation at run time, can inject the Sisu `BeanLocator` (`org.eclipse.sisu.inject.BeanLocator`) and query it. A standalone application builds its own injector with Guice, Sisu's `SpaceModule` and `WireModule`, as shown in the [Sisu overview](https://eclipse.dev/sisu/org.eclipse.sisu.inject/).

## Lifecycle

`Initializable.initialize()` usually moves to the end of the constructor, or to a method annotated `@PostConstruct`. `Contextualizable` becomes an injected `@Parameters Map`. `Startable` and `Disposable` have no direct counterpart; Sisu supports `@PostConstruct` and `@PreDestroy` when its lifecycle module is enabled, which Maven 3.5 and later do. See the Sisu lifecycle page for what each annotation does and does not call.

## Configuration

The `<configuration>` block of a component descriptor and `@Configuration` become `@Named` parameters with an expression, and a default after `:-`.

```java
@Inject
public MyComponent( @Named( "${configDir:-target/config}" ) String configDir )
{
    ...
}
```

In the Plexus compatibility layer the values come from the container context data; the Javadoc of the `org.eclipse.sisu.wire` package describes the expression syntax. A component that took a complex configuration object can instead take a plain constructor parameter.

## Testing components

The old test base class is a JUnit 3 class, so tests built on it cannot move to JUnit 5 without a rewrite. The replacement is the `@PlexusTest` annotation of `org.codehaus.plexus:plexus-testing`, which creates a container for each test method and injects the `@Inject` fields of the test class.

Before:

```java
public class MyComponentTest extends PlexusTestCase
{
    public void testIt() throws Exception
    {
        MyComponent component = (MyComponent) lookup( MyComponent.class );
    }
}
```

After:

```java
import javax.inject.Inject;

import org.codehaus.plexus.testing.PlexusTest;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertNotNull;

@PlexusTest
class MyComponentTest
{
    @Inject
    MyComponent component;

    @Test
    void itWorks()
    {
        assertNotNull( component );
    }
}
```

```xml
<dependency>
  <groupId>org.codehaus.plexus</groupId>
  <artifactId>plexus-testing</artifactId>
  <version><!-- latest release --></version>
  <scope>test</scope>
</dependency>
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter</artifactId>
  <version><!-- latest release --></version>
  <scope>test</scope>
</dependency>
```

With constructor injection a plain JUnit 5 test, with or without Mockito, often needs no container at all. Customizing the container, and resolving paths such as the base directory, are covered on the Plexus Testing site.

## Testing Maven plugins

`AbstractMojoTestCase`, with `lookupMojo` and a test POM, is replaced by the `@MojoTest` and `@InjectMojo` annotations of `org.apache.maven.plugin-testing:maven-plugin-testing-harness` 3.4.0 and later, which run on JUnit 5.

Before:

```java
public class MyMojoTest extends AbstractMojoTestCase
{
    public void testExecute() throws Exception
    {
        MyMojo mojo = (MyMojo) lookupMojo( "touch", pom );
        mojo.execute();
    }
}
```

After:

```java
import org.apache.maven.api.plugin.testing.InjectMojo;
import org.apache.maven.api.plugin.testing.MojoTest;
import org.junit.jupiter.api.Test;

@MojoTest
class MyMojoTest
{
    @Test
    @InjectMojo( goal = "touch", pom = "src/test/resources/unit/project-to-test/pom.xml" )
    void execute( MyMojo mojo ) throws Exception
    {
        mojo.execute();
    }
}
```

The harness migration page lists the other replacements, such as stubs becoming `@Provides` methods.

## Logging

The container provided `org.codehaus.plexus.logging.Logger` through `AbstractLogEnabled` or a `@Requirement`. Use [SLF4J](https://www.slf4j.org/) instead: `LoggerFactory.getLogger( MyComponent.class )`, or `getLog()` inside a Mojo. A custom `LoggerManager` has no counterpart; the logging backend is chosen by the application, and Maven picks its own.

## plexus-utils and plexus-xml

The XML classes (`Xpp3Dom`, `Xpp3DomBuilder`, `Xpp3DomWriter`, the pull parser) moved out of `plexus-utils` 4 into [plexus-xml](https://codehaus-plexus.github.io/plexus-xml/), so a project that used them from `plexus-utils` adds `org.codehaus.plexus:plexus-xml`. The rest stays in [plexus-utils](https://codehaus-plexus.github.io/plexus-utils/). Where the JDK offers an equivalent, such as `java.nio.file.Files` instead of `FileUtils`, prefer the JDK.

## Parts without a replacement

The following parts of the old guides have no replacement, because the container they depended on does not exist any more.

- The Plexus embedder, `PlexusServletContextListener` and the application server with its PAR packaging: use Guice with Sisu in a standalone application, or the dependency injection of your web framework.
- The Plexus archetypes and the Codehaus snapshot repositories are gone. Create a plain Maven project from Maven Central.
- The Plexus Javadoc tags (`@plexus.component` and friends) and the `plexus-maven-plugin` are not supported by any current tool. Use the annotations above.
