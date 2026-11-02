# Hospital Management Application

A full-stack hospital operations platform with a Flask REST API and a Vue.js single-page frontend. It supports role-based workflows for administrators, doctors, and patients, including appointments, departments, treatment history, dashboards, exports, reminders, and email notifications.

## Features

- Authentication and role-based access for administrators, doctors, and patients
- Patient, doctor, department, appointment, and treatment management
- Separate dashboards and workflows for each role
- Appointment scheduling and treatment-history tracking
- Background reminder, reporting, and export tasks with Celery
- Redis-backed caching and Celery broker/result storage
- REST API consumed by the Vue frontend
- Email notifications through configurable SMTP settings

## Technology Stack

- **Backend:** Python, Flask, Flask-Security, SQLAlchemy, Flask-RESTX, Celery
- **Frontend:** Vue 3, Vite, Vue Router, Pinia, Bootstrap 5
- **Data and services:** SQLite for local development, Redis for caching and task queues

## Project Structure

```text
.
├── backend/
│   ├── app.py              # Flask and Celery application entry point
│   ├── models.py           # Database models
│   ├── resources/          # Authentication and REST API resources
│   ├── services/           # Application and business logic
│   ├── tasks/              # Celery reminder, report, and export tasks
│   └── req.txt             # Backend dependencies
└── frontend/
    ├── src/pages/          # Role-specific Vue pages
    ├── src/components/     # Shared UI components
    ├── src/stores/         # Pinia state stores
    ├── src/utils/api.js    # Backend API client
    └── package.json        # Frontend scripts and dependencies
```

## Prerequisites

- Python 3.10+
- Node.js 18+
- Redis running on `localhost:6379` for cache and Celery services
- An SMTP service for email notifications (MailHog is suitable for local development)

## Run Locally

### Backend

```bash
cd backend
python -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
pip install -r req.txt
python app.py
```

The backend selects an available port starting at `5000` and exposes the API under `/api`.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Use the URL printed by Vite. Configure the frontend API base URL in `frontend/src/utils/api.js` if the backend is running on a different host or port.

### Background Workers

Redis must be running before starting Celery workers. From the `backend` directory, use the project’s Celery application to start a worker and beat scheduler according to your environment.

## Configuration

The backend loads environment variables from `.env`. Important settings include:

- `SECRET_KEY`
- `SECURITY_PASSWORD_SALT`
- `REDIS_URL`
- `SMTP_HOST`, `SMTP_PORT`, `SMTP_USERNAME`, `SMTP_PASSWORD`
- `SENDER_EMAIL`

The checked-in defaults are intended for local development only. Set production secrets and use a production database and SMTP provider before deployment.

## Deployment

No production deployment configuration is currently included. Because the application requires a Flask API, Redis, a database, and background workers, it should be deployed as coordinated frontend, backend, Redis, and worker services rather than as a static site.

## License

See [LICENSE](LICENSE) for license information.
