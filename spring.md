# Java Spring Framework Cheat Sheet

## Spring Inversion of Control

--------
**Two primary functions of Spring Container:**

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


#### Setter Injection:
Again just simply put ***@Autowired*** annotation to the method. And the name of the method does not matter, it can be anything.

```Java
@Autowired
public void setFortune(FortuneService theFs) {
	this.fs = theFs;
}

```


### Field Injection:
We can inject dependencies by setting field values, and this is accomplished by using ***Java Reflection***.

set the field directly with annotation. No need for setter method even if it would be private.





### Scope with annotation:

Use ***@Scope*** annotation to specify a scope for a bean.

1. Singleton: ***@Scope("singleton")***
2. Prototype: ***@Scope("prototype")***

### Lifecycle methods with annotation:

Use annotations ***@PostConstruct*** and ***@PreDestroy***.


## Spring Configuration with Java Code:

Instead of configuring Spring container using XML, configure it using Java code.

Need to use ***@Configuration*** and ***@Bean*** annotations.
define methods to expose bean, the method name will be the bean id.

```Java
package com.kilicaslan.enes;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
//@ComponentScan("com.kilicaslan.enes")
public class CoachConfig {
	@Bean
	public FortuneService fortuneService() {
		return new SadFortuneService();
	}

	@Bean
	public Coach swimCoach() {
		return new SwimCoach(fortuneService());
	}

}

```



## Spring MVC

***@Controller*** annotation is inherited from ***@Component*** annotation.

- ***@RequestMapping*** annotation maps url to the corresponding method
- you can use ***request.getParameter("studentName");*** to read form data

```Java
@Controller
public class HelloWorldController {

	@RequestMapping("/showForm")
	public String showForm() {
		return "helloworld-form";
	}

	@RequestMapping("/processForm")
	public String processForm() {
		return "helloworld";
	}

  @RequestMapping("/processFormTwo")
	public String processsFormTwo(HttpServletRequest request, Model model) {

		String name = request.getParameter("studentName");

		name = name.toUpperCase();
		name += " 1984";

		model.addAttribute("new_name", name);

		return "helloworld";
	}
}

```

***@RequestParam*** annotation directly binds form data to variable.

```Java
@RequestMapping("/processFormThree")
	public String processsFormThree(
      @RequestParam("studentName") String name,
      Model model) {

  ...

}

```

*You can use Controller level mapping to create a sub urls.*

In order to use Spring MVC form tags, we need to send the model to the form.
The following line imports spring tags for html

```html
<%@ taglib  prefix="form" uri="http://www.springframework.org/tags/form"%>

```



Then you can create form like the following in the jsp file

```html
<form:form action="processForm" modelAttribute="student">
			First Name: <form:input path="firstName" />
			<br/><br/>

			Last Name:  <form:input path="lastName" />
			<br/><br/>

			Country:
				<form:select path="country">
					<form:option value="BRA" label="brazil"/>
					<form:option value="TR" label="turkey"/>
					<form:option value="USA" label="amerika"/>
				</form:select>

			<input type="submit" value="Submit" />
		</form:form>

```


in order to read form options from a properties file, we need to add configuration for util.

and then add the file to the xml configuration file like the following.

```html
<util:properties id="countryOptions" location="classpath:../countries.properties" />
```

And then you can inject the elements in the controller like following

```Java
@Value("#{countryOptions}")
private LinkedHashMap<String, String> countryOptions;
```

Radio buttons are represented by form:radiobutton tags

Following is example of radio buttons in Spring MVC
```html
Java: <form:radiobutton path="favoriteLanguage" value="Java"/>
Python: <form:radiobutton path="favoriteLanguage" value="Python"/>
Ruby: <form:radiobutton path="favoriteLanguage" value="Ruby"/>
```

There is a field in Student Class named favoriteLanguage, and when the user submits the form Spring MVC will call setFavoriteLanguage method automatically.


## Spring MVC Validation Rules

you can use various annotations for Validation.

The following annotations are added to the class field

```Java
@NotNull(message="required")
@Size(min=1)
private String lastName;
```

- you can check if the form pass validations like in the following.
- BindingResult must come right after @Valid attribute

```Java
@RequestMapping("/processForm")
public String processForm(
		@Valid @ModelAttribute("customer") Customer customer,
		BindingResult bindingResult) {

	System.out.println(customer.getLastName());

	if( bindingResult.hasErrors())
		return "customer-form";
	else
		return "customer-confirmation";
}
```

But now we have whitespace problem.

To solve this problem there is an annotation @InitBinder .
This works like preprocessor, e.i. it basically preprocess all requests coming in our controller.

Method signature is always same, takes WebDataBinder as argument.

```Java
@InitBinder
public void initBinder(WebDataBinder dataBinder) {
	StringTrimmerEditor trimmer = new StringTrimmerEditor(true);
	dataBinder.registerCustomEditor(String.class, trimmer);
}
```

We can validate number ranges with **@Min** and **@Max** annotations, and value parameter is required.

