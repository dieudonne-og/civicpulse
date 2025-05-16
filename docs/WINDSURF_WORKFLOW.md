# Windsurf Project Workflows

## Overview

This is the main index for Windsurf workflow documentation. The workflows have been split into focused documents to ensure each stays under the 6,000 character limit.

## Core Workflow Documents

1. **[Development Workflow](WINDSURF_DEVELOPMENT_WORKFLOW.md)**
   - Starting new features/bugfixes
   - Local development practices
   - Branching strategy
   - Code review process
   - Testing procedures

2. **[Release & Deployment Workflow](WINDSURF_RELEASE_WORKFLOW.md)**
   - Deployment environments
   - Release process
   - Hotfix procedures
   - Feature flags

3. **[Onboarding & Documentation](WINDSURF_ONBOARDING.md)**
   - New developer setup
   - Documentation standards
   - Project documentation
   - Maintenance guidelines

## Related Documents

- [WINDSURF_RULES.md](WINDSURF_RULES.md) - Coding standards and best practices
- [TASKS.md](TASKS.md) - Task tracking and implementation details
- [CONTRIBUTING.md](CONTRIBUTING.md) - Contribution guidelines
- [SECURITY.md](SECURITY.md) - Security policies and procedures

## Document Maintenance

When updating workflows:
1. Keep content focused and concise
2. Split into separate documents if exceeding 6,000 characters
3. Update this index with new document links
4. Maintain consistent formatting across all workflow documents

## Development Workflow

### 1. Starting a New Feature/Bugfix
1. Pull latest changes from `develop` branch
2. Create a new branch following naming conventions:
   - `feature/feature-name` for new features
   - `fix/issue-description` for bug fixes
   - `chore/task-description` for maintenance tasks
3. Implement changes following the rules in [WINDSURF_RULES.md](WINDSURF_RULES.md)
4. Write tests for new functionality
5. Update documentation as needed

### 2. Local Testing
1. Run unit tests: `composer test`
2. Run static analysis: `composer analyze`
3. Test in multiple browsers
4. Verify responsive behavior
5. Check accessibility

### 3. Committing Changes
1. Stage only related changes
2. Write a clear commit message following conventions
3. Push to remote repository
4. Create a pull request to `develop`

### 4. Continuous Integration
1. Automated tests run on push
2. Code quality checks
3. Build verification
4. Test coverage report

## Branching Strategy

### Main Branches
- `main` - Production code (always deployable)
- `develop` - Integration branch for features
- `staging` - Pre-production testing

### Supporting Branches
- Feature branches: `feature/*`
- Bugfix branches: `fix/*`
- Hotfix branches: `hotfix/*`
- Release branches: `release/*`
- Documentation: `docs/*`

### Branch Workflow
1. Create branch from `develop`
2. Make changes and commit
3. Open PR to `develop`
4. After review, squash and merge
5. Delete source branch after merge

## Branching Strategy

### Main Branches

- `main` - Production-ready code
- `develop` - Integration branch for features
- `staging` - Pre-production testing

### Feature Branches

- Branch naming: `feature/feature-name`
- Always branch from: `develop`
- Merge back into: `develop`

### Bugfix Branches

- Branch naming: `fix/issue-description`
- Always branch from: `develop` (or `main` for hotfixes)
- Merge back into source branch

### Release Branches

- Branch naming: `release/v1.x.x`
- Always branch from: `develop`
- Merge back into: `main` AND `develop`

### Branch Lifecycle

1. Create feature branch from `develop`
2. Develop feature with regular commits
3. Open pull request to `develop`
4. Code review and approval
5. Merge to `develop`
6. Delete feature branch after merge

## Code Review Process

### 1. Creating a Pull Request
1. Ensure all tests pass
2. Update documentation if needed
3. Fill out PR template completely
4. Add relevant reviewers
5. Link related issues

### 2. Review Process
1. First review within 24 hours
2. Use GitHub's review features
3. Be constructive and specific
4. Request changes or approve
5. Address all comments before merging

