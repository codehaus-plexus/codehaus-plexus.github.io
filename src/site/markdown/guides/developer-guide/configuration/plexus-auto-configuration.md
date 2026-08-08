---
title: How auto-configuration works
author: Michal Maczka
date: 2004-10-13
---

`The contents of this document are a work in progress`

# Plexus Auto Configuration

## Auto configuration is ...

If you are familiar with libraries like [xstream](http://xstream.codehaus.org) you will feel at home. If not, it's simple enough anyway.

The best way to understand how the plexus ComponentConfigurator functions is by example. Assuming that the auto-configuration mechanism is applied to class:

**com.MyComponent**

```
class com.MyComponent implements com.SomeInterface
{
   private String propertyA;

   private int propertyB;

   ...
}
```

with a plexus configuration like so:

```
<component>
  <role>com.SomeInterface</role>
  <implementation>com.MyComponent</implementation>
    <configuration>
        <propertyA>foo</propertyA>
        <propertyB>1</propertyA>
    </configuration>
</component>
```

The actions the plexus ComponentConfigurator would perform on the instance of component would be something like this:

```
com.SomeInterface component = new com.MyComponent();

component.propertyA = "foo";

component.propertyB = 1;
```

### Types Supported by Autoconfiguration

#### Basic

- java.lang.Boolean & boolean

- java.lang.Byte & byte

- java.lang.Character & char

- java.lang.Double & double

- java.lang.Float & float

- java.lang.Integer & int

- java.lang.Long & long

- java.lang.Short & short

- java.lang.StringBuffer

- java.lang.String

- java.util.Date (todo: document supported patterns)

- java.math.BigDecimal

- java.math.BigInteger

#### Composite types (Object properties)

- **Object with properties**

_Component implementation:_

```
class com.MyComponent
{
   private Person person;

   ...
}

class com.Person
{
   private String firstname;

   private String lastname;

   ...
}
```

_Component configuration :_

```
<configuration>
   <person>
       <firstname>Baltzar<firstname>
       <lastname>Gabka</lastname>
   </person>
</configuration>
```

_Actions taken by Component Configurator:_

```
Person person = new Person();

person.firstname = "Baltazar";
person.lastname = "Gabka";

component.person = person;
```

- **Collections**

TODO: Collections example.

- **java.lang.Properties**

_Component implementation:_

```
class com.MyComponent
{
   private Properties propertiesA;
   ...
}
```

_Component configuration :_

```
<configuration>
   <propertiesA>
      <property>
          <name>key1</name>
          <value>value1</value>
      </property>
      <property>
          <name>key2</name>
          <value>value2</value>
      </property>
   </propertiesA>
</configuration>
```

_Actions taken by Component Configurator:_

```

com.MyComponent component;

Properties properties = new Properties();

properties.put( "key1", "name1" )

properties.put( "key2", "name2" )

setFieldValue( component, "propertiesA", properties );

component.propertiesA = properties;

```

#### Advanced mapping

<!-- michal: Doesn't this belong in the Collection's section? -->
<!-- TODO: Flesh out... -->

```
<foos elements="java.lang.String"/>
    <id>ala</id>
    <id>ala</id>
    <id>ala</id>
</foos>
```

or possibly (pending implementation)

```
<foos elements="com.MyBean" implementation="java.util.LinkedList"/>
    <bean>
        <id>foo</id>
    </bean>
    <bean>
        </id>ala</id>
    </bean>
</foos>
```
