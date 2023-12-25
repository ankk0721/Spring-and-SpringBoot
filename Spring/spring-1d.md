# Spring example with bean(user defined class e.g person)
	* record: it creates a class without writing long code
			Public accessor methods, constructor, equals, hashcode and toString are automatically created. 
			Released in JDK 16.
			e.g. record Person (String name, int age) { };


### /learn-spring-framework/src/main/java/com/in28minutes/learnspringframework/App02HelloWorldSpring.java

```java
package com.in28minutes.learnspringframework;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App02HelloWorldSpring {

public static void main(String[] args) {

//1: Launch a Spring Context
var context = 
new AnnotationConfigApplicationContext(HelloWorldConfiguration.class);

//2: Configure the things that we want Spring to manage - 
//HelloWorldConfiguration - @Configuration
//name - @Bean

//3: Retrieving Beans managed by Spring
System.out.println(context.getBean("name"));

System.out.println(context.getBean("age"));

System.out.println(context.getBean("person"));

System.out.println(context.getBean("address"));

}

}
```
---

### HelloWorldConfiguration.java

```java
package com.in28minutes.learnspringframework;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

//Eliminate verbosity in creating Java Beans
//Public accessor methods, constructor, 
//equals, hashcode and toString are automatically created. 
//Released in JDK 16.

record Person (String name, int age) { };

//Address - firstLine & city
record Address(String firstLine, String city){ };

@Configuration
public class HelloWorldConfiguration {

@Bean
public String name() {
return "Ranga";
}

@Bean
public int age() {
return 15;
}

@Bean
public Person person() {
return new Person("Ravi", 20);		
}

@Bean
public Address address() {
return new Address("Baker Street", "London");		
}


}
```
