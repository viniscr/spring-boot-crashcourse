# Spring Boot Course - Software Engineer Management API

A RESTful API built with Spring Boot 4.0.5 for managing software engineer profiles, including their tech stacks. The API integrates with PostgreSQL and provides full CRUD operations.

## Overview

This project demonstrates core Spring Boot concepts including:
- **REST API Development** with Spring Web MVC
- **Database Integration** with Spring Data JPA and PostgreSQL
- **Entity Management** with automatic schema generation (Hibernate DDL)
- **Dependency Injection** and component lifecycle
- **RESTful CRUD Operations** (Create, Read, Update, Delete)

## Project Structure

```
src/
├── main/
│   ├── java/com/vinicius/
│   │   ├── SpringBootCourseApplication.java      # Main application entry point
│   │   ├── SoftwareEngineer.java                 # Entity model
│   │   ├── SoftwareEngineerController.java       # REST controller
│   │   ├── SoftwareEngineerService.java          # Business logic service
│   │   └── SoftwareEngineerRepository.java       # Database repository (JPA)
│   └── resources/
│       └── application.properties                 # Application configuration
├── test/
│   └── SpringBootCourseApplicationTests.java     # Integration tests
├── docker-compose.yml                            # PostgreSQL container setup
└── pom.xml                                       # Maven build configuration
```

## Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| **Spring Boot Starter Web MVC** | 4.0.5 | REST API framework and web support |
| **Spring Boot Starter Data JPA** | 4.0.5 | Object-relational mapping and database access |
| **PostgreSQL Driver** | (latest) | Database connection driver |
| **Spring Boot Starter Test** | 4.0.5 | Unit and integration testing framework |
| **Java** | 25 | Minimum required Java version |

## Prerequisites

- **Java 25** or higher
- **Maven** 3.6+
- **Docker** and **Docker Compose** (for PostgreSQL)

## Quick Start

### 1. Start PostgreSQL with Docker Compose

```bash
docker compose up -d
```

This starts a PostgreSQL container on port `5332` with:
- Database: `amigos` (auto-created by Spring)
- Credentials: configured via environment variables (see Configuration section below)

### 2. Build the Project

```bash
./mvnw clean install
```

### 3. Run the Application

```bash
./mvnw spring-boot:run
```

The API will start on `http://localhost:8080`

## Configuration

Edit `src/main/resources/application.properties` to customize:

```properties
# Server & App Settings
spring.application.name=spring-boot-course

# PostgreSQL Configuration
spring.datasource.url=jdbc:postgresql://localhost:5332/amigos
spring.datasource.username=${DB_USER:admin}              # Set DB_USER env var or defaults to 'admin'
spring.datasource.password=${DB_PASSWORD:admin}          # Set DB_PASSWORD env var or defaults to 'admin'
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA/Hibernate Configuration
spring.jpa.hibernate.ddl-auto=create-drop  # Auto-creates/drops tables on startup/shutdown
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.show-sql=true
```

### Environment Variables

The database credentials use environment variables for security:

**On Linux/Mac:**
```bash
export DB_USER=your_username
export DB_PASSWORD=your_password
./mvnw spring-boot:run
```

**On Windows (CMD):**
```batch
set DB_USER=your_username
set DB_PASSWORD=your_password
mvnw spring-boot:run
```

**Or set them inline (Linux/Mac):**
```bash
DB_USER=admin DB_PASSWORD=admin ./mvnw spring-boot:run
```

If environment variables are not set, the defaults (`admin`/`admin`) are used.

## API Endpoints

All endpoints are prefixed with `/api/v1/software-engineers`

### GET - Retrieve All Engineers

```bash
curl -X GET http://localhost:8080/api/v1/software-engineers
```

**Response:** `200 OK` with list of all engineers

---

### GET - Retrieve Single Engineer by ID

```bash
curl -X GET http://localhost:8080/api/v1/software-engineers/1
```

**Response:** `200 OK` with engineer details  
**Error:** `500 Internal Server Error` if ID not found

---

### POST - Create New Engineer

```bash
curl -X POST http://localhost:8080/api/v1/software-engineers \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "techStack": "Java, Spring Boot, PostgreSQL"
  }'
```

**Response:** `200 OK` (engineer saved to database)

---

### PUT - Update Engineer

```bash
curl -X PUT http://localhost:8080/api/v1/software-engineers/1 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "techStack": "Java, Spring Boot, PostgreSQL, Docker"
  }'
```

**Response:** `200 OK`  
**Error:** `500 Internal Server Error` if ID not found

---

### DELETE - Remove Engineer

```bash
curl -X DELETE http://localhost:8080/api/v1/software-engineers/1
```

**Response:** `204 No Content` (successful deletion)  
**Error:** `500 Internal Server Error` if ID not found

## Entity Model

### SoftwareEngineer

```java
@Entity
public class SoftwareEngineer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;           // Auto-incremented primary key
    private String name;          // Engineer's full name
    private String techStack;     // Comma-separated tech skills
}
```

## Running Tests

```bash
./mvnw test
```

Runs all unit and integration tests in `src/test/`

## Building for Production

```bash
./mvnw clean package
java -jar target/spring-boot-course-0.0.1-SNAPSHOT.jar
```

## Development Tips

- **Hot Reload:** Add `spring-boot-devtools` dependency for automatic restart on file changes
- **Database Console:** Spring doesn't include H2 console by default; use PostgreSQL tools or DBeaver for inspection
- **Logging:** Adjust log levels in `application.properties`:
  ```properties
  logging.level.org.springframework=DEBUG
  logging.level.org.hibernate.SQL=DEBUG
  ```

## Troubleshooting

### PostgreSQL Connection Refused
Ensure Docker container is running:
```bash
docker compose ps
docker compose logs db
```

### Port Already in Use
Change port in `application.properties` or stop conflicting process:
```bash
lsof -i :8080
kill -9 <PID>
```

### Schema Mismatch
With `ddl-auto=create-drop`, tables are recreated on startup. Existing data will be lost. Switch to `validate` or `update` in production.

## Future Enhancements

- Integration with AI service for learning path recommendations (commented in `SoftwareEngineerService.java`)
- Global exception handling with `@ControllerAdvice`
- Input validation with Jakarta Bean Validation
- API documentation with Springdoc OpenAPI/Swagger
- Authentication & authorization with Spring Security

## License

Open source - feel free to modify and distribute.

## Author

Vinicius - Spring Boot Course Project



