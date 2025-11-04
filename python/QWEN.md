---

# AI Coding Guidelines for Python Projects  
**Version 1.1**  
*Last updated: November 5, 2025*

This document defines how AI assistants (e.g., GitHub Copilot, Claude, etc.) should generate or modify code in this repository.  
**Goal**: Ensure all AI-generated output is consistent, production-ready, secure, and aligned with our engineering standards.

---

## ✅ General Rules

- **Follow PEP 8** strictly for formatting, naming (`snake_case`), and imports (grouped in standard → third-party → local order).
- **Use type hints** for all function parameters, return types, and class attributes (Python ≥3.8 assumed).
- **Include docstrings** for all public functions, classes, and modules using **Google style** (preferred) or NumPy style—*choose one per project and stay consistent*.
- **Prefer f-strings** over `.format()` or `%` formatting.
- **Write tests** for all new functionality using `pytest`. New code without tests should not be accepted.
- **Never use `print()` for logging**—use the `logging` module instead.
- **Avoid placeholder code** like `# TODO` or commented-out logic unless linked to a tracked issue.

---

## 📁 Project Structure

- **Do not generate monolithic files**. Break logic into small, focused modules.
- **Keep modules under ~800 lines** for readability and testability.
- Use the following directory layout:
  ```
  project/
  ├── src/
  │   └── your_package/       # Python package root (contains __init__.py if needed)
  ├── tests/
  │   └── test_*.py           # Match module names: test_utils.py ↔ utils.py
  ├── scripts/                # Standalone runnable scripts (e.g., data ingestion)
  ├── docs/                   # All documentation except README.md
  ├── README.md
  ├── .env                    # Environment variables (add to .gitignore!)
  └── config.yaml             # Optional structured config
  ```
- **Never hardcode configuration** (API keys, paths, URLs). Use `.env` (loaded via `python-dotenv`) or `config.yaml`.
- Add `.env` to `.gitignore` to prevent secrets leakage.

---

## 📦 Dependencies

- Declare dependencies in **`pyproject.toml`** (preferred) or `requirements.txt`.
- **Prefer the Python standard library** over external packages when functionality overlaps.
- **When adding a new library**, include a brief justification in a comment:
  ```toml
  # pydantic: required for runtime config validation and data parsing
  pydantic = ">=2.0"
  ```
- Pin dependency versions in deployment contexts (e.g., `requirements.lock.txt`).

---

## 🛑 Error Handling

- **Never silently ignore errors** (`except: pass` is forbidden).
- **Define a base custom exception** for the project:
  ```python
  class ProjectError(Exception):
      """Base exception for all project-specific errors."""
  ```
- Use derived exceptions for specific cases (e.g., `DataLoadError(ProjectError)`).
- **Log errors with context** using the `logging` module:
  ```python
  logger.error("Failed to load file", exc_info=True, extra={"path": path})
  ```
- Catch **specific exceptions** (e.g., `FileNotFoundError`, `ValueError`) rather than generic `Exception`.

---

## 🧪 Testing

- All new logic must include **unit tests** in the `tests/` directory.
- Use `pytest` fixtures and `@pytest.mark.parametrize` for reusable, data-driven tests.
- **Mock external dependencies** (files, APIs, databases) to keep tests fast and isolated.
- Aim for **≥90% coverage** on core modules (enforced via CI if possible).

---

## 📝 Documentation

- Keep `README.md` in the root for project overview and setup.
- Store all other documentation (tutorials, architecture notes, API docs) in `docs/`.
- Write docstrings so they work with tools like **Sphinx** or **MkDocs** (include types and examples when helpful).
- Optionally maintain a `CHANGELOG.md` for versioned releases.

---

## 🧠 AI-Specific Guardrails

- **Never generate insecure or placeholder code** (e.g., hardcoded credentials, `eval()`, `exec()`).
- **Avoid redundant comments** (e.g., `# return the result`).
- Prefer **pure functions** (no side effects) where possible to improve testability.
- Ensure generated code is **idempotent** and **deterministic** unless randomness is explicitly required.

---

## 📌 Examples

### ✅ Good
```python
import logging
import os
from pathlib import Path
import pandas as pd

logger = logging.getLogger(__name__)

def load_data(path: str) -> pd.DataFrame:
    """Load a CSV file into a pandas DataFrame.

    Args:
        path: Path to the CSV file.

    Returns:
        DataFrame containing the loaded data.

    Raises:
        FileNotFoundError: If the file does not exist.
    """
    file_path = Path(path)
    if not file_path.exists():
        logger.error("Data file not found: %s", path)
        raise FileNotFoundError(f"File not found: {path}")
    return pd.read_csv(file_path)
```

### ❌ Bad
```python
def loaddata(p):
    return pd.read_csv(p)  # No type hints, poor naming, no error handling, no docstring
```

---
