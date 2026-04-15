# FastAPI Todo App — Unit & Integration Tests

A full-featured RESTful Todo API built with **FastAPI**, **SQLAlchemy**, and **SQLite**, complete with JWT authentication, role-based access control, database migrations via Alembic, and a comprehensive test suite covering both unit and integration tests.

---

## Features

- JWT-based authentication (register, login, token refresh)
- Role-based access control (admin vs regular user)
- Full CRUD for todos (create, read, update, delete)
- User profile management (change password, change phone number)
- Database migrations with Alembic
- Unit tests and integration tests using `pytest` and `TestClient`

---

## Project Structure

```
fastapi-todo-unit-integration-tests/
├── __init__.py
├── main.py                  # FastAPI app entry point
├── database.py              # SQLAlchemy engine and session setup
├── models.py                # Users and Todos ORM models
├── alembic.ini              # Alembic configuration
├── alembic/
│   ├── env.py               # Alembic migration environment
│   ├── script.py.mako       # Migration file template
│   └── versions/
│       └── aeff25f89db0_create_phone_number_for_user_col.py
├── routers/
│   ├── __init__.py
│   ├── auth.py              # Registration and JWT login
│   ├── todos.py             # Todo CRUD endpoints
│   ├── admin.py             # Admin-only endpoints
│   └── users.py             # User profile endpoints
└── test/
    ├── __init__.py
    ├── utils.py             # Shared fixtures and test database setup
    ├── test_example.py      # Basic pytest examples (unit tests)
    ├── test_main.py         # Health check endpoint test
    ├── test_auth.py         # Auth router integration tests
    ├── test_todos.py        # Todos router integration tests
    ├── test_admin.py        # Admin router integration tests
    └── test_users.py        # Users router integration tests
```

---

## API Endpoints

### Auth — `/auth`

| Method | Endpoint       | Description              | Auth Required |
|--------|----------------|--------------------------|---------------|
| POST   | `/auth/`       | Register a new user      | No            |
| POST   | `/auth/token`  | Login and get JWT token  | No            |

### Todos — `/`

| Method | Endpoint          | Description            | Auth Required |
|--------|-------------------|------------------------|---------------|
| GET    | `/`               | Get all your todos     | Yes           |
| GET    | `/todo/{id}`      | Get a single todo      | Yes           |
| POST   | `/todo`           | Create a new todo      | Yes           |
| PUT    | `/todo/{id}`      | Update a todo          | Yes           |
| DELETE | `/todo/{id}`      | Delete a todo          | Yes           |

### Admin — `/admin`

| Method | Endpoint             | Description         | Auth Required    |
|--------|----------------------|---------------------|------------------|
| GET    | `/admin/todo`        | Get all todos       | Yes (admin only) |
| DELETE | `/admin/todo/{id}`   | Delete any todo     | Yes (admin only) |

### Users — `/user`

| Method | Endpoint                       | Description             | Auth Required |
|--------|--------------------------------|-------------------------|---------------|
| GET    | `/user/`                       | Get current user info   | Yes           |
| PUT    | `/user/password`               | Change password         | Yes           |
| PUT    | `/user/phonenumber/{number}`   | Change phone number     | Yes           |

### Health

| Method | Endpoint    | Description  |
|--------|-------------|--------------|
| GET    | `/healthy`  | Health check |

---

## Getting Started

### Prerequisites

- Python 3.10+
- `pip`

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/devarifkhan/fastapi-todo-unit-integration-tests.git
cd fastapi-todo-unit-integration-tests

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate       # Linux/macOS
venv\Scripts\activate          # Windows

# 3. Install dependencies
pip install fastapi uvicorn sqlalchemy alembic passlib[bcrypt] python-jose[cryptography] pytest httpx pytest-asyncio
```

### Run the Application

```bash
uvicorn fastapi-todo-unit-integration-tests.main:app --reload
```

The API will be available at `http://127.0.0.1:8000`.  
Interactive docs: `http://127.0.0.1:8000/docs`

### Run Database Migrations

```bash
alembic upgrade head
```

---

## Running Tests

```bash
pytest
```

To see detailed output:

```bash
pytest -v
```

Tests use an in-memory SQLite database (`testdb.db`) and override FastAPI dependencies so no real database or JWT token is needed during testing.

---

## Authentication Flow

1. Register a user via `POST /auth/`
2. Login via `POST /auth/token` — returns a JWT access token (valid 20 minutes)
3. Include the token in subsequent requests:
   ```
   Authorization: Bearer <your_token>
   ```

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| FastAPI | Web framework |
| SQLAlchemy | ORM and database layer |
| SQLite | Database (development) |
| Alembic | Database migrations |
| Passlib + bcrypt | Password hashing |
| python-jose | JWT encoding/decoding |
| pytest + httpx | Testing |
