# Heal hospital System

A modular backend for hospital management built using Clean Architecture, featuring a clear separation of layers, repositories, use cases, and extension points for email, automation, and analytics.

---

# 1. Overview

Provides a consistent REST API for clinical, administrative, financial, operational monitoring, and analytical reporting workflows.

---

# 2. Project Scope

* Patient management
* Doctor management
* Health insurance providers
* Hospital departments
* Beds
* Medications
* Medical specialties
* Appointments
* Hospitalizations
* Prescriptions
* Medical exams
* Billing
* Email notifications
* Analytics
* Process automation

---

# 3. Functional Requirements

| Code | Requirement            | Description                                                            |
| ---- | ---------------------- | ---------------------------------------------------------------------- |
| RF01 | Patient Management     | Full CRUD with address association                                     |
| RF02 | Doctor Management      | Full CRUD with specialties                                             |
| RF03 | Medical Specialties    | CRUD                                                                   |
| RF04 | Appointment Scheduling | Appointment date must be today or later. Doctor and patient must exist |
| RF05 | Medical Exams          | Register and list exams by patient or doctor                           |
| RF06 | Prescriptions          | Associate medications with doctors and patients                        |
| RF07 | Medications            | Medication catalog CRUD                                                |
| RF08 | Hospitalizations       | Admit, update, and discharge patients                                  |
| RF09 | Beds                   | Occupancy management                                                   |
| RF10 | Departments            | CRUD                                                                   |
| RF11 | Health Insurance       | CRUD                                                                   |
| RF12 | Financial Management   | Validated financial records                                            |
| RF13 | Addresses              | Linked to patients                                                     |
| RF14 | Validation             | Cerberus schemas                                                       |
| RF15 | Error Handling         | Standardized HTTP responses                                            |
| RF16 | Email Notifications    | Welcome emails, password reset, verification tokens                    |
| RF17 | Occupancy Reports      | Bed occupancy by department and period                                 |
| RF18 | Healthcare Statistics  | Statistics by doctor, specialty, and period                            |
| RF19 | Financial Analytics    | Revenue, average ticket, outstanding payments                          |
| RF20 | Average Length of Stay | Average hospitalization duration                                       |
| RF21 | Readmission Indicators | Patients readmitted within a configurable period                       |
| RF22 | Appointment Reminder   | Automated email/webhook reminders                                      |
| RF23 | Bed Availability Event | Publish event when a bed becomes available                             |
| RF24 | Automatic Billing      | Generate billing on patient discharge                                  |
| RF25 | Daily Report           | Generate PDF/CSV reports automatically                                 |
| RF26 | Exception Monitoring   | Structured logging and alerts                                          |

---

# 4. Non-Functional Requirements

| Code  | Category            | Description                            |
| ----- | ------------------- | -------------------------------------- |
| NFR01 | Architecture        | Clean Architecture                     |
| NFR02 | Testability         | Mockable repositories and use cases    |
| NFR03 | Maintainability     | Domain isolated from infrastructure    |
| NFR04 | Extensibility       | Dependency injection through composers |
| NFR05 | Consistency         | Standard HTTP response model           |
| NFR06 | Observability       | Structured logging                     |
| NFR07 | Security            | Authentication and RBAC (future)       |
| NFR08 | Portability         | Environment-based configuration        |
| NFR09 | Data Integrity      | Domain validations and foreign keys    |
| NFR10 | Analytics           | Extensible analytics layer             |
| NFR11 | SMTP Isolation      | Email provider abstraction             |
| NFR12 | Scalable Automation | Future queue integration               |

---

# 5. Architecture

Flow:

```
Route
    ↓
Adapter
    ↓
Controller
    ↓
Use Case
    ↓
Repository Interface
    ↓
Infrastructure Repository
    ↓
Database
```

Project layers:

```
domain/
data/
infra/
presentation/
main/
validation/
errors/
```

Future modules:

* analytics/
* automation/
* event bus

---

# 6. Data Models

Example entities:

* Patient
* Doctor
* Appointment
* Hospitalization
* Bed
* Financial Record

Aggregated analytics are calculated dynamically and are not persisted.

---

# 7. Analytics

Implemented metrics include:

* Bed occupancy rate
* Appointments by specialty
* Revenue by insurance provider
* Average billing
* Average hospitalization duration
* Readmission rate

---

# 8. Automation

Automated workflows include:

* Appointment reminders
* Automatic billing
* Bed availability events
* Daily report generation
* Exception monitoring

Designed to be compatible with schedulers and future queue systems.

---

# 9. Error Handling

Standard response format:

```json
{
  "error": [
    {
      "title": "HttpBadRequestError",
      "message": "Error details"
    }
  ]
}
```

Centralized error mapping is handled by `errors/error_handler.py`.

---

# 10. Email Service

Interface:

```
SMTPServiceInterface
```

Implementation:

```
SMTPEmailService
```

Supported templates:

* Welcome
* Verification Token
* Password Reset
* Resend Token

Future support:

* Appointment reminders
* Daily reports

---

# 11. Project Structure

```
src/
├── domain/
├── data/
├── infra/
├── presentation/
├── main/
├── validation/
├── errors/
└── analytics/ (planned)
```

---

# 12. Environment Variables

```env
DB_USER=
DB_PASSWORD=
DB_HOST=localhost
DB_PORT=3306
DB_NAME=

MAIL_USERNAME=
MAIL_PASS=
MAIL_FROM=
MAIL_FROM_NAME=Hospital
MAIL_PORT=587
MAIL_SERVER=smtp.gmail.com
```

---

# 13. Running the Project

```bash
python3 -m venv venv

source venv/bin/activate

pip install -r requirements.txt

cp src/.env.example src/.env

python run.py
```

Base URL:

```
http://127.0.0.1:8000/v1/
```

---

# 14. Tests

```bash
pytest -q
```

Testing strategy:

* Repository spies
* Isolated use case tests
* Mocked analytics and automation

---

# 15. Roadmap

| Phase | Goal                    |
| ----- | ----------------------- |
| 1     | CRUD modules            |
| 2     | Email service           |
| 3     | Analytics               |
| 4     | Automation              |
| 5     | Daily reporting         |
| 6     | Async tasks and queues  |
| 7     | Authentication and RBAC |

---

# 16. License

Educational and internal use.

---

# 17. Technical Summary

A modular backend designed for scalability and maintainability through Clean Architecture. The system separates business rules from infrastructure, making it straightforward to extend with analytics, automation, event-driven workflows, and future integrations without impacting the core domain.

---

# 18. Planned Analytics

| Code | Analysis                  | Purpose                        |
| ---- | ------------------------- | ------------------------------ |
| AD01 | Occupancy Trend           | Bed occupancy over time        |
| AD02 | Appointments by Specialty | Healthcare demand analysis     |
| AD03 | Revenue Analytics         | Billing insights               |
| AD04 | Readmission Analysis      | Patient readmission monitoring |
| AD05 | Medication Consumption    | Inventory forecasting          |

Implementation strategy:

* Dedicated `analytics/` module
* Read-only endpoints
* SQL aggregation queries
* Pure functions for easy testing

---

# 19. Planned Automations

| Code   | Automation                     |
| ------ | ------------------------------ |
| AUTO01 | Low bed availability alert     |
| AUTO02 | Outstanding payment reminder   |
| AUTO03 | Delayed exam notification      |
| AUTO04 | Long hospitalization detection |
| AUTO05 | Medication restocking alert    |

Initial implementation:

* Scheduler
* Automation services
* Central dispatcher
* Future support for Redis/Celery

---

# 20. Incremental Development Plan

| Step | Deliverable          |
| ---- | -------------------- |
| 1    | Analytics module     |
| 2    | Financial analytics  |
| 3    | Scheduler            |
| 4    | Automation workflows |
| 5    | Materialized metrics |
| 6    | Queue abstraction    |

Design principles:

* No Big Data dependencies
* SQL-based analytics
* SMTP-based notifications
* Highly testable pure functions
