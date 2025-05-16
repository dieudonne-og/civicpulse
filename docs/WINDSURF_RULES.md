# Windsurf Project Rules

> **Note**: This document contains the core rules. For detailed guidelines, refer to:
> - [Coding Standards](./WINDSURF_CODING_STANDARDS.md)
> - [Security Guidelines](./SECURITY.md)
> - [Database Standards](./DATABASE.md)
> - [API Guidelines](./API_GUIDELINES.md)

## Core Principles

1. **Code Quality**
   - Follow SOLID principles
   - Keep functions/methods small and focused
   - Write self-documenting code
   - Regular code reviews

2. **Security**
   - Validate all inputs
   - Escape all outputs
   - Follow principle of least privilege
   - Keep dependencies updated

3. **Documentation**
   - Document public APIs
   - Keep documentation up-to-date
   - Write clear commit messages

## Security Rules

> **See [SECURITY.md](./SECURITY.md) for complete security guidelines**

- Validate all inputs (client & server)
- Use prepared statements for queries
- Implement CSRF protection
- Follow secure authentication practices
- Encrypt sensitive data
- Regular security audits

## Documentation

> **See [WINDSURF_DOCS.md](./WINDSURF_DOCS.md) for complete documentation standards**

- Document all public APIs
- Keep documentation current
- Include examples
- Document error cases

## Testing

> **See [TESTING.md](./TESTING.md) for complete testing standards**

- Write unit and integration tests
- Follow AAA pattern
- Test edge cases
- Maintain 80%+ coverage

## Code Quality

- Follow SOLID principles
- Keep functions small (<20 lines)
- Avoid deep nesting
- Remove dead code
- Optimize performance

## Version Control

### Commit Messages
- Follow conventional commits
- Use present tense, imperative mood
- Header < 50 chars
- Reference issues


### Required
- PHP 8.0+
- MySQL 8.0+
- Web server (Apache/Nginx)
- Composer
- Node.js 16+

### Recommended
- Xdebug
- PHP_CodeSniffer
- PHPStan
- PHPUnit

## Project Rules

### Core Requirements
- Follow complaint handling workflow
- Implement proper validation
- Track status changes
- Use standard API patterns
- Document all features

## Compliance

> **See [COMPLIANCE.md](./COMPLIANCE.md) for complete guidelines**

- Follow GDPR requirements
- Implement WCAG 2.1 AA
- Support major browsers
- Ensure data privacy

## Performance

### Key Guidelines
- Optimize database queries
- Implement caching
- Use pagination
- Monitor response times
- Optimize assets

## Review Process

### Self-Review
- Run all tests locally
- Check for code smells
- Verify documentation is updated
- Ensure no sensitive data is committed
- Check for performance issues

### Peer Review
- At least one approval required
- Check for security vulnerabilities
- Verify test coverage
- Ensure code follows standards
- Check for potential bugs

## Deployment

### Requirements
- All tests must pass
- Code review approved
- Documentation updated
- Version bumped
- Changelog updated

### Process
- Deploy to staging first
- Run smoke tests
- Verify functionality
- Monitor for issues
- Deploy to production with rollback plan
