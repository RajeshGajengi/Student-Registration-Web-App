# Database Integration with Application
below are the chnages we have to do in application for databse integration,

#### 1. Add dependencies to pom.xml:
```bash
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
  <groupId>com.mysql</groupId>
  <artifactId>mysql-connector-j</artifactId>
  <scope>runtime</scope>
</dependency>

```
#### 2. Update StudentRegistrationBackendApplicationTests.java
```
nano src/test/java/com/student/registration/student_registration_backend/StudentRegistrationBackendApplicationTests.java
```
clear all code from this file, and paste below code
```
	package com.student.registration.student_registration_backend;
	
	import org.junit.jupiter.api.Test;
	import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
	import org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration;
	import org.springframework.boot.test.context.SpringBootTest;
	
	@SpringBootTest
	class StudentRegistrationBackendApplicationTests {
	
	        @Test
	        void contextLoads() {
	        }
	
	}
```

#### 3. Update StudentRegistrationBackendApplication.java
```
src/main/java/com/student/registration/student_registration_backend/StudentRegistrationBackendApplication.java
```
Paste below code,
```
	package com.student.registration.student_registration_backend;
	
	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	import org.springframework.boot.autoconfigure.domain.EntityScan;
	import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
	
	@SpringBootApplication
	@EnableJpaRepositories(basePackages = "com.student.registration.student_registration_backend.repository")
	@EntityScan(basePackages = "com.student.registration.student_registration_backend.model")
	public class StudentRegistrationBackendApplication {
	
	        public static void main(String[] args) {
	                SpringApplication.run(StudentRegistrationBackendApplication.class, args);
	        }
	
	}

```

#### 4. Update UserController.Java 
)
```
nano src/main/java/com/student/registration/student_registration_backend/controller/UserController.java
```
Paste below code
```
package com.student.registration.student_registration_backend.controller;

import com.student.registration.student_registration_backend.model.User;
import com.student.registration.student_registration_backend.repository.UserRepository;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api")
public class UserController {

    private final UserRepository userRepository;

    public UserController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Register user -> save in DB
    @PostMapping("/register")
    public User registerUser(@RequestBody User user) {
        return userRepository.save(user);
    }

    // Get all users -> fetch from DB
    @GetMapping("/users")
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    // Delete user by ID
    @DeleteMapping("/users/{id}")
    public ResponseEntity<String> deleteUser(@PathVariable Long id) {
        if (userRepository.existsById(id)) {
            userRepository.deleteById(id);
            return ResponseEntity.ok("User deleted successfully");
        } else {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body("User not found");
        }
    }
}
```
#### 5. Update User.java
```
nano src/main/java/com/student/registration/student_registration_backend/model/User.java
```
Paste below code
```
package com.student.registration.student_registration_backend.model;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import lombok.Data;
import jakarta.persistence.Table; 
import java.math.BigDecimal;
import jakarta.persistence.Column;


@Entity // This annotation from Lombok automatically generates getters, setters, and other methods.
@Table(name = "users")   // table name in DB
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String email;
    private String course;
    private String studentClass;

    @Column(precision = 5, scale = 2)  // e.g. 99.99
    private BigDecimal percentage;
    private String branch;
    private String mobileNumber;
}

```
**Note**: Make sure in Database, run below command 
```sql
 ALTER TABLE users MODIFY COLUMN percentage DECIMAL(5,2);
```

#### 6. Create Repository directory and UserRepository.java file
```
mkdir src/main/java/com/student/registration/student_registration_backend/repository
nano src/main/java/com/student/registration/student_registration_backend/repository/UserRepository.java
```
paste below code,
```
package com.student.registration.student_registration_backend.repository;

import com.student.registration.student_registration_backend.model.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```



#### 5. Update src/main/resources/application.properties

```
# Database Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/student_db
spring.datasource.username=admin
spring.datasource.password=rajesh
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.properties.hibernate.format_sql=true

# Connection Pool
spring.datasource.hikari.maximum-pool-size=10
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.idle-timeout=300000
```








