# Different ways of getting bean
    * default name of the bean is name of the bean is the name f the method
	* we can specify the name of the bean e.g @Bean(name = "address2")
	* we can also retrieve the beans using class type e.g. context.getBean(Address.class)
	* Create a new bean using existing bean:
	  -> Method call: passing the name of method e.g person2MethodCall()
	  -> parameters: passing ithe specified bean name e.g. person3Parameters()

### App02HelloWorldSpring.java

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

//System.out.println(context.getBean(Address.class));

System.out.println(context.getBean("person2MethodCall"));

System.out.println(context.getBean("person3Parameters"));

System.out.println(context.getBean("address2"));


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

record Person (String name, int age, Address address) { };

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
return new Person("Ravi", 20, new Address("Main Street", "Utrecht"));		
}

@Bean
public Person person2MethodCall() {
return new Person(name(), age(), address()); //name, age		
}

@Bean
public Person person3Parameters(String name, int age, Address address3) {
//name,age,address2
return new Person(name, age, address3); //name, age		
}

@Bean(name = "address2")
public Address address() {
return new Address("Baker Street", "London");		
}

@Bean(name = "address3")
public Address address3() {
return new Address("Motinagar", "Hyderabad");		
}

}
```

