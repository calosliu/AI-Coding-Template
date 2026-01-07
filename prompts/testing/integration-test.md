# Integration Test Prompt

Use this template to create integration tests for system components.

## Prompt Template

```
Please create integration tests for [COMPONENT/FEATURE].

## System Under Test
**Component**: [Name of component being tested]
**Purpose**: [What it does]
**Dependencies**: [List of external services, databases, APIs]

## Test Environment
- **Language/Framework**: [e.g., Python/PyTest, JavaScript/Jest]
- **Test Type**: [API tests, database integration, service integration]
- **Infrastructure**: [Docker, test database, mock services]

## Integration Points
1. **[Service/Database A]**
   - Connection: [How to connect]
   - Test Data: [How to populate]
   - Cleanup: [How to reset]

2. **[Service/Database B]**
   - Connection: [How to connect]
   - Test Data: [How to populate]
   - Cleanup: [How to reset]

## Test Scenarios
### Scenario 1: [Name]
- **Setup**: [Initial state]
- **Action**: [What to do]
- **Expected**: [Expected outcome]
- **Verification**: [How to verify]

### Scenario 2: [Name]
- **Setup**: [Initial state]
- **Action**: [What to do]
- **Expected**: [Expected outcome]
- **Verification**: [How to verify]

## Test Data
[Describe test data needs, fixtures, or sample data]

## Requirements
- Use real database/services (not mocked)
- Tests should be idempotent
- Include proper setup and teardown
- Handle async operations correctly
- Test both success and failure scenarios

## Output Format
Provide:
1. Test setup and teardown code
2. Test data fixtures or factories
3. Complete integration tests
4. Instructions for running tests
5. Any environment variables or config needed
```

## Example Usage

```
Please create integration tests for the user registration and authentication flow.

## System Under Test
**Component**: Auth API (/api/auth/register and /api/auth/login endpoints)
**Purpose**: User registration with email verification and JWT-based login
**Dependencies**: 
- PostgreSQL database (users table)
- Redis cache (for rate limiting)
- Email service (SendGrid)

## Test Environment
- **Language/Framework**: Python 3.11 with PyTest and FastAPI
- **Test Type**: API integration tests
- **Infrastructure**: 
  - Docker containers for PostgreSQL and Redis
  - Mock email service (don't send real emails)

## Integration Points
1. **PostgreSQL Database**
   - Connection: Use test database URL from environment
   - Test Data: Create test users via SQLAlchemy fixtures
   - Cleanup: Rollback transactions after each test

2. **Redis Cache**
   - Connection: Connect to Redis test instance (db=15)
   - Test Data: No pre-population needed
   - Cleanup: Flush test database after each test

3. **SendGrid Email Service**
   - Connection: Use mock SMTP server or mock SendGrid client
   - Test Data: N/A
   - Cleanup: Clear sent emails list

## Test Scenarios
### Scenario 1: Successful Registration
- **Setup**: Empty database, no existing users
- **Action**: POST to /api/auth/register with valid email and password
- **Expected**: 201 status, user created in DB, verification email sent
- **Verification**: Check user exists, password is hashed, email in mock queue

### Scenario 2: Successful Login
- **Setup**: User exists in database (created via fixture)
- **Action**: POST to /api/auth/login with correct credentials
- **Expected**: 200 status, valid JWT token returned
- **Verification**: Decode token, verify claims, check expiration

### Scenario 3: Registration with Duplicate Email
- **Setup**: User with email "test@example.com" exists
- **Action**: POST to /api/auth/register with same email
- **Expected**: 409 Conflict status, error message
- **Verification**: Only one user with that email in database

### Scenario 4: Login with Wrong Password
- **Setup**: User exists in database
- **Action**: POST to /api/auth/login with incorrect password
- **Expected**: 401 Unauthorized status
- **Verification**: No token returned, rate limit counter incremented

### Scenario 5: Rate Limiting
- **Setup**: Clean Redis
- **Action**: POST to /api/auth/login 6 times in 1 minute (rate limit is 5)
- **Expected**: First 5 requests succeed/fail normally, 6th returns 429 Too Many Requests
- **Verification**: Check Redis rate limit counter

### Scenario 6: Token Validation
- **Setup**: User logged in (has valid token)
- **Action**: GET to /api/user/profile with token in Authorization header
- **Expected**: 200 status, user profile returned
- **Verification**: Response contains correct user data

## Test Data
```python
# Sample test user data
test_users = [
    {
        "email": "alice@example.com",
        "password": "SecurePass123!",
        "username": "alice"
    },
    {
        "email": "bob@example.com", 
        "password": "AnotherPass456!",
        "username": "bob"
    }
]
```

## Requirements
- Use real PostgreSQL and Redis (via Docker)
- Mock email service (use pytest-mock or unittest.mock)
- Tests should be idempotent (can run multiple times)
- Include proper setup and teardown with database transactions
- Handle async operations with async/await and pytest-asyncio
- Test both success and failure scenarios
- Use FastAPI TestClient for API calls

## Output Format
Provide:
1. conftest.py with pytest fixtures for database, Redis, and test client
2. Test data factories or fixtures
3. Complete integration tests in test_auth_integration.py
4. Requirements.txt additions for test dependencies
5. Docker Compose snippet for test services
6. README section on running integration tests
```

## Tips

- **Real Services**: Use actual databases/services, not mocks
- **Isolation**: Each test should be independent
- **Cleanup**: Always clean up test data
- **Docker**: Consider using docker-compose for test services
- **Speed**: Integration tests are slower; optimize where possible
