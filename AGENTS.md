# AI Agents for go-jira CLI

This document defines AI agents designed to assist with development, maintenance, and usage of the go-jira command-line interface. Each agent specializes in specific domains and workflows within the Jira CLI ecosystem.

## Agent Architecture Overview

The go-jira CLI follows a modular architecture with three main layers:
- **CLI Layer** (`jiracli/`) - Command-line interface, configuration, authentication
- **Command Layer** (`jiracmd/`) - Individual command implementations
- **Data Layer** (`jiradata/`) - Jira API data structures and types

## Core Agents

### 1. Jira Workflow Agent
**Domain**: Issue lifecycle management and workflow automation  
**Expertise**: Jira workflows, issue transitions, status management

**Responsibilities**:
- Automate complex issue transition workflows
- Handle multi-step issue operations (create → assign → transition → comment)
- Validate workflow constraints and business rules
- Assist with custom workflow implementations

**Key Files**: `jiracmd/transition.go`, `jiracmd/create.go`, `jiracmd/assign.go`, `jiradata/Transition.go`

**Example Tasks**:
- "Create a bug issue, assign to current user, and move to In Progress"
- "Batch transition all issues in sprint to Done status"
- "Implement custom workflow validation for epic management"

### 2. Authentication & Configuration Agent
**Domain**: Authentication, configuration management, security  
**Expertise**: API tokens, session management, configuration hierarchy

**Responsibilities**:
- Configure authentication methods (API token, bearer token, session)
- Manage configuration inheritance and hierarchy (.jira.d/ structure)
- Handle secure credential storage (keyring, pass, gopass)
- Troubleshoot authentication and connectivity issues

**Key Files**: `jiracli/cli.go`, `jiracmd/login.go`, `jiracmd/session.go`, `jiracli/password.go`

**Example Tasks**:
- "Set up API token authentication for Atlassian Cloud"
- "Configure project-specific settings with .jira.d/config.yml"
- "Troubleshoot 401 authentication errors"

### 3. Template & Output Agent
**Domain**: Output formatting, template management, data presentation  
**Expertise**: Go templates, YAML/JSON processing, custom output formats

**Responsibilities**:
- Create and modify output templates
- Handle data transformation and filtering (GJSON queries)
- Manage template inheritance and customization
- Design table, JSON, and custom output formats

**Key Files**: `jiracli/templates.go`, `jiracmd/exportTemplates.go`, `jiracmd/list.go`

**Example Tasks**:
- "Create a custom table template for sprint planning"
- "Design JSON output format for CI/CD integration"
- "Build template for issue status dashboard"

### 4. API Integration Agent
**Domain**: Jira REST API integration, data structures, HTTP client  
**Expertise**: Jira REST API, HTTP client configuration, data serialization

**Responsibilities**:
- Implement new Jira API endpoints
- Handle API versioning and compatibility
- Manage HTTP client configuration (proxies, SSL, timeouts)
- Design data structures for API responses

**Key Files**: `jiradata/*.go`, `jira.go`, `httpClient.go`, `search.go`

**Example Tasks**:
- "Add support for Jira Service Management API"
- "Implement new field types for custom Jira configurations"
- "Handle API deprecations and migrations"

### 5. CLI Command Agent
**Domain**: Command-line interface design, argument parsing, user experience  
**Expertise**: Command structure, CLI UX, argument validation

**Responsibilities**:
- Design new CLI commands and subcommands
- Implement command aliases and shortcuts
- Handle argument parsing and validation
- Improve command discoverability and help text

**Key Files**: `jiracmd/registry.go`, `jiracli/cli.go`, `cmd/jira/main.go`

**Example Tasks**:
- "Add 'jira dashboard' command for issue overview"
- "Implement bulk operations with '--batch' flag"
- "Design command shortcuts for common workflows"

### 6. Testing & Quality Agent
**Domain**: Test coverage, integration testing, quality assurance  
**Expertise**: Go testing, integration tests, CLI testing patterns, Makefile automation

**Responsibilities**:
- Write unit and integration tests (using `_t/*.t` test framework)
- Run quality checks with `make vet` and `make lint`
- Design test fixtures and mocks
- Implement CLI testing strategies using prove framework
- Ensure backward compatibility
- Manage test password store setup for authentication tests

**Key Files**: `test/*.go`, `_t/*.t`, `*_test.go`, `Makefile`

