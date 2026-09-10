# Django Course Management & Admin Project

**Django + PostgreSQL project focused on relational modeling, ORM relationships, migrations, and admin-site customization.**

This repository is an academic / hands-on Django project for modeling an online-course domain and managing that data through Django Admin. The most valuable part of the project is the data model: instructors, learners, courses, lessons, questions, choices, enrollments, and submissions are connected through foreign keys and many-to-many relationships.

## What this project demonstrates

- Django project/application structure
- Django ORM models and migrations
- PostgreSQL-backed development
- Foreign-key and many-to-many relationships
- Explicit relationship models with additional attributes
- Django Admin registration and customization
- Authentication-user integration through `AUTH_USER_MODEL`
- Model methods and domain logic
- Environment-based configuration instead of committed secrets

## Domain model

The application contains the following main entities:

```text
User
├── Instructor
└── Learner

Course
├── many Instructors
├── many Users through Enrollment
├── Lessons
└── Questions
    └── Choices

Enrollment
├── User
├── Course
├── enrollment date
├── course mode
└── rating

Submission
├── Enrollment
└── selected Choices
```

### Why `Enrollment` is an explicit model

A direct many-to-many relationship between users and courses would only record membership. In this project the relationship itself has data — enrollment date, mode, and rating — so Django's `through='Enrollment'` pattern is used.

That is a useful relational-modeling example because it separates:

- the **User** entity;
- the **Course** entity;
- the **relationship between them**, which has its own attributes.

## Main models

### Instructor

Associates a Django user with instructor-specific information such as full-time status and learner count.

### Learner

Associates a Django user with learner-specific data, including an occupation choice and social link.

### Course

Stores course information, instructors, enrolled users, publication data, and aggregate enrollment state.

### Lesson

Belongs to a course and stores ordered lesson content.

### Question / Choice

Represents assessment questions and answer options. `Question.is_get_score()` provides a small example of model-level scoring logic based on selected choice IDs.

### Enrollment / Submission

`Enrollment` models the user-course relationship. `Submission` connects an enrollment with selected choices for assessment-related workflows.

## Django Admin

The project uses Django Admin as a management interface for the domain models.

Examples include:

- registering models with the admin site;
- controlling which fields are shown/editable;
- using inline related models;
- managing course-related entities from a central interface.

This is useful for understanding how ORM models, admin configuration, and relational data work together before building a separate custom frontend.

## Project structure

```text
school_adminsite_django/
├── adminsite/
│   ├── admin.py        # Django Admin configuration
│   ├── models.py       # domain / relational models
│   ├── migrations/     # database schema history
│   ├── tests.py
│   └── views.py
├── myproject/
│   ├── settings.py     # project configuration
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
├── manage.py
├── requirements.txt
└── .env.example
```

## Local setup

### 1. Clone and create an environment

```bash
git clone https://github.com/josuecross/school_adminsite_django.git
cd school_adminsite_django

python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows
# .venv\Scripts\activate

pip install -r requirements.txt
```

### 2. Configure PostgreSQL

Copy the values from `.env.example` into your shell or preferred local environment loader.

Required settings include:

```env
DJANGO_SECRET_KEY=replace-with-a-local-development-secret
DJANGO_DEBUG=true
POSTGRES_DB=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your-local-postgres-password
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
```

`settings.py` reads these values from environment variables; credentials are not intended to be stored in source control.

### 3. Create the schema

```bash
python manage.py makemigrations
python manage.py migrate
```

### 4. Create an admin user

```bash
python manage.py createsuperuser
```

### 5. Run

```bash
python manage.py runserver
```

Then open:

```text
http://127.0.0.1:8000/admin/
```

## Engineering observations

This project is intentionally a learning project rather than a production-ready course platform. Useful next improvements would include:

- stronger automated tests around model behavior and negative cases;
- correcting exact-match scoring so extra incorrect selections cannot be accepted accidentally;
- moving repeated domain rules into clearer service/model boundaries;
- modernizing the Django/Python dependency versions;
- adding a custom API or frontend on top of the same relational model.

The scoring example is especially useful for QA reasoning: a happy-path test that selects all correct answers is not enough if the rule requires **exactly** the correct choices and no additional wrong choices.

## Portfolio relevance

This repository demonstrates my hands-on foundation in **Python web frameworks, Django ORM, relational modeling, migrations, PostgreSQL, administration workflows, and testing-oriented reasoning**. It complements my newer Python/FastAPI and API-oriented projects by showing another approach to persistence and application structure.

## Author

**Josue David Cruz Lopez**  
Costa Rica  
GitHub: [@josuecross](https://github.com/josuecross)  
LinkedIn: [josue-david-c](https://www.linkedin.com/in/josue-david-c/)