### 3. PR Approval
1. Requires at least one approval
2. All CI checks must pass
3. No merge conflicts
4. Documentation updated
5. Tests added/updated

### 4. Merging
1. Use squash and merge
2. Clean up commit message
3. Delete source branch
4. Link to deployed environment if applicable

## Code Reviews

### Before Requesting Review

1. Run all tests locally
2. Ensure code meets definition of done
3. Self-review your changes
4. Update documentation if necessary

### Review Process

1. Author creates pull request with detailed description
2. At least one peer reviewer must approve
3. Reviewer checks for:
   - Functionality: Does it work as intended?
   - Code quality: Follows standards and best practices
   - Security: No vulnerabilities introduced
   - Performance: No unnecessary inefficiencies
   - Tests: Appropriate test coverage
4. Address all review comments
5. Final approval from tech lead

### Pull Request Template

```
## Description
[Brief description of the changes]

## Related Issue
[Link to the issue]

## Type of Change
- [ ] New feature
- [ ] Bug fix
- [ ] Refactoring
- [ ] Documentation update

## How Has This Been Tested?
[Describe testing approach]

## Checklist:
- [ ] My code follows the project style guidelines
- [ ] I have performed a self-review
- [ ] I have commented my code where necessary
- [ ] I have made corresponding documentation changes
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix/feature works
```

## Testing Workflow

### 1. Unit Testing
- Run locally before each commit
- Focus on individual components
- Mock external dependencies
- Aim for high coverage

### 2. Integration Testing
- Test component interactions
- Verify API endpoints
- Test database operations
- Include edge cases

### 3. E2E Testing
- Test complete user flows
- Cover critical paths
- Run in CI pipeline
- Visual regression testing

### 4. Manual Testing
- Cross-browser testing
- Mobile responsiveness
- Accessibility verification
- User acceptance testing (UAT)

## Deployment Workflow

### 1. Development Environment
- Automatically deployed from `develop`
- Used for initial testing
- Reset data as needed
- Accessible to developers only

### 2. Staging Environment
- Mirrors production
- Used for QA and UAT
- Protected test data
- Accessible to testers and stakeholders

### 3. Production Environment
- Stable releases only
- Zero-downtime deployment
- Rollback capability
- Monitoring and alerting

## Release Process

### 1. Release Preparation
1. Create `release/vX.Y.Z` from `develop`
2. Update version numbers
3. Update changelog
4. Run final tests

### 2. Release Candidate
1. Deploy to staging
2. Perform smoke tests
3. Get stakeholder approval
4. Address any issues

### 3. Production Deployment
1. Merge release branch to `main`
2. Tag the release
3. Deploy to production
4. Monitor for issues

### 4. Post-Release
1. Merge `main` back to `develop`
2. Delete release branch
3. Announce the release
4. Update documentation

## Hotfix Process

### 1. Critical Bug Identification
1. Create `hotfix/description` from `main`
2. Implement fix following the same workflow
3. Add regression tests

### 2. Hotfix Deployment
1. Merge to `main` and tag
2. Deploy to production
3. Verify fix
4. Merge to `develop`

## Feature Flag Workflow

### 1. Implementing Feature Flags
1. Add flag to configuration
2. Wrap new features in flag checks
3. Default to disabled

### 2. Testing with Flags
1. Test both enabled/disabled states
2. Verify no impact when disabled
3. Test in staging first

### 3. Rollout Strategy
1. Enable for internal users
2. Gradual rollout to users
3. Monitor metrics
4. Complete rollout or rollback

## Documentation Workflow

### 1. Code Documentation
1. Document as you code
2. Update with each PR
3. Keep examples current

### 2. User Documentation
1. Update with feature changes
2. Include screenshots where helpful
3. Maintain version-specific docs

### 3. API Documentation
1. Keep OpenAPI spec updated
2. Document all endpoints
3. Include example requests/responses

## Project Documentation

### Document Overview

