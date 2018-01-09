# Java Spring Framework Cheat Sheet

## Spring Inversion of Control

--------
** Two primary functions of Spring Container:**

1. Inversion of Control: Create and manage objects
2. Dependency injection: Inject object's dependencies
--------

### What is inversion of control?

  It is outsource creation and managing the objects.
  That Outsourcing will be handled by object factory

  for Spring:
    1. configure spring beans (xml, annotation, ..)
    2. create a spring container on main method. (This is factory, or known as application context)
    3. retrieve beans from factory

**Spring Bean:** A "Spring Bean" is simply a Java object.
When Java objects are created by the Spring Container, then Spring refers to them as "Spring Beans".


## Spring Dependency injection
**Formally:**
  The client delegates to call to another object the responsibility of providing its dependencies.

**Unformally:**
Outsource the construction and injection of your object to an external entity.

***Two main types of injection***

1. **Constructor Injection**

```xml
<bean id="myCoach"
       class="com.kilicaslan.enes.BaseballCoach">
       <constructor-arg ref="myFortune" />
</bean>

```

2. **Setter Injection**


```xml
<bean id="myCoach3"
		class="com.kilicaslan.enes.SoccerClass">
		<property name="coachFortune" ref="myFortune" />
		<property name="name" value="Morinho" />
		<property name="email" value="Morinho@gmail.com" />
</bean>

```


## Spring Bean Scopes and Lifecycle

You can create beans like  the following on a configuration file.

```xml
<bean id="myCoach2" class="com.kilicaslan.enes.TrackCoach">
</bean>

```

The default scope for beans is ***Singleton***

***What is Singleton?*** Spring Container creates only one instance of the bean. It's cached on the memory. All requests for the bean will return shared reference to the same bean.


```xml
<bean id="myCoach2" class="com.kilicaslan.enes.TrackCoach"
  scope="singleton">
</bean>
```


***Prototype Scope:***
New object is created for each request of container request

```xml

<bean id="myCoach2" class="com.kilicaslan.enes.TrackCoach"
  scope="prototype">
</bean>

```



### Bean Lifecycle Methods:
Hook are bean Lifecycle methods those can be called during bean initialization and destruction.
For example, you can set up handle methods for db, socket and files. And then, you can clean up handles for db, socket and files.

***init-method="doSturtupStuff"*** attribute is used for initialization. But ***doSturtupStuff*** method must be public void method.

***destroy-method="doMyCleanupStuff"*** attribute is used for destruction.



## Java Annotations:

- They are special label/markers added to Java classes.
- Provides meta-date about the classes

**Exampel:** ***@Override***

We use them in Spring Because XML configuration files are verbose.  Instead we can create Spring beans with annotations to minimize XML configuration.

in order to enable component scan of Spring use following
piece of line on the applicationContext file

```XML
<context:component-scan base-package="com.kilicaslan.enes"/>

```

and add ***@Component*** line to the class.

The default bean id is same as the class name except from the first letter which is lowercase.

#### Constructor Injection:
We can do this using annotation and auto-wiring. Spring will wire up dependencies automatically. ***@Autowired*** annotation is used.

```Java
// put the keyword to the Constructor
@Autowired
public TennisCoach(FortuneService theFs) {
	fs = theFs;
}

```
















*********************

***useful links:***

- [Spring Documentation Site](https://spring.io)


*********************



***@author: [Enes Kilicaslan](http://eneskilicaslan.github.io)***

***@ref: [Udemy](https://www.udemy.com/spring-hibernate-tutorial)***
