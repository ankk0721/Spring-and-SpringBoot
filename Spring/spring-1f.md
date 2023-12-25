# Spring Container
  * manages spring beans and their lifecycle
  * takes input as configuration class(class specified with @configuration) and POJOs(normal java class)
  * also called spring context
  * It gives running system as output
  * Types:
     -> Bean Factory: simple spring container
	 -> Application Context: advanced with enterprise specific features
# POJO
  * Plain old java object
  * any java object is a pojo

# JavaBean
  * Earlier, EJB introduced this concept and its mostly used earlier
  * zero parameter constructor should be there
  * getters and setters should be there
  * must implements serializable
  * not much used now

* context.getDefinitionBeans() return names of all available beans as string array

# SpringBean
  * any java object managed by spring IOC
-> Spring create and manage beans(java objects)
-> For this, first combine Configuration class and Application class
-> Add @Component annotation to Bean class (GameRunner, MariGame, SuperContraGAme)
-> Then add @ComponentScan("package") on Application class
-> component scan first scan all the class present in that package and creates beans 
   for those class which have component scan annotation
-> pass same application.class to context

# Primary and qualifier
 * Primary: If multiple beans of same type are there, then we have to add @Primary annotaion on bean to give prefernce for autowiring
 * @Qualifier is used along with @Autowired to specify which exact bean should be injected when there are multiple beans of the same type.


				```java
				package com.in28minutes.learnspringframework;

				import java.util.Arrays;

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
						
						System.out.println(context.getBean("person2MethodCall"));
						
						System.out.println(context.getBean("person3Parameters"));
						
						System.out.println(context.getBean("address2"));
						
						System.out.println(context.getBean(Person.class));
						
						System.out.println(context.getBean(Address.class));
						
						System.out.println(context.getBean("person5Qualifier"));
						
						
						//System.out.println
				//		Arrays.stream(context.getBeanDefinitionNames())
				//			.forEach(System.out::println);
						
						
						
					}

				}
				```
				---

				### /learn-spring-framework/src/main/java/com/in28minutes/learnspringframework/HelloWorldConfiguration.java

				```java
				package com.in28minutes.learnspringframework;

				import org.springframework.beans.factory.annotation.Qualifier;
				import org.springframework.context.annotation.Bean;
				import org.springframework.context.annotation.Configuration;
				import org.springframework.context.annotation.Primary;

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

					@Bean
					@Primary
					//No qualifying bean of type 'com.in28minutes.learnspringframework.Address' 
					//available: expected single matching bean but found 2: address2,address3
					public Person person4Parameters(String name, int age, Address address) {
						//name,age,address2
						return new Person(name, age, address); //name, age		
					}

					@Bean
					public Person person5Qualifier(String name, int age, @Qualifier("address3qualifier") Address address) {
						//name,age,address2
						return new Person(name, age, address); //name, age		
					}

					
					@Bean(name = "address2")
					@Primary
					public Address address() {
						return new Address("Baker Street", "London");		
					}

					@Bean(name = "address3")
					@Qualifier("address3qualifier")
					public Address address3() {
						return new Address("Motinagar", "Hyderabad");		
					}

				}
				```
				---