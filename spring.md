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






*********************

***useful links:***

- [Spring Documentation Site](https://spring.io)


*********************



***@author: [Enes Kilicaslan](http://eneskilicaslan.github.io)***

***@ref: [Udemy](https://www.udemy.com/spring-hibernate-tutorial)***
