---
name: Python Testing Guide
description: >
  Comprehensive testing prompt for Python projects. Covers pytest conventions,
  fixtures, mocking, async testing, coverage, property-based testing,
  and CI integration patterns.
---

# 🧪 Python Testing Guide — AI Coding Agent

## Purpose
Standardize Python testing practices across AI-assisted development for reliable, maintainable test suites.

## Testing Stack
- **pytest** — Main test framework
- **pytest-asyncio** — Async test support
- **pytest-cov** — Coverage reporting
- **pytest-mock** — Mocking utilities
- **hypothesis** — Property-based testing
- **factory-boy** — Test data factories
- **httpx** — Async HTTP client for API testing

## Test Organization

```
tests/
├── unit/           # Fast, isolated unit tests
├── integration/    # Cross-component tests
├── e2e/            # End-to-end tests
├── conftest.py     # Shared fixtures
└── factories.py    # Factory-boy definitions
```

## Core Conventions

### 1. Test Naming
```python
def test_user_service_returns_none_for_missing_user(): ...
def test_database_connection_raises_on_bad_credentials(): ...
```
Format: `test_<method>_<scenario>_<expected_behavior>`

### 2. Fixture Patterns
```python
@pytest.fixture
def sample_user():
    return {"id": 1, "name": "Test User", "email": "test@example.com"}

@pytest.fixture
def database_url(monkeypatch):
    return "postgresql://test:test@localhost:5432/testdb"
```

### 3. Async Testing
```python
@pytest.mark.asyncio
async def test_async_user_creation():
    result = await service.create_user(valid_data)
    assert result.id is not None
    assert result.name == "New User"
```

### 4. Parameterized Tests
```python
@pytest.mark.parametrize("input_data,expected", [
    ({"email": "a@b.com"}, True),
    ({"email": "invalid"}, False),
])
def test_email_validation(input_data, expected):
    assert is_valid_email(input_data["email"]) == expected
```

### 5. Mocking Patterns
```python
from unittest.mock import AsyncMock, patch

@pytest.fixture
def mock_repo():
    repo = AsyncMock()
    repo.get_user.return_value = {"id": 1, "name": "Mocked"}
    return repo

def test_with_mock(mock_repo):
    service = UserService(repo=mock_repo)
    user = await service.get_user(1)
    assert user["name"] == "Mocked"
    mock_repo.get_user.assert_called_once_with(1)
```

### 6. Coverage Targets
- Unit tests: >90%
- Integration tests: >70%
- Total project: >80%

## Anti-Patterns
- ❌ Testing implementation details instead of behavior
- ❌ Test interdependence (tests relying on previous test state)
- ❌ Over-mocking production code
- ❌ Long test functions (>3 assertions per test)
- ❌ Hardcoded test data in assertions
- ❌ Missing cleanup in teardown

---
*Version: 1.0*
