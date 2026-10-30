# LedgerLens — Personal Finance Management System

LedgerLens is a full-stack personal finance management application designed to support secure expense tracking, transaction processing, and automated categorization through a web interface.

Built with React, FastAPI, and PostgreSQL, the application enables users to manage financial transactions, import records from CSV files, and organize expenses through rule-based categorization and authenticated REST APIs.

> **Project attribution:** This repository is based on an existing SmartSpend implementation. The original author and repository should be credited appropriately, and publication should be authorized by the original code owner.

---

## Overview

Managing personal finances often involves repetitive manual entry, inconsistent transaction categorization, and difficulties handling transaction data in bulk.

LedgerLens addresses these challenges by bringing together:

* Secure user authentication and authorization
* RESTful API development
* PostgreSQL database persistence
* Personal expense management
* Automated transaction categorization
* CSV-based bulk transaction imports
* Input validation and duplicate detection
* Automated backend testing

The project demonstrates practical full-stack software engineering concepts through the integration of frontend development, backend APIs, database operations, security, data processing, and testing.

---

## Key Features

### 1. Authentication and Authorization

The application provides authenticated access to personal financial records.

Features include:

* User registration and login
* JWT-based authentication
* Password hashing using Argon2
* Protected API endpoints
* User-specific expense access
* Authorization checks for individual transactions

These mechanisms help ensure that expense operations are associated with the authenticated user.

### 2. Expense Management

LedgerLens supports the core operations required to manage personal transactions.

Features include:

* Create expense records
* Retrieve personal expenses
* Update existing transactions
* Delete expense records
* Search transactions
* Filter transactions by category
* Assign or override transaction categories manually

The backend exposes REST APIs to handle these operations and persist transaction data in PostgreSQL.

### 3. Automated Transaction Categorization

The application uses a rule-based categorization service to assign categories to transactions.

Example classifications:

| Transaction Description | Category      |
| ----------------------- | ------------- |
| Swiggy dinner           | Food          |
| Uber ride               | Transport     |
| Amazon purchase         | Shopping      |
| Netflix subscription    | Entertainment |

The categorization service also records:

* Categorization method
* Categorization confidence

This approach provides structured transaction classification without requiring a machine-learning model.

### 4. CSV Transaction Import Pipeline

LedgerLens supports bulk transaction imports through CSV files.

The import workflow includes:

1. CSV upload
2. File validation
3. Column validation
4. Individual record validation
5. Duplicate detection
6. Transaction categorization
7. Database persistence
8. Import summary generation

The application supports partial imports by separating valid and rejected records rather than rejecting an entire file because of individual invalid transactions.

This workflow demonstrates practical file processing, validation, and data handling.

---

## System Architecture

The application follows a frontend-backend architecture.

```text
              React Frontend
              Vite + JavaScript
                     |
                     | HTTP / REST
                     |
                     v
               FastAPI Backend
                     |
          +----------+----------+
          |          |          |
          v          v          v
   Authentication  CSV       Expense
   and Validation  Import      APIs
          |          |          |
          +----------+----------+
                     |
           +---------+---------+
           |                   |
           v                   v
   Categorization          PostgreSQL
      Service               Database
```

### Architecture Components

**Frontend — React and Vite**

Provides the web interface for interacting with the expense management application.

**Backend — FastAPI**

Handles API requests, authentication, expense operations, CSV imports, validation, and authorization.

**Categorization Service**

Applies rule-based logic to categorize transactions and determine categorization confidence.

**Database — PostgreSQL**

Stores user accounts and expense records.

**Testing — Pytest**

Validates backend behavior, authentication, API operations, and transaction categorization.

---

## Technology Stack

| Layer             | Technologies                 |
| ----------------- | ---------------------------- |
| Frontend          | React, Vite, JavaScript, CSS |
| Backend           | Python, FastAPI              |
| ORM               | SQLAlchemy                   |
| Validation        | Pydantic                     |
| Authentication    | JWT                          |
| Password Security | Argon2                       |
| Database          | PostgreSQL                   |
| Testing           | Pytest, FastAPI TestClient   |
| API Testing       | Postman                      |
| Version Control   | Git, GitHub                  |

