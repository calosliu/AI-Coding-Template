# README Generation Prompt

Use this template to create or improve README files for your project.

## Prompt Template

```
Please create a comprehensive README.md file for [PROJECT NAME].

## Project Information
- **Name**: [Project name]
- **Type**: [Library, Application, API, CLI tool, etc.]
- **Language/Framework**: [Technology stack]
- **Purpose**: [What problem does it solve]
- **Status**: [In development, Beta, Production, etc.]

## Target Audience
- **Primary Users**: [Developers, end-users, etc.]
- **Use Cases**: [When would someone use this]

## Repository Context
- **License**: [MIT, Apache 2.0, GPL, etc.]
- **Contribution Status**: [Open to contributions, Private, etc.]

## README Sections
Please include:
- [ ] Project title and description
- [ ] Badges (build status, coverage, version, license)
- [ ] Features/Highlights
- [ ] Screenshots or demo (if applicable)
- [ ] Installation instructions
- [ ] Quick start / Getting started
- [ ] Usage examples
- [ ] API documentation or link
- [ ] Configuration options
- [ ] Development setup
- [ ] Testing instructions
- [ ] Contributing guidelines
- [ ] License information
- [ ] Contact/Support information
- [ ] Acknowledgments
- [ ] Roadmap (optional)

## Special Requirements
[Any specific formatting, sections, or emphasis needed]

## Style Preferences
- **Tone**: [Professional, Friendly, Technical, etc.]
- **Length**: [Concise, Detailed, Comprehensive]
- **Code Examples**: [Minimal, Extensive]
```

## Example Usage

```
Please create a comprehensive README.md file for TaskFlow API.

## Project Information
- **Name**: TaskFlow API
- **Type**: REST API / Backend Service
- **Language/Framework**: Python 3.11, FastAPI, PostgreSQL, Redis
- **Purpose**: Project and task management API for teams, featuring real-time collaboration, webhooks, and integrations
- **Status**: Beta (v0.8.0)

## Target Audience
- **Primary Users**: 
  - Developers building productivity apps
  - Teams needing task management backend
  - SaaS companies wanting white-label task management
- **Use Cases**: 
  - Building custom project management tools
  - Integrating task management into existing apps
  - Creating team collaboration platforms

## Repository Context
- **License**: MIT License
- **Contribution Status**: Open to contributions (actively maintained)
- **GitHub**: https://github.com/example/taskflow-api

## README Sections
Please include:
- [x] Project title and description
- [x] Badges (build status, coverage, version, license, Python version)
- [x] Features/Highlights (list key features)
- [ ] Screenshots or demo (N/A for API)
- [x] Installation instructions (Docker and local setup)
- [x] Quick start / Getting started (how to run and make first API call)
- [x] Usage examples (common API operations with curl and Python)
- [x] API documentation link (link to full docs)
- [x] Configuration options (environment variables)
- [x] Development setup (how to contribute)
- [x] Testing instructions (how to run tests)
- [x] Contributing guidelines (brief, link to CONTRIBUTING.md)
- [x] License information
- [x] Contact/Support (GitHub issues, Discord link)
- [x] Acknowledgments (thank contributors, list main dependencies)
- [x] Roadmap (mention v1.0 features)

## Special Requirements
- Include Docker Compose example for quick start
- Show example API calls with both curl and Python requests
- Mention performance characteristics (handles 1000+ req/s)
- Include link to Postman collection
- Add security note about API key management
- Include architecture diagram (ASCII art is fine)

## Key Features to Highlight
1. RESTful API with comprehensive endpoints
2. Real-time updates via WebSockets
3. Webhook support for integrations
4. Role-based access control (RBAC)
5. Full-text search
6. Audit logging
7. Rate limiting
8. API versioning

## Style Preferences
- **Tone**: Professional but approachable
- **Length**: Comprehensive (include all important details but stay organized)
- **Code Examples**: Extensive (show multiple examples for clarity)
- **Structure**: Use clear headers and table of contents
```

## Tips

- **First Impression**: The README is often the first thing people see
- **Scannable**: Use headers, lists, and formatting for easy scanning
- **Actionable**: Include working code examples that can be copied
- **Complete**: Cover installation, usage, and contribution
- **Maintained**: README should evolve with the project
