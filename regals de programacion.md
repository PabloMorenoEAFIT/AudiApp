# Programming Rules for Django Projects

This document outlines the coding standards, project structure, and best practices for Django projects. Follow these rules to ensure clean, maintainable, and scalable code.

---

## 1. Project Structure

Organize your Django project in a modular and clear structure. Example:
mi_proyecto/
├── config/
│ ├── settings/
│ │ ├── base.py
│ │ ├── local.py
│ │ ├── production.py
│ ├── urls.py
│ ├── wsgi.py
├── apps/
│ ├── core/
│ ├── usuarios/
│ ├── blog/
├── static/
├── templates/
├── manage.py
├── requirements/
│ ├── base.txt
│ ├── local.txt
│ ├── production.txt


- **`config/`**: Contains project configuration, split by environment (base, local, production).
- **`apps/`**: Each project functionality should be in a separate app (e.g., `usuarios`, `blog`).
- **`requirements/`**: Split dependencies by environment.

---

## 2. Naming Conventions

- **Classes**: Use `PascalCase`.
  
  ```python
  class UserProfile(models.Model):
      pass
- **Functions and Methods**: Use `snake_case`.
    ```python
    def get_user_profile(user_id):
    pass
    ```
- **Variables**: Use `snake_case`.
    ```python
    user_profile = UserProfile.objects.get(id=1)
     ```
- **Constants**: Use `UPPER_CASE`.
    ```python
    MAX_USERS = 100
     ```

## 3. Documentation and comments
- **Docstrings**: Use docstrings to document modules, classes, and functions.
 
     ```python
    def calculate_tax(amount):
    """
    Calculate tax for a given amount.

    :param amount: The base amount for calculation.
    :return: The calculated tax.
    """
    return amount * 0.16
     ```
- **Comments**: Use comments to explain the "why," not the "what." Avoid obvious comments.
   ```python
  # Check if the user has permissions (prevent unauthorized access)
  if user.has_perm('blog.edit_post'):
    ...
     ```
## 4. Code Formatting
-  **Indentation**: Use 4 spaces (no tabs).
- **Line Length**: Limit lines to 79 characters (PEP 8).
- **Imports**: Organize imports into sections and use absolute imports.

   ```python
   # Standard library
  import os
  from datetime import datetime

  # Third-party
  from django.db import models
  fr om rest_framework import serializers

  # Local
  from apps.usuarios.models import UserProfile
   ```

## 5. Django Best Practices
### Models
- Use `snake_case` for field names
- Define `__str__` for readable representation

   ```python
  class Post(models.Model):
    title = models.CharField(max_length=255)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
   ```
   ### Views
- Use Class-Based Views (CBVs) when possible.
- Keep business logic out of views.

   ```python
      from django.views.generic import ListView
  from .models import Post

  class PostListView(ListView):
    model = Post
    template_name = 'blog/post_list.html'
   ```

### Templates
- Use descriptive names for template files
- Avoid complex logic in templates; use custom template tags or filters if needed

## 6. Testing
- Write unit and integration test for every functionality
- Use `pytest` or Python's `unittest` module
- Name test files with the `test_` prefix

     ```python
      # tests/test_models.py
  from django.test import TestCase
  from apps.blog.models import Post

  class PostModelTest(TestCase):
    def test_create_post(self):
        post = Post.objects.create(title="Test Post", content="Test Content")
        self.assertEqual(post.title, "Test Post")
   ```
## 7. Security
- Never store passwords in plain text; use `make_password` and `check_password`.
- Protect sensitive views with `@login_required` or `@permission_required`
- Use `django-environ` to manage sensitive environment variables

## 8. Maintainability
- DRY Principle (Don't Repeat Yourself): Reuse code with mixins, helper functions, or custom template tags.
- KISS Principle (Keep It Simple, Stupid): Avoid over-engineering.
- YAGNI Principle (You Aren't Gonna Need It): Don't implement unnecessary features.

## 9. Commit Style
- Use clear and descriptive commit messages.
- Follow the Conventional Commits standard:
```bash
  feat: Add user registration functionality
fix: Fix tax calculation bug
docs: Update project documentation
  ```
## 10. Additional Tools
- **Formatting**: Use `black` for automatic code formatting
- **Linting**: Use `flake8` or `pylint` to keep the code clean
- **Pre-commit**: Set up pre-commit hooks to run `black`, `flake8`, and tests before each commit.