---

## Backend Design

The backend uses a modular structure to separate API handling, authentication, database connectivity, data models, validation schemas, and business logic.

```text
backend/
├── services/
│   └── categorizer.py
├── tests/
│   ├── test_api.py
│   └── test_categorizer.py
├── auth.py
├── database.py
├── main.py
├── models.py
└── schemas.py
```

### Module Responsibilities

| Module                    | Responsibility                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------- |
| `main.py`                 | API routes, request processing, authentication dependencies, CSV imports, and application configuration |
| `auth.py`                 | JWT token creation and verification                                                                     |
| `database.py`             | SQLAlchemy configuration and PostgreSQL connectivity                                                    |
| `models.py`               | Database entities and relationships                                                                     |
| `schemas.py`              | Pydantic request and response validation models                                                         |
| `services/categorizer.py` | Rule-based transaction categorization                                                                   |
| `tests/`                  | Automated tests for API behavior and categorization                                                     |

---

## Database Design

LedgerLens uses PostgreSQL for persistent storage.

### User Entity

Stores account information, including:

* User ID
* Name
* Email
* Hashed password

### Expense Entity

Stores transaction information, including:

* Expense ID
* User ID
* Amount
* Description
* Transaction date
* Category
* Categorization confidence
* Categorization method

Each expense is associated with its authenticated owner through a foreign-key relationship.

---

## API Endpoints

The backend exposes REST endpoints for authentication, expense management, database connectivity, and CSV imports.

| HTTP Method | Endpoint           | Description                     |
| ----------- | ------------------ | ------------------------------- |
| GET         | `/`                | API health check                |
| GET         | `/db-test`         | Database connectivity check     |
| POST        | `/register`        | Register a user                 |
| POST        | `/login`           | Authenticate a user             |
| GET         | `/me`              | Retrieve the authenticated user |
| POST        | `/expenses`        | Create an expense               |
| GET         | `/expenses`        | Retrieve the user's expenses    |
| GET         | `/expenses/{id}`   | Retrieve a specific expense     |
| PUT         | `/expenses/{id}`   | Update an expense               |
| DELETE      | `/expenses/{id}`   | Delete an expense               |
| POST        | `/expenses/import` | Import transactions from CSV    |

FastAPI provides interactive API documentation through Swagger UI when the backend is running:

```text
http://127.0.0.1:8000/docs
```

---

## Validation and Error Handling

The application validates input at multiple stages to support consistent data processing.

### Validation Includes

* Required field validation
* Email validation
* Password length validation
* Positive transaction amounts
* Transaction date validation
* CSV file type validation
* CSV column validation
* Invalid record handling
* Duplicate transaction detection
* Database error handling

### Example Import Summary

A successful CSV import can return a summary containing the number of imported and rejected records.

```json
{
  "message": "CSV import completed",
  "imported": 97,
  "rejected": 3
}
```

The numbers above are illustrative and do not represent measured results from a particular import.

---

## Security

The application incorporates several security-related mechanisms:

* JWT-based authentication
* Argon2 password hashing
* Protected expense endpoints
* User-level authorization
* Pydantic request validation
* PostgreSQL foreign-key constraints
* Environment-based secret configuration
* `.env` exclusion from version control
* Generic database error responses

Sensitive credentials should be supplied through environment variables rather than committed to the repository.

---

## Automated Testing

LedgerLens includes automated backend tests using Pytest and FastAPI's `TestClient`.

### Test Areas

* API health checks
* Database connectivity
* Authentication
* Authorization
* Expense creation
* CSV validation
* Transaction categorization

### Run the Tests

From the project root:

```bash
cd backend
pytest
```

The test suite helps verify expected backend behavior and identify regressions during development.

---

## Project Structure