1. **README.md** - Project overview, setup instructions, and quick start guide
2. **FEATURES.md** - Detailed description of all features and functionality
3. **DATABASE.md** - Database schema, relationships, and data dictionary
4. **INSTALLATION.md** - Complete installation and configuration guide
5. **TASKS.md** - Task tracking and implementation details
6. **WINDSURF_RULES.md** - Code standards and development rules
7. **WINDSURF_WORKFLOW.md** - Development processes and workflows (this document)
8. **CONTRIBUTING.md** - Guidelines for contributing to the project
9. **SECURITY.md** - Security policies and procedures
10. **LICENSE.md** - Project licensing information

### Documentation Maintenance

1. **Update Frequency**
   - Documentation should be updated alongside code changes
   - Major changes require documentation updates before merging

2. **Style Guidelines**
   - Use consistent Markdown formatting
   - Include code examples where helpful
   - Keep lines under 100 characters
   - Use relative links for internal documentation

3. **Review Process**
   - Documentation changes require peer review
   - Ensure all links are valid
   - Verify code examples are accurate

4. **Versioning**
   - Update version numbers in documentation with each release
   - Maintain changelog entries for significant updates

5. **Accessibility**
   - Use descriptive link text
   - Add alt text for images
   - Ensure proper heading hierarchy
   - Use semantic HTML where applicable

## Onboarding Workflow

### 1. New Developer Setup
1. Set up development environment
2. Run through setup guide
3. Review architecture
4. Pair with team member

### 2. First Tasks
1. Start with well-defined issues
2. Follow code review process
3. Attend team ceremonies
4. Ask questions early and often

### Deployment Steps

1. Merge release branch to `main`
2. Tag release with version number
3. Deploy to staging environment
4. Run smoke tests
5. Upon approval, deploy to production
6. Verify deployment
7. Monitor for issues

### Hotfix Procedure

1. Create hotfix branch from `main`
2. Implement fix
3. Test thoroughly
4. Create pull request for `main` and `develop`
5. Deploy after approval

## Project Management

### Sprint Cycle

- 2-week sprints
- Sprint planning at beginning
- Daily standups
- Sprint review/demo at end
- Retrospective after each sprint

### Task Management

- Use TASKS.md for tracking progress
- Update task status when:
  - Starting work (In Progress)
  - Submitting for review (Review)
  - Completing task (Completed)
- Include task ID in branch names and commits

### Milestones

1. **Foundation Phase**: Basic structure and authentication
2. **Core Functionality**: Complaint submission and routing
3. **Agency Interface**: Department dashboards and response system
4. **Enhancements**: Analytics and extended features
5. **Deployment**: Testing and production release

## Communication

### Channels

- **Daily Updates**: Quick status updates
- **Technical Discussion**: Detailed technical conversations
- **Documentation**: Long-term knowledge sharing

### Meeting Schedule

- Weekly team meeting
- Daily 15-minute standup
- Technical deep-dives as needed

### Reporting

- Weekly progress reports
- Bug/issue tracking
- Performance metrics

## Documentation Standards

### Code Documentation

- Classes and methods must have docblocks
- Complex logic requires inline comments
- Use meaningful variable and function names

Example:
```php
/**
 * Register a new user in the system
 *
 * @param string $username User's chosen username
 * @param string $email User's email address
 * @param string $password Plain text password (will be hashed)
 * @return int|bool User ID on success, false on failure
 */
function registerUser($username, $email, $password) {
    // Implementation
}
```

### Project Documentation

- All features must be documented in FEATURES.md
- Update README.md for significant changes
- Document API endpoints if created
- Keep installation instructions current

## Quality Assurance

### Code Quality Tools

- PHP_CodeSniffer for style checking
- PHPStan for static analysis
- ESLint for JavaScript
- Browser DevTools for frontend debugging

### Security Practices

- Follow OWASP security guidelines
- Use prepared statements for database queries
- Validate all user inputs
- Sanitize outputs to prevent XSS
- Use HTTPS for all connections in production
- Regular security audits

### Performance Considerations

- Optimize database queries
- Minimize HTTP requests
- Use caching where appropriate
- Optimize images and assets
- Regular performance testing

---

This workflow document is subject to refinement as the project evolves. All team members are encouraged to suggest improvements to the process.

Last updated: May 16, 2025
