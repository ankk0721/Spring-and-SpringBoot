# Starting REST API for social Media App
  * Resources:
          Users: id,name,birthDate
		  Posts: id,description
  * GET: Retrieve details of a resource
  * POST: Create a new resource
  * PUT: Update a aexisting resource
  * PATCH: Update part of a resource e.g update only name of existing user
  * DELETE: delete a resource

  * Users REST API
    -> Retrieve all users: GET /users
	-> create user: POST /users
	-> retrieve one user: GET /users/{id}
	-> delete user: DELETE /users/{id}
  * Posts REST API
    -> retrieve all posts of a user: GET /users/{id}/posts
	-> create a post for a user: POST /users/{id}/posts
	-> retrieve detail of a post: GET /users/{id}/posts/{post_id}


# DAO: Data Access Object
  * Data Access Object (DAO) is a design pattern used to provide an abstract interface to a database or other persistence 	 mechanisms. 
  * The primary purpose of a DAO is to separate the business logic from the data storage and retrieval operations.

### user/User.java

```java
package com.in28minutes.rest.webservices.restfulwebservices.user;

import java.time.LocalDate;

public class User {
	
	private Integer id;
	private String name;
	private LocalDate birthDate;
	
	public User(Integer id, String name, LocalDate birthDate) {
		super();
		this.id = id;
		this.name = name;
		this.birthDate = birthDate;
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public LocalDate getBirthDate() {
		return birthDate;
	}

	public void setBirthDate(LocalDate birthDate) {
		this.birthDate = birthDate;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", birthDate=" + birthDate + "]";
	}
	
}


```
---

### user/UserDaoService.java

```java
package com.in28minutes.rest.webservices.restfulwebservices.user;

import java.time.LocalDate;
import java.util.ArrayList;
import java.util.List;

import org.springframework.stereotype.Component;

@Component
public class UserDaoService {
	// JPA/Hibernate > Database
	// UserDaoService > Static List
	
	private static List<User> users = new ArrayList<>();
	
	static {
		users.add(new User(1,"Adam",LocalDate.now().minusYears(30)));
		users.add(new User(2,"Eve",LocalDate.now().minusYears(25)));
		users.add(new User(3,"Jim",LocalDate.now().minusYears(20)));
	}
	
	public List<User> findAll() {
		return users;
	}

}
```
---


