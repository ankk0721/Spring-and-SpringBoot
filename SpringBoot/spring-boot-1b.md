# Spring Boot Goals
  * goal of spring boot is to make production ready app quickly
  * Build quickly:
		-> spring boot starter: these are something like packages for service which provides all the dependencies required for that service. e.g spring-boot-starter-web contains RestFulApi, spring mvc,etc.

		-> auto configuration: spring boot auto configure the properties for the dependencies present in pom.xml.

		-> dev-tools: If we add this dependecy, then we don't need to restart the application every time after changes.

   * Production-Ready:
        -> Logging
		-> Monitoring(spring actuator)
		-> different conf for diff env:
			-> Profiles: we can create profiles for differenet env (dev, qa, stage, prod)
						for this, we have to create new application.properties file with name syntax "application-envName.properties" e.g application-dev.properties
						then to make this active configure this property in application.properties  "spring.profiles.active=dev"

						application.properties:
							logging.level.org.springframework=trace
							spring.profiles.active=dev

						application-dev.properties:
							logging.level.org.springframework=debug

# Configuration properties: 
  * we can provide value to fields of the class through application.properties using below codes, we can provide value   "defaultkey" to key field of CurrencyServiceConfiguration class object
  * @ConfigurationProperties: The @ConfigurationProperties annotation in Spring Boot is used to bind and map external configuration properties (usually defined in application.properties or application.yml) to Java objects
  * @Component and @Autowired: @Component tells the spring to create bean automatically while scanning and @Autowired injects that bean to configuration field of CurrencyConfigurationController class.
								


### /src/main/java/com/in28minutes/springboot/learnspringboot/CurrencyConfigurationController.java

```java
package com.in28minutes.springboot.learnspringboot;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CurrencyConfigurationController {
	
	@Autowired
	private CurrencyServiceConfiguration configuration;
	
	@RequestMapping("/currency-configuration")
	public CurrencyServiceConfiguration retrieveAllCourses() {
		return configuration;
	}

}
```
---

### /src/main/java/com/in28minutes/springboot/learnspringboot/CurrencyServiceConfiguration.java

```java
package com.in28minutes.springboot.learnspringboot;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

//currency-service.url=
//currency-service.username=
//currency-service.key=

@ConfigurationProperties(prefix = "currency-service")
@Component
public class CurrencyServiceConfiguration {

	private String url;
	private String username;
	private String key;

	public String getUrl() {
		return url;
	}

	public void setUrl(String url) {
		this.url = url;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getKey() {
		return key;
	}

	public void setKey(String key) {
		this.key = key;
	}

}
```

### /src/main/resources/application.properties

```properties

currency-service.url=http://default1.in28minutes.com
currency-service.username=defaultusername
currency-service.key=defaultkey


-> Embedded servers: Earlier for deployment, we need to install java -> web server(tomcat) -> build war ->  install war file
                     Now, install java -> build jar file of project -> run jar file "java -jar name.jar"

-> Actuator: it provides monitoring features for production
             run "localhost:8080/actuator", it provides link for monitoring different things e.g health
             To get all monitoring links, add "management.endpoints.web.exposure.include=*" in application.properties
             But its not recommended to enable all because it will take more cpu and ram
             So, enable only those required e.g management.endpoints.web.exposure.include=health,metrics