# Contributing to Citizen Complaints and Engagement System

Thank you for considering contributing to this project! This document outlines the process and guidelines for contributing to the Citizen Complaints and Engagement System.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Workflow](#development-workflow)
4. [Coding Standards](#coding-standards)
5. [Commit Guidelines](#commit-guidelines)
6. [Pull Request Process](#pull-request-process)
7. [Testing Guidelines](#testing-guidelines)
8. [Documentation](#documentation)
9. [Issue Reporting](#issue-reporting)

## Code of Conduct

### Our Pledge

We pledge to make participation in our project a harassment-free experience for everyone, regardless of age, body size, disability, ethnicity, gender identity and expression, level of experience, nationality, personal appearance, race, religion, or sexual identity and orientation.

### Our Standards

Examples of behavior that contributes to creating a positive environment include:

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

Examples of unacceptable behavior include:

- The use of sexualized language or imagery and unwelcome sexual attention or advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

## Getting Started

### Prerequisites

- PHP 8.0+
- MySQL 8.0+
- XAMPP, WAMP, or similar local server environment
- Git

### Setting Up the Development Environment

1. Fork the repository.
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR-USERNAME/citizen-complaints-system.git
   ```
3. Add the original repository as upstream:
   ```bash
   git remote add upstream https://github.com/ORIGINAL-OWNER/citizen-complaints-system.git
   ```
4. Set up the database by following the instructions in [INSTALLATION.md](./INSTALLATION.md).

## Development Workflow

1. Create a new branch for your feature or bugfix:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/issue-you-are-fixing
   ```

2. Make your changes, ensuring you follow the coding standards.

3. Commit your changes (see [Commit Guidelines](#commit-guidelines)).

4. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

5. Create a pull request from your branch to the main repository's `develop` branch.

6. Incorporate feedback from code reviews.

7. Once approved, your changes will be merged.

## Coding Standards

### PHP

- Follow PSR-12 coding standard.
- Use camelCase for method names and variables.
- Use PascalCase for class names.
- Document all classes and methods with PHPDoc.
- Keep files under 500 lines of code when possible.
- Indent using 4 spaces, not tabs.

Example:
```php
/**
 * User class for handling user-related operations.
 */
class User
{
    /**
     * Get user by ID.
     *
     * @param int $userId The user ID
     * @return array|null The user data or null if not found
     */
    public function getUserById($userId)
    {
        // Implementation...
    }
}
```

### JavaScript

- Use ES6+ features when possible.
- Use camelCase for function names and variables.
- Use PascalCase for class names.
- Document functions with JSDoc.
- Use semicolons at the end of statements.
- Use single quotes for strings.

Example:
```javascript
/**
 * Validates form input.
 * @param {HTMLFormElement} form - The form to validate
 * @return {boolean} True if valid, false otherwise
 */
function validateForm(form) {
    // Implementation...
    return true;
}
```

### HTML/CSS

- Use lowercase for HTML elements and attributes.
- Use double quotes for attribute values.
- Close all HTML tags properly.
- Follow the BEM (Block, Element, Modifier) methodology for CSS class names.
- Keep CSS selectors specific but not too nested.

## Commit Guidelines

- Use clear and descriptive commit messages.
- Follow the conventional commits pattern:
  - `feat:` for new features
  - `fix:` for bug fixes
  - `docs:` for documentation changes
  - `style:` for formatting changes that don't affect code functionality
  - `refactor:` for code changes that neither fix bugs nor add features
  - `test:` for adding or updating tests
  - `chore:` for routine tasks, dependency updates, etc.

Examples:
```
feat: add user registration form
fix: resolve login validation issues
docs: update installation instructions
```

- Keep commits focused on a single responsibility.
- Squash multiple commits if they're related to the same issue.

## Pull Request Process

1. Update the README.md or relevant documentation with details of changes.
2. Include screenshots or examples if UI changes are involved.
3. Link the PR to any related issues.
4. Ensure all tests pass.
5. Request a review from at least one maintainer.
6. PRs must be approved by at least one maintainer before being merged.
7. Once approved, PRs will be merged by a maintainer.

### PR Title Format
```
[Type] Short description of changes
```

Example:
```
[Feature] Add complaint categorization functionality
```

## Testing Guidelines

- Write tests for new features and bug fixes.
- Ensure tests are isolated and don't depend on external services.
- Run the existing test suite before submitting a PR to ensure you haven't broken existing functionality.
- Test in multiple browsers if making UI changes.

## Documentation

- Update the documentation when you change code functionality.
- Use clear, concise language.
- Include examples where appropriate.
- Keep API documentation up-to-date.

## Issue Reporting

### Bug Reports

When reporting bugs, please include:

1. A clear, descriptive title.
2. Steps to reproduce the issue.
3. Expected behavior.
4. Actual behavior.
5. Screenshots or screen recordings if applicable.
6. Environment details (OS, browser, PHP version, etc.).

Example format:
```
Title: Login button unresponsive on mobile devices

Steps to reproduce:
1. Open the login page on a mobile device or simulator
2. Enter valid credentials
3. Tap the login button

Expected behavior:
User should be logged in and redirected to the dashboard.

Actual behavior:
Nothing happens when tapping the login button. No errors in console.

Environment:
- iPhone 12, iOS 15
- Chrome mobile 98.0
- Server running PHP 8.1
```

### Feature Requests

When suggesting features, please include:

1. A clear, descriptive title.
2. A detailed description of the proposed feature.
3. The problem this feature would solve.
4. Any alternative solutions you've considered.
5. Mockups or examples (if applicable).

---

Thank you for contributing to the Citizen Complaints and Engagement System!
