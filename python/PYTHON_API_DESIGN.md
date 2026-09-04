---
name: Python API Design Guide
description: >
  Best practices for designing and building RESTful and GraphQL APIs in Python.
  Covers FastAPI conventions, validation, error handling, pagination, versioning,
  authentication, and documentation patterns.
---

# 🌐 Python API Design Guide — AI Coding Agent

## Purpose
Align AI coding agents with modern Python API development standards for building robust, documented, and scalable APIs.

## Tech Stack
- **FastAPI** — Primary framework (ASGI, auto OpenAPI docs)
- **Pydantic v2** — Request/response validation
- **SQLModel** or **SQLAlchemy 2.0** — ORM
- **Alembic** — Database migrations
- **Uvicorn** — ASGI server
- **httpx** — Async testing client

## API Design Principles

### 1. RESTful Conventions
```
GET    /api/v1/users           # List all users
GET    /api/v1/users/{id}      # Get single user
POST   /api/v1/users           # Create user
PUT    /api/v1/users/{id}      # Full update
PATCH  /api/v1/users/{id}      # Partial update
DELETE /api/v1/users/{id}      # Delete user
```

### 2. Versioning
- Use URL path versioning: `/api/v1/`, `/api/v2/`
- Bump major version on breaking changes
- Maintain previous version for 6+ months

### 3. Request/Response Schemas
```python
from pydantic import BaseModel, EmailStr, Field
from typing import Optional

class UserCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    email: EmailStr
    age: Optional[int] = Field(None, ge=0, le=150)

class UserResponse(BaseModel):
    id: int
    name: str
    email: EmailStr
    created_at: datetime
```

### 4. Error Handling
```python
from fastapi import HTTPException, status

class APIError(HTTPException):
    def __init__(self, detail: str, code: int = status.HTTP_400_BAD_REQUEST):
        super().__init__(status_code=code, detail=detail)

@app.exception_handler(APIError)
async def api_error_handler(request, exc):
    return JSONResponse(
        status_code=exc.status_code,
        content={"error": {"code": exc.status_code, "message": exc.detail}},
    )
```

### 5. Pagination
```python
from fastapi import Query
from typing import TypeVar, Generic

T = TypeVar("T")

class PaginatedResponse(BaseModel, Generic[T]):
    items: list[T]
    total: int
    page: int
    size: int
    pages: int

def paginate(
    items: list, page: int = Query(1, ge=1), size: int = Query(20, ge=1, le=100)
):
    start = (page - 1) * size
    end = start + size
    return PaginatedResponse(
        items=items[start:end],
        total=len(items),
        page=page,
        size=size,
        pages=(len(items) + size - 1) // size,
    )
```

### 6. Authentication & Authorization
- Use JWT tokens for authentication
- Implement role-based access control (RBAC)
- Never store plain-text passwords (use `bcrypt`)
- Use `security_scopes` for fine-grained permissions

### 7. Validation & Middleware
```python
@app.middleware("http")
async def add_security_headers(request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    return response
```

### 8. Documentation
- Auto-generated via FastAPI/OpenAPI at `/docs`
- Use `summary`, `description`, `responses` on routes
- Include example requests/responses

### 9. Testing APIs
```python
from httpx import AsyncClient, ASGITransport

@pytest.mark.asyncio
async def test_create_user():
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as client:
        response = await client.post("/api/v1/users", json={
            "name": "Test User",
            "email": "test@example.com",
        })
        assert response.status_code == 201
        assert response.json()["name"] == "Test User"
```

### 10. Rate Limiting & Performance
- Implement rate limiting (per IP, per user)
- Use Redis for distributed rate limiting
- Cache frequently accessed data with TTL
- Profile with `py-spy` or `viztracer`

## Anti-Patterns
- ❌ Returning raw SQLAlchemy models directly
- ❌ No input validation on endpoints
- ❌ Mixing business logic in route handlers
- ❌ Hardcoded secrets in API code
- ❌ No pagination on list endpoints
- ❌ Silent failures without error codes

---
*Version: 1.0*
