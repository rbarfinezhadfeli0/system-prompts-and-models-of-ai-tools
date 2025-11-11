# Best Practices for Public Routes - AI Tool Prompts

> Compiled from analysis of 30+ AI development tool system prompts

---

## Table of Contents

1. [Route Structure & Naming](#1-route-structure--naming)
2. [Security Best Practices](#2-security-best-practices)
3. [Input Validation](#3-input-validation)
4. [Error Handling](#4-error-handling)
5. [API Endpoint Guidelines](#5-api-endpoint-guidelines)
6. [Testing & Monitoring](#6-testing--monitoring)

---

## 1. Route Structure & Naming

### Use Explicit `/api` Prefix for Backend Routes

**Source: Emergent**

- All backend API routes MUST use `/api` prefix
- Routes without `/api` are directed to frontend
- Routes with `/api` are redirected to backend services
- **Critical:** Failing to use '/api' prefix results in incorrect routing and service failures

**Example:**
```
✅ /api/users
✅ /api/users/:id
✅ /api/status-check
❌ /users (frontend route)
❌ /check-status (frontend route)
```

### Endpoint Metadata Structure

**Source: Leap.new**

Define endpoints with explicit metadata:

```json
{
  "endpoints": [
    {
      "name": "unique_endpoint_name",
      "method": "GET|POST|PUT|DELETE|PATCH",
      "path": "/api/resource/:id",
      "expose": true,
      "auth": true
    }
  ]
}
```

**Key Fields:**
- `expose`: Boolean - Is endpoint publicly accessible?
- `auth`: Boolean - Does endpoint require authentication?
- `method`: HTTP verb (GET, POST, PUT, DELETE, PATCH)
- `path`: API path with parameter support

### Environment-Based URL Configuration

**Source: Emergent**

**DO:**
- Store backend URL in environment variables
- Use `REACT_APP_BACKEND_URL` or `process.env.REACT_APP_BACKEND_URL`
- Read from `import.meta.env.REACT_APP_BACKEND_URL`

**DON'T:**
- Hardcode URLs or ports in code
- Modify URLs or ports in .env files
- Include environment-specific values in version control

---

## 2. Security Best Practices

### Never Expose Credentials

**Source: Multiple (Windsurf, Devin AI, Trae, Claude Code)**

**Critical Rules:**
- ❌ **NEVER hardcode API keys** in code that can be exposed
- ❌ **NEVER log secrets or keys** unless explicitly requested
- ❌ **NEVER commit secrets to repositories**
- ✅ Use environment variables exclusively
- ✅ Use secrets management systems

### Authentication Patterns

**Source: Lovable, Orchids.app, Leap.new**

**JWT vs Session-Based:**

**JWT (JSON Web Tokens):**
- ✅ Stateless
- ✅ Scalable across services
- ❌ Token management complexity
- ❌ Cannot easily revoke tokens

**Session-Based:**
- ✅ Simpler implementation
- ✅ Easy token revocation
- ❌ Requires server-side state
- ❌ Scalability challenges

**Recommended Frameworks:**
- **better-auth** (Orchids.app): Proper error handling patterns built-in
- **Clerk** (Leap.new): Modern authentication service
- **Supabase Auth** (Lovable): Integrated with database

### Authentication as Prerequisite

**Source: Orchids.app**

Before implementing sensitive features (payments, user data), ensure:
- ✅ Complete authentication system
- ✅ Login page with validation
- ✅ Register page with validation
- ✅ Error handling
- ✅ Loading states

### CORS Configuration

**Source: Emergent**

Properly configure Cross-Origin Resource Sharing:

```python
app.add_middleware(
    CORSMiddleware,
    allow_credentials=True,
    allow_origins=["*"],  # Restrict in production
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Production Considerations:**
- Restrict `allow_origins` to specific domains
- Limit `allow_methods` to required HTTP verbs
- Specify necessary `allow_headers` only

### Security Scanning

**Source: Lovable (Supabase)**

Use automated security tools:
- `security--run_security_scan`: Detect exposed data, missing policies
- `security--get_security_scan_results`: Review findings
- `security--get_table_schema`: Analyze database security

**Common Issues to Scan:**
- Exposed sensitive data
- Missing Row Level Security (RLS) policies
- Database misconfigurations
- Weak access controls

### Defensive Security Approach

**Source: Claude Code/Anthropic**

**Allow:**
- ✅ Security analysis
- ✅ Detection rules
- ✅ Vulnerability explanations
- ✅ Defensive security tasks

**Prohibit:**
- ❌ Creating malicious code
- ❌ Credential discovery/harvesting
- ❌ Bulk crawling for sensitive data
- ❌ Improving exploitation code

---

## 3. Input Validation

### General Validation Principles

**Source: Dia, Kiro**

- **Always validate and sanitize untrusted content** before processing
- Implement validation as explicit phase in development
- Create validation functions for data integrity
- Write unit tests for all validation logic

### Structured Validation Approach

**Source: Kiro**

**Step 1:** Create data models
```python
class User:
    def __init__(self, email: str, password: str):
        self.email = email
        self.password = password
```

**Step 2:** Implement validation functions
```python
def validate_user(user: User) -> bool:
    if not is_valid_email(user.email):
        raise ValueError("Invalid email format")
    if not is_strong_password(user.password):
        raise ValueError("Password too weak")
    return True
```

**Step 3:** Write unit tests
```python
def test_validate_user():
    valid_user = User("user@example.com", "StrongP@ss123")
    assert validate_user(valid_user) == True

    invalid_user = User("invalid-email", "weak")
    with pytest.raises(ValueError):
        validate_user(invalid_user)
```

### Pydantic Models for Type Safety

**Source: Emergent**

Use Pydantic for automatic validation:

```python
from pydantic import BaseModel, Field
from datetime import datetime
import uuid

class StatusCheckCreate(BaseModel):
    client_name: str

class StatusCheck(BaseModel):
    id: str = Field(default_factory=lambda: str(uuid.uuid4()))
    client_name: str
    timestamp: datetime = Field(default_factory=datetime.utcnow)
```

**Benefits:**
- Automatic type validation
- Default value generation
- JSON serialization/deserialization
- IDE autocompletion support

### Form Validation

**Source: Lovable, Orchids.app**

For authentication forms, include:
- ✅ Field-level validation (email format, password strength)
- ✅ Error message display
- ✅ Loading states during submission
- ✅ Success/failure feedback
- ✅ Prevent double submission

**Example Requirements:**
```
Login Page:
- Email/password form
- Email format validation
- Password minimum length
- Error handling with user-friendly messages
- Loading state during authentication
- Redirect on success
```

### Environment Validation

**Source: Emergent**

Validate environment configuration at startup:

```javascript
// Frontend validation
const backendUrl = import.meta.env.REACT_APP_BACKEND_URL
                   || process.env.REACT_APP_BACKEND_URL;
if (!backendUrl) {
    throw new Error("REACT_APP_BACKEND_URL not configured");
}
```

```python
# Backend validation
import os
mongo_url = os.environ.get('MONGO_URL')
if not mongo_url:
    raise EnvironmentError("MONGO_URL environment variable required")
```

---

## 4. Error Handling

### Comprehensive Error Handling Strategy

**Source: Orchids.app, Gemini**

**Key Principles:**
- Use proper error handling patterns with frameworks
- Implement robust handling for API errors (4xx/5xx)
- Handle unexpected responses gracefully
- Provide user-friendly error messages

### HTTP Error Code Handling

**Source: Gemini AI Studio**

```javascript
// Specific error message checking
const response = await fetch(`${downloadLink}&key=${process.env.API_KEY}`);

if (!response.ok) {
    if (response.status === 404
        && errorMessage.includes("Requested entity was not found.")) {
        // Reset state and notify user
        resetKeySelection();
        showError("Resource not found. Please try again.");
    } else if (response.status >= 500) {
        showError("Server error. Please try again later.");
    } else {
        showError("Request failed. Please check your input.");
    }
}
```

### Async Operation Error Handling

**Source: Emergent, Devin AI**

- Test each phase of async operations
- Document failures and passes in test results
- For long-running operations, return output while keeping process alive
- Sub-agents should report failures with implementation details

**Example Pattern:**
```python
import logging

async def process_request(data):
    try:
        logging.info(f"Processing request with data: {data}")
        result = await perform_operation(data)
        logging.info(f"Request successful: {result}")
        return result
    except ValidationError as e:
        logging.error(f"Validation failed: {str(e)}")
        raise HTTPException(status_code=400, detail=str(e))
    except DatabaseError as e:
        logging.error(f"Database error: {str(e)}")
        raise HTTPException(status_code=500, detail="Internal server error")
    except Exception as e:
        logging.error(f"Unexpected error: {str(e)}")
        raise HTTPException(status_code=500, detail="Unexpected error occurred")
```

### API Response Validation

**Source: Cursor Prompts, Orchids.app**

```typescript
export async function fetchData(endpoint: string) {
    const headers = getAuthHeaders();

    try {
        const response = await fetch(endpoint, { headers });

        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }

        const data = await response.json();

        // Validate response structure
        if (!isValidResponseShape(data)) {
            throw new Error("Invalid response format");
        }

        return data;
    } catch (error) {
        if (error instanceof NetworkError) {
            // Handle network issues
            throw new Error("Network connection failed");
        } else if (error instanceof SyntaxError) {
            // Handle JSON parsing errors
            throw new Error("Invalid response format");
        } else {
            throw error;
        }
    }
}
```

### Graceful Degradation

**Source: Same.dev**

Operations should fail gracefully when:
- The resource doesn't exist
- The operation is rejected for security reasons
- The resource cannot be modified
- Network connectivity is lost

**Pattern:**
```javascript
async function deleteFile(path: string): Promise<boolean> {
    try {
        await fs.unlink(path);
        return true;
    } catch (error) {
        if (error.code === 'ENOENT') {
            console.warn(`File not found: ${path}`);
            return false; // Not an error - already doesn't exist
        } else if (error.code === 'EACCES') {
            console.error(`Permission denied: ${path}`);
            return false;
        } else {
            throw error; // Unexpected error - propagate
        }
    }
}
```

### CLI Testing for Error Verification

**Source: Orchids.app**

Use curl to test error handling:

```bash
# Test successful request
curl localhost:3000/api/users -X GET

# Test 404 error
curl localhost:3000/api/users/nonexistent -X GET

# Test 400 error (invalid data)
curl localhost:3000/api/users -X POST -d '{"invalid": "data"}'

# Test authentication error
curl localhost:3000/api/protected -X GET
# Should return 401 without auth header

# Test with authentication
curl localhost:3000/api/protected -X GET \
     -H "Authorization: Bearer TOKEN"
```

---

## 5. API Endpoint Guidelines

### Endpoint Documentation Standard

**Source: Leap.new (Encore.ts)**

**Best Practices:**
- Each API endpoint in its own file
- Unique endpoint names across application
- Document all endpoints with comments
- Include proper authentication markers
- Specify request/response types

**File Structure:**
```
/api
  /users
    get-user.ts          // GET /api/users/:id
    list-users.ts        // GET /api/users
    create-user.ts       // POST /api/users
    update-user.ts       // PUT /api/users/:id
    delete-user.ts       // DELETE /api/users/:id
```

**Endpoint Documentation:**
```typescript
/**
 * Get user by ID
 * @endpoint GET /api/users/:id
 * @auth required
 * @param id - User unique identifier
 * @returns User object or 404 if not found
 */
export async function getUser(id: string): Promise<User> {
    // Implementation
}
```

### Public vs Protected Endpoints

**Source: Leap.new**

Explicitly distinguish endpoint exposure:

```json
{
  "endpoints": [
    {
      "name": "health_check",
      "method": "GET",
      "path": "/api/health",
      "expose": true,
      "auth": false
    },
    {
      "name": "get_user_profile",
      "method": "GET",
      "path": "/api/users/me",
      "expose": true,
      "auth": true
    },
    {
      "name": "admin_dashboard",
      "method": "GET",
      "path": "/api/admin/dashboard",
      "expose": false,
      "auth": true
    }
  ]
}
```

### Response Type Safety

**Source: Orchids.app**

**Use proper typing for responses:**

```typescript
// Define response interface
interface UserResponse {
    id: string;
    email: string;
    createdAt: string;
}

// Type the fetch response
async function getUser(id: string): Promise<UserResponse> {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) {
        throw new Error(`Failed to fetch user: ${response.status}`);
    }
    return await response.json() as UserResponse;
}
```

**Python equivalent with Pydantic:**
```python
from fastapi import FastAPI
from pydantic import BaseModel

class UserResponse(BaseModel):
    id: str
    email: str
    created_at: datetime

@app.get("/api/users/{user_id}", response_model=UserResponse)
async def get_user(user_id: str) -> UserResponse:
    # Implementation with automatic validation
    pass
```

### Parallel Testing

**Source: Orchids.app**

Test multiple endpoints simultaneously:

```bash
# Test health check, auth, and user endpoints in parallel
curl localhost:3000/api/health -X GET & \
curl localhost:3000/api/auth/login -X POST \
     -H "Content-Type: application/json" \
     -d '{"email":"test@example.com","password":"test123"}' & \
curl localhost:3000/api/users -X GET \
     -H "Authorization: Bearer TOKEN" &
wait
```

### Contract-Driven Development

**Source: Emergent**

Create API contracts before implementation:

**contracts.md example:**
```markdown
# API Contracts

## Status Check Endpoint

**Endpoint:** POST /api/status-check
**Authentication:** Required
**Description:** Record client status check

**Request:**
```json
{
  "client_name": "string"
}
```

**Response (200):**
```json
{
  "id": "uuid",
  "client_name": "string",
  "timestamp": "ISO 8601 datetime"
}
```

**Response (400):**
```json
{
  "detail": "Error message"
}
```

**Implementation Notes:**
- Validate client_name is non-empty
- Generate UUID server-side
- Timestamp in UTC
- Store in MongoDB collection 'status_checks'
```

---

## 6. Testing & Monitoring

### Immediate Endpoint Testing

**Source: Emergent, Orchids.app**

Test endpoints immediately after creation:

1. **Positive Cases:**
   - Valid data succeeds
   - Returns expected structure
   - Status code is correct

2. **Negative Cases:**
   - Invalid data rejected
   - Missing required fields caught
   - Authentication enforced

3. **Edge Cases:**
   - Empty strings
   - Very long inputs
   - Special characters
   - Null values

### Logging Configuration

**Source: Emergent**

Configure comprehensive logging:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

@app.post("/api/status-check")
async def status_check(data: StatusCheckCreate):
    logger.info(f"Status check received from: {data.client_name}")
    try:
        result = await process_status_check(data)
        logger.info(f"Status check successful: {result.id}")
        return result
    except Exception as e:
        logger.error(f"Status check failed: {str(e)}", exc_info=True)
        raise
```

**Log Levels:**
- `DEBUG`: Detailed diagnostic information
- `INFO`: General informational messages
- `WARNING`: Warning messages for potentially harmful situations
- `ERROR`: Error messages for serious problems
- `CRITICAL`: Critical messages for very serious errors

### Test Result Documentation

**Source: Emergent**

Create `test_result.md` to track:
- Test cases executed
- Pass/fail status
- Error messages
- Code changes made
- Retry attempts

**Example:**
```markdown
# Test Results - Status Check API

## Test Run: 2025-01-15 14:30:00

### Test 1: Valid Status Check
**Status:** ✅ PASS
**Request:**
```json
{"client_name": "TestClient"}
```
**Response:** 200 OK
**Duration:** 45ms

### Test 2: Missing Client Name
**Status:** ✅ PASS
**Request:**
```json
{}
```
**Response:** 400 Bad Request
**Error:** "client_name is required"
**Duration:** 12ms

### Test 3: Authentication Required
**Status:** ✅ PASS
**Request:** No auth header
**Response:** 401 Unauthorized
**Duration:** 8ms
```

### Performance Monitoring

While not extensively covered in the prompts, consider:

- Response time tracking
- Error rate monitoring
- Endpoint usage analytics
- Database query performance
- Cache hit rates

### Health Check Endpoints

**Pattern:**
```typescript
// Simple health check
app.get('/api/health', (req, res) => {
    res.json({
        status: 'ok',
        timestamp: new Date().toISOString(),
        uptime: process.uptime()
    });
});

// Detailed health check
app.get('/api/health/detailed', async (req, res) => {
    const health = {
        status: 'ok',
        timestamp: new Date().toISOString(),
        services: {
            database: await checkDatabase(),
            cache: await checkCache(),
            external_api: await checkExternalAPI()
        }
    };

    const isHealthy = Object.values(health.services).every(s => s === 'ok');
    const statusCode = isHealthy ? 200 : 503;

    res.status(statusCode).json(health);
});
```

---

## Summary Checklist

Use this checklist when creating public route prompts:

### Route Structure
- [ ] Use `/api` prefix for backend routes
- [ ] Define endpoint metadata (expose, auth, method, path)
- [ ] Use environment variables for URLs
- [ ] Never hardcode URLs or ports

### Security
- [ ] Never expose API keys or credentials
- [ ] Implement proper authentication
- [ ] Configure CORS appropriately
- [ ] Run security scans
- [ ] Follow defensive security practices

### Validation
- [ ] Validate all untrusted input
- [ ] Use type-safe models (Pydantic/TypeScript)
- [ ] Implement form validation
- [ ] Validate environment configuration

### Error Handling
- [ ] Handle all HTTP error codes
- [ ] Implement graceful degradation
- [ ] Log errors appropriately
- [ ] Return user-friendly error messages
- [ ] Test error scenarios

### Documentation & Testing
- [ ] Document API contracts before implementation
- [ ] Use unique endpoint names
- [ ] Test immediately after creation
- [ ] Test positive, negative, and edge cases
- [ ] Configure comprehensive logging
- [ ] Create health check endpoints

---

## Referenced Tools & Sources

This guide compiled insights from:

- **Emergent** (`/home/user/system-prompts-and-models-of-ai-tools/Emergent/Prompt.txt`)
- **Leap.new** (`/home/user/system-prompts-and-models-of-ai-tools/Leap.new/`)
- **Lovable** (`/home/user/system-prompts-and-models-of-ai-tools/Lovable/`)
- **Orchids.app** (`/home/user/system-prompts-and-models-of-ai-tools/Orchids.app/`)
- **Windsurf** (`/home/user/system-prompts-and-models-of-ai-tools/Windsurf/`)
- **Devin AI** (`/home/user/system-prompts-and-models-of-ai-tools/Devin%20AI/`)
- **Claude Code** (`/home/user/system-prompts-and-models-of-ai-tools/Claude%20Code/`)
- **Cursor** (`/home/user/system-prompts-and-models-of-ai-tools/Cursor%20Prompts/`)
- **Gemini CLI** (`/home/user/system-prompts-and-models-of-ai-tools/Open%20Source%20prompts/Gemini%20CLI/`)
- **Kiro** (`/home/user/system-prompts-and-models-of-ai-tools/Kiro/`)
- **Dia** (`/home/user/system-prompts-and-models-of-ai-tools/dia/`)
- **Same.dev** (`/home/user/system-prompts-and-models-of-ai-tools/Same.dev/`)
- **Trae AI** (`/home/user/system-prompts-and-models-of-ai-tools/Trae/`)

---

**Last Updated:** 2025-01-11
**Repository:** [system-prompts-and-models-of-ai-tools](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools)
