# Student Management REST API

A simple Student Management REST API built with Spring Boot 3+ that provides CRUD operations for managing student records.

## Features

- ✅ Add Student (POST)
- ✅ Get All Students (GET)
- ✅ Get Student By ID (GET)
- ✅ Update Student (PUT)
- ✅ Delete Student (DELETE)
- ✅ Service Layer Implementation
- ✅ Global Exception Handling
- ✅ Input Validation
- ✅ MySQL Database Integration

## Technologies Used

- **Spring Boot 3.5.7**
- **Spring Data JPA**
- **Spring Validation**
- **MySQL Database**
- **Lombok**
- **Maven**

## Prerequisites

Before running this application, ensure you have:

- Java 17 or higher
- Maven 3.6+
- XAMPP (for MySQL)
- Postman

## Database Setup

1. **Start XAMPP and MySQL Server**
   - Open XAMPP Control Panel
   - Start Apache and MySQL services

2. **Create Database**
   - Open phpMyAdmin (http://localhost/phpmyadmin)
   - Create a new database named `student_db`
   - Or run the SQL file:
   ```sql
   mysql -u root -p < student_db.sql
   ```

3. **Database Configuration**
   - MySQL port: 3306
   - Username: student_user
   - Password: admin
   - Database name: studentdb

## Running the Application

### Option 1: Using Maven Wrapper (Recommended)

```bash
# On macOS/Linux
./mvnw clean install
./mvnw spring-boot:run

# On Windows
mvnw.cmd clean install
mvnw.cmd spring-boot:run
```

### Option 2: Using Maven

```bash
mvn clean install
mvn spring-boot:run
```

The application will start on **http://localhost:8080**

## API Endpoints

### Base URL
```
http://localhost:8080/api/students
```

### 1. Add Student
**Endpoint:** `POST /api/students`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "course": "Computer Science",
  "age": 20
}
```

**Response:** `201 Created`
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "course": "Computer Science",
  "age": 20
}
```

### 2. Get All Students
**Endpoint:** `GET /api/students`

**Response:** `200 OK`
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "course": "Computer Science",
    "age": 20
  },
  {
    "id": 2,
    "name": "Jane Smith",
    "email": "jane.smith@example.com",
    "course": "Information Technology",
    "age": 22
  }
]
```

### 3. Get Student By ID
**Endpoint:** `GET /api/students/{id}`

**Example:** `GET /api/students/1`

**Response:** `200 OK`
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "course": "Computer Science",
  "age": 20
}
```

**Error Response:** `404 Not Found`
```json
{
  "timestamp": "2025-11-10T23:30:00",
  "status": 404,
  "error": "Not Found",
  "message": "Student not found with id: 1",
  "path": "/api/students/1"
}
```

### 4. Update Student
**Endpoint:** `PUT /api/students/{id}`

**Example:** `PUT /api/students/1`

**Request Body:**
```json
{
  "name": "John Updated",
  "email": "john.updated@example.com",
  "course": "Software Engineering",
  "age": 21
}
```

**Response:** `200 OK`
```json
{
  "id": 1,
  "name": "John Updated",
  "email": "john.updated@example.com",
  "course": "Software Engineering",
  "age": 21
}
```

### 5. Delete Student
**Endpoint:** `DELETE /api/students/{id}`

**Example:** `DELETE /api/students/1`

**Response:** `204 No Content`

## Validation Rules

The API enforces the following validation rules:

- **Name:** Required, cannot be blank
- **Email:** Required, must be valid email format, unique
- **Course:** Required, cannot be blank
- **Age:** Required, must be greater than 18

### Validation Error Response
**Response:** `400 Bad Request`
```json
{
  "timestamp": "2025-11-10T23:30:00",
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/students",
  "validationErrors": {
    "email": "Email should be valid",
    "age": "Age must be greater than 18"
  }
}
```

## Testing with Postman

1. **Import the collection or create requests manually**
2. **Set the base URL:** `http://localhost:8080/api/students`
3. **Test each endpoint:**
   - POST: Create a new student
   - GET: Retrieve all students
   - GET by ID: Retrieve specific student
   - PUT: Update a student
   - DELETE: Remove a student

## Bonus Features (+10 Marks)

### 1. Swagger UI API Documentation
Access interactive API documentation at: **http://localhost:8080/swagger-ui.html**

- View all endpoints
- Test API directly from browser
- See request/response schemas

### 2. Pagination & Sorting
**Endpoint:** `GET /api/students/paginated`

**Parameters:**
- `page` - Page number (0-based), default: 0
- `size` - Items per page, default: 10
- `sortBy` - Field to sort by (name, age, email, course), default: id
- `direction` - Sort direction (asc/desc), default: asc

**Example:**
```bash
GET /api/students/paginated?page=0&size=5&sortBy=name&direction=asc
```

### 3. Search Functionality
Search students by name or course with pagination:

**Search by Name:**
```bash
GET /api/students/search/name?name=John&page=0&size=10
```

**Search by Course:**
```bash
GET /api/students/search/course?course=Computer&page=0&size=10
```

**Search by Keyword (Name or Course):**
```bash
GET /api/students/search?keyword=software&page=0&size=10
```

### 4. Docker Support
Run the application using Docker:

```bash
# Build and run with Docker Compose
docker-compose up -d

# Stop
docker-compose down
```

## Project Structure

```
src/
├── main/
│   ├── java/
│   │   └── com/example/student_api/
│   │       ├── StudentApiApplication.java
│   │       ├── config/
│   │       │   └── OpenAPIConfig.java
│   │       ├── controller/
│   │       │   └── StudentController.java
│   │       ├── service/
│   │       │   └── StudentService.java
│   │       ├── repository/
│   │       │   └── StudentRepository.java
│   │       ├── entity/
│   │       │   └── Student.java
│   │       └── exception/
│   │           ├── GlobalExceptionHandler.java
│   │           ├── ResourceNotFoundException.java
│   │           └── ErrorResponse.java
│   └── resources/
│       └── application.properties
└── test/
    └── java/
        └── com/example/student_api/
            └── StudentApiApplicationTests.java
```

## Error Handling

The API implements global exception handling with proper HTTP status codes:

- **200 OK** - Successful GET/PUT request
- **201 Created** - Successful POST request
- **204 No Content** - Successful DELETE request
- **400 Bad Request** - Validation errors
- **404 Not Found** - Resource not found
- **500 Internal Server Error** - Server errors






