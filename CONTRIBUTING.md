# Contributing to LTC Smart Care Platform

Thank you for your interest in contributing! This document outlines the process and guidelines for contributing to this project.

---

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR_USERNAME/LTC.git
   cd LTC
   ```
3. **Create a new branch** for your feature or fix:
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. **Make your changes** and commit with clear messages (see Commit Guidelines below)
5. **Push to your fork** and **submit a Pull Request**

---

## Commit Guidelines

Use clear, descriptive commit messages following this format:

```
type(scope): description

Optional longer description explaining the change.
```

**Types:**
- `feat` - A new feature
- `fix` - A bug fix
- `docs` - Documentation only
- `style` - Code style changes (formatting, missing semicolons, etc.)
- `refactor` - Code refactoring without feature changes
- `test` - Adding or updating tests
- `chore` - Maintenance tasks, dependency updates

**Examples:**
```
feat(patients): add patient export to CSV
fix(auth): resolve JWT token expiration issue
docs(README): improve installation instructions
chore: update dependencies
```

---

## Code Style

- Use **Prettier** for formatting (if configured)
- Follow **ESLint** rules (if configured)
- Use meaningful variable and function names
- Add comments for complex logic
- Keep functions small and focused

### Backend (Node.js)
- Use async/await for async operations
- Handle errors properly with try-catch
- Use consistent indentation (2 spaces)

### Frontend (React)
- Use functional components and hooks
- Keep components reusable and focused
- Extract repeated JSX into sub-components
- Use meaningful prop names

---

## Pull Request Process

1. **Ensure your branch is up to date** with `main` (or default branch)
   ```bash
   git fetch origin
   git rebase origin/main
   ```

2. **Test your changes thoroughly:**
   - Backend: Run the server and test endpoints manually or with automated tests
   - Frontend: Test in the browser, check all user flows

3. **Update documentation** if you:
   - Add new API endpoints
   - Change database schema
   - Add new features or commands

4. **Write a clear PR description:**
   - What does it do?
   - Why is this change needed?
   - How should it be tested?
   - Any breaking changes?

5. **Link related issues** (if applicable):
   ```
   Closes #123
   ```

6. **Wait for review** – maintainers will provide feedback

---

## Testing

- Write tests for new features when possible
- Ensure all tests pass before submitting a PR
- Test both happy paths and edge cases

Example test commands (if tests exist):
```bash
npm test
npm run test:watch
npm run test:coverage
```

---

## Documentation

When adding features, update relevant documentation:
- **README.md** – Major features
- **API.md** – New endpoints
- **Code comments** – Complex logic
- **CHANGELOG.md** (if it exists) – Notable changes

---

## Reporting Issues

If you find a bug or have a feature request:

1. **Check existing issues** to avoid duplicates
2. **Open a new issue** with:
   - Clear title
   - Detailed description
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - Your environment (OS, Node version, etc.)

---

## Code Review

All pull requests will be reviewed by maintainers. Be open to feedback and ready to make requested changes.

---

## License

By contributing, you agree that your code will be licensed under the project's license (see LICENSE file or README).

---

## Questions?

Have questions? Open an issue with the `question` label or ask in the PR discussion.

Thank you for contributing! 🙏
