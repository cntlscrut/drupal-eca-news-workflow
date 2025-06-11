# News Editorial Workflow Automation Recipe

![Editorial Workflow](https://img.shields.io/badge/Drupal-Recipe-blue.svg) ![ECA](https://img.shields.io/badge/ECA-Automation-green.svg) ![Version](https://img.shields.io/badge/version-1.0.0-brightgreen.svg)

A comprehensive automated editorial workflow system built with ECA (Event Condition Action) for news organizations, magazines, and content-driven websites using Drupal.

## 🚀 Features

### 📝 Editorial Workflow
- **Three-State Process**: Draft → In Review → Published
- **Bidirectional Editing**: Content can move back and forth between Draft and In Review
- **Smart Transitions**: Automatic validation and checks during state changes
- **Bulk Operations**: Manage multiple articles efficiently
- **Content Scheduling**: Plan publication dates and times

### 📧 Intelligent Notifications
- **Role-Based Alerts**: Targeted notifications based on user roles
- **Author Updates**: Real-time feedback on content status
- **Editor Assignments**: Automatic notification of pending reviews
- **Publication Alerts**: Stakeholder updates on published content
- **Customizable Templates**: Personalize notification messages

### 📊 Monitoring & Analytics
- **Activity Logging**: Complete audit trail of all editorial actions
- **Performance Metrics**: Track editorial efficiency
- **User Analytics**: Monitor team productivity
- **Content Analytics**: Publication success metrics
- **Workflow Optimization**: Data-driven process improvements

### ⚡ Automation Engine
- **Content Validation**: Automatic quality checks
- **SEO Compliance**: Built-in optimization checks
- **Category Assignment**: Smart content categorization
- **Publication Automation**: Scheduled publishing
- **Workflow Rules**: Customizable business logic

## 🛠️ Installation

### Prerequisites
- Drupal 11.0+
- ECA module 2.1+
- Content Moderation module
- PHP 8.1+

### Quick Install
```bash
# Using Drupal's recipe system
drush recipe recipes/news_editorial_workflow

# Or install manually
drush en eca_base eca_content eca_log eca_workflow content_moderation workflows
drush cim --partial --source=recipes/news_editorial_workflow/config
```

### Composer Installation
```bash
# If installing from a repository
composer require drupal/news_editorial_workflow_recipe
drush recipe news_editorial_workflow
```

## 📖 Usage

### 1. Basic Workflow
```php
// Content starts in Draft state
$node = Node::create([
  'type' => 'news',
  'title' => 'Breaking News Article',
  'moderation_state' => 'draft',
]);
$node->save();

// Submit for review
$node->set('moderation_state', 'in_review');
$node->save(); // Triggers notification to editors

// Publish content
$node->set('moderation_state', 'published');
$node->save(); // Triggers publication notifications
```

### 2. Customizing Notifications
Edit the ECA model to customize notification behavior:

```yaml
# In eca.eca.news_editorial_workflow.yml
events:
  content_moderation_state_change:
    conditions:
      - workflow_id: basic_editorial
      - from_state: draft
      - to_state: in_review
    actions:
      - send_email:
          to: '[node:author:mail]'
          subject: 'Article submitted for review'
```

### 3. Adding Custom States
Extend the workflow by adding new states:

```yaml
# In workflows.workflow.basic_editorial.yml
type_settings:
  states:
    fact_check:
      label: 'Fact Check'
      weight: 3
  transitions:
    fact_check_to_review:
      from: [fact_check]
      to: in_review
```

## 🏗️ Architecture

### ECA Model Structure
```
News Editorial Workflow
├── Content Events
│   ├── State Change Triggers
│   ├── User Action Events
│   └── Scheduled Events
├── Conditions
│   ├── Role Checks
│   ├── Content Validation
│   └── Business Rules
└── Actions
    ├── Email Notifications
    ├── User Messages
    ├── Logging
    └── Content Operations
```

### State Machine
```mermaid
graph LR
    A[Draft] -->|Submit| B[In Review]
    B -->|Approve| C[Published]
    B -->|Reject| A
    C -->|Unpublish| D[Unpublished]
    D -->|Republish| C
```

## 🎨 Customization

### Adding New Roles
1. Create new role: `drush role:create fact_checker "Fact Checker"`
2. Update ECA conditions to include new role
3. Modify notification templates

### Custom Email Templates
```yaml
email_templates:
  submission_notification:
    subject: '[site:name] - Article Submitted: [node:title]'
    body: |
      Dear Editor,
      
      A new article has been submitted for review:
      Title: [node:title]
      Author: [node:author:display-name]
      
      Review at: [node:url]
```

### Integration Examples

#### With Views Bulk Operations
```yaml
# Custom VBO action for bulk state changes
views_bulk_operations:
  actions:
    bulk_moderate_content:
      plugin_id: node_moderation_action
      configuration:
        state: published
```

#### With Scheduler Integration
```yaml
# Automatic publication scheduling
scheduler_integration:
  publish_on: '[node:field_publish_date:value]'
  unpublish_on: '[node:field_unpublish_date:value]'
```

## 🧪 Testing

### Running Tests
```bash
# Unit tests
phpunit --group news_editorial_workflow

# Functional tests
drush test-run --group EditorialWorkflow

# Behat scenarios
behat features/editorial-workflow.feature
```

### Test Scenarios Included
- ✅ Basic state transitions
- ✅ Role-based permissions
- ✅ Email notification delivery
- ✅ Bulk operations
- ✅ Content validation
- ✅ Performance under load

## 🔧 Configuration

### Environment Variables
```bash
# Email configuration
EDITORIAL_SMTP_HOST=smtp.example.com
EDITORIAL_FROM_EMAIL=editorial@example.com

# Notification settings
EDITORIAL_ENABLE_SLACK=true
EDITORIAL_SLACK_WEBHOOK=https://hooks.slack.com/...
```

### Configuration Files
- `workflows.workflow.basic_editorial.yml` - Workflow definition
- `eca.eca.news_editorial_workflow.yml` - ECA configuration
- `eca.model.news_editorial_workflow.yml` - ECA model definition

## 📚 Examples

### 1. News Website Setup
Perfect for online newspapers with multiple reporters and editors:
```bash
drush recipe news_editorial_workflow
drush role:create reporter "Reporter"
drush role:create editor "Editor"
drush role:create publisher "Publisher"
```

### 2. Magazine Integration
Ideal for monthly/weekly magazines with longer review cycles:
```yaml
# Extended review process
workflow_modifications:
  additional_states:
    - copy_edit
    - fact_check
    - legal_review
```

### 3. Corporate Blog
Streamlined workflow for corporate content:
```yaml
# Simplified two-state workflow
simple_workflow:
  states: [draft, published]
  auto_publish: true
```

## 🤝 Contributing

We welcome contributions! Here's how to get involved:

### Development Setup
```bash
git clone https://github.com/your-org/news-editorial-workflow-recipe.git
cd news-editorial-workflow-recipe
composer install
ddev start
```

### Contribution Guidelines
1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Code Standards
- Follow Drupal coding standards
- Write comprehensive tests
- Document all changes
- Update README as needed

## 📄 License

This project is licensed under the GPL-2.0+ License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

- **Documentation**: [docs.example.com](https://docs.example.com)
- **Issues**: [GitHub Issues](https://github.com/your-org/news-editorial-workflow-recipe/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-org/news-editorial-workflow-recipe/discussions)
- **Slack**: [#editorial-workflow](https://drupal.slack.com/channels/editorial-workflow)

## 🎖️ Credits

- Built with ❤️ by the [ECA Team](https://www.drupal.org/project/eca)
- Inspired by real-world newsroom workflows
- Community contributions from editorial professionals

## 🗺️ Roadmap

### Version 1.1 (Planned)
- [ ] AI-powered content suggestions
- [ ] Advanced analytics dashboard
- [ ] Multi-site workflow synchronization
- [ ] Mobile editorial app integration

### Version 1.2 (Future)
- [ ] Video/multimedia workflow support
- [ ] Social media integration
- [ ] Real-time collaborative editing
- [ ] Advanced permission granularity

---

**Made with ❤️ for the Drupal Community**

[![Drupal](https://img.shields.io/badge/Drupal-blue.svg)](https://drupal.org) [![ECA](https://img.shields.io/badge/ECA-green.svg)](https://www.drupal.org/project/eca) [![Community](https://img.shields.io/badge/Community-Driven-orange.svg)](https://github.com/your-org/news-editorial-workflow-recipe) 