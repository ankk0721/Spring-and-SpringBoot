# H2 dataabase
	* It is memory database(stored data in RAM)
	* not persist data(data removed when system restart)
	* only for development
	* Properties required 
	spring.h2.console.enabled=true // it enables h2 console, we can access using " localhost:8080/h2-console"
	spring.datasource.url=jdbc:h2:mem:testdb // provide custom url, and we used it inside h2-console
	spring.jpa.show-sql=true // it shows how query is executing in logs
	* create schema.sql inside resources
	* provide schema of all tables inside this 
	* e.g create table course 
		(
			id bigint not null,
			name varchar(255) not null,
			author varchar(255) not null,
			primary key (id)
			);
	* spring boot will automatically create that tables in h2


# Spring JDBC
  * JDBC: write lot of sql queries and also lot of java code
  * Spring JDBC: lot of sql queries but less java code
  * @Repository: Its a specialization of component and used to annonate class which access database or perform database oprrations
  * CommandLineRunner: It is an interface that allows a Spring Boot application to execute specific pieces of code when the application starts up


				### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/Course.java

				```java
				package com.in28minutes.springboot.learnjpaandhibernate.course;

				public class Course {
					
					private long id;
					
					private String name;
					
					private String author;

					public Course() {

					}

					public Course(long id, String name, String author) {
						super();
						this.id = id;
						this.name = name;
						this.author = author;
					}

					public long getId() {
						return id;
					}

					public String getName() {
						return name;
					}

					public String getAuthor() {
						return author;
					}

					public void setId(long id) {
						this.id = id;
					}

					public void setName(String name) {
						this.name = name;
					}

					public void setAuthor(String author) {
						this.author = author;
					}

					@Override
					public String toString() {
						return "Course [id=" + id + ", name=" + name + ", author=" + author + "]";
					}

					// constructor
					// getters
					// toString

				}
				```
				---

				### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/CourseCommandLineRunner.java

				```java
				package com.in28minutes.springboot.learnjpaandhibernate.course;

				import org.springframework.beans.factory.annotation.Autowired;
				import org.springframework.boot.CommandLineRunner;
				import org.springframework.stereotype.Component;

				import com.in28minutes.springboot.learnjpaandhibernate.course.jpa.CourseJpaRepository;
				import com.in28minutes.springboot.learnjpaandhibernate.course.springdatajpa.CourseSpringDataJpaRepository;

				@Component
				public class CourseCommandLineRunner implements CommandLineRunner{

					@Autowired
					private CourseJdbcRepository repository;

					
					@Override
					public void run(String... args) throws Exception {
						repository.insert(new Course(1, "Learn AWS Jpa!", "in28minutes"));
						repository.insert(new Course(2, "Learn Azure Jpa!", "in28minutes"));
						repository.insert(new Course(3, "Learn DevOps Jpa!", "in28minutes"));
						
						repository.deleteById(1);
						
						System.out.println(repository.findById(2));
						System.out.println(repository.findById(3));

						
					}

				}
				```
				---

				### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/jdbc/CourseJdbcRepository.java

				```java
				package com.in28minutes.springboot.learnjpaandhibernate.course.jdbc;

				import org.springframework.beans.factory.annotation.Autowired;
				import org.springframework.jdbc.core.BeanPropertyRowMapper;
				import org.springframework.jdbc.core.JdbcTemplate;
				import org.springframework.stereotype.Repository;

				import com.in28minutes.springboot.learnjpaandhibernate.course.Course;

				@Repository
				public class CourseJdbcRepository {
					
					@Autowired
					private JdbcTemplate springJdbcTemplate;
					
					private static String INSERT_QUERY = 
							
							"""
								insert into course (id, name, author)
								values(?, ?,?);
					
							""";

					private static String DELETE_QUERY = 
							
							"""
								delete from course
								where id = ?
					
							""";

					private static String SELECT_QUERY = 
							
							"""
								select * from course
								where id = ?
					
							""";
					
					public void insert(Course course) {
						springJdbcTemplate.update(INSERT_QUERY, 
								course.getId(), course.getName(), course.getAuthor());
					}
					
					public void deleteById(long id) {
						springJdbcTemplate.update(DELETE_QUERY,id);
					}
					
					public Course findById(long id) {
						//ResultSet * Bean => Row Mapper => 
						return springJdbcTemplate.queryForObject(SELECT_QUERY,
								new BeanPropertyRowMapper<>(Course.class), id);
						
					}

				}
				```




