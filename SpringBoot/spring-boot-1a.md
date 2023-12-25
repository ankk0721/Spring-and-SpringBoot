# Before SpringBoot
  * we need to manage dependencies and framwork versions manually. e.g spring mvc, junit,etc.
  * manage web.xml
  * spring configration e.g define component scan, view resolver, etc.
  * we need to do Logging, error handling, monitoring manually.
  * have to do for each project and it takes lots of time.

# Spring initializr
  * start.spring.io
  * here we can create spring boot application with requuired dependencies

# @SpringBootApplication
  * The @SpringBootApplication annotation in Spring Boot is used to mark a class as the main application class
  * It combines three other annotations:
  * @Configuration: Indicates that the class contains Spring bean definitions.
  * @EnableAutoConfiguration: Enables Spring Boot's automatic configuration based on the project's dependencies and settings.
  * @ComponentScan: Instructs Spring to scan and discover beans (components, services, configurations, etc.) automatically.

# @RestController
  * The @RestController annotation in Spring Boot is used to mark a class as a RESTful controller where the methods return data directly as the response body (rather than relying on a view for rendering).
  * This annotation combines @Controller and @ResponseBody.
  * @Controller: In Spring MVC, the @Controller annotation is used to mark a class as a controller that handles HTTP requests
  * @ResponseBody: The @ResponseBody annotation in Spring MVC is used to indicate that the return value of a method should be bound directly to the response body of the HTTP response.

# @RequestMapping("courses")
  * used to map web requests to handler methods in a controller


# Hello world API with springboot
  
      ### Course.java

          ```java

          package com.example.HelloWorldAPI;

          public class Course {
              private String name;
              private int id;

              public Course(String name, int id) {
                  this.name = name;
                  this.id = id;
              }

              public String getName() {
                  return name;
              }

              public int getId() {
                  return id;
              }

              @Override
              public String toString() {
                  return "Course [name=" + name + ", id=" + id + "]";
              }

          }
          ```
      
      ### CourseController.java
          
  ```java

  package com.example.HelloWorldAPI;

  import java.util.Arrays;
  import java.util.List;

  import org.springframework.web.bind.annotation.RestController;
  import org.springframework.web.bind.annotation.RequestMapping;

  @RestController
  public class CourseController {

      @RequestMapping("courses")
      public List<Course> retrieveAllCourses() {
          return Arrays.asList(
                  new Course("abc", 1),
                  new Course("asd", 2));
      }

  }
  ```
    
      ### HelloWorldApiApplication.java

          ```java

          package com.example.HelloWorldAPI;

          import org.springframework.boot.SpringApplication;
          import org.springframework.boot.autoconfigure.SpringBootApplication;

          @SpringBootApplication
          public class HelloWorldApiApplication {

            public static void main(String[] args) {
              SpringApplication.run(HelloWorldApiApplication.class, args);
            }

          }

          ```
      