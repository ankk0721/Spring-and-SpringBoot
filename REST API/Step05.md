# How request works?
  * First request goes to Dispatcher servlet(front controller pattern) because it maps to '/'.
  * You can check the whole process in logs after enabling debug log level
  * Then dispatcher servlet send request to appropriate controller then controller reteturn response.
  * Dispatcher servlet is configured by 'DispatcherServletAutoConfiguration, its a feature of spring boot 'auto configuration'.

  * HelloWorldBean object converted to json by @ResponseBody which a  part of @RestController and JacksonHttpMessageConverters which is configured by spring boot feature 'auto configuration'

  * Error page is also auto configured ErrorMvcAutoConfiguation


# @PathVariable
  * example of Path Parameters: /users/{id}/todos/{id}  => /users/2/todos/200
  * here id is path paramater and its mapped to name
    public HelloWorldBean helloWorldPathVariable(@PathVariable String name)
	{

	}


### helloworld/HelloWorldController.java

```java
package com.in28minutes.rest.webservices.restfulwebservices.helloworld;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {
	
	@GetMapping(path = "/hello-world")
	public String helloWorld() {
		return "Hello World"; 
	}
	
	@GetMapping(path = "/hello-world-bean")
	public HelloWorldBean helloWorldBean() {
		return new HelloWorldBean("Hello World"); 
	}
	
	// Path Parameters
	// /users/{id}/todos/{id}  => /users/2/todos/200
	// /hello-world/path-variable/{name}
	// /hello-world/path-variable/Ranga

//    you can also use @GetMapping(path="hello-world-bean/{name}")
	@GetMapping(path = "/hello-world/path-variable/{name}")
	public HelloWorldBean helloWorldPathVariable(@PathVariable String name) {
		return new HelloWorldBean(String.format("Hello World, %s", name)); 
	}

	
	
}
```
