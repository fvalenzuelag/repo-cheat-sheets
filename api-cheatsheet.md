# API Development Cheat Sheet

## API Basics

### REST API Concepts
- **REST** - Representational State Transfer
- **Resources** - Entities exposed via URIs (e.g., `/users`, `/products`)
- **HTTP Methods**:
  - `GET` - Retrieve data
  - `POST` - Create data
  - `PUT` - Update entire resource
  - `PATCH` - Update partial resource
  - `DELETE` - Remove a resource
- **Stateless** - Server doesn't store client state between requests
- **Uniform Interface** - Consistent resource identification and manipulation

### HTTP Status Codes
- **2xx Success**
  - `200 OK` - Request succeeded
  - `201 Created` - Resource created successfully
  - `204 No Content` - Success with no response body
- **3xx Redirection**
  - `301 Moved Permanently` - Resource URI changed permanently
  - `302 Found` - Temporary redirection
- **4xx Client Errors**
  - `400 Bad Request` - Malformed request
  - `401 Unauthorized` - Authentication required
  - `403 Forbidden` - Not allowed to access
  - `404 Not Found` - Resource doesn't exist
  - `405 Method Not Allowed` - HTTP method not supported
  - `422 Unprocessable Entity` - Validation errors
- **5xx Server Errors**
  - `500 Internal Server Error` - Generic server error
  - `502 Bad Gateway` - Invalid response from upstream
  - `503 Service Unavailable` - Server temporarily unavailable

### API Header Fields
- `Accept` - Response formats client accepts (e.g., `application/json`)
- `Content-Type` - Format of request body (e.g., `application/json`)
- `Authorization` - Authentication credentials
- `Cache-Control` - Caching directives
- `User-Agent` - Client information
- `X-Request-ID` - Tracking ID for requests

## API Design Best Practices

### URL Structure
- Use nouns for resources: `/users` instead of `/getUsers`
- Use plural nouns: `/products` instead of `/product`
- Use resource hierarchy: `/users/123/orders`
- Include API version: `/api/v1/users`
- Use kebab-case for paths: `/product-categories`
- Use query parameters for filtering/sorting: `/products?category=electronics&sort=price`

### Request/Response Design
- Use consistent case style (camelCase or snake_case)
- Include pagination for large collections:
  ```json
  {
    "data": [...],
    "pagination": {
      "total": 100,
      "page": 2,
      "per_page": 20,
      "next": "/api/v1/products?page=3",
      "previous": "/api/v1/products?page=1"
    }
  }
  ```
- Use HATEOAS links:
  ```json
  {
    "id": 123,
    "name": "Product",
    "_links": {
      "self": "/api/products/123",
      "reviews": "/api/products/123/reviews"
    }
  }
  ```
- Return proper error responses:
  ```json
  {
    "error": {
      "code": "INVALID_PARAMETER",
      "message": "Invalid parameter: id",
      "details": ["Parameter must be a positive integer"]
    }
  }
  ```

## Authentication & Authorization

### Authentication Methods
- **API Keys** - Simple key in header or query param: `X-API-Key: abcd1234`
- **Basic Auth** - Base64 encoded username/password: `Authorization: Basic dXNlcjpwYXNz`
- **JWT** - JSON Web Tokens: `Authorization: Bearer eyJhbGciOiJIUzI1NiIsI...`
- **OAuth 2.0** - Delegated authorization framework with different grant types
- **API Gateway** - External authentication layer in front of APIs

### JWT Structure
```
header.payload.signature
```
- **Header**: Algorithm and token type
- **Payload**: Claims (data)
- **Signature**: Verification code

### OAuth 2.0 Grant Types
- **Authorization Code** - For server-side web apps
- **Implicit** - For client-side apps (deprecated)
- **Resource Owner Password** - Direct username/password (use sparingly)
- **Client Credentials** - For server-to-server communication
- **Refresh Token** - For obtaining new access tokens

## API Testing

### HTTP Testing Tools
- **cURL** - Command-line tool:
  ```bash
  curl -X POST https://api.example.com/users \
    -H "Content-Type: application/json" \
    -d '{"name": "John", "email": "john@example.com"}'
  ```
