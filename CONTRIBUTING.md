# Contributing to News Editorial Workflow Automation Recipe

Thank you for your interest in contributing to the News Editorial Workflow Automation Recipe! This project is built with ❤️ for the Drupal community and we welcome contributions from developers, editors, and content professionals.

## 🚀 Getting Started

### Prerequisites
- Drupal 11.0+
- ECA module 2.1+
- Git knowledge
- Basic understanding of Drupal workflows
- Familiarity with YAML configuration

### Development Environment Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/news-editorial-workflow-recipe.git
   cd news-editorial-workflow-recipe
   ```

2. **Set up local Drupal environment**
   ```bash
   # Using DDEV (recommended)
   ddev start
   ddev composer install
   ddev drush si minimal
   
   # Or using Lando
   lando start
   lando composer install
   lando drush si minimal
   ```

3. **Install the recipe**
   ```bash
   ddev drush recipe recipes/news_editorial_workflow
   ```

4. **Enable development modules**
   ```bash
   ddev drush en devel webprofiler
   ```

## 🛠️ How to Contribute

### Reporting Issues

Before creating an issue, please:

1. **Search existing issues** to avoid duplicates
2. **Use the issue template** when creating new issues
3. **Provide detailed information** including:
   - Drupal version
   - ECA module version
   - Steps to reproduce
   - Expected vs actual behavior
   - Error messages or logs

### Suggesting Features

Feature requests are welcome! Please:

1. **Check the roadmap** in README.md first
2. **Open a discussion** before implementing large features
3. **Explain the use case** and business value
4. **Consider backwards compatibility**

### Code Contributions

#### Pull Request Process

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow coding standards (see below)
   - Add/update tests
   - Update documentation

4. **Test your changes**
   ```bash
   # Run code quality checks
   ddev exec phpcs --standard=Drupal recipes/
   
   # Run tests
   ddev drush test-run --group EditorialWorkflow
   ```

5. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add amazing new feature"
   ```

6. **Push and create pull request**
   ```bash
   git push origin feature/your-feature-name
   ```

#### Commit Message Convention

We use [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation changes
- `style:` Code style changes
- `refactor:` Code refactoring
- `test:` Test additions or changes
- `chore:` Maintenance tasks

Examples:
```
feat: add bulk state transition action
fix: resolve notification email template issue
docs: update installation instructions
test: add unit tests for state validation
```

## 📝 Coding Standards

### Drupal Standards
- Follow [Drupal Coding Standards](https://www.drupal.org/docs/develop/standards)
- Use `phpcs --standard=Drupal` for code quality
- Ensure all code is properly documented

### YAML Configuration
```yaml
# Use consistent indentation (2 spaces)
workflow_config:
  states:
    draft:
      label: 'Draft'
      weight: 0
  transitions:
    submit_for_review:
      from: [draft]
      to: in_review
```

### ECA Model Guidelines
- **Descriptive names** for all components
- **Clear conditions** with readable logic
- **Proper error handling** in all actions
- **Performance considerations** for large sites

### Testing Requirements

#### Unit Tests
```php
<?php

/**
 * @group EditorialWorkflow
 */
class EditorialWorkflowTest extends UnitTestCase {
  
  public function testStateTransition() {
    // Test implementation
  }
}
```

#### Integration Tests
```php
<?php

/**
 * @group EditorialWorkflow
 */
class EditorialWorkflowIntegrationTest extends BrowserTestBase {
  
  protected $defaultTheme = 'stark';
  
  protected static $modules = [
    'eca_base',
    'eca_content',
    'content_moderation',
  ];
  
  public function testWorkflowNotifications() {
    // Test implementation
  }
}
```

## 📚 Documentation Guidelines

### README Updates
- Keep installation instructions current
- Add examples for new features
- Update configuration options
- Include troubleshooting tips

### Code Documentation
```php
/**
 * Validates content before state transition.
 *
 * @param \Drupal\node\NodeInterface $node
 *   The node being transitioned.
 * @param string $target_state
 *   The target moderation state.
 *
 * @return bool
 *   TRUE if validation passes, FALSE otherwise.
 */
public function validateContentTransition(NodeInterface $node, string $target_state): bool {
  // Implementation
}
```

### Configuration Documentation
```yaml
# Configuration example with comments
eca_model:
  # Event that triggers the workflow
  events:
    content_moderation_state_change:
      # Condition to check workflow type
      conditions:
        workflow_check:
          workflow_id: basic_editorial
```

## 🧪 Testing Guidelines

### Test Coverage Requirements
- **Unit tests** for all custom logic
- **Integration tests** for workflow scenarios
- **Functional tests** for user interactions
- **Performance tests** for large datasets

### Test Data
```php
// Use consistent test data
protected function createTestNode(): NodeInterface {
  return $this->drupalCreateNode([
    'type' => 'news',
    'title' => 'Test Article',
    'moderation_state' => 'draft',
  ]);
}
```

### Running Tests Locally
```bash
# Unit tests
ddev exec phpunit --group EditorialWorkflow

# Functional tests
ddev drush test-run --group EditorialWorkflow

# Performance tests
ddev exec ab -n 100 -c 10 http://local-site.ddev.site/
```

## 🔍 Code Review Process

### Reviewer Guidelines
- **Functionality**: Does it work as intended?
- **Code quality**: Follows standards and best practices?
- **Performance**: No unnecessary database queries or loops?
- **Security**: Proper validation and access controls?
- **Documentation**: Clear and comprehensive?
- **Tests**: Adequate coverage and quality?

### Author Guidelines
- **Self-review** your code before requesting review
- **Respond promptly** to reviewer feedback
- **Ask questions** if feedback is unclear
- **Test thoroughly** on different scenarios

## 🎯 Specific Contribution Areas

### 🔧 ECA Model Enhancements
- New conditional logic
- Additional action types
- Performance optimizations
- Error handling improvements

### 📧 Notification System
- Email template improvements
- New notification channels (Slack, Teams, etc.)
- Personalization features
- Delivery optimization

### 📊 Analytics & Reporting
- Editorial performance metrics
- Workflow bottleneck analysis
- User productivity reports
- Content success tracking

### 🌐 Internationalization
- Translation improvements
- Multi-language workflow support
- Regional compliance features
- Localized notification templates

### 🔐 Security Enhancements
- Permission system improvements
- Audit trail enhancements
- Data privacy features
- Compliance tools

## 🏆 Recognition

Contributors will be recognized in:
- README.md credits section
- CHANGELOG.md for significant contributions
- Release notes
- Community showcases

## 📞 Getting Help

- **GitHub Discussions**: For questions and ideas
- **GitHub Issues**: For bugs and feature requests
- **Drupal Slack**: #editorial-workflow channel
- **ECA Documentation**: https://www.drupal.org/project/eca

## 📄 License

By contributing, you agree that your contributions will be licensed under the GPL-2.0+ License.

---

**Thank you for making the News Editorial Workflow Automation Recipe better! 🎉** 