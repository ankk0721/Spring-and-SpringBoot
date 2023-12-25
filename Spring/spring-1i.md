# Dependency Injection
	* Dependency injection (DI) in Spring is a powerful technique used to achieve loose coupling between components in a software application
	* In this spring identify beans, their dependencies and then wired them together.
	* Also called IOC(Inversion of control)
	* There are three types of dependency injection: construction injection, getter-setter injection
      , field injection
	* @Autowired annotation is used for dependency injection
	* Field injection: add @Autowired on instance variable
	* getter-setter: create getter and setter methods and ad @Autowired on setter methods
	* constructor: add @Autowired on constructor

	* Autowiring @Autowired: In which spring injects the dependencies automatically. It cann't be used with primitive and string values.
	         Work only with refernce values. 


				### SimpleSpringContextLauncherApplication.java

				```java
				package com.in28minutes.learnspringframework.examples.a0;

				import java.util.Arrays;

				import org.springframework.context.annotation.AnnotationConfigApplicationContext;
				import org.springframework.context.annotation.ComponentScan;
				import org.springframework.context.annotation.Configuration;

				@Configuration
				@ComponentScan
				public class SimpleSpringContextLauncherApplication {
					
					public static void main(String[] args) {

						try (var context = 
								new AnnotationConfigApplicationContext
									(SimpleSpringContextLauncherApplication.class)) {
							
							Arrays.stream(context.getBeanDefinitionNames())
								.forEach(System.out::println);

						}
					}
				}
				```
				---

				### DepInjectionLauncherApplication.java

				```java
				package com.in28minutes.learnspringframework.examples.a1;

				import java.util.Arrays;

				import org.springframework.beans.factory.annotation.Autowired;
				import org.springframework.context.annotation.AnnotationConfigApplicationContext;
				import org.springframework.context.annotation.ComponentScan;
				import org.springframework.context.annotation.Configuration;
				import org.springframework.stereotype.Component;

				@Component
				class YourBusinessClass {

					//@Autowired - Field injection 
					Dependency1 dependency1;
					//@Autowired
					Dependency2 dependency2;

					//@Autowired - constructor injection
					public YourBusinessClass(Dependency1 dependency1, Dependency2 dependency2) {
						super();
						System.out.println("Constructor Injection - YourBusinessClass ");
						this.dependency1 = dependency1;
						this.dependency2 = dependency2;
					}

				//	@Autowired -getter setter injection
				//	public void setDependency1(Dependency1 dependency1) {
				//		System.out.println("Setter Injection - setDependency1 ");
				//		this.dependency1 = dependency1;
				//	}
				//
				//	@Autowired
				//	public void setDependency2(Dependency2 dependency2) {
				//		System.out.println("Setter Injection - setDependency2 ");
				//		this.dependency2 = dependency2;
				//	}

					public String toString() {
						return "Using " + dependency1 + " and " + dependency2;
					}

				}

				@Component
				class Dependency1 {

				}

				@Component
				class Dependency2 {

				}

				@Configuration
				@ComponentScan
				public class DepInjectionLauncherApplication {

					public static void main(String[] args) {

						try (var context = new AnnotationConfigApplicationContext(DepInjectionLauncherApplication.class)) {

							Arrays.stream(context.getBeanDefinitionNames()).forEach(System.out::println);

							System.out.println(context.getBean(YourBusinessClass.class));

						}
					}
				}
				```