We can even validate a string with regular expression by using **@Pattern** annotation and its regexp parameter.

```Java
@Min(value = 0, message="number of passes must be greater than zero")
@Max(value = 10, message="number of passes must be less than 10")
private int numberOfFreePasses;

@Pattern(regexp="^[a-zA-Z0-9]{5}", message="wrong format")
private String postCode;
```

***Note:*** In order to make an int field required, we can add @NotNull annotation, but this will throw String to int type conversion exception. So we need to use  class type Integer. This will work Because we are running a string trimmer as preprocess with @InitBinder

But this is not enough to validate Integer field, we need to create custom message. To make it work we need to create a properties file which includes custom messages

```
file: messages.properties

typeMismatch.customer.numberOfFreePasses=Invalid Number
```
We can find the related error by printing bindingResult.

Here;
- **typeMismatch:** error type
- **customer:** object name
- **numberOfFreePasses:** is field name

### How to create custom validation rules

Here we basically create our own annotation.
  1. create custom annotation
  2. create constraint validator

To create annotation use **@interface** notation.

```Java
package com.kilicaslan.enes.mvc.validation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import javax.validation.Constraint;
import javax.validation.Payload;

@Constraint(validatedBy = CourseCodeConstraintValidator.class)
@Target( {ElementType.FIELD, ElementType.METHOD } )
@Retention(RetentionPolicy.RUNTIME)
public @interface CourseCode {

	public String value() default "LUV";

	public String message() default "must start with LUV";


	public Class<?>[ ] groups() default {};

	public Class<? extends Payload> [] payload() default {};

}
```

```Java
package com.kilicaslan.enes.mvc.validation;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;

public class CourseCodeConstraintValidator implements ConstraintValidator<CourseCode, String> {

	private String prefix;

	@Override
	public void initialize(CourseCode arg0) {
		prefix = arg0.value();
	}

	@Override
	public boolean isValid(String theCode, ConstraintValidatorContext arg1) {

		if( theCode != null)
			return theCode.startsWith(prefix);
		return false;
	}

}
```

Now you can use the validation annotation like the following

```java
@CourseCode(value="CENG", message="Must start with CENG")
private String courseCode;

```

## Hibernate

Hibernate uses JDBC behind the scenes.

In order to run hibernate, we must have correctly running mysql database and all necessary jar files added to class path.

Steps:
1. Add Hibernate configuration file
2. Annotate Java Class
3. Develop Java Code to perform database operations

Configuration file basically tells Hibernate how to connect to database. Because of that we put JDBC url to the configuration file.

Example basic configuration file for Hibernate:

```XML
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- JDBC Database connection settings -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false</property>
        <property name="connection.username">hbstudent</property>
        <property name="connection.password">hbstudent</property>

        <!-- JDBC connection pool settings ... using built-in test pool -->
        <property name="connection.pool_size">1</property>

        <!-- Select our SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

        <!-- Echo the SQL to stdout -->
        <property name="show_sql">true</property>

		<!-- Set the current session context -->
		<property name="current_session_context_class">thread</property>

    </session-factory>

</hibernate-configuration>

```


Hibernate has a concept of entity class.
**entity class** is a Java class that is mapped to a database table.


in order to map a class to a database table and its fields to columns, we can uses annotations like @Entity, @Table, @Column.

@Id is used for primary key.

***javax persinstant*** API should be used, because it is the standard.

Hibernate Example Mapping with annotations:
```java
package com.kilicaslan.enes.hibernate.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table(name="student")
public class Student {

	@Id
	@Column(name="id")
	int id;

	@Column(name="first_name")
	String firstName;

	@Column(name="last_name")
	String lastName;

	@Column(name="email")
	String email;
}
```


To write some code on hibernate, we need to create a hibernate session factory which creates sessions.

***Session Factory:***
- Reads hibernate config file
- Creates session objects
- We create once in our application

***Session:***
- Wraps a JDBC connection
- Main objects to save/retrieve objects to DB
- Retrieve from Session Factory


```Java
// create session factory
SessionFactory factory = new Configuration().configure("hibernate.cfg.xml")
							.addAnnotatedClass(Student.class)
              .buildSessionFactory();

// create session
Session session = factory.getCurrentSession();


try {

			// create new Student object
			System.out.println("Creating new Student Object");
			Student st =  new Student("Enes", "Kilicaslan" , "eeneskilicaslan@gmail.com");

			// start transaction
			session.beginTransaction();

			// save the object
			System.out.println("Saving the student");
			session.save(st);			

			//commit transaction
			session.getTransaction().commit();
			System.out.println("Done!");

} finally {

			factory.close();

		}
```


##### Primary Keys:

There is implicit annotation when we use ***@Id*** annotation, that is ***@GeneratedValue(strategy=GenerationType.IDENTITY)***

###### Sql Note:
We can reset auto increment value by ***truncate hb_student_tracker.student***


We can retrieve an Entity object by using primary key id with **get()** method like following:

