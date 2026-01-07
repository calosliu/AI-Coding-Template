# API Documentation Prompt

Use this template to generate comprehensive API documentation.

## Prompt Template

```
Please generate API documentation for [API NAME/ENDPOINT].

## API Overview
- **Base URL**: [e.g., https://api.example.com/v1]
- **Authentication**: [Bearer token, API key, OAuth, etc.]
- **Format**: [JSON, XML, etc.]

## Endpoints to Document
[List endpoints or provide the code]

## Documentation Format
Please provide documentation in [FORMAT] format:
- **OpenAPI/Swagger**: Machine-readable spec
- **Markdown**: Human-readable docs
- **Postman Collection**: For testing
- **README**: For GitHub repository

## Include in Documentation
- [ ] Endpoint URL and HTTP method
- [ ] Description of what the endpoint does
- [ ] Authentication requirements
- [ ] Request parameters (path, query, headers, body)
- [ ] Request examples (with curl and code samples)
- [ ] Response format and fields
- [ ] Response examples (success and error cases)
- [ ] Status codes and their meanings
- [ ] Rate limiting information
- [ ] Error handling and error codes

## Code Examples
Include examples in these languages:
- [e.g., curl, Python, JavaScript, etc.]

## Additional Information
- **Versioning**: [How API is versioned]
- **Rate Limits**: [Requests per minute/hour]
- **Pagination**: [How to paginate results]
- **Filtering**: [How to filter/sort results]
```

## Example Usage

```
Please generate API documentation for a User Management API.

## API Overview
- **Base URL**: https://api.myapp.com/v1
- **Authentication**: Bearer JWT token in Authorization header
- **Format**: JSON

## Endpoints to Document

```python
# User endpoints
@app.post("/users")
async def create_user(user: UserCreate):
    """Create a new user"""
    pass

@app.get("/users/{user_id}")
async def get_user(user_id: UUID):
    """Get user by ID"""
    pass

@app.get("/users")
async def list_users(skip: int = 0, limit: int = 100, role: str = None):
    """List users with pagination and filtering"""
    pass

@app.put("/users/{user_id}")
async def update_user(user_id: UUID, user: UserUpdate):
    """Update user details"""
    pass

@app.delete("/users/{user_id}")
async def delete_user(user_id: UUID):
    """Delete a user"""
    pass
```

## Documentation Format
Please provide documentation in **Markdown** format suitable for a README or docs folder.

## Include in Documentation
- [x] Endpoint URL and HTTP method
- [x] Description of what the endpoint does
- [x] Authentication requirements
- [x] Request parameters (path, query, headers, body)
- [x] Request examples (with curl and code samples)
- [x] Response format and fields
- [x] Response examples (success and error cases)
- [x] Status codes and their meanings
- [x] Rate limiting information
- [x] Error handling and error codes

## Code Examples
Include examples in these languages:
- curl (with actual curl commands)
- Python (using requests library)
- JavaScript (using fetch API)

## Additional Information
- **Versioning**: API version in URL path (/v1)
- **Rate Limits**: 100 requests per minute per API key
- **Pagination**: Use skip and limit query parameters, max limit is 100
- **Filtering**: Use role query parameter to filter users by role
- **User Roles**: admin, user, guest (for filtering)
- **User Schema**:
  ```json
  {
    "id": "uuid",
    "email": "string",
    "username": "string",
    "role": "string (admin|user|guest)",
    "created_at": "ISO 8601 timestamp",
    "updated_at": "ISO 8601 timestamp"
  }
  ```
```

## Tips

- **Complete**: Include all necessary details for developers to use the API
- **Examples**: Provide working examples that can be copied and pasted
- **Errors**: Document all possible error responses
- **Standards**: Follow OpenAPI/Swagger standards if generating specs
- **Testing**: Include examples that developers can test immediately
