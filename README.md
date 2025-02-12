# Learning Management System (LMS)

## Overview
This project is a Learning Management System (LMS) built using **Java with Spring Boot** following a **Layered Architecture**. The system supports role-based access control for **Admins, Instructors, and Students**, and provides functionalities for **User Management, Course Management, Assessments & Grading, Performance Tracking, and Notifications**.

---

## Architecture
The LMS follows a **Layered Architecture** pattern, which separates concerns into distinct layers:
- **Controller Layer**: Handles HTTP requests and responses using RESTful APIs.
- **Service Layer**: Contains business logic and application rules.
- **Repository Layer**: Manages data access using Spring Data JPA and PostgreSQL.
- **Entity Layer**: Defines database entity models.
- **DTO Layer**: Contains Data Transfer Objects for request/response models.
- **Mapper Layer**: Transforms data between entities and DTOs.
- **Security Layer**: Implements authentication and authorization using Spring Security.

---

## Features
### 1. User Management
- **User Roles**: 
  - **Admin**: Manages system settings, users, and courses.
  - **Instructor**: Creates and manages courses, assessments, and grading.
  - **Student**: Enrolls in courses, submits assignments, and takes quizzes.
- **Functionalities**:
  - User registration and login (JWT-based authentication).
  - Profile management (view/update profile information).

### 2. Course Management
- **Course Creation**:
  - Instructors can create courses with a title, description, duration, and media files.
  - Courses consist of multiple lessons.
- **Enrollment Management**:
  - Students can view and enroll in available courses.
  - Admins and Instructors can track enrolled students.
- **Attendance Management**:
  - Instructors generate OTPs for lesson attendance.
  - Students enter OTPs to mark attendance.

### 3. Assessments & Grading
- **Assessment Types**: Quizzes and Assignments.
- **Quiz Creation**:
  - Instructors create quizzes with MCQs, true/false, and short-answer questions.
  - Randomized question selection per quiz attempt.
- **Assignment Submission**:
  - Students upload assignments for review.
- **Grading & Feedback**:
  - Instructors grade assignments and provide feedback.
  - Students receive automated feedback for quizzes.

### 4. Performance Tracking
- Instructors track quiz scores, assignment submissions, and attendance.
- Admins and Instructors generate performance analytics and reports.

### 5. Notifications
- **System Notifications**:
  - Students receive notifications for enrollments, grades, and course updates.
  - Instructors receive notifications for student enrollments.
- **Email Notifications**:
  - Students receive email alerts for course-related updates.

---

## Technical Stack
### Backend
- **Java** with **Spring Boot** (for RESTful API services)
- **PostgreSQL** (database management)
- **Spring Security** (authentication & authorization)

### Testing
- **JUnit** for unit testing

---

## API Endpoints
### Authentication
| Method | Endpoint              | Description                                      |
|--------|----------------------|--------------------------------------------------|
| POST   | /api/auth/login      | Authenticate user and return JWT token.         |
| POST   | /api/auth/register   | Register a new user.                            |
| GET    | /api/auth/confirm    | Confirm user registration via token.            |

### Admin Management
| Method | Endpoint                       | Description                                 |
|--------|--------------------------------|---------------------------------------------|
| GET    | /api/admin/users              | Get all users (Admin only).                |
| PUT    | /api/admin/assign-role/{id}   | Assign role to user (Admin only).         |
| PUT    | /api/admin/deactivate/{id}    | Deactivate a user (Admin only).           |

### Course Management
| Method | Endpoint                                     | Description                                        |
|--------|---------------------------------------------|----------------------------------------------------|
| POST   | /api/instructor/course/create             | Create a new course (Instructor only).           |
| GET    | /api/instructor/courses                   | Get courses by instructor (Instructor only).     |
| DELETE | /api/instructor/course/{id}               | Delete a course (Instructor/Admin).              |
| POST   | /api/instructor/upload/course/{id}        | Upload media files for a course.                 |
| GET    | /api/student/courses                      | View available courses (Student).                |
| GET    | /api/student/get/course/{id}             | Get course details by ID.                        |
| POST   | /api/student/enroll/course/{id}          | Enroll in a course (Student).                    |