- **Postman** - GUI-based API testing tool
- **Insomnia** - REST client alternative to Postman
- **HTTPie** - User-friendly command-line HTTP client:
  ```bash
  http POST api.example.com/users name=John email=john@example.com
  ```

### Testing Frameworks
- **Python**: Requests, pytest, Robot Framework
- **JavaScript**: Axios, Supertest, Jest, Mocha
- **Java**: REST Assured, JUnit, TestNG
- **Contract Testing**: Pact, Postman Contract Testing

## API Implementation

### Python FastAPI Example
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

class User(BaseModel):
    name: str
    email: str

@app.post("/users/", status_code=201)
async def create_user(user: User):
    # Save user to database
    return {"id": 1, **user.dict()}

@app.get("/users/{user_id}")
async def get_user(user_id: int):
    if user_id != 1:
        raise HTTPException(status_code=404, detail="User not found")
    return {"id": user_id, "name": "John", "email": "john@example.com"}
```

### Node.js Express Example
```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/users', (req, res) => {
  const { name, email } = req.body;
  // Save to database
  res.status(201).json({ id: 1, name, email });
});

app.get('/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  if (id !== 1) {
    return res.status(404).json({ error: 'User not found' });
  }
  res.json({ id, name: 'John', email: 'john@example.com' });
});

app.listen(3000, () => console.log('API running on port 3000'));
```

## API Documentation

### Documentation Standards
- **OpenAPI (Swagger)** - Industry standard for REST API documentation
- **API Blueprint** - Markdown-based API documentation
- **RAML** - YAML-based API modeling language

### OpenAPI Example
```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0
paths:
  /users:
    post:
      summary: Create a user
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '201':
          description: User created
  /users/{userId}:
    get:
      summary: Get user by ID
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
      responses:
        '200':
          description: User found
        '404':
          description: User not found
components:
  schemas:
    User:
      type: object
      properties:
        name:
          type: string
        email:
          type: string
```

## API Security

### Security Best Practices
- Use HTTPS for all API endpoints
- Implement proper authentication and authorization
- Validate all input data
- Set appropriate CORS headers
- Rate limiting to prevent abuse
- Security headers (`X-Content-Type-Options`, `X-Frame-Options`, etc.)
- API gateway for security controls
- Regular security audits and penetration testing

### CORS Headers
```
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

## API Integration

### API Client Examples

#### Python Requests
```python
import requests

# GET request
response = requests.get('https://api.example.com/users/1', 
                       headers={'Authorization': 'Bearer token'})
user = response.json()

# POST request
response = requests.post('https://api.example.com/users',
                        headers={'Content-Type': 'application/json'},
                        json={'name': 'John', 'email': 'john@example.com'})
```

#### JavaScript Fetch
```javascript
// GET request
fetch('https://api.example.com/users/1', {
  headers: {
    'Authorization': 'Bearer token'
  }
})
.then(response => response.json())
.then(user => console.log(user));

// POST request
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer token'
  },
  body: JSON.stringify({ name: 'John', email: 'john@example.com' })
})
.then(response => response.json())
```

### Webhooks
- Event-driven architecture from server to client
- Register callback URL with API provider
- Secure with HMAC signatures
- Implement retry logic for failed delivery
- Process events asynchronously
- Example signature verification:
  ```python
  import hmac, hashlib
  
  def verify_signature(payload, signature, secret):
      computed = hmac.new(secret.encode(), payload.encode(), hashlib.sha256).hexdigest()
      return hmac.compare_digest(computed, signature)
  ```

## API Performance & Scaling

### Performance Optimization
- Enable compression (gzip, Brotli)
- Implement caching with ETags and Cache-Control headers
- Use connection pooling
- Batch operations for multiple resources
- Pagination for large result sets
- Query optimization for database operations
- CDN for static resources

### Scaling Strategies
- Horizontal scaling (more instances)
- Load balancing
- Database sharding
- Caching layers (Redis, Memcached)
- Asynchronous processing with message queues
- Microservices architecture
- Serverless functions for variable load
