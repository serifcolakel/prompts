---
name: Python Best Practices
description: >
  Comprehensive prompt for aligning AI coding agents with modern Python development standards.
  Covers Python 3.12+ features, type hints, project structure, virtual environments,
  dependency management, async patterns, and testing conventions.
  Targets Claude Code, Cursor, Codex, OpenCode, and ChatGPT.
---

# 🐍 Python Best Practices — AI Coding Agent Guide

## Purpose
This prompt aligns AI coding agents with modern Python development standards to ensure consistent, maintainable, and production-ready code.

## Scope
- Python 3.12+ (latest stable)
- Type hints and static analysis
- Modern project structure and packaging
- Async/await patterns
- Testing with pytest
- Dependency management (uv, pip, poetry)

## Guidelines

### 1. Python Version & Features
- Always target Python 3.12+ as minimum
- Use structural pattern matching (`match/case`) for complex conditionals
- Use walrus operator (`:=`) for concise assignments in expressions
- Leverage f-string improvements (PEP 701) for efficient string formatting
- Use parenthesized context managers where beneficial

### 2. Type Hints & Static Analysis
- Always use type hints for function signatures, variables, and return types
- Use `typing.Optional`, `typing.Union`, or `X | None` syntax
- Prefer `typing.Protocol` and `typing.Generic` for abstractions
- Enable `mypy` in strict mode for all new projects
- Use `typing.TypeAlias` for complex type definitions
- Use `typing.Literal` for finite value constraints
- Use `typing.NamedTuple` or `typing.TypedDict` for structured data

### 3. Project Structure
```
project/
├── pyproject.toml          # Build system and dependencies
├── README.md
├── .python-version         # Minimum Python version
├── src/
│   └── package_name/
│       ├── __init__.py
│       ├── module_a.py
│       └── module_b.py
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   └── test_module_a.py
├── scripts/                # CLI scripts
└── docs/                   # Documentation
```

### 4. Dependency Management
- Use `uv` for fast dependency management and virtual environments
- Define all dependencies in `pyproject.toml` (no `requirements.txt`)
- Pin exact versions for production: `numpy==1.26.4`
- Use `[tool.uv]` configuration for reproducible builds
- Separate dev dependencies with `[dependency-groups.dev]`

### 5. Error Handling
- Prefer specific exceptions over bare `except:`
- Create custom exception hierarchies for domain-specific errors
- Use `logging` module instead of `print()` for production code
- Implement proper error propagation with context (`raise ... from ...`)
- Use `contextlib.suppress` only when intentional and documented

### 6. Async Patterns
- Use `async/await` for I/O-bound operations
- Prefer `asyncio.TaskGroup` for concurrent tasks (Python 3.11+)
- Use `asyncio.Semaphore` for concurrency limiting
- Implement proper graceful shutdown for async applications
- Never block the event loop — use `asyncio.to_thread` for CPU-bound work

### 7. Testing (pytest)
- Use pytest with `conftest.py` for shared fixtures
- Name test files `test_*.py` and test functions `test_*()`
- Use parameterized tests with `@pytest.mark.parametrize`
- Enable `pytest-asyncio` for async test support
- Aim for >80% coverage on new code
- Use `pytest-mock` for mocking dependencies
- Separate unit, integration, and E2E tests into distinct directories

### 8. Code Style
- Follow PEP 8 with modern conventions
- Use `ruff` as linter and formatter (replaces flake8, black, isort)
- Max line length: 100 characters
- Sort imports with `isort`-compatible ordering
- Use docstrings for public APIs (Google or Sphinx style)
- Keep functions focused and under 50 lines
- Single responsibility principle for modules

### 9. Documentation
- Use Markdown in `docs/` for architectural decisions
- Include docstrings with `Args/Returns/Yields/Raises` sections
- Generate API docs with `pdoc` or `sphinx`
- Maintain a `CHANGELOG.md` following Keep a Changelog format

### 10. CI/CD Checklist
- [ ] Lint with `ruff check`
- [ ] Type check with `mypy --strict`
- [ ] Run tests with `pytest --cov`
- [ ] Build wheel with `build`
- [ ] Publish to PyPI with `twine` or `uv publish`

## Anti-Patterns to Avoid
- ❌ Bare `except:` clauses
- ❌ Mutable default arguments (`def foo(items=[])`)
- ❌ Global mutable state in modules
- ❌ `from module import *` imports
- ❌ Nested functions for simple logic
- ❌ Deeply nested conditionals (>3 levels)
- ❌ Using `print()` for logging in production
- ❌ Mixing sync and async code without boundaries

## Example Pattern

```python
import logging
from typing import Protocol

logger = logging.getLogger(__name__)

class UserRepository(Protocol):
    async def get_user(self, user_id: int) -> dict | None: ...

class UserService:
    def __init__(self, repo: UserRepository) -> None:
        self._repo = repo

    async def get_user_profile(self, user_id: int) -> dict | None:
        user = await self._repo.get_user(user_id)
        if user is None:
            logger.warning("User %d not found", user_id)
            return None
        return self._format_profile(user)

    @staticmethod
    def _format_profile(user: dict) -> dict:
        return {
            "id": user["id"],
            "name": user["name"],
            "email": user["email"],
        }
```

---
*Last updated: 2025*
*Version: 1.0*
