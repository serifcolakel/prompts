---
name: Python DevOps & CI/CD Guide
description: >
  Comprehensive guide for Python project DevOps, CI/CD pipelines, Docker containerization,
  and deployment patterns. Covers GitHub Actions, Docker multi-stage builds,
  Docker Compose, and production deployment strategies.
---

# 🚀 Python DevOps & CI/CD Guide — AI Coding Agent

## Purpose
Standardize Python DevOps practices including CI/CD pipelines, containerization, and deployment workflows.

## CI/CD Pipeline — GitHub Actions

### `.github/workflows/ci.yml`
```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install uv
      - run: uv sync --group dev
      - run: ruff check .
      - run: mypy --strict src/

  test:
    runs-on: ubuntu-latest
    needs: lint
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install uv
      - run: uv sync --group dev
      - run: pytest --cov=src --cov-report=xml
      - uses: codecov/codecov-action@v4

  build:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install build
      - run: python -m build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/
```

### `.github/workflows/publish.yml`
```yaml
name: Publish
on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install build twine
      - run: python -m build
      - run: twine upload dist/*
        env:
          TWINE_USERNAME: __token__
          TWINE_PASSWORD: ${{ secrets.PYPI_TOKEN }}
```

## Docker — Multi-Stage Build

### `Dockerfile`
```dockerfile
FROM python:3.12-slim AS base
WORKDIR /app
RUN pip install --no-cache-dir uv

FROM base AS builder
COPY pyproject.toml .
RUN uv sync --frozen --no-install-project

FROM base AS runtime
COPY --from=builder /app/.venv /app/.venv
COPY src/ /app/src/
ENV PATH="/app/.venv/bin:$PATH"
EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### `docker-compose.yml`
```yaml
services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql://user:pass@db:5432/app
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: app
    healthcheck:
      test: pg_isready
      interval: 5s
      timeout: 5s
      retries: 5
```

## Environment Management
- Use `.env` files with `python-dotenv` or `pydantic-settings`
- Never commit `.env` files or secrets to version control
- Separate dev, staging, and production configs
- Use environment variables for all sensitive data

## Production Checklist
- [ ] Gunicorn or Uvicorn workers configured
- [ ] Health check endpoint (`/health`) implemented
- [ ] Structured logging (JSON format)
- [ ] Graceful shutdown handling
- [ ] Database connection pooling enabled
- [ ] Redis cache configured (if applicable)
- [ ] Error tracking (Sentry) integrated
- [ ] Log aggregation configured

## Anti-Patterns
- ❌ Using `python:latest` tag in Docker
- ❌ Running containers as root
- ❌ Building images with root-owned files
- ❌ Exposing development interfaces in production
- ❌ Missing health check endpoints
- ❌ Hardcoded environment-specific values

---
*Version: 1.0*