# JPA
  * In JPA, we are mapping class(bean) directly to table present in database
  * no sql queries required
  * first create @Entity: Entity class corresponds to a table in the database, where each instance of the entity class represents a row in that table.
  * @Id: makes that field as PRIMARY KEY
  * @Column(name=""): map that field to column, if we not provide this then it will use field name
  * @Entity(name="table_name"): If we not provide the table name then it will use class name 
  * EntityManager: The EntityManager in JPA is a central interface responsible for managing entities within the persistence context
  * @PersistenceContext: similar to @Autowired but specific for entity manager
  * @Transactional: add when we have to execute queries

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/Course.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course;

			import jakarta.persistence.Entity;
			import jakarta.persistence.Id;

			@Entity
			public class Course {
				
				@Id
				private long id;
				
				private String name;
				@Column(name="author")
				private String author;

				public Course() {

				}

				public Course(long id, String name, String author) {
					super();
					this.id = id;
					this.name = name;
					this.author = author;
				}

				public long getId() {
					return id;
				}

				public String getName() {
					return name;
				}

				public String getAuthor() {
					return author;
				}

				public void setId(long id) {
					this.id = id;
				}

				public void setName(String name) {
					this.name = name;
				}

				public void setAuthor(String author) {
					this.author = author;
				}

				@Override
				public String toString() {
					return "Course [id=" + id + ", name=" + name + ", author=" + author + "]";
				}

				// constructor
				// getters
				// toString

			}
			```
			---

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/CourseCommandLineRunner.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course;

			import org.springframework.beans.factory.annotation.Autowired;
			import org.springframework.boot.CommandLineRunner;
			import org.springframework.stereotype.Component;

			import com.in28minutes.springboot.learnjpaandhibernate.course.jpa.CourseJpaRepository;
			import com.in28minutes.springboot.learnjpaandhibernate.course.springdatajpa.CourseSpringDataJpaRepository;

			@Component
			public class CourseCommandLineRunner implements CommandLineRunner{

				@Autowired
				private CourseJpaRepository repository;

				
				@Override
				public void run(String... args) throws Exception {
					repository.insert(new Course(1, "Learn AWS Jpa!", "in28minutes"));
					repository.insert(new Course(2, "Learn Azure Jpa!", "in28minutes"));
					repository.insert(new Course(3, "Learn DevOps Jpa!", "in28minutes"));
					
					repository.deleteById(1l);
					
					System.out.println(repository.findById(2l));
					System.out.println(repository.findById(3l));

					
				}

			}
			```

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/jpa/CourseJpaRepository.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course.jpa;

			import org.springframework.stereotype.Repository;

			import com.in28minutes.springboot.learnjpaandhibernate.course.Course;

			import jakarta.persistence.EntityManager;
			import jakarta.persistence.PersistenceContext;
			import jakarta.transaction.Transactional;

			@Repository
			@Transactional
			public class CourseJpaRepository {
				
				@PersistenceContext
				private EntityManager entityManager;
				
				public void insert(Course course) {
					entityManager.merge(course);  // merge for insert
				}
				
				public Course findById(long id) {
					return entityManager.find(Course.class, id);
				}

				public void deleteById(long id) {
					Course course = entityManager.find(Course.class, id);
					entityManager.remove(course);
				}

			}
			```



# Spring Data JPA
  * make JPA more simple
  * only Entity required
  * No need to use EntityManager, its automatically running in background
  * lots of methods predefined
  * create interface and extends JpaRepository<Entity,ID_Type>
  * JPA defines specification. Its an API. Hibernates implements JPA.

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/Course.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course;

			import jakarta.persistence.Entity;
			import jakarta.persistence.Id;

			@Entity
			public class Course {
				
				@Id
				private long id;
				
				private String name;
				
				private String author;

				public Course() {

				}

				public Course(long id, String name, String author) {
					super();
					this.id = id;
					this.name = name;
					this.author = author;
				}

				public long getId() {
					return id;
				}

				public String getName() {
					return name;
				}

				public String getAuthor() {
					return author;
				}

				public void setId(long id) {
					this.id = id;
				}

				public void setName(String name) {
					this.name = name;
				}

				public void setAuthor(String author) {
					this.author = author;
				}

				@Override
				public String toString() {
					return "Course [id=" + id + ", name=" + name + ", author=" + author + "]";
				}

				// constructor
				// getters
				// toString

			}
			```
			---

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/CourseCommandLineRunner.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course;

			import org.springframework.beans.factory.annotation.Autowired;
			import org.springframework.boot.CommandLineRunner;
			import org.springframework.stereotype.Component;

			import com.in28minutes.springboot.learnjpaandhibernate.course.jpa.CourseJpaRepository;
			import com.in28minutes.springboot.learnjpaandhibernate.course.springdatajpa.CourseSpringDataJpaRepository;

			@Component
			public class CourseCommandLineRunner implements CommandLineRunner{


				@Autowired
				private CourseSpringDataJpaRepository repository;
				
				@Override
				public void run(String... args) throws Exception {
					repository.save(new Course(1, "Learn AWS Jpa!", "in28minutes"));
					repository.save(new Course(2, "Learn Azure Jpa!", "in28minutes"));
					repository.save(new Course(3, "Learn DevOps Jpa!", "in28minutes"));
					
					repository.deleteById(1l);
					
					System.out.println(repository.findById(2l));
					System.out.println(repository.findById(3l));
					
					System.out.println(repository.findAll());
					System.out.println(repository.count());
					
					System.out.println(repository.findByAuthor("in28minutes"));
					System.out.println(repository.findByAuthor(""));

					System.out.println(repository.findByName("Learn AWS Jpa!"));
					System.out.println(repository.findByName("Learn DevOps Jpa!"));

					
				}

			}
			```

			### /src/main/java/com/in28minutes/springboot/learnjpaandhibernate/course/springdatajpa/CourseSpringDataJpaRepository.java

			```java
			package com.in28minutes.springboot.learnjpaandhibernate.course.springdatajpa;

			import java.util.List;

			import org.springframework.data.jpa.repository.JpaRepository;

			import com.in28minutes.springboot.learnjpaandhibernate.course.Course;

			public interface CourseSpringDataJpaRepository extends JpaRepository<Course, Long>{
				
				List<Course> findByAuthor(String author); // we can cretae new methods just by using naming convention
				List<Course> findByName(String name);

			}
			```