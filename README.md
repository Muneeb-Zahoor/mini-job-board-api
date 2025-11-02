# 🚀 Mini Job Board API (Django REST Framework) mini-job-board-api
A simplified backend API for a job board application built with Django REST Framework (DRF), featuring JWT authentication, custom permissions, and job filtering.

## Project Overview

This is a robust backend API for a simplified job board application built using **Django REST Framework (DRF)**. [cite_start]It enables users to register, authenticate using JWT, manage their own companies, post jobs linked to their company, and apply to job listings[cite: 13, 14].

[cite_start]The project successfully implements all core requirements, including JWT authentication, custom permissions, query filtering, and the use of Django Signals[cite: 41, 51, 48, 54].

### Key Features Implemented

| Feature | Description | Status |
| :--- | :--- | :--- |
| **Authentication** | [cite_start]JWT-based authentication using `djangorestframework-simplejwt`[cite: 41]. | ✅ Complete |
| **Object Security** | [cite_start]Custom Permissions ensure only the **owner** can manage their Company and Jobs[cite: 26, 51]. | ✅ Complete |
| **Signals Task** | [cite_start]A Django Signal automatically creates a **Default Company** upon user registration (e.g., "Default Company for `<username>`")[cite: 55, 56]. | ✅ Complete |
| **Job Filtering** | [cite_start]Supports powerful filtering on `GET /api/jobs/` by **location** and **company name**. | ✅ Complete |
| **Job Application** | [cite_start]Implements the `POST /api/jobs/{id}/apply/` endpoint[cite: 52]. | ✅ Complete |
| **Bonus Task** | [cite_start]Prevents duplicate job applications by the same user[cite: 60]. | ✅ Complete |

---

## ⚙️ Setup and Installation

Follow these steps to set up and run the project locally.

### 1. Clone the Repository

Clone the project code and navigate to the root directory (where `manage.py` is located).

```
git clone [https://github.com/your-username/mini-job-board-api.git](https://github.com/your-username/mini-job-board-api.git)
cd mini-job-board-api
```
### 2. Create and Activate Virtual Environment
We recommend creating and activating a Python virtual environment to manage dependencies cleanly.
```
# For Linux/macOS
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
.\venv\Scripts\activate
```
### 3. Install Dependencies
Install all necessary packages, including DRF, Simple JWT, and Django Filters.
```
pip install django djangorestframework djangorestframework-simplejwt django-filter
```
### 4. Database Setup
Apply all database migrations to set up the models (User, Company, Job, JobApplication).
```
python manage.py makemigrations jobs
python manage.py migrate
```
### 5. Run the Server
Start the Django development server.
```
python manage.py runserver
```
The API Root (Documentation) is now accessible at: `http://127.0.0.1:8000/`

### API Endpoints and Usage Guide
All endpoints are built on REST principles. Use Postman or cURL for testing.

#### Authentication Flow
Step,Method,Endpoint,Description
1. Register,POST,/api/register/ [cite: 44],Register a new user. (Public)
2. Login,POST,/api/token/ [cite: 45],Obtain Access/Refresh tokens. (Public)
3. Refresh,POST,/api/token/refresh/,Obtain a new Access Token using the refresh token.

Auth Requirement: For all subsequent authenticated endpoints, include the Access Token in the header: `Authorization: Bearer <ACCESS_TOKEN>`

#### Company and Job Management Endpoints
Resource,Method,Endpoint,Permissions,Description
Companies,POST,/api/companies/ [cite: 47],IsAuthenticated,Create a new company. created_by is set automatically.
Companies,PUT/DELETE,/api/companies/{id}/,Custom Owner,Update/Delete company. Only allowed by the creator[cite: 26].
Jobs,POST,/api/jobs/ [cite: 51],Custom Owner,Create a new job. User must own the company ID in the payload.
Jobs,POST,/api/jobs/{id}/apply/ [cite: 52],IsAuthenticated,Apply to a job. Prevents duplicate applications.

#### Filtering Jobs Example
The `GET /api/jobs/` endpoint supports filtering by location and company.
Query Parameter,Example URL,Result
location,GET `/api/jobs/?location=Lahore`,"Lists jobs with the exact location ""Lahore""."
company_name,GET `/api/jobs/?company_name=Acme`,Filters jobs by company name (case-insensitive partial match).
