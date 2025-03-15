# Foxo.Club Health Companion App

## Overview
This project is a production–grade FastAPI application for Foxo.Club, an AI–based health companion platform. It provides:
- **Stateless Authentication & Role–Based Authorization** using JWT.
- **PDF Report Upload**: Registered clients can upload health checkup PDFs; files are stored in AWS S3 with a custom path.
- **Health Parameter Extraction**: Extracts and normalizes health parameters from reports (simulated) and stores key–value pairs in DynamoDB.
- **Admin Portal**: Internal endpoints for admin users to review clients, reports, and approve/reject extracted health parameters.

It is designed to help users track and understand their health by uploading their health checkup reports. The application extracts health parameters from PDF reports, normalizes the data, and provides a rich, responsive admin portal to manage and review the information. This project is built with production-ready technologies and follows best practices using OOP, SOLID principles, and common design patterns.

## Features
- **User Authentication & Authorization**:  
  - Sign-up and Login APIs using JWT-based stateless authentication.
  - Role-based access control for clients, admin, and superusers.
- **PDF Report Upload**:  
  - Registered clients can upload health checkup PDF reports.
  - Reports are stored in AWS S3 using a custom path format.
- **Asynchronous Processing**:  
  - Utilizes Celery with RabbitMQ as the broker (and optionally Redis) to process heavy PDF extraction tasks.
  - Extracts health parameters from PDF reports automatically.
- **Data Storage**:  
  - SQL Database (PostgreSQL): Stores user, report, and health parameter metadata.
  - NoSQL Database (DynamoDB): Stores extracted health parameters documents for flexible, high-volume data storage.
- **Admin Portal**:  
  - Section 1: Registered Client Details (client phone, unique client ID, and user ID).
  - Section 2: Uploaded Report Details (client phone, user ID, upload timestamp, report unique ID, processing status).
  - Section 3: Approved Health Parameters (with mapping options and approval details).
  - Section 4: Pending/Rejected Health Parameters (with a common remarks field and Approve/Reject buttons).
  - Responsive UI built with FastAPI’s Jinja2 templates and Bootstrap.
- **Normalization & Validation**:  
  - Normalizes lab units, reference ranges, and converts varying report structures into a common format.
  - Reports are stored in AWS S3 using a custom path format.

## Tech Stack
  - **Backend**: Python 3.11, FastAPI, Uvicorn
  - **Database**: PostgreSQL (SQL), DynamoDB (NoSQL)
  - **Asynchronous Tasks**: Celery, RabbitMQ, Redis
  - **Cloud Services**: AWS
  - **ORM & Migrations**: SQLAlchemy, Alembic
  - **Authentication**: JWT, Passlib (with bcrypt)
  - **Containerization & Deployment**: Docker, AWS Elastic Beanstalk/ECS/EKS
  - **Frontend (Admin UI)**: Jinja2 Templates, Bootstrap 5

## Design Principles & Patterns
- **OOP Concepts**:
  - Encapsulation: Models encapsulate data; services encapsulate business logic.
  - Inheritance & Polymorphism: Custom exception classes and model base classes.
  - Abstraction: CRUD operations are abstracted in dedicated modules.
- **Design Principles**:  
  - **Separation of Concerns**: Different modules (auth, pdf, admin) each have specific responsibilities.
  - **Dependency Injection**: FastAPI’s dependency system is used for database sessions, authentication, etc.
  - **SOLID Principles**: Ensured via modular code where each class/function has a single responsibility, is open for extension but closed for modification, and depends on abstractions rather than concretions.
- **SOLID Principles**:
  - **Single Responsibility**: Each module/class is responsible for one aspect (e.g., authentication, PDF processing).
  - **Open/Closed**: New functionality (like additional PDF parsers) can be added without modifying existing code.
  - **Liskov Substitution**: Functions operate on abstract interfaces (e.g., using dependency injection for DB sessions).
  - **Interface Segregation**: Endpoints and services are designed to have minimal and clear dependencies.
  - **Dependency Inversion**: High-level modules depend on abstractions provided via FastAPI dependencies.
- **Design Patterns**:  
  - **Factory Pattern**: For JWT token creation.
  - **Repository Pattern**: For encapsulating CRUD operations in separate modules (e.g., app/auth/crud.py).
  - **Singleton Pattern**: Used for database connection management (via SQLAlchemy sessions).
  - **Strategy Pattern**: Different PDF extraction strategies can be plugged in based on report type.
  - **Facade Pattern**: S3 and DynamoDB utility modules provide a simplified interface to AWS services.
- **RESTful API Principles**:  
  - Consistent endpoint naming, proper HTTP status codes, and stateless architecture.

## Project Structure
```bash
foxo-health-companion-app-interview-task/
├── app/
│   ├── admin/
│   │   ├── __init__.py
│   │   ├── routes.py           # Admin routes and dashboard logic
│   ├── auth/
│   │   ├── __init__.py
│   │   ├── crud.py             # CRUD operations for users
│   │   ├── routes.py           # Signup, login, JWT handling
│   │   ├── schemas.py          # Pydantic models for auth
│   ├── db/
│   │   ├── __init__.py
│   │   ├── models.py           # SQLAlchemy models (User, Report, HealthParameter, etc.)
│   │   ├── session.py          # Database session and engine creation
│   ├── pdf/
│   │   ├── __init__.py
│   │   ├── routes.py           # PDF upload and extraction endpoints
│   │   ├── s3_utils.py         # AWS S3 integration for file upload/download
│   │   ├── parser.py           # PDF parser that extracts health parameters
│   ├── nosql/
│   │   ├── __init__.py
│   │   ├── dynamodb_client.py  # DynamoDB client functions
│   ├── templates/
│   │   ├── admin_dashboard     # HTML file for Admin dashboard
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── cache.py            # Configuration of Redis connection
│   │   ├── jwt_utils.py        # Creation and verification of JWT Token
│   ├── main.py                 # FastAPI application instance and route inclusions
│   ├── config.py               # Application configuration and settings
├── alembic/
│   ├── env.py                  # Alembic configuration and target_metadata
│   ├── versions/               # Migration scripts
├── celery_worker.py            # Celery worker configuration and tasks
├── alembic.ini                 # alembic.ini for database migrations
├── Dockerfile                  # Containerization configuration
├── requirements.txt            # Python dependencies (pinned versions)
├── .env                        # Environment variables (DB URL, AWS keys, etc.)
├── README.md
```

## Contributing
  Contributions are welcome! Please fork the repository, create a new branch for your feature or bug fix, and submit a pull request with a detailed description of your changes.

## License
  This project is licensed under the MIT License.
