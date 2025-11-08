# Development Guide

This guide provides comprehensive instructions for setting up your development environment and working with the AI Real Estate Assistant codebase.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Project Structure](#project-structure)
- [Development Workflow](#development-workflow)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [Code Style](#code-style)
- [Troubleshooting](#troubleshooting)

## Prerequisites

### Required Software

- **Python 3.11+**: Download from [python.org](https://www.python.org/downloads/)
- **Poetry**: Python dependency management tool
  ```bash
  curl -sSL https://install.python-poetry.org | python3 -
  ```
- **Git**: Version control system
  ```bash
  # macOS
  brew install git

  # Ubuntu/Debian
  sudo apt-get install git

  # Windows
  # Download from https://git-scm.com/
  ```

### Optional Tools

- **Docker**: For containerized development
- **VS Code**: Recommended IDE with Python extension
- **DevContainers**: For consistent development environment

## Environment Setup

### 1. Clone the Repository

```bash
git clone https://github.com/AleksNeStu/ai-real-estate-assistant.git
cd ai-real-estate-assistant
```

### 2. Install Dependencies

Using Poetry (recommended):

```bash
# Install all dependencies
poetry install

# Activate virtual environment
poetry shell
```

Using pip (alternative):

```bash
# Create virtual environment
python -m venv .venv

# Activate virtual environment
# On macOS/Linux:
source .venv/bin/activate
# On Windows:
.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt  # (if available)
# or
poetry export -f requirements.txt --output requirements.txt
pip install -r requirements.txt
```

### 3. Configure Environment Variables

Create a `.streamlit/secrets.toml` file in the project root:

```toml
# OpenAI API Configuration
OPENAI_API_KEY = "sk-your-api-key-here"

# Optional: Other LLM API Keys
# ANTHROPIC_API_KEY = "your-anthropic-key"
# COHERE_API_KEY = "your-cohere-key"

# Application Settings
DEBUG = false
LOG_LEVEL = "INFO"
```

**Security Note**: Never commit `secrets.toml` to version control! It's already in `.gitignore`.

### 4. Verify Installation

```bash
# Check Python version
python --version  # Should be 3.11+

# Check Poetry installation
poetry --version

# Verify dependencies
poetry show

# Test Streamlit
streamlit hello
```

## Project Structure

```
ai-real-estate-assistant/
├── src/                    # Source code
│   ├── ai/                # AI agent implementation
│   │   ├── __init__.py
│   │   └── agent.py       # RealEstateGPT class
│   ├── common/            # Shared configuration
│   │   ├── __init__.py
│   │   └── cfg.py         # Configuration constants
│   ├── data/              # Data loading modules
│   │   ├── __init__.py
│   │   └── csv_loader.py  # CSV utilities
│   ├── ui/                # UI components and styling
│   │   ├── __init__.py
│   │   ├── theme_manager.py  # Theme management
│   │   └── styles.css     # Dark mode CSS
│   └── utils/             # Utility functions
│       ├── __init__.py
│       ├── utils.py       # General utilities
│       └── streaming.py   # Streaming handlers
├── config/                # Configuration files
│   └── streamlit/         # Streamlit config
│       └── config.toml    # Theme and app settings
├── docs/                  # Documentation
│   ├── images/           # Screenshots
│   ├── PRD.md            # Product requirements
│   └── TODO.md           # Project roadmap
├── dataset/              # Sample datasets
│   └── pl/               # Polish real estate data
├── scripts/              # Utility scripts
│   ├── run_v1.sh         # Run V1 app
│   └── run_v2.sh         # Run V2 app
├── tests/                # Test suite
│   └── __init__.py
├── app.py                # V1 application entry
├── app_v2.py             # V2 application entry (RAG)
├── pyproject.toml        # Poetry configuration
├── .gitignore            # Git ignore rules
├── README.md             # Project readme
├── CONTRIBUTING.md       # Contribution guidelines
├── CODE_OF_CONDUCT.md    # Code of conduct
└── DEVELOPMENT.md        # This file
```

## Development Workflow

### Creating a New Feature

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes:**
   - Edit code in `src/`
   - Add tests in `tests/`
   - Update documentation as needed

3. **Test your changes:**
   ```bash
   # Run tests
   pytest

   # Run the application
   ./scripts/run_v1.sh  # or run_v2.sh
   ```

4. **Commit your changes:**
   ```bash
   git add .
   git commit -m "feat: add new feature description"
   ```

5. **Push and create PR:**
   ```bash
   git push origin feature/your-feature-name
   ```

### Code Organization Best Practices

- **Separation of Concerns**: Keep UI, business logic, and data access separate
- **Modularity**: Create small, focused modules and functions
- **Reusability**: Extract common functionality into utility modules
- **Type Hints**: Use type annotations for better code clarity
- **Documentation**: Write docstrings for all public functions and classes

## Running the Application

### Version 1 (Dataframe Agent)

Uses LangChain's pandas dataframe agent for direct CSV querying:

```bash
# Using shell script
./scripts/run_v1.sh

# Or directly
streamlit run app.py
```

**Features:**
- Direct CSV data loading
- Pandas dataframe agent
- Simple query interface
- Real-time responses

### Version 2 (RAG Implementation)

Uses Retrieval-Augmented Generation with vector databases:

```bash
# Using shell script
./scripts/run_v2.sh

# Or directly
streamlit run app_v2.py
```

**Features:**
- Vector database (ChromaDB)
- Document chunking and embeddings
- Conversational memory
- Source citations
- Streaming responses

### Development Server Options

```bash
# Run with auto-reload on file changes
streamlit run app.py --server.fileWatcherType=auto

# Run on specific port
streamlit run app.py --server.port=8502

# Run with debugging
streamlit run app.py --logger.level=debug

# Run in headless mode (no browser)
streamlit run app.py --server.headless=true
```

## Testing

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage report
pytest --cov=src tests/

# Run specific test file
pytest tests/test_agent.py

# Run with verbose output
pytest -v

# Run and stop on first failure
pytest -x
```

### Writing Tests

Create test files in `tests/` directory:

```python
# tests/test_theme_manager.py
import pytest
from src.ui.theme_manager import ThemeManager


def test_theme_initialization():
    """Test that theme manager initializes correctly."""
    manager = ThemeManager()
    assert manager.get_current_theme() == "auto"


def test_theme_switching():
    """Test theme switching functionality."""
    manager = ThemeManager()
    manager.apply_theme("dark")
    assert manager.get_current_theme() == "dark"
```

## Code Style

### Formatting Tools

```bash
# Format code with Black
black src/ tests/

# Sort imports with isort
isort src/ tests/

# Check code style with flake8
flake8 src/ tests/

# Type checking with mypy
mypy src/
```

### Pre-commit Hooks

Install pre-commit hooks to automatically check code before commits:

```bash
# Install pre-commit
pip install pre-commit

# Install hooks
pre-commit install

# Run manually
pre-commit run --all-files
```

Create `.pre-commit-config.yaml`:

```yaml
repos:
  - repo: https://github.com/psf/black
    rev: 23.3.0
    hooks:
      - id: black
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
```

### Code Style Guidelines

- **Line Length**: Maximum 100 characters
- **Imports**: Grouped (stdlib, third-party, local) and sorted
- **Docstrings**: Google style for all public APIs
- **Type Hints**: Required for function signatures
- **Comments**: Use sparingly, prefer self-documenting code

## Troubleshooting

### Common Issues

#### Poetry Installation Issues

```bash
# If poetry command not found, add to PATH:
export PATH="$HOME/.local/bin:$PATH"

# Or reinstall Poetry
curl -sSL https://install.python-poetry.org | python3 - --uninstall
curl -sSL https://install.python-poetry.org | python3 -
```

#### Dependency Conflicts

```bash
# Clear Poetry cache
poetry cache clear pypi --all

# Remove lock file and reinstall
rm poetry.lock
poetry install
```

#### Streamlit Not Starting

```bash
# Check if port is already in use
lsof -i :8501

# Kill process using the port
kill -9 <PID>

# Or use a different port
streamlit run app.py --server.port=8502
```

#### Import Errors

```bash
# Ensure you're in the virtual environment
poetry shell

# Verify PYTHONPATH includes project root
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Reinstall dependencies
poetry install --no-cache
```

#### API Key Issues

- Verify `.streamlit/secrets.toml` exists and contains valid keys
- Check that the file is not committed to Git
- Ensure proper format: `OPENAI_API_KEY = "sk-..."`
- Test API key validity with OpenAI's API

### Getting Help

1. Check existing [GitHub Issues](https://github.com/AleksNeStu/ai-real-estate-assistant/issues)
2. Review [documentation](docs/)
3. Search community forums and Stack Overflow
4. Create a new issue with detailed information:
   - Python version
   - Operating system
   - Error messages
   - Steps to reproduce

## Additional Resources

- [Streamlit Documentation](https://docs.streamlit.io/)
- [LangChain Documentation](https://python.langchain.com/)
- [OpenAI API Documentation](https://platform.openai.com/docs)
- [Poetry Documentation](https://python-poetry.org/docs/)
- [Python Best Practices](https://docs.python-guide.org/)

---

Happy coding! If you have questions or need help, don't hesitate to open an issue or reach out to the maintainers.