**Example Tasks**:
- "Add test coverage for authentication methods"
- "Create integration tests for workflow commands using _t/ framework"
- "Run `make prove` to execute full integration test suite"
- "Implement regression tests for template system"
- "Set up test environment with `make test-password-store`"

### 7. Custom Commands Agent
**Domain**: Custom command development, scripting, automation  
**Expertise**: Custom command configuration, shell scripting, workflow automation

**Responsibilities**:
- Design custom command frameworks
- Implement parameterized command templates
- Create reusable command libraries
- Build workflow automation scripts

**Key Files**: Configuration system, template processing

**Example Tasks**:
- "Create 'mine' command for personal issue dashboard"
- "Implement 'sprint' command for agile workflows"
- "Build custom commands for deployment workflows"

## Agent Collaboration Patterns

### Multi-Agent Workflows

1. **New Feature Implementation**
   - CLI Command Agent: Design command interface
   - API Integration Agent: Implement API calls
   - Template & Output Agent: Create output formats
   - Testing & Quality Agent: Add test coverage

2. **Authentication Setup**
   - Authentication & Configuration Agent: Handle auth setup
   - CLI Command Agent: Provide user-friendly commands
   - Testing & Quality Agent: Validate security practices

3. **Workflow Automation**
   - Jira Workflow Agent: Design workflow logic
   - Custom Commands Agent: Create automation scripts
   - Template & Output Agent: Format results
   - CLI Command Agent: Provide intuitive interface

## Integration Guidelines

### Agent Communication
- Agents should communicate through well-defined interfaces
- Use standard Go patterns for data exchange
- Maintain backward compatibility across agent interactions

### Configuration Management
- Respect the hierarchical configuration system (.jira.d/)
- Use environment variables for runtime configuration
- Maintain secure credential handling practices

### Error Handling
- Provide actionable error messages
- Include context for troubleshooting
- Support debug modes for detailed diagnostics

### Documentation
- Maintain inline code documentation
- Update help text and usage examples
- Provide agent-specific troubleshooting guides

## Development Workflow

### Agent Development Process
1. **Analysis Phase**: Understand domain requirements and existing code
2. **Design Phase**: Define agent responsibilities and interfaces
3. **Implementation Phase**: Develop agent functionality
4. **Testing Phase**: Validate functionality and integration
5. **Documentation Phase**: Update guides and examples

### Code Organization
- Follow existing package structure
- Maintain separation of concerns
- Use dependency injection patterns
- Keep agents modular and testable

### Build & Test Commands
Essential commands from the Makefile:
- **Build**: `make build` (local build) or `make install` (install to ~/bin)
- **Tests**: `make prove` (integration tests using _t/*.t files)
- **Quality**: `make vet` (static analysis) and `make lint` (style checking)
- **Release**: `make all` (cross-platform builds), `make release` (full release process)
- **Development**: `make generate` (schema generation), `make clean` (cleanup)

### Quality Standards
- Follow Go coding conventions
- Maintain test coverage above 80%
- Use static analysis tools (golint, go vet)
- Implement proper error handling
- Run `make prove` for integration testing
- Use `make vet` and `make lint` before commits

## Usage Examples

### Developer Workflow
```bash
# Authentication setup by Configuration Agent
jira login --endpoint=https://company.atlassian.net --user=dev@company.com

# Custom command creation by Custom Commands Agent
jira create-custom-command --name="daily-standup" --template="standup.yml"

# Workflow automation by Jira Workflow Agent  
jira workflow-batch --query="sprint in openSprints()" --transition="Done"
```

### Build & Development Tasks
```bash
# Build and install by CLI Command Agent
make build              # Build binary locally
make install            # Install to ~/bin
make all               # Cross-platform builds

# Quality assurance by Testing & Quality Agent
make vet               # Static analysis
make lint              # Style checking
make prove             # Integration tests

# Development workflow
make generate          # Generate schemas
make clean            # Clean build artifacts
make test-password-store  # Set up test auth

# Release management
make update-changelog  # Update changelog
make release          # Full release process
```

### Maintenance Tasks
```bash
# Template management by Template & Output Agent
jira export-templates --format=json --output=./templates/

# Schema updates by API Integration Agent
make generate  # Fetch and update Jira API schemas

# Quality assurance by Testing & Quality Agent
make vet && make lint && make prove  # Full quality check
```

This agent architecture provides comprehensive coverage of the go-jira CLI ecosystem while maintaining modularity, testability, and clear separation of concerns.