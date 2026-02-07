# Contributing to Policy Document Navigator

Thank you for your interest in contributing to Policy Document Navigator! This guide will help you get started.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Submitting Changes](#submitting-changes)
- [Project Structure](#project-structure)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive experience for everyone. We expect all contributors to:

- Be respectful and considerate
- Welcome newcomers and help them get started
- Focus on what is best for the community
- Show empathy towards other community members

### Unacceptable Behavior

- Harassment, trolling, or discriminatory comments
- Publishing others' private information
- Other conduct which could reasonably be considered inappropriate

---

## Getting Started

### Prerequisites

- Python 3.8 or higher
- Git
- Basic understanding of:
  - Python programming
  - Virtual environments
  - Git version control
  - RAG (Retrieval-Augmented Generation) concepts (helpful but not required)

### Areas to Contribute

We welcome contributions in these areas:

1. **Bug Fixes**: Fix reported issues
2. **Features**: Add new functionality
3. **Documentation**: Improve guides and API docs
4. **Testing**: Add unit tests and integration tests
5. **Performance**: Optimize processing speed
6. **UI/UX**: Enhance the Streamlit interface
7. **Examples**: Add sample policies and use cases

---

## Development Setup

### 1. Fork and Clone

```bash
# Fork the repository on GitHub
# Then clone your fork
git clone https://github.com/YOUR_USERNAME/policy-navigator.git
cd policy-navigator
```

### 2. Create Virtual Environment

```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: .\venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
# Install production dependencies
pip install -r requirements.txt

# Install development dependencies
pip install -r requirements-dev.txt  # If available
```

### 4. Set Up Environment Variables

```bash
# Create .env file
cp .env.example .env  # If .env.example exists
# Or create .env manually:
echo "GEMINI_API_KEY=your_key_here" > .env
```

### 5. Verify Setup

```bash
# Run the application
streamlit run app.py

# Run tests (if available)
pytest
```

---

## How to Contribute

### Reporting Bugs

Before creating a bug report, please:

1. **Search existing issues** to avoid duplicates
2. **Use a clear, descriptive title**
3. **Include steps to reproduce**
4. **Describe expected vs actual behavior**
5. **Add screenshots if applicable**
6. **Include system information**:
   - OS and version
   - Python version
   - Dependency versions

**Bug Report Template**:

```markdown
### Description
Brief description of the bug

### Steps to Reproduce
1. Step one
2. Step two
3. ...

### Expected Behavior
What should happen

### Actual Behavior
What actually happens

### System Information
- OS: [e.g., Windows 11]
- Python: [e.g., 3.10.5]
- Dependencies: [paste `pip list` output]

### Additional Context
Screenshots, error logs, etc.
```

---

### Suggesting Features

Feature requests are welcome! Please:

1. **Check existing feature requests**
2. **Explain the use case** - why is this needed?
3. **Describe the proposed solution**
4. **Consider alternatives**
5. **Note any breaking changes**

**Feature Request Template**:

```markdown
### Feature Description
Clear description of the feature

### Use Case
Why is this feature valuable?

### Proposed Solution
How should it work?

### Alternatives Considered
Other approaches you've thought about

### Additional Context
Mockups, examples, references
```

---

### Working on Issues

1. **Comment on the issue** to claim it
2. **Wait for approval** from maintainers
3. **Create a branch** for your work:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/issue-number-description
   ```

---

## Coding Standards

### Python Style Guide

Follow [PEP 8](https://pep8.org/) style guidelines:

```python
# Good
def load_pdf(self, pdf_path: str) -> None:
    """Load and process a PDF file."""
    reader = PdfReader(pdf_path)
    # ...

# Bad
def LoadPDF(self,pdfPath):
    reader=PdfReader(pdfPath)
    #...
```

### Code Formatting

Use `black` for automatic formatting:

```bash
# Format a file
black app.py

# Format entire project
black .

# Check without modifying
black --check .
```

### Import Organization

Use `isort` to organize imports:

```bash
# Sort imports
isort app.py

# Check import order
isort --check-only .
```

**Import Order**:
1. Standard library imports
2. Third-party imports
3. Local application imports

```python
# Correct order
import os
from typing import List

import streamlit as st
from sentence_transformers import SentenceTransformer

from scaledown import compress
```

### Type Hints

Add type hints to function signatures:

```python
# Good
def ask(self, question: str) -> str:
    """Answer a question based on policy."""
    pass

# Acceptable (for simple cases)
def ask(self, question):
    pass
```

### Documentation

#### Docstrings

Use Google-style docstrings:

```python
def ask(self, question: str) -> str:
    """
    Answer a question based on the loaded policy document.

    Args:
        question (str): Natural language question about the policy

    Returns:
        str: AI-generated answer based on relevant context

    Raises:
        ValueError: If no document has been loaded
        
    Examples:
        >>> rag = PolicyRAG()
        >>> rag.load_pdf("policy.pdf")
        >>> answer = rag.ask("What is the vacation policy?")
    """
    pass
```

#### Comments

Use comments for complex logic:

```python
# Good - explains why
# Compress text to reduce token costs for LLM API calls
compressed = compress(full_text)

# Bad - states the obvious
# Call compress function
compressed = compress(full_text)
```

---

## Testing

### Writing Tests

Place tests in a `tests/` directory:

```
policy-navigator/
├── tests/
│   ├── __init__.py
│   ├── test_rag_engine.py
│   ├── test_scaledown.py
│   └── test_app.py
├── app.py
├── rag_engine.py
└── scaledown.py
```

### Test Example

```python
# tests/test_rag_engine.py
import pytest
from rag_engine import PolicyRAG

def test_rag_init():
    """Test RAG initialization."""
    rag = PolicyRAG()
    assert rag.embedder is not None
    assert rag.index is None
    assert rag.text_chunks == []

def test_load_pdf(tmp_path):
    """Test PDF loading."""
    # Create a test PDF
    test_pdf = tmp_path / "test.pdf"
    # ... create test PDF ...
    
    rag = PolicyRAG()
    rag.load_pdf(str(test_pdf))
    
    assert rag.index is not None
    assert len(rag.text_chunks) > 0
```

### Running Tests

```bash
# Run all tests
pytest

# Run specific test file
pytest tests/test_rag_engine.py

# Run with coverage
pytest --cov=. --cov-report=html
```

---

## Submitting Changes

### 1. Make Your Changes

```bash
# Create a branch
git checkout -b feature/my-new-feature

# Make changes
# ... edit files ...

# Test your changes
pytest
streamlit run app.py
```

### 2. Commit Your Changes

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Format: <type>(<scope>): <description>

# Examples:
git commit -m "feat(rag): add support for multiple document formats"
git commit -m "fix(app): resolve PDF upload error on Windows"
git commit -m "docs(readme): update installation instructions"
git commit -m "refactor(scaledown): simplify compression logic"
git commit -m "test(rag): add unit tests for chunking"
```

**Commit Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

### 3. Push to Your Fork

```bash
git push origin feature/my-new-feature
```

### 4. Create Pull Request

1. Go to the original repository on GitHub
2. Click "New Pull Request"
3. Select your fork and branch
4. Fill in the PR template:

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Refactoring
- [ ] Other (please describe)

## Testing
- [ ] Tested locally
- [ ] Added unit tests
- [ ] Verified on multiple platforms

## Checklist
- [ ] Code follows project style guidelines
- [ ] Documentation updated
- [ ] No new warnings
- [ ] All tests pass

## Related Issues
Closes #[issue number]
```

### 5. Respond to Review Feedback

- Address reviewer comments
- Make requested changes
- Push updates to the same branch
- Re-request review

---

## Project Structure

```
policy-navigator/
├── app.py                    # Streamlit UI (entry point)
├── rag_engine.py             # Core RAG logic
├── scaledown.py              # Text compression utility
├── requirements.txt          # Production dependencies
├── requirements-dev.txt      # Development dependencies
├── .env.example              # Example environment variables
├── .gitignore                # Git ignore rules
│
├── docs/                     # Documentation
│   ├── API_REFERENCE.md      # API documentation
│   ├── ARCHITECTURE.md       # System design
│   ├── INSTALLATION.md       # Setup guide
│   ├── USER_GUIDE.md         # User manual
│   └── CONTRIBUTING.md       # This file
│
├── tests/                    # Test suite
│   ├── __init__.py
│   ├── test_rag_engine.py
│   ├── test_scaledown.py
│   └── test_app.py
│
├── temp_uploads/             # Temporary PDF storage
└── README.md                 # Project overview
```

### Module Responsibilities

| File | Purpose | Key Classes/Functions |
|------|---------|----------------------|
| `app.py` | UI and user interaction | `init_rag()`, Streamlit components |
| `rag_engine.py` | Document processing and QA | `PolicyRAG` class |
| `scaledown.py` | Text compression | `compress()` function |

---

## Development Guidelines

### Adding New Features

1. **Plan the Feature**:
   - Write a design doc for large features
   - Discuss in an issue first
   - Consider backward compatibility

2. **Implement Incrementally**:
   - Start with a minimal working version
   - Add features in small commits
   - Test after each change

3. **Update Documentation**:
   - Add docstrings to new functions
   - Update relevant `.md` files
   - Add usage examples

4. **Add Tests**:
   - Write unit tests for new functions
   - Add integration tests if needed
   - Aim for >80% code coverage

---

### Code Review Checklist

Before submitting a PR, verify:

- [ ] Code follows style guidelines
- [ ] All tests pass
- [ ] New tests added for new features
- [ ] Documentation updated
- [ ] No commented-out code
- [ ] No debug print statements
- [ ] Type hints added
- [ ] Error handling implemented
- [ ] Performance considered

---

## Release Process

(For maintainers)

### Version Numbering

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR**: Breaking changes
- **MINOR**: New features (backward compatible)
- **PATCH**: Bug fixes

### Creating a Release

1. Update version in relevant files
2. Update CHANGELOG.md
3. Create a git tag:
   ```bash
   git tag -a v1.2.0 -m "Release version 1.2.0"
   git push origin v1.2.0
   ```
4. Create GitHub release with notes

---

## Community

### Communication Channels

- **Issues**: Bug reports and feature requests
- **Pull Requests**: Code contributions
- **Discussions**: General questions and ideas

### Getting Help

- Check the [User Guide](USER_GUIDE.md)
- Read the [API Reference](API_REFERENCE.md)
- Search existing issues
- Ask in GitHub Discussions

---

## Recognition

Contributors will be:
- Listed in the project README
- Mentioned in release notes
- Credited in the contributors page

---

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

## Questions?

If you have questions about contributing:

1. Check this guide
2. Search existing issues
3. Open a new discussion
4. Contact the maintainers

**Thank you for contributing to Policy Document Navigator! 🎉**
