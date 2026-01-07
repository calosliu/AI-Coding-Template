# Architectural Review Prompt

Use this template for reviewing overall architecture and design decisions.

## Prompt Template

```
I need an architectural review of my [PROJECT TYPE] system.

## System Overview
**Purpose**: [What the system does]
**Scale**: [Expected users, data volume, request rate]
**Current State**: [In development / Production / Planning]

## Architecture Diagram
[Describe components and their relationships, or paste ASCII/link to diagram]

## Components
1. **[Component Name]**
   - Technology: [e.g., React, Node.js]
   - Purpose: [What it does]
   - Scale: [Size, complexity]

2. **[Component Name]**
   - Technology: [e.g., PostgreSQL]
   - Purpose: [What it does]
   - Scale: [Size, complexity]

## Data Flow
[Describe how data moves through the system]

## Current Concerns
1. [Concern 1 - e.g., scalability]
2. [Concern 2 - e.g., reliability]
3. [Concern 3 - e.g., complexity]

## Review Criteria
Please evaluate:
- **Scalability**: Can this handle growth?
- **Reliability**: Single points of failure?
- **Performance**: Bottlenecks or inefficiencies?
- **Security**: Vulnerabilities in design?
- **Maintainability**: Easy to modify and debug?
- **Cost**: Resource efficiency?
- **Complexity**: Is it over-engineered or too simple?

## Constraints
- Budget: [Limited / Moderate / Flexible]
- Timeline: [Tight / Normal / Flexible]
- Team Size: [Number of developers]
- Expertise: [Team's technical strengths]

## Questions
1. [Specific question 1]
2. [Specific question 2]
```

## Example Usage

```
I need an architectural review of my e-commerce platform system.

## System Overview
**Purpose**: Online marketplace for buying/selling products with real-time inventory
**Scale**: 100K daily active users, 1M products, 10K orders per day
**Current State**: In development (planning phase)

## Architecture Diagram
```
[Client] -> [Load Balancer] -> [API Gateway]
                                    |
                    +---------------+---------------+
                    |               |               |
              [Auth Service]  [Order Service]  [Product Service]
                    |               |               |
                [Redis]      [PostgreSQL]      [PostgreSQL]
                                    |
                            [Message Queue] -> [Inventory Worker]
                                                      |
                                                [MongoDB]
```

## Components
1. **API Gateway**
   - Technology: Node.js with Express
   - Purpose: Route requests, rate limiting, basic auth check
   - Scale: Entry point for all traffic

2. **Auth Service**
   - Technology: Python with FastAPI
   - Purpose: User authentication and JWT token management
   - Scale: ~5K auth requests per minute

3. **Order Service**
   - Technology: Java with Spring Boot
   - Purpose: Handle order creation, payment processing
   - Scale: ~10K orders per day, peak 50 orders/minute

4. **Product Service**
   - Technology: Node.js with Express
   - Purpose: Product catalog, search, filtering
   - Scale: 1M products, 50K searches per hour

5. **Inventory Worker**
   - Technology: Python
   - Purpose: Process inventory updates asynchronously
   - Scale: Real-time inventory sync across warehouses

## Data Flow
1. User browses products (Product Service -> PostgreSQL)
2. User adds to cart (cached in Redis)
3. User checks out -> Order Service creates order (PostgreSQL)
4. Order Service publishes inventory event (Message Queue)
5. Inventory Worker updates stock (MongoDB)
6. If payment fails, compensating transaction rolls back inventory

## Current Concerns
1. Different languages for different services - is this too complex?
2. Using both PostgreSQL and MongoDB - should we consolidate?
3. Inventory sync latency - is async queue the right approach?
4. API Gateway might become a bottleneck

## Review Criteria
Please evaluate:
- **Scalability**: Can this handle 10x growth?
- **Reliability**: What happens if order service fails during checkout?
- **Performance**: Search performance with 1M products?
- **Security**: Is the auth architecture secure?
- **Maintainability**: Is polyglot architecture sustainable?
- **Cost**: Database and infrastructure costs reasonable?
- **Complexity**: Are we over-engineering for current scale?

## Constraints
- Budget: Moderate (prefer cloud services, avoid expensive licenses)
- Timeline: 6 months to MVP
- Team Size: 6 developers (2 backend Python, 2 backend Node.js, 2 frontend)
- Expertise: Strong in Python/Node.js, limited Java experience

## Questions
1. Should we use a single database technology instead of mixing PostgreSQL and MongoDB?
2. Is microservices overkill for our initial scale, or does it position us well for growth?
3. What's the best way to handle distributed transactions across services?
4. Should API Gateway do more (like request aggregation) or keep it simple?
```

## Tips

- **Visual**: Include diagrams or ASCII art to show structure
- **Quantify**: Provide numbers for scale expectations
- **Trade-offs**: Mention what you've considered and why
- **Future**: Discuss both current needs and future growth plans
