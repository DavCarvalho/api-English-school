# English School API

A professional RESTful API for managing an English school system, built with Node.js, Express, and Sequelize ORM. This API provides complete CRUD operations for managing students, courses, enrollments, and administrative tasks.

## 🎯 Overview

This project implements a robust backend solution for English language schools, offering endpoints to manage:
- **Students**: Student registration, profiles, and enrollment tracking
- **Courses**: Course creation, scheduling, and management
- **Classes**: Class organization and student-teacher assignments
- **Enrollments**: Student course enrollments and progress tracking
- **People**: User management for students, teachers, and administrators

## 🏗️ Architecture

### Project Structure

```
api-English-school/
├── api/
│   ├── config/          # Database and environment configuration
│   ├── controllers/     # Request handlers and business logic
│   ├── models/          # Sequelize database models
│   ├── routes/          # API endpoint definitions
│   ├── services/        # Business logic layer
│   ├── migrations/      # Database schema migrations
│   ├── seeders/         # Sample data for testing
│   └── index.js         # Application entry point
├── package.json         # Dependencies and scripts
├── .sequelizerc         # Sequelize configuration
└── .gitignore
```

### Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **ORM**: Sequelize
- **Database**: PostgreSQL / MySQL / SQLite (configurable)
- **Architecture Pattern**: MVC (Model-View-Controller) + Service Layer

## 🚀 Getting Started

### Prerequisites

```bash
- Node.js 14.x or higher
- npm or yarn
- PostgreSQL 12+ (or MySQL 8+)
- Git
```

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/DavCarvalho/api-English-school.git
cd api-English-school
```

2. **Install dependencies**
```bash
npm install
```

3. **Configure environment variables**
Create a `.env` file in the root directory:
```env
NODE_ENV=development
PORT=3000
DATABASE_URL=postgres://user:password@localhost:5432/english_school
DATABASE_DIALECT=postgres
```

4. **Run database migrations**
```bash
npx sequelize-cli db:migrate
```

5. **Seed the database (optional)**
```bash
npx sequelize-cli db:seed:all
```

6. **Start the server**
```bash
npm start
```

The API will be available at `http://localhost:3000`

## 📡 API Endpoints

### Students

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/students` | List all students |
| GET | `/students/:id` | Get student by ID |
| POST | `/students` | Create new student |
| PUT | `/students/:id` | Update student |
| DELETE | `/students/:id` | Delete student |

### Courses

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/courses` | List all courses |
| GET | `/courses/:id` | Get course by ID |
| POST | `/courses` | Create new course |
| PUT | `/courses/:id` | Update course |
| DELETE | `/courses/:id` | Delete course |

### Enrollments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/enrollments` | List all enrollments |
| GET | `/enrollments/:id` | Get enrollment by ID |
| POST | `/enrollments` | Create new enrollment |
| PUT | `/enrollments/:id` | Update enrollment |
| DELETE | `/enrollments/:id` | Delete enrollment |
| GET | `/students/:id/enrollments` | Get student's enrollments |

### People (Users)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/people` | List all people |
| GET | `/people/:id` | Get person by ID |
| POST | `/people` | Create new person |
| PUT | `/people/:id` | Update person |
| DELETE | `/people/:id` | Delete person |

## 📊 Database Schema

### Models

#### Person
```javascript
{
  id: Integer (PK),
  name: String,
  email: String (Unique),
  role: Enum ['student', 'teacher', 'admin'],
  active: Boolean,
  createdAt: DateTime,
  updatedAt: DateTime
}
```

#### Course
```javascript
{
  id: Integer (PK),
  title: String,
  description: Text,
  startDate: Date,
  endDate: Date,
  level: Enum ['beginner', 'intermediate', 'advanced'],
  teacherId: Integer (FK),
  createdAt: DateTime,
  updatedAt: DateTime
}
```

#### Enrollment
```javascript
{
  id: Integer (PK),
  studentId: Integer (FK),
  courseId: Integer (FK),
  status: Enum ['active', 'completed', 'cancelled'],
  enrollmentDate: Date,
  grade: Float,
  createdAt: DateTime,
  updatedAt: DateTime
}
```

### Relationships

- A **Person** can have many **Courses** (as teacher)
- A **Person** can have many **Enrollments** (as student)
- A **Course** belongs to one **Person** (teacher)
- A **Course** has many **Enrollments**
- An **Enrollment** belongs to one **Person** and one **Course**

## 🔧 Configuration

### Sequelize Configuration (`.sequelizerc`)

This file defines the paths for Sequelize CLI:

```javascript
{
  config: './api/config/config.json',
  'models-path': './api/models',
  'seeders-path': './api/seeders',
  'migrations-path': './api/migrations'
}
```

### Database Configuration

Edit `api/config/config.json`:

```json
{
  "development": {
    "username": "your_username",
    "password": "your_password",
    "database": "english_school_dev",
    "host": "localhost",
    "dialect": "postgres"
  },
  "production": {
    "use_env_variable": "DATABASE_URL",
    "dialect": "postgres",
    "dialectOptions": {
      "ssl": {
        "require": true,
        "rejectUnauthorized": false
      }
    }
  }
}
```

## 🛠️ Development

### Available Scripts

```bash
# Start development server with hot reload
npm run dev

# Run migrations
npm run migrate

# Rollback last migration
npm run migrate:undo

# Create new migration
npx sequelize-cli migration:generate --name migration-name

# Create new model
npx sequelize-cli model:generate --name ModelName --attributes field1:type,field2:type

# Seed database
npm run seed

# Run tests
npm test
```

### Creating Migrations

```bash
npx sequelize-cli migration:generate --name add-field-to-students

# Edit the generated file in api/migrations/
# Then run:
npm run migrate
```

### Creating Seeders

```bash
npx sequelize-cli seed:generate --name demo-students

# Edit the generated file in api/seeders/
# Then run:
npm run seed
```

## 📝 Example Requests

### Create a Student

```bash
POST /students
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "role": "student",
  "active": true
}
```

### Enroll Student in Course

```bash
POST /enrollments
Content-Type: application/json

{
  "studentId": 1,
  "courseId": 5,
  "status": "active",
  "enrollmentDate": "2025-01-15"
}
```

### Get Student's Enrollments

```bash
GET /students/1/enrollments
```

Response:
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "status": "active",
      "enrollmentDate": "2025-01-15",
      "course": {
        "id": 5,
        "title": "English for Beginners",
        "level": "beginner",
        "startDate": "2025-01-20"
      }
    }
  ]
}
```

## 🔐 Security Considerations

- **Input Validation**: Implement validation middleware for all endpoints
- **Authentication**: Consider adding JWT-based authentication
- **Authorization**: Implement role-based access control (RBAC)
- **SQL Injection**: Sequelize ORM provides protection through parameterized queries
- **Rate Limiting**: Implement rate limiting to prevent abuse
- **CORS**: Configure CORS appropriately for production

## 🚀 Deployment

### Heroku Deployment

```bash
# Login to Heroku
heroku login

# Create app
heroku create your-app-name

# Add PostgreSQL addon
heroku addons:create heroku-postgresql:hobby-dev

# Push to Heroku
git push heroku main

# Run migrations
heroku run npx sequelize-cli db:migrate

# Open app
heroku open
```

### Environment Variables for Production

```env
NODE_ENV=production
PORT=3000
DATABASE_URL=your_production_database_url
JWT_SECRET=your_secure_secret_key
CORS_ORIGIN=https://your-frontend-domain.com
```

## 📚 Service Layer Pattern

This API uses a **Service Layer** to separate business logic from controllers:

```javascript
// controllers/StudentController.js
const StudentService = require('../services/StudentService');

class StudentController {
  static async getAll(req, res) {
    try {
      const students = await StudentService.getAllStudents();
      return res.status(200).json(students);
    } catch (error) {
      return res.status(500).json({ error: error.message });
    }
  }
}

// services/StudentService.js
const { Student } = require('../models');

class StudentService {
  static async getAllStudents() {
    return await Student.findAll();
  }
}
```

## 🧪 Testing

```bash
# Install testing dependencies
npm install --save-dev jest supertest

# Run tests
npm test

# Run with coverage
npm run test:coverage
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is available for educational and commercial use.

## 📧 Contact

**Developer**: David Carvalho  
**GitHub**: [@DavCarvalho](https://github.com/DavCarvalho)  
**Repository**: [api-English-school](https://github.com/DavCarvalho/api-English-school)

---

**Built with:** Node.js • Express.js • Sequelize • PostgreSQL

**Architecture**: RESTful API • MVC Pattern • Service Layer • ORM
