# Contributing to Player Similarity Model

First off, thank you for considering contributing to the Player Similarity Model! 🎉

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Setup](#development-setup)
- [Pull Request Process](#pull-request-process)
- [Style Guide](#style-guide)
- [Feature Priorities](#feature-priorities)

---

## Code of Conduct

This project adheres to a Code of Conduct. By participating, you are expected to uphold this code. Please be respectful and constructive in all interactions.

---

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/player-similarity-model.git
   cd player-similarity-model
   ```
3. **Add upstream remote**:
   ```bash
   git remote add upstream https://github.com/ORIGINAL_OWNER/player-similarity-model.git
   ```

---

## How to Contribute

### Reporting Bugs

Before creating a bug report, please check existing issues. When creating a bug report, include:

- **Clear title** describing the issue
- **Steps to reproduce** the behavior
- **Expected behavior** vs **actual behavior**
- **Environment details** (Python version, OS, package versions)
- **Code samples** or error messages

### Suggesting Features

Feature suggestions are welcome! Please include:

- **Clear description** of the feature
- **Use case** - why is this valuable?
- **Possible implementation** approach (if you have ideas)
- **Priority** assessment (see [Feature Priorities](#feature-priorities))

### Contributing Code

1. Check if there's an existing issue for your contribution
2. Create or comment on an issue before starting major work
3. Follow the [Development Setup](#development-setup) instructions
4. Make your changes following the [Style Guide](#style-guide)
5. Submit a [Pull Request](#pull-request-process)

---

## Development Setup

### Prerequisites

- Python 3.8 or higher
- Git

### Setup Steps

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/player-similarity-model.git
cd player-similarity-model

# Create virtual environment
python -m venv .venv

# Activate (Windows)
.venv\Scripts\activate

# Activate (Linux/Mac)
source .venv/bin/activate

# Install dev dependencies
pip install -e ".[dev]"
```

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=player_similarity --cov-report=html

# Run specific test file
pytest tests/test_similarity.py
```

### Code Formatting

```bash
# Format code
black .

# Check linting
flake8 .

# Type checking
mypy .
```

---

## Pull Request Process

### Before Submitting

1. **Sync with upstream**:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run tests** and ensure they pass

3. **Format code** with black

4. **Update documentation** if needed

### PR Template

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Refactoring
- [ ] Performance improvement

## Related Issue
Fixes #(issue number)

## Testing
Describe how you tested the changes.

## Checklist
- [ ] Code follows style guidelines
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
```

### Review Process

1. Maintainers will review your PR
2. Address any feedback
3. Once approved, PR will be merged
4. Delete your branch after merge

---

## Style Guide

### Python Code Style

- Follow **PEP 8** conventions
- Use **Black** for formatting (line length: 100)
- Use **type hints** for function signatures
- Write **docstrings** for all public functions

### Example

```python
def compute_similarity(
    profile1: Dict[str, np.ndarray],
    profile2: Dict[str, np.ndarray],
    weights: Optional[Dict[str, float]] = None
) -> float:
    """
    Compute overall similarity between two player profiles.
    
    Parameters
    ----------
    profile1 : Dict[str, np.ndarray]
        Feature profile for first player
    profile2 : Dict[str, np.ndarray]
        Feature profile for second player
    weights : Optional[Dict[str, float]]
        Custom weights for feature blocks (default: equal weights)
    
    Returns
    -------
    float
        Similarity score between 0 and 1
    
    Examples
    --------
    >>> profile1 = {'spatial': np.array([0.1, 0.2, ...])}
    >>> profile2 = {'spatial': np.array([0.15, 0.18, ...])}
    >>> similarity = compute_similarity(profile1, profile2)
    """
    if weights is None:
        weights = DEFAULT_WEIGHTS
    
    total = 0.0
    for block, weight in weights.items():
        block_sim = compute_block_similarity(profile1[block], profile2[block])
        total += weight * block_sim
    
    return total
```

### Documentation Style

- Use **Markdown** for documentation
- Include **code examples** where helpful
- Keep explanations **concise but complete**
- Update **CHANGELOG.md** for significant changes

### Commit Messages

Follow conventional commits:

```
type(scope): description

[optional body]

[optional footer]
```

Types:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Tests
- `chore`: Maintenance

Example:
```
feat(similarity): add weighted block similarity

Implement configurable weights for each feature block
in the similarity computation.

Closes #42
```

---

## Feature Priorities

When contributing new features, consider these priorities:

### P0 - Critical (High Impact, Foundation)
- Learned feature weights
- More training data support
- Position-aware filtering

### P1 - Important (Clear Value)
- Improved passing features
- Better action chain encoding
- Evaluation metrics

### P2 - Nice to Have (Future Direction)
- Graph Attention Networks
- Opposition adjustment
- Real-time updates

### P3 - Long-term (Research)
- Video feature extraction
- Tracking data integration
- Web interface

---

## Questions?

Feel free to:
- Open a GitHub issue for questions
- Tag maintainers for guidance
- Join discussions in existing issues

Thank you for contributing! ⚽
