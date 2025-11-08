# Contributing to AI Real Estate Assistant

First off, thank you for considering contributing to AI Real Estate Assistant! It's people like you that make this project such a great tool.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Process](#development-process)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Documentation](#documentation)

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Getting Started

### Prerequisites

- Python 3.11 or higher
- Poetry for dependency management
- Git for version control
- OpenAI API key (for using GPT models)

### Development Setup

1. **Fork and clone the repository:**
   ```bash
   git clone https://github.com/your-username/ai-real-estate-assistant.git
   cd ai-real-estate-assistant
   ```

2. **Install dependencies using Poetry:**
   ```bash
   poetry install
   ```

3. **Activate the virtual environment:**
   ```bash
   poetry shell
   ```

4. **Set up environment variables:**
   Create a `.streamlit/secrets.toml` file with your API keys:
   ```toml
   OPENAI_API_KEY = "your-api-key-here"
   ```

5. **Run the application:**
   ```bash
   # For V1 (Dataframe Agent)
   ./scripts/run_v1.sh

   # For V2 (RAG Implementation)
   ./scripts/run_v2.sh
   ```

## Development Process

### Branch Naming Convention

- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation changes
- `refactor/description` - Code refactoring
- `test/description` - Adding or updating tests

### Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(ui): add dark mode theme toggle

- Implement CSS variables for theming
- Add system preference detection
- Ensure WCAG 2.1 AA compliance

Closes #123
```

```
fix(agent): handle empty dataframe gracefully

- Add validation for empty CSV data
- Display user-friendly error message
- Prevent agent initialization with no data

Fixes #456
```

## Pull Request Process

### Before Submitting

1. **Ensure your code follows our coding standards**
2. **Add or update tests as necessary**
3. **Update documentation to reflect your changes**
4. **Run all tests and ensure they pass**
5. **Test the UI in both dark and light modes**
6. **Check accessibility compliance**

### Submitting a Pull Request

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes and commit:**
   ```bash
   git add .
   git commit -m "feat: your feature description"
   ```

3. **Push to your fork:**
   ```bash
   git push origin feature/your-feature-name
   ```

4. **Open a Pull Request:**
   - Provide a clear title and description
   - Reference any related issues
   - Include screenshots for UI changes
   - Describe testing performed

### PR Review Process

- At least one maintainer review is required
- All CI checks must pass
- Address review feedback promptly
- Keep PR scope focused and manageable

## Coding Standards

### Python Style Guide

We follow [PEP 8](https://pep8.org/) with some modifications:

- **Line length:** 100 characters maximum
- **Imports:** Grouped and sorted (stdlib, third-party, local)
- **Docstrings:** Google style for all public functions and classes
- **Type hints:** Required for function signatures

### Code Formatting

We use the following tools:

```bash
# Black for code formatting
black src/

# isort for import sorting
isort src/

# flake8 for linting
flake8 src/
```

### Example Code Structure

```python
"""
Module description goes here.

This module handles XYZ functionality...
"""

from typing import List, Optional
import pandas as pd
import streamlit as st

from src.common.cfg import SETTINGS
from src.utils import utils


class MyClass:
    """
    Brief class description.

    Detailed description of what this class does,
    its responsibilities, and usage examples.

    Attributes:
        attribute_name (type): Description of attribute
    """

    def __init__(self, param: str):
        """
        Initialize MyClass.

        Args:
            param: Description of parameter
        """
        self.attribute_name = param

    def public_method(self, arg1: str, arg2: Optional[int] = None) -> bool:
        """
        Brief method description.

        Detailed description of what this method does.

        Args:
            arg1: Description of first argument
            arg2: Description of optional second argument

        Returns:
            Description of return value

        Raises:
            ValueError: When arg1 is invalid
        """
        if not arg1:
            raise ValueError("arg1 cannot be empty")

        return True
```

## Testing Guidelines

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src tests/

# Run specific test file
pytest tests/test_agent.py
```

### Writing Tests

- Write unit tests for all new functions
- Aim for >80% code coverage
- Use descriptive test names: `test_<function>_<scenario>_<expected_result>`
- Include both positive and negative test cases

### Example Test

```python
"""Tests for the theme manager module."""

import pytest
from src.ui.theme_manager import ThemeManager


class TestThemeManager:
    """Test suite for ThemeManager class."""

    def test_get_current_theme_returns_default(self):
        """Test that default theme is 'auto' when not set."""
        manager = ThemeManager()
        assert manager.get_current_theme() == "auto"

    def test_apply_theme_updates_preference(self):
        """Test that applying a theme updates the preference."""
        manager = ThemeManager()
        manager.apply_theme("dark")
        assert manager.get_current_theme() == "dark"
```

## Documentation

### Code Documentation

- **All public functions must have docstrings**
- **Complex logic should have inline comments**
- **Use type hints for clarity**
- **Keep docstrings up to date with code changes**

### Project Documentation

When adding features or making significant changes:

1. Update relevant documentation files in `/docs`
2. Add examples to demonstrate usage
3. Update the README if user-facing changes
4. Document any new configuration options

### Documentation Structure

```
docs/
├── images/           # Screenshots and diagrams
├── PRD.md           # Product Requirements Document
├── TODO.md          # Project roadmap and todos
└── ARCHITECTURE.md  # (Future) System architecture
```

## UI/UX Guidelines

### Dark Mode Compliance

- Test all UI changes in both light and dark modes
- Ensure contrast ratios meet WCAG 2.1 AA standards (4.5:1 minimum)
- Use CSS variables from `src/ui/styles.css`
- Verify form labels are clearly visible

### Accessibility

- All interactive elements must be keyboard accessible
- Provide clear focus indicators
- Use semantic HTML
- Include proper ARIA labels where needed
- Test with screen readers when possible

### Responsive Design

- Test on mobile, tablet, and desktop viewports
- Use Streamlit's responsive column layouts
- Ensure touch targets are at least 44x44px
- Avoid horizontal scrolling

## Questions?

If you have questions about contributing:

1. Check existing issues and discussions
2. Review our documentation in `/docs`
3. Open a new issue with the `question` label
4. Reach out to maintainers

Thank you for contributing to AI Real Estate Assistant! 🎉
