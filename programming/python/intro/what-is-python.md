# What Is Python

## Description

Python is a high-level, interpreted language known for its readability and simplicity. It dominates data science, machine learning, and automation while being widely used for web development, scripting, and backend services.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [Why Python](#why-python)
- [Where Python Shines](#where-python-shines)
- [Key Concepts](#key-concepts)
- [Pythonic Code](#pythonic-code)

## Content / Material

### Why Python

- **Readable** — clean syntax that reads like English
- **Batteries included** — extensive standard library
- **Ecosystem** — PyPI hosts over 500,000 packages
- **Community** — one of the largest and most active

### Where Python Shines

| Domain | Frameworks / Tools |
|--------|-------------------|
| Web backend | Django, Flask, FastAPI |
| Data science | NumPy, Pandas, Jupyter |
| Machine learning | PyTorch, TensorFlow, scikit-learn |
| Automation | Ansible, Fabric, custom scripts |
| DevOps | AWS CDK, Airflow, Pytest |

### Key Concepts

- **Dynamic typing** — types are checked at runtime
- **Everything is an object** — functions, classes, and even modules
- **Indentation matters** — blocks are defined by whitespace
- **Duck typing** — "if it walks like a duck and quacks like a duck, it's a duck"

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"
```

### Pythonic Code

Python has a philosophy ("The Zen of Python") that encourages:

- Simple over complex
- Explicit over implicit
- Readability counts

```python
# Pythonic
squares = [x**2 for x in range(10)]

# Unpythonic
squares = []
for x in range(10):
    squares.append(x**2)
```

## Glossary

| Term | Definition |
|------|------------|
| Duck typing | An object's suitability is determined by its methods and properties, not its type |
| PyPI | The Python Package Index — the official repository of third-party Python packages |
| Zen of Python | A collection of 19 guiding principles for writing Python |

## Next Steps

- [Variables & Data Types](../beginner/variables-and-types.md) — numbers, strings, lists, dicts
- [Control Flow](../beginner/control-flow.md) — conditionals, loops, comprehensions
