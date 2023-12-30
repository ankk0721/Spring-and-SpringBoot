# Rest API
	* REST API stands for Representational State Transfer Application Programming Interface
	* First define that class as @RestController
	* Then create function which will return something
	* @RequestMapping(method=RequestMethod.GET,path="hello-world") and
	  @GetMapping(path = "/hello-world") are same
	* @RestController is a specialized versin of @Controller
	* @Controler is spring mvc (web) and another one is for restful web api
	* controller will return view but restController not
	* @RequestMapping methods assume @ResponseBody semantics by default in @RestController

### HelloWorldController.java

```java
package com.in28minutes.rest.webservices.restfulwebservices.helloworld;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {

	// @RequestMapping(method=RequestMethod.GET,path="hello-world")
	@GetMapping(path = "/hello-world")
	public String helloWorld() {
		return "Hello World"; 
	}
	
	
}
```