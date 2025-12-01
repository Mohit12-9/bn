# BookFlow Library Management System

BookFlow is a Streamlit-based library management system built to modernize borrowing, returns, and reservations for Bennett University. The app supports student, teacher, and admin roles with secure authentication, catalog browsing, and reservation notifications.

## Features

- **Role-based dashboards** for students, teachers, and administrators, including tailored catalog views and analytics. See `login_page()` and admin entry points in `bookflow.py`. @bookflow.py#2681-2751 @admin_portal.py#12-90
- **Secure credential storage** using PBKDF2 hashing with salt, guaranteeing passwords are never stored in plain text. @security_utils.py#1-74
- **Reservation workflow with email notifications**: creates queue records and optionally emails users via SMTP when a copy becomes available. @bookflow.py#2332-2363 @bookflow.py#1992-2067 @bookflow.py#3239-3365
- **Programme-specific catalogues** seeded from `program_catalog.py`, with automatic metadata normalization. @bookflow.py#1800-1954 @program_catalog.py#1-80
- **Streamlit dialogs and animations** for borrow/return confirmations and status feedback across the app. @bookflow.py#3239-3414

## Project Structure

```
├── admin_app.py          # Admin-only Streamlit entry helpers
├── admin_portal.py       # Admin dashboard UI and login screen
├── bookflow.py           # Main Streamlit application and business logic
├── bookflow_data.json    # Seed data for users, books, and transactions
├── program_catalog.py    # Programme categories and default catalog entries
├── security_utils.py     # Password hashing and legacy credential migration
├── .env.example          # Sample environment variables
├── requirements.txt      # Python dependencies
└── README.md             # This documentation
```

## Prerequisites

- Python 3.10+ (tested on 3.11)
- `pip` for dependency management
- Optional SMTP account (e.g., Gmail with App Password) for email notifications

## Installation

1. **Clone or extract the repository.**
2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
3. **Create the environment file**:
   ```bash
   copy .env.example .env  # Windows
   # or
   cp .env.example .env    # macOS / Linux
   ```
4. **Update `.env`** with real SMTP credentials and admin defaults before running.

## Running the Application

```bash
streamlit run bookflow.py
```

This starts the Streamlit server (default `http://localhost:8501`). Use the left sidebar to log in as a student or teacher; admins can launch the admin app separately using `streamlit run admin_app.py` if you want a dedicated portal.

## SMTP Configuration

The reservation email workflow uses the following variables (see `.env.example`):

- `BOOKFLOW_SMTP_SERVER`
- `BOOKFLOW_SMTP_PORT`
- `BOOKFLOW_SMTP_USERNAME`
- `BOOKFLOW_SMTP_PASSWORD`
- `BOOKFLOW_EMAIL_SENDER`

If any values are missing, the app gracefully warns users and logs the error in the UI flash banner. @bookflow.py#1992-2067 @bookflow.py#2840-2862

## Sample Credentials

If the JSON data is untouched, the app seeds default users:

- Student: `sairam / sairam123`
- Teacher: `prof_bohra / teacher123`
- Admin: defaults to the credentials configured in `.env` or `admin / morningstar123` (hashed at runtime).

All passwords are hashed on load via `ensure_password_fields`, so plain text credentials are converted automatically. @bookflow.py#2361-2403 @security_utils.py#53-74

## Troubleshooting

- **Email not sending:** Check SMTP credentials and network access; review the reservation banner for error details. @bookflow.py#2840-2862
- **Data not persisting:** Ensure the app has write permissions to `bookflow_data.json`.
- **Admin login failure:** Confirm the admin username/password in `.env` and restart the app to reload defaults.

## License

This project is for educational purposes within Bennett University coursework. Adapt as needed for academic submissions.