### Lesson Management
| Method | Endpoint                                           | Description                                |
|--------|---------------------------------------------------|--------------------------------------------|
| POST   | /api/instructor/lessons/course/{id}              | Add a lesson to a course.                 |
| POST   | /api/instructor/generate-otp/course/{id}/lessons/{id} | Generate OTP for lesson attendance. |
| POST   | /api/student/course/{id}/lessons/{id}/validate-otp | Validate OTP for lesson attendance. |

### Assignment Management
| Method | Endpoint                               | Description                                 |
|--------|---------------------------------------|---------------------------------------------|
| POST   | /api/instructor/assignment/create    | Create a new assignment (Instructor only). |
| GET    | /api/student/assignment/{id}        | Get assignment by ID (Student).            |
| GET    | /api/student/assignments            | Get all assignments (Student).             |
| PUT    | /api/instructor/assignment/{id}     | Update an assignment (Instructor only).    |
| DELETE | /api/instructor/assignment/{id}     | Delete an assignment (Instructor only).    |

### Enrollment Management
| Method | Endpoint                                          | Description                                |
|--------|--------------------------------------------------|--------------------------------------------|
| GET    | /api/instructor/enrollments/course/{id}        | Get enrolled students for a course.       |
| DELETE | /api/instructor/course/{id}/enrollments/{id}   | Remove student from a course.             |
| POST   | /api/instructor/enrollments/{id}/confirm      | Confirm student enrollment.               |

### Performance Tracking
| Method | Endpoint                                              | Description                                |
|--------|------------------------------------------------------|--------------------------------------------|
| GET    | /api/instructor/performance/course/{id}/student/{id} | Get student performance in a course.      |
| GET    | /api/instructor/attendance/course/lessons/{id}      | Get attendance records for a lesson.      |

### Quiz & Questions Management
| Method | Endpoint                                        | Description                                |
|--------|-----------------------------------------------|--------------------------------------------|
| POST   | /api/instructor/add-questions/course/{id}   | Add questions to a course.                |
| GET    | /api/student/questions/course/{id}          | Get quiz questions for a course.          |

---

## Project Structure

```plaintext
src/
|-- main/
|   |-- java/com/lms/
|   |   |-- controllers/    # REST controllers
|   |   |-- services/       # Business logic
|   |   |-- repositories/   # Database repositories
|   |   |-- models/         # Data models
|   |   |-- config/         # Security and app configuration
|   |-- resources/
|       |-- application.yml  # Application configuration
|-- test/
    |-- java/com/lms/        # Test cases
```

---

## Installation and Setup

### Prerequisites

1. **Java**: Ensure JDK 17 or above is installed.
2. **PostgreSQL**: Install PostgreSQL and create a database for the project.
3. **Postman**: For testing API endpoints.

### Steps

1. Clone the repository:

   ```bash
   git clone https://github.com/Yousef-karem/Learning-Management-System.git
   cd Learning-Management-System/lms-backend
   ```

2. Configure the database:

   - Update `src/main/resources/application.yml` with your PostgreSQL credentials.

   ```yaml
   spring:
     datasource:
       url: jdbc:postgresql://localhost:5432/lms_db
       username: your_username
       password: your_password
     jpa:
       hibernate:
         ddl-auto: update
   jwt:
     secret: your_jwt_secret
   ```

3. Build and run the project:

   ```bash
   ./mvnw spring-boot:run
   ```

4. Access the API:

   - API base URL: `http://localhost:8080`

---

## How to test the EndPoints:

1. Open Postman.
2. Create requests based on the endpoints listed above.
3. Test the endpoints using appropriate roles and JWT tokens.

---

## Contact

For further inquiries or collaboration, please contact:

- **Yousef Karem**\
  [GitHub Profile](https://github.com/Yousef-karem)
- **Juliana George**\
[GitHub Profile](https://github.com/julianageorge)
- **Ephraim Youssef**\
[GitHub Profile](https://github.com/EphraimYoussef)
- **Ramez Ragaay**\
[GitHub Profile](https://github.com/RamezRagaay)
- **Arsany Nageh**\
[GitHub Profile](https://github.com/Arsany11)

---

This backend system is a crucial component of the Learning Management System and ensures secure and efficient management of users, roles, and courses.

