# Property Management API

A Django-based property management system that handles organization onboarding, contact management, charge creation, and payment processing with seamless integration to HubSpot CRM and other third-party services.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
- [API Documentation](#api-documentation)
- [External Integrations](#external-integrations)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Architecture Notes](#architecture-notes)
- [Logging](#logging)
- [Task Checklist](#task-checklist)

## 🎯 Overview

This application manages the complete lifecycle of property management operations:

1. **Organization Onboarding**: An organization (e.g., "Test Org") approaches a property management company
2. **Property Investigation**: The property management company investigates the property and determines a rate
3. **Quotation**: A quotation is sent to the organization
4. **Payment Processing**: Upon agreement, a payment link is generated and sent to the organization
5. **Payment Recording**: Payment records are saved in the database and synchronized with HubSpot CRM

## ✨ Features

- **Organization/Company Management**: Create and manage organization records
- **Contact Management**: Onboard contacts and associate them with organizations
- **Charge Management**: Create charges for organizations with rate determination
- **Payment Processing**: Generate payment links and process payments via Stripe
- **Email Notifications**: Send emails using SendGrid integration
- **CRM Integration**: Synchronize data with HubSpot CRM
- **Request Logging**: Log all requests to AWS DynamoDB
- **RESTful API**: Well-structured API endpoints with versioning

## 🛠 Technology Stack

- **Framework**: Django
- **Database**: PostgreSQL (with SQLAlchemy ORM)
- **Migrations**: Alembic
- **Testing**: pytest, pytest-asyncio
- **Code Quality**: Black (code formatter)
- **External Services**:
  - HubSpot API (CRM integration)
  - SendGrid (Email service)
  - Stripe (Payment processing)
  - AWS DynamoDB (Logging)

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- Python 3.8+ 
- PostgreSQL database
- pip (Python package manager)

## 🚀 Installation

1. **Clone the repository** (if applicable):
   ```bash
   git clone <repository-url>
   cd PropertyAppWithFastAPI
   ```

2. **Create a virtual environment** (recommended):
   ```bash
   python -m venv venv
   
   # On Windows
   venv\Scripts\activate
   
   # On Linux/Mac
   source venv/bin/activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

## ⚙️ Configuration

1. **Create a `.env` file** in the root directory:
   ```bash
   cp env-example .env
   ```

2. **Configure environment variables** in `.env`:
   ```env
   # Database Configuration
   DATABASE_NAME=your_database_name
   DATABASE_HOST=localhost
   DATABASE_USER=your_db_user
   DATABASE_PASSWORD=your_db_password
   DATABASE_DRIVER=postgresql
   TEST_DATABASE_NAME=your_test_database_name
   
   # Stripe Configuration
   STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
   STRIPE_SECRET_KEY=your_stripe_secret_key
   
   # HubSpot Configuration
   HUBSPOT_API_KEY=your_hubspot_api_key
   
   # SendGrid Configuration
   SENDGRID_API_KEY=your_sendgrid_api_key
   WELCOME_MESSAGE_TEMPLATE_ID=your_welcome_template_id
   PAYMENT_CONFIRMATION_TEMPLATE_ID=your_payment_template_id
   
   # AWS Configuration
   AWS_SECRET_KEY_ID=your_aws_access_key_id
   AWS_SECRET_KEY=your_aws_secret_key
   AWS_REGION=your_aws_region
   ```

## 🗄️ Database Setup

1. **Create PostgreSQL database**:
   ```sql
   CREATE DATABASE your_database_name;
   CREATE DATABASE your_test_database_name;
   ```

2. **Run database migrations**:
   ```bash
   alembic upgrade head
   ```

## ▶️ Running the Application

Start the Django development server:

```bash
uvicorn app:app --reload
```

The API will be available at:
- **API**: http://localhost:8000
- **Interactive API Documentation (Swagger UI)**: http://localhost:8000/docs
- **Alternative API Documentation (ReDoc)**: http://localhost:8000/redoc

## 📚 API Documentation

### Version 1 Endpoints

#### Contact Endpoints

- `POST /api/v1/contacts/` - Create a new contact
- `GET /api/v1/contact/<email>/` - Get contact by email address

#### Company/Organization Endpoints

- `POST /api/v1/companies/` - Create a new company/organization
- `GET /api/v1/company/<company-name>/` - Get company by name

#### Charge Endpoints

- Charge-related endpoints for managing property charges

#### Email Endpoints

- `POST /api/v1/email/send/` - Send an email notification

> **Note**: For complete API documentation with request/response schemas, visit `/docs` when the server is running.

## 🔌 External Integrations

The application integrates with the following external services:

- **HubSpot**: CRM integration for storing organization and contact data
- **SendGrid**: Email delivery service for sending notifications
- **Stripe**: Payment processing for handling organization payments
- **AWS DynamoDB**: Request logging and audit trail

## 🧪 Testing

Run the test suite using pytest:

```bash
# Run all tests
pytest

# Run tests with coverage
pytest --cov=.

# Run specific test file
pytest tests/routes/test_contact.py
```

Test files are located in the `tests/` directory and include:
- Unit tests for API endpoints
- Integration tests for external services (HubSpot, SendGrid, Stripe, DynamoDB)
- Database tests

## 📁 Project Structure

```
PropertyAppWithFastAPI/
├── alembic.ini                 # Alembic configuration
├── app.py                      # Django application entry point
├── database.py                 # Database connection and session management
├── settings.py                 # Application settings
├── requirements.txt            # Python dependencies
├── env-example                 # Environment variables template
├── readme.md                   # This file
├── task_checklist.md           # Development task checklist
│
├── alembic/                    # Database migrations (if using Alembic folder structure)
├── migrations/                 # Database migration scripts
│   └── versions/              # Migration version files
│
├── routes/                     # API route handlers
│   └── v1/                    # Version 1 API routes
│       ├── contact.py         # Contact endpoints
│       ├── company.py         # Company endpoints
│       ├── charge.py          # Charge endpoints
│       └── email.py           # Email endpoints
│
├── models/                     # SQLAlchemy database models
│   ├── contact.py
│   ├── message.py
│   └── payment.py
│
├── schemas/                    # Pydantic schemas for request/response validation
│   └── schema.py
│
├── crud/                       # Database CRUD operations
│   ├── contact.py
│   ├── company.py
│   ├── charge.py
│   └── message.py
│
├── dependencies/               # Dependency injection
│   └── dependencies.py
│
├── hubspot_api/                # HubSpot integration
│   └── utils.py
│
├── stripe_api/                 # Stripe integration
│   └── payment.py
│
├── email_api/                  # SendGrid integration
│   └── email.py
│
├── aws/                        # AWS services
│   └── services/
│       └── dynamo_db/
│           └── logs.py
│
├── logger/                     # Logging configuration
│   └── log.py
│
├── templates/                  # HTML templates
│   ├── charge.html
│   └── status.html
│
└── tests/                      # Test suite
    ├── conftest.py            # Pytest configuration
    ├── routes/                # Route tests
    ├── integrations/          # Integration tests
    └── data/                  # Test data fixtures
```

## 🏗️ Architecture Notes

### Data Synchronization

Since data resides in two places (PostgreSQL database and HubSpot CRM), data consistency is critical. The current implementation:

- **Sequential Processing**: Records are created sequentially to maintain consistency
- **Rollback Mechanism**: If creation succeeds in the database but fails in HubSpot, the database record is rolled back
- **Future Enhancement**: Plans to migrate to an event-based architecture to decouple the logic and improve scalability

### Data Storage

- **PostgreSQL**: Primary internal datastore for all application data
- **HubSpot**: External CRM for business use and customer relationship management

## 📝 Logging

Application logs are saved to:
- **Local Logs**: `logs/app.json` (JSON format)
- **AWS DynamoDB**: Request logs are pushed to DynamoDB for centralized logging and audit purposes

## ✅ Task Checklist

For detailed development tasks and progress tracking, see [task_checklist.md](task_checklist.md).

## 🤝 Contributing

1. Create a feature branch
2. Make your changes
3. Write/update tests
4. Ensure all tests pass
5. Submit a pull request

## 📄 License

[Add your license information here]

---

For more information or support, please refer to the API documentation at `/docs` when the server is running.