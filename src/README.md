# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- View activity participants without logging in
- Allow authenticated teachers to manage activity enrollments

## Getting Started

1. Install the dependencies:

   ```
   pip install -r requirements.txt
   ```

2. Configure a teacher account and a session-signing secret in the environment.
   The secret must be at least 32 characters; generate one with:

   ```
   python -c "import secrets; print(secrets.token_urlsafe(32))"
   ```

   Set the generated value as `SESSION_SECRET`, and set `TEACHER_USERNAME` and
   `TEACHER_PASSWORD` to the assigned teacher credentials. These values are
   read from environment variables and must not be committed to the repository.

3. Start the application from the repository root:

   ```
   uvicorn src.app:app --reload
   ```

   For deployments served over HTTPS, set `COOKIE_SECURE=true` so the teacher
   session cookie is sent only over secure connections. Leave it unset for
   local HTTP development.

4. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc
   - Activities page: http://localhost:8000/

## API Endpoints

| Method | Endpoint                                                          | Description                                                |
| ------ | ----------------------------------------------------------------- | ---------------------------------------------------------- |
| GET    | `/activities`                                                     | Publicly view activities and current participants          |
| GET    | `/auth/status`                                                    | Check whether the current browser has a teacher session    |
| POST   | `/auth/login`                                                     | Log in with the configured teacher username and password   |
| POST   | `/auth/logout`                                                    | End the current teacher session                             |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Teacher-only enrollment                                    |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Teacher-only unenrollment                               |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

Activity and enrollment data is stored in memory, which means it will be reset
when the server restarts.
