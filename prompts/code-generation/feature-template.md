# Feature Implementation Prompt

Use this template when asking an AI to generate code for a new feature.

## Prompt Template

```
I need help implementing a new feature in my [LANGUAGE/FRAMEWORK] project.

## Context
- **Project Type**: [web app, API, CLI tool, library, etc.]
- **Tech Stack**: [e.g., Python/Django, JavaScript/React, Java/Spring, etc.]
- **Current Architecture**: [Brief description of relevant parts]

## Feature Requirements
[Describe what the feature should do]

### Functional Requirements
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

### Non-Functional Requirements
- **Performance**: [Any performance constraints]
- **Security**: [Security considerations]
- **Scalability**: [Scalability needs]
- **Compatibility**: [Browser/platform requirements]

## Constraints
- Follow [CODING STANDARD/STYLE GUIDE]
- Must work with existing [COMPONENT/MODULE]
- [Any other limitations]

## Expected Deliverables
Please provide:
1. Complete, production-ready code
2. Inline comments for complex logic
3. Error handling and edge cases
4. Any necessary imports/dependencies

## Example Usage (if applicable)
```[language]
// Show how the feature should be used
```
```

## Example Usage

```
I need help implementing a new feature in my Python/FastAPI project.

## Context
- **Project Type**: REST API
- **Tech Stack**: Python 3.11, FastAPI, PostgreSQL, SQLAlchemy
- **Current Architecture**: Clean architecture with separate layers (routes, services, repositories)

## Feature Requirements
Implement user authentication with JWT tokens

### Functional Requirements
1. Users can register with email and password
2. Users can login and receive a JWT token
3. Protected endpoints verify the JWT token
4. Tokens expire after 24 hours

### Non-Functional Requirements
- **Performance**: Token validation should be < 10ms
- **Security**: Passwords must be hashed with bcrypt, tokens signed with RS256
- **Scalability**: Stateless authentication for horizontal scaling
- **Compatibility**: Follow OAuth 2.0 bearer token standard

## Constraints
- Follow PEP 8 style guide
- Must work with existing user model
- Use existing database session management
- No external authentication services (implement in-house)

## Expected Deliverables
Please provide:
1. Complete, production-ready code for auth routes, service, and middleware
2. Inline comments for security-critical sections
3. Error handling for invalid credentials, expired tokens, etc.
4. List of required dependencies (python-jose, passlib, etc.)

## Example Usage
```python
# Registration
POST /auth/register
{
  "email": "user@example.com",
  "password": "secure_password"
}

# Login
POST /auth/login
{
  "email": "user@example.com", 
  "password": "secure_password"
}
Response: {"access_token": "eyJ...", "token_type": "bearer"}

# Protected endpoint
GET /users/me
Headers: Authorization: Bearer eyJ...
```
```

## Tips

- **Be Specific**: The more details you provide, the better the code
- **Show Examples**: Include sample data structures or API responses
- **Mention Standards**: Reference coding standards or style guides you follow
- **Think Edge Cases**: Mention any special scenarios to handle
