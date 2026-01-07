# API Endpoint Generation Prompt

Use this template for generating REST API endpoints or GraphQL resolvers.

## Prompt Template

```
I need to create a [REST/GraphQL] API endpoint for [PURPOSE].

## API Details
- **Method/Type**: [GET/POST/PUT/DELETE/PATCH or Query/Mutation]
- **Path**: [/api/resource]
- **Description**: [What this endpoint does]

## Request Format
### Parameters
- **Path Parameters**: [id, slug, etc.]
- **Query Parameters**: [filter, sort, page, etc.]
- **Headers**: [Authorization, Content-Type, etc.]
- **Body**: 
```[format]
[Request body structure]
```

## Response Format
### Success Response (200/201)
```[format]
[Response body structure]
```

### Error Responses
- **400**: [Bad request scenario]
- **401**: [Unauthorized scenario]
- **404**: [Not found scenario]
- **500**: [Server error scenario]

## Business Logic
1. [Step 1 of what the endpoint should do]
2. [Step 2]
3. [Step 3]

## Validation Rules
- [Validation rule 1]
- [Validation rule 2]

## Security Considerations
- [Authentication required?]
- [Authorization rules]
- [Rate limiting]
- [Input sanitization]

## Technical Requirements
- **Framework**: [Express, FastAPI, Spring Boot, etc.]
- **Database**: [PostgreSQL, MongoDB, etc.]
- **Authentication**: [JWT, OAuth, Session, etc.]
```

## Example Usage

```
I need to create a REST API endpoint for creating a new blog post.

## API Details
- **Method/Type**: POST
- **Path**: /api/posts
- **Description**: Create a new blog post with title, content, and tags

## Request Format
### Parameters
- **Path Parameters**: None
- **Query Parameters**: None
- **Headers**: 
  - Authorization: Bearer {token} (required)
  - Content-Type: application/json
- **Body**: 
```json
{
  "title": "string (required, 1-200 chars)",
  "content": "string (required, min 10 chars)",
  "tags": ["string"] (optional, max 5 tags),
  "published": "boolean (optional, default: false)"
}
```

## Response Format
### Success Response (201 Created)
```json
{
  "id": "uuid",
  "title": "string",
  "content": "string",
  "tags": ["string"],
  "published": false,
  "author_id": "uuid",
  "created_at": "ISO 8601 timestamp",
  "updated_at": "ISO 8601 timestamp"
}
```

### Error Responses
- **400**: Invalid request body (missing title/content, title too long, too many tags)
- **401**: Missing or invalid authentication token
- **403**: User not authorized to create posts
- **500**: Database error or server error

## Business Logic
1. Validate authentication token and extract user ID
2. Validate request body against schema
3. Sanitize content to prevent XSS
4. Create post record in database with author_id from token
5. If tags provided, create/link tag associations
6. Return created post with generated ID and timestamps

## Validation Rules
- Title: Required, 1-200 characters, no special characters except spaces and hyphens
- Content: Required, minimum 10 characters, HTML tags allowed but sanitized
- Tags: Optional, maximum 5 tags, each tag 1-30 characters, alphanumeric only
- Published: Optional boolean, defaults to false

## Security Considerations
- JWT authentication required (validate token expiry and signature)
- User must have "author" role to create posts
- Rate limit: 10 posts per hour per user
- Sanitize HTML content to prevent XSS attacks
- Prevent SQL injection by using parameterized queries

## Technical Requirements
- **Framework**: Node.js with Express 4.x
- **Database**: PostgreSQL 14 with Sequelize ORM
- **Authentication**: JWT tokens (verify with existing auth middleware)
- **Validation**: Use express-validator
- **Sanitization**: Use DOMPurify for HTML content
```

## Tips

- **RESTful Design**: Follow REST conventions for HTTP methods and status codes
- **Versioning**: Consider including API version in the path
- **Documentation**: Generated code should be self-documenting
- **Consistency**: Match patterns used in existing endpoints
