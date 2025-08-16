# Student Management System

Student Management System

A backend student management system built with Node.js, Express.js, and Sequelize ORM, integrated with a relational database. It delivers secure RESTful APIs for creating, reading, updating, and deleting student records, with transaction management, standardized HTTP status codes, and custom error handling to ensure data integrity and reliable validation. Designed for scalability and ease of maintenance, it streamlines structured data management for academic and administrative use.


## Features

* RESTful API for Students: Create, Read, Update, Delete

* Sequelize models with PK-based lookups (findByPk)

* Database transactions for create/update/delete

* Centralized error handling with custom errors (e.g., ConflictError, BadRequestError)

* Standardized HTTP status codes via http-status-codes

## Tech Stack

Runtime: Node.js

Framework: Express.js

ORM: Sequelize

Database: Any SQL DB supported by Sequelize (e.g., PostgreSQL, MySQL, SQLite)

Validation & Errors: Custom error classes + http-status-codes

## Project Structure
``` src/
  controllers/
    studentController.js     # CRUD handlers with transactions
  errors/
    errors.js                # Custom error classes (BadRequestError, ConflictError, etc.)
  models/
    index.js                 # Sequelize init & model registry
    student.js               # Student model (e.g., PK: nic)
  routes/
    students.js              # /students routes
  app.js                     # Express app
  server.js                  # Entrypoint
```
## API Overview
### Create Student

POST /students
Request Body (example):
```
{
  "nic": "199912345678",
  "firstName": "Jane",
  "lastName": "Doe",
  "email": "jane.doe@university.edu",
  "program": "Computer Science",
  "year": 3
}
```

**Responses**

* 201 Created: Returns created student

* 409 Conflict: Student already exists

### Get Student by NIC

GET /students/:nic

**Responses**

* 200 OK: Returns student

* 400 Bad Request: Student doesn’t exist

### Update Student

PUT /students/:nic
Request Body (partial or full):
```
{
  "email": "jane.updated@university.edu",
  "year": 4
}
```

**Responses**

201 Created: Returns updated payload

400 Bad Request: Student doesn’t exist

### Delete Student

DELETE /students/:nic

**Responses**

204: No Content

400 Bad Request: Student doesn’t exist

**cURL Examples**
```
### Create
curl -X POST http://localhost:3000/students \
  -H "Content-Type: application/json" \
  -d '{"nic":"199912345678","firstName":"Jane","lastName":"Doe","email":"jane.doe@university.edu"}'

### Read
curl http://localhost:3000/students/199912345678

### Update
curl -X PUT http://localhost:3000/students/199912345678 \
  -H "Content-Type: application/json" \
  -d '{"year":4}'

### Delete
curl -X DELETE http://localhost:3000/students/199912345678
```

## Getting Started
### Prerequisites

* Node.js (LTS recommended)

* A SQL database supported by Sequelize (PostgreSQL/MySQL/SQLite/etc.)

### Installation
```
git clone https://github.com/victoria-field7/student-management.git
cd student-management
npm install
```

### Environment Variables

Create a .env file:
```
NODE_ENV=development
PORT=3000

DB_DIALECT=postgres           # or mysql | mariadb | sqlite
DB_HOST=localhost
DB_PORT=5432
DB_NAME=studentdb
DB_USER=postgres
DB_PASSWORD=postgres
```

## Database Setup (Sequelize)

If you’re using Sequelize CLI and migrations:

```
# Optional: initialize if not already
npx sequelize-cli db:create
npx sequelize-cli db:migrate
# (Add seeders if applicable)
```

If using SQLite, configure storage in Sequelize options instead of host/user/password.

### Run
```
npm run dev     # with nodemon (if configured)
# or
npm start
```


Server starts at http://localhost:${PORT} (default: 3000).

## Error Handling

ConflictError (409) – Duplicate entity (e.g., student already exists)

BadRequestError (400) – Invalid input or missing entity

Uses transactions (sequelize.transaction()) to ensure atomic writes.

## Testing (Optional)

If you add tests later:
```
npm test
```


Recommended: Jest or Mocha + Chai + Supertest for API tests.

## Roadmap

Pagination & filtering for listing students

Input validation (e.g., zod or Joi)

AuthN/AuthZ (e.g., JWT)

OpenAPI/Swagger docs

CI pipeline & containerization (Docker)

## License

MIT
