## Explaining Spring with helloworld ( only primitive data type beans e.g string)
    * All coponenets discussed in detail later
	* first create a Configuration class by adding @Configuration, inside this create a bean by adding @Bean
	* Bean is an object managed by Spring IOC
	* Create a context inside main app using annotation config
	* retrieve the bean using name 
	* name is same as the method name

### HelloWorldConfiguration.java

```java
package com.in28minutes.learnspringframework;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HelloWorldConfiguration {

@Bean
public String name() {
return "Ranga";
}	
}
```

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

}

}
```
---