```text
LedgerLens/
├── backend/
│   ├── services/
│   │   └── categorizer.py
│   ├── tests/
│   │   ├── test_api.py
│   │   └── test_categorizer.py
│   ├── auth.py
│   ├── database.py
│   ├── main.py
│   ├── models.py
│   └── schemas.py
├── frontend/
│   ├── src/
│   │   ├── App.jsx
│   │   ├── App.css
│   │   └── main.jsx
│   ├── package.json
│   └── vite.config.js
├── data/
│   └── sample_transactions.csv
├── README.md
├── .gitignore
└── LICENSE
```

*Note: The structure above reflects the documented project layout. Ensure the filenames match the actual files in the repository before publishing.*

---

## Local Development Setup

### Prerequisites

Install the following software:

* Python 3.12 or compatible version
* Node.js and npm
* PostgreSQL
* Git

### 1. Clone the Repository

Replace the placeholder below with the URL of your authorized GitHub repository.

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>
```

### 2. Configure PostgreSQL

Create a PostgreSQL database for the application.

The existing implementation's configuration may use the database name `smartspend`. If you retain that configuration, create a database with that name.

```sql
CREATE DATABASE smartspend;
```

The database name in the connection string must match the database you create.

### 3. Set Up the Backend

Navigate to the backend directory:

```bash
cd backend
```

Create a virtual environment:

```bash
python -m venv venv
```

Activate it on Windows:

```powershell
venv\Scripts\activate
```

Install the required Python packages:

```bash
pip install fastapi uvicorn sqlalchemy "psycopg[binary]" "pwdlib[argon2]" email-validator "python-jose[cryptography]" python-dotenv python-multipart pytest httpx
```

### 4. Configure Environment Variables

Create a `.env` file inside the `backend/` directory.

Use the following template and replace the placeholders with your own local configuration:

```env
DATABASE_URL=postgresql+psycopg://<username>:<password>@localhost:5432/smartspend
SECRET_KEY=<your-secret-key>
```

Generate a strong, private secret key for your local environment.

**Never commit your actual `.env` file or secret values to GitHub.**

### 5. Start the Backend

From the `backend/` directory, run:

```bash
uvicorn main:app --reload
```

The backend should be accessible at:

```text
http://127.0.0.1:8000
```

Interactive API documentation:

```text
http://127.0.0.1:8000/docs
```

### 6. Start the Frontend

Open a second terminal and navigate to the frontend directory:

```bash
cd frontend
```

Install frontend dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The Vite development server typically runs at:

```text
http://localhost:5173
```

If the frontend cannot connect to the backend, verify the API base URL and the application's CORS configuration.

---

## Example CSV Format

The transaction importer expects a CSV file with the appropriate columns.

Example:

```csv
amount,description,transaction_date
450,Swiggy dinner,2026-09-10
250,Uber ride,2026-09-11
1200,Amazon purchase,2026-09-12
```

Each record is validated before being processed for database storage.

Use the application's expected column names and formats when preparing CSV files.

---

## Future Improvements

Potential extensions to the current implementation include:

* Machine-learning fallback for low-confidence transactions
* Refresh-token authentication
* Password reset functionality
* Pagination for large transaction datasets
* Background CSV processing
* API rate limiting
* Alembic database migrations
* Docker containerization
* CI/CD pipeline
* Cloud deployment
* Production monitoring and logging

These items are potential future enhancements, not features currently claimed as implemented.

---

## Engineering Concepts Demonstrated

The project brings together several practical software engineering concepts:

* Full-stack application development
* REST API design
* Authentication and authorization
* Relational database design
* ORM-based persistence
* Input validation
* File-processing pipelines
* Business logic separation
* Error handling
* Automated testing
* Secure configuration management
* Git-based version control

By combining these components, the application illustrates how a web-based financial management system can be structured around frontend interactions, backend business logic, persistent storage, and automated verification.

---

## License and Attribution

This repository is based on an existing implementation. The original author's permission and applicable license terms must be respected before redistributing or modifying the code.

Retain the original copyright and license notices as required by the applicable license. Do not claim ownership of code written by another person.

If publication under the MIT License is authorized, include the complete MIT License text in the root-level `LICENSE` file with the correct copyright attribution.

For questions about the implementation, consult the source code and the original project documentation.