```Java
int id = 1;
Student mySt =  session.get(Student.class, id);

```

#### Hibernate Query Language:

- similar to sql
- where, like, order by, join, in, ..etc

We can use this queries like following:

```Java
students = session
    .createQuery("from Student s where s.lastName like '%uck'")
    .list();

System.out.println("\n\nStudents whose last names end with uck:");
displayStudents(students);
```

We can update table elements with ***executeUpdate*** method, like in the following example

```Java

session.createQuery("update Student  set firstName='Bomba'"
 + "where lastName='Kilicaslan'").executeUpdate();
```

We can delete table elements with ***delete*** method of the session if we can retrieve the corresponding object. If we don't have the object, then we can use ***executeUpdate*** like we did before.




## Advanced Mapping:

***One-to-one mapping:*** user -> profile

***One-to-many mapping:*** instructor -> many courses.

***Many-to many mapping:*** many students -> many courses.


#### Important Database Concepts:

- Primary Key
- Foreign key
- Cascade



1. Primary Key:
It is used to identify a unique row in a table.

2. Foreign Key:
It is a field in one table that refers to primary key in another table. Links tables together

3. Cascade: apply the same operation to the related entities, like Foreign keys.

4. Eager Loading: retrieve everything

5. Lazy Loading: retrieve on request


### One to One mapping in Hibernate:

After adding Foreign key to the database we can use ***@OneToOne*** annotation to create a relation with two tables.

##### Entity Lifecycle:
1. Detach: Entity is not associated with hibernate session.
2. Merge:  If instance is detached, reattach to the session.
3. Persist: Transitions new instances to managed stage, next commit/flush will save it in db.
4. Remove: Transitions managed entity to be removed, next commit/flush will delete it from db.
5. Refresh: Reload/Synch object with data from db, prevents stale data.


***Note:***
By default no cascade is configured, we need to specify it. And we can do it with ***@OneToOne(cascade=CascadeType.ALL)***, here all means all of the cascade types; i.e save, remove, refresh, detach and merge.


***Steps need to Remember! to create Entity class:***
1. Annotate the class as entity and map to db table
2. define  fields
3. annotate the fields with column names in db table
4. Create constructors
5. Create getter and setters
6. generate toString


#### Unidirectional Mapping

```Java
@Entity
@Table(name="instructor")
public class Instructor {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@Column(name="first_name")
	private String firstName;

	@Column(name="last_name")
	private String lastName;

	@Column(name="email")
	private String email;

	@OneToOne(cascade=CascadeType.ALL)
	@JoinColumn(name="instructor_detail_id")
	private InstructorDetail instructorDetail;

}

```


```java
@Entity
@Table(name="instructor_detail")
public class InstructorDetail {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@Column(name="youtube_channel")
	private String youtubeChannel;

	@Column(name="hobby")
	private String hobby;
}
```


```java
public class CreateDemo {
	public static void main(String[] args) {

		// create session factory
		SessionFactory factory = new Configuration().configure("hibernate.cfg.xml")
									.addAnnotatedClass(Instructor.class).
									addAnnotatedClass(InstructorDetail.class)
									.buildSessionFactory();

		// create session
		Session session = factory.getCurrentSession();


		try {
			// create object
			Instructor enes = new Instructor("black", "white", "eeneskilicaslan@gmai.com"
											, new InstructorDetail("youtube", "love code"));

			// start transaction
			session.beginTransaction();
			session.save(enes);

			//commit transaction
			session.getTransaction().commit();
			System.out.println("Done!");

		} finally {
			factory.close();
		}
	}
}
```


#### Bidirectional Mapping

- No need to change database.
- We need to use ***@MappedBy*** annotation.

***Note:*** We can solve connection polling leak issue by closing session. It is always safer to check the null pointer exception.

```Java
@Entity
@Table(name="instructor")
public class Instructor {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@Column(name="first_name")
	private String firstName;

	@Column(name="last_name")
	private String lastName;

	@Column(name="email")
	private String email;

	@OneToOne(cascade=CascadeType.ALL)
	@JoinColumn(name="instructor_detail_id")
	private InstructorDetail instructorDetail;
}
```

```Java
@Entity
@Table(name="instructor_detail")
public class InstructorDetail {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY)
	@Column(name="id")
	private int id;

	@Column(name="youtube_channel")
	private String youtubeChannel;

	@Column(name="hobby")
	private String hobby;

	// to make bi-directional possible, create the field
	@OneToOne(mappedBy="instructorDetail", cascade=CascadeType.ALL)
	private Instructor instructor;

	public Instructor getInstructor() {
		return instructor;
	}

	public void setInstructor(Instructor instructor) {
		this.instructor = instructor;
	}
}
```






*********************

***useful links:***

- [Spring Documentation Site](https://spring.io)



*********************



***@author: [Enes Kilicaslan](http://eneskilicaslan.github.io)***

***@ref: [Udemy](https://www.udemy.com/spring-hibernate-tutorial)***
