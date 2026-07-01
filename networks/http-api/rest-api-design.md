# REST API Design

## Description

REST is the dominant architectural style for web APIs. Designing a RESTful API well determines whether your API is intuitive, maintainable, and scalable. This document covers principles, URL conventions, HTTP method mapping, request/response design, error handling, authentication, documentation, and rate limiting with code examples in Python (FastAPI), JavaScript (Express.js), and curl.

## Prerequisites

- [Headers, Body & JSON](headers-body-and-json.md) — understanding HTTP headers, content types, JSON serialization, cookies, and CORS
- [HTTP Methods & Status Codes](http-methods-and-status.md) — familiarity with GET, POST, PUT, DELETE, PATCH and status code ranges

## Table of Contents

- [What is REST](#what-is-rest)
- [REST Principles](#rest-principles)
- [Resource-Oriented Design](#resource-oriented-design)
- [URL Naming Conventions](#url-naming-conventions)
- [CRUD Mapping to HTTP](#crud-mapping-to-http)
- [Request and Response Design](#request-and-response-design)
- [Error Responses](#error-responses)
- [Authentication in APIs](#authentication-in-apis)
- [API Documentation](#api-documentation)
- [Rate Limiting](#rate-limiting)
- [Best Practices](#best-practices)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is REST

REST stands for **Representational State Transfer**, an architectural style introduced by Roy Fielding in 2000. It defines constraints for building distributed hypermedia systems. APIs following these constraints are called RESTful.

REST is not a protocol — it is a design philosophy leveraging HTTP to its fullest. Every piece of information on the server is a **resource**. Clients interact with resources through a uniform interface (HTTP methods). Each request contains all information needed to process it (statelessness).

```bash
curl -X GET https://api.example.com/v1/users/42 \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9..."
```

```python
import requests
requests.get("https://api.example.com/v1/users/42",
    headers={"Authorization": "Bearer eyJhbGciOiJIUzI1NiJ9..."}).json()
```

```javascript
fetch("https://api.example.com/v1/users/42", {
  headers: {Authorization: "Bearer eyJhbGciOiJIUzI1NiJ9..."}
}).then(r=>r.json()).then(console.log);
```

### REST Principles

Six architectural constraints define REST.

**Stateless.** Every request contains all information needed to process it. The server stores no client context between requests. Any server can handle any request, improving scalability.

```python
from fastapi import FastAPI, Depends
from fastapi.security import HTTPBearer
app=FastAPI(); security=HTTPBearer()
@app.get("/api/v1/orders")
def list_orders(credentials=Depends(security)):
    return {"data": query_orders(decode_token(credentials.credentials)["sub"])}
```

**Client-Server.** Client handles UI; server handles data and logic. Both evolve independently.

**Cacheable.** Responses must indicate cacheability via headers (`Cache-Control`, `ETag`). Clients and CDNs reuse cached responses.

```python
@app.get("/api/v1/products")
def list_products(response:Response):
    response.headers["Cache-Control"]="public, max-age=300"
    return {"data": get_all_products()}
```

```bash
curl -H "If-None-Match: w/\"v42\"" https://api.example.com/v1/products
# 304 Not Modified if unchanged
```

**Uniform Interface.** Resources identified by URLs. Manipulation through representations. Self-descriptive messages. HATEOAS provides navigational links.

```json
{"data":{"id":42,"name":"Alice","_links":{
    "self":"/v1/users/42","orders":"/v1/users/42/orders"}}}
```

**Layered System.** Clients talk to the entry point (CDN, load balancer, gateway), not directly to the application server.

**Code on Demand (Optional).** Server sends executable code to the client. Rare in practice.

### Resource-Oriented Design

Everything is a **resource** — a noun, not a verb. HTTP methods express the action.

```
Good (nouns):   /users, /orders, /products
Bad (verbs):    /getUsers, /createOrder, /deleteProduct
```

The URL identifies the resource. The HTTP method identifies the operation.

```
GET    /users          -> Retrieve all users
GET    /users/42       -> Retrieve user 42
POST   /users          -> Create a user
PUT    /users/42       -> Replace user 42
DELETE /users/42       -> Delete user 42
```

Non-CRUD operations as POST to a sub-resource: `/users/42/activate`, `/orders/42/cancel`.

### URL Naming Conventions

**Plural nouns for collections.** `/users`, `/orders`, `/products`.

**Singular for singletons.** `/users/me` for the current authenticated user.

**Hierarchical nesting.** Resources belonging to parents are nested. `/users/42/orders`, `/users/42/orders/7/items`. Do not nest more than three levels — flatten by referencing by ID.

**Query parameters for filtering.** Not for identifying resources.

```
GET /users?role=admin
GET /products?category=electronics&min_price=100
```

**URL params vs query params.** URL params identify specific resources; query params filter collections.

```
URL param:  /users/42           (specific user)
Query:      /users?role=admin   (filtered collection)
```

**Versioning.** URL path versioning is most common:

```python
from fastapi import APIRouter
v1=APIRouter(prefix="/api/v1"); v2=APIRouter(prefix="/api/v2")
@v1.get("/users")
def list_users_v1(): return {"data":[{"id":1,"name":"Alice"}]}
@v2.get("/users")
def list_users_v2(): return {"data":[{"id":1,"name":"Alice","phone":"+123"}]}
```

Accept header versioning: `Accept: application/vnd.example.v1+json`. URL versioning is simpler: visible in logs, cacheable, easy to test.

**Avoid verbs in URLs.** HTTP methods are the verbs. `POST /api/createUser` -> `POST /api/users`.

### CRUD Mapping to HTTP

| Operation | Method | URL Pattern | Status | Idempotent | Safe |
|-----------|--------|-------------|--------|------------|------|
| Create | POST | `/resources` | 201 Created | No | No |
| Read all | GET | `/resources` | 200 OK | Yes | Yes |
| Read one | GET | `/resources/{id}` | 200 OK | Yes | Yes |
| Replace | PUT | `/resources/{id}` | 200 OK | Yes | No |
| Partial | PATCH | `/resources/{id}` | 200 OK | No* | No |
| Delete | DELETE | `/resources/{id}` | 204 No Content | Yes | No |

*PATCH idempotency depends on format — merge patch is idempotent, JSON Patch increment is not.

**Create — POST:**

```bash
curl -X POST https://api.example.com/v1/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'
```

```python
from pydantic import BaseModel
class UserCreate(BaseModel): name:str; email:str; role:str="user"
@app.post("/api/v1/users",status_code=201)
def create_user(data:UserCreate):
    uid=insert_user(data.dict())
    return {"data":{"id":uid,**data.dict()}}
```

The server assigns the identifier and returns it with a Location header.

**Read — GET:**

```python
@app.get("/api/v1/users/{user_id}")
def get_user(user_id:int):
    user=find_user_by_id(user_id)
    if not user: raise HTTPException(404)
    return {"data":user}
```

GET is safe, idempotent, and cacheable.

**Replace — PUT.** Replaces the entire resource. Omitted fields reset to defaults. Idempotent.

```python
@app.put("/api/v1/users/{user_id}")
def replace_user(user_id:int,data:UserCreate):
    if not find_user_by_id(user_id): raise HTTPException(404)
    update_user(user_id,data.dict())
    return {"data":{"id":user_id,**data.dict()}}
```

**Partial Update — PATCH.** Sends only changed fields.

```python
@app.patch("/api/v1/users/{user_id}")
def patch_user(user_id:int,updates:dict):
    existing=find_user_by_id(user_id)
    if not existing: raise HTTPException(404)
    merged={**existing,**updates}
    update_user(user_id,merged)
    return {"data":merged}
```

```bash
curl -X PATCH https://api.example.com/v1/users/42 \
  -H "Content-Type: application/json" \
  -d '{"email":"new@example.com"}'
```

**Delete — DELETE.** Returns 204 No Content with no body. Idempotent.

```python
@app.delete("/api/v1/users/{user_id}",status_code=204)
def delete_user(user_id:int):
    if not find_user_by_id(user_id): raise HTTPException(404)
    delete_user_by_id(user_id)
```

### Request and Response Design

**Consistent JSON format.** Every response uses the same top-level structure: `{"data":...}` on success, `{"error":...}` on failure.

```python
from pydantic import BaseModel
from typing import Optional
class Envelope(BaseModel): data:Optional[dict]=None; error:Optional[dict]=None

@app.get("/api/v1/users/{user_id}")
def get_user(user_id:int):
    user=find_user_by_id(user_id)
    if not user: return Envelope(error={"code":"NOT_FOUND","message":"User not found"})
    return Envelope(data=user)
```

**Pagination.** Collections must paginate results.

Page-based:
```bash
GET /api/v1/users?page=2&per_page=20
```

```python
@app.get("/api/v1/users")
def list_users(page:int=1,per_page:int=20):
    offset=(page-1)*per_page
    users,total=query_users(limit=per_page,offset=offset)
    tp=(total+per_page-1)//per_page; base="/api/v1/users"
    return {"data":users,"pagination":{"page":page,"per_page":per_page,"total":total,"total_pages":tp,
        "next":f"{base}?page={page+1}&per_page={per_page}" if page<tp else None,
        "prev":f"{base}?page={page-1}&per_page={per_page}" if page>1 else None}}
```

Cursor-based pagination is stable when data changes:

```python
import base64,json
@app.get("/api/v1/users")
def list_cursor(cursor:str=None,limit:int=20):
    users=(query_users_after(json.loads(base64.urlsafe_b64decode(cursor))["id"],limit+1)
           if cursor else query_users_first(limit+1))
    hm=len(users)>limit
    if hm: users=users[:limit]; nc=base64.urlsafe_b64encode(json.dumps({"id":users[-1]["id"]}).encode()).decode()
    return {"data":users,"pagination":{"next_cursor":nc if hm else None,"has_more":hm}}
```

**Sorting:** `GET /users?sort=name:asc`, `GET /users?sort=name:asc,created_at:desc`. Validate fields against a whitelist.

**Filtering:**

```
GET /users?role=admin&status=active
GET /products?category=electronics&min_price=100&max_price=500
```

Standard conventions: `q` for search, `field` for exact match, `field__gt`/`field__gte` for range, `field__in` for lists.

**Field selection.** Let clients specify fields: `GET /users?fields=id,name,email`. Whitelist allowed fields; never expose internals.

```python
@app.get("/api/v1/users")
def list_users(fields:str=None):
    allowed={"id","name","email","role"}
    users=query_all_users()
    if fields:
        selected=set(fields.split(","))&allowed
        if selected: users=[{k:u[k] for k in selected} for u in users]
    return {"data":users}
```

### Error Responses

A consistent error format lets clients write one error handler for all endpoints.

```json
{"error":{"code":"VALIDATION_ERROR","message":"The request data is invalid",
  "details":[{"field":"email","issue":"must be a valid email"}]}}
```

```json
{"error":{"code":"NOT_FOUND","message":"User 999 not found","details":[]}}
```

```python
class APIException(Exception):
    def __init__(self,code,message,status,details=None):
        self.code=code;self.message=message;self.status=status;self.details=details or []
@app.exception_handler(APIException)
def handler(request,exc:APIException):
    return JSONResponse(exc.status,content={"error":{"code":exc.code,"message":exc.message,"details":exc.details}})
@app.get("/api/v1/users/{user_id}")
def get_user(user_id:int):
    user=find_user_by_id(user_id)
    if not user: raise APIException("NOT_FOUND",f"User {user_id} not found",404)
    return {"data":user}
```

```javascript
class APIError extends Error {
  constructor(code,message,status,details=[]){super(message);this.code=code;this.status=status;this.details=details;}
}
function errorHandler(err,req,res,next){
  if(err instanceof APIError)
    return res.status(err.status).json({error:{code:err.code,message:err.message,details:err.details}});
  console.error("Unhandled:",err);
  res.status(500).json({error:{code:"INTERNAL_ERROR",message:"Unexpected error",details:[]}});
}
app.use(errorHandler);
```

**Status code mapping:** 422 VALIDATION_ERROR, 400 BAD_REQUEST, 401 UNAUTHENTICATED, 403 FORBIDDEN, 404 NOT_FOUND, 409 CONFLICT, 429 RATE_LIMIT_EXCEEDED, 500 INTERNAL_ERROR.

**Never expose stack traces.** Log the full error server-side with a correlation ID.

```python
import logging,uuid
logger=logging.getLogger(__name__)
@app.middleware("http")
async def add_request_id(request,call_next):
    request.state.request_id=str(uuid.uuid4())
    r=await call_next(request)
    r.headers["X-Request-ID"]=request.state.request_id
    return r
@app.exception_handler(Exception)
def global_handler(request,exc):
    logger.error(f"Request {request.state.request_id} failed",exc_info=True)
    return JSONResponse(500,content={"error":{"code":"INTERNAL_ERROR",
        "message":f"Unexpected error. ID: {request.state.request_id}","details":[]}})
```

### Authentication in APIs

**API keys.** Simple static tokens identifying the client.

```python
from fastapi.security import APIKeyHeader
def verify_key(key:str=Depends(APIKeyHeader(name="X-API-Key"))):
    if key not in {"key123"}: raise HTTPException(401,"Invalid API key")
```

**Bearer tokens (JWT).** Short-lived credentials issued after authentication.

```
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI0MiJ9.dy0GZ3s...
```

```python
import jwt
from fastapi.security import HTTPBearer
def verify_token(credentials=Depends(HTTPBearer())):
    try: return jwt.decode(credentials.credentials,"secret",algorithms=["HS256"])
    except jwt.ExpiredSignatureError: raise HTTPException(401,"Token expired")
    except jwt.InvalidTokenError: raise HTTPException(401,"Invalid token")
```

```javascript
const jwt = require("jsonwebtoken");
function authenticate(req,res,next){
  const h=req.headers.authorization;
  if(!h?.startsWith("Bearer ")) return res.status(401).json({error:{code:"UNAUTHENTICATED",message:"Missing token"}});
  try{req.user=jwt.verify(h.split(" ")[1],"secret");next();}
  catch{res.status(401).json({error:{code:"INVALID_TOKEN",message:"Invalid token"}});}
}
```

JWT flow: POST /auth/login -> receive `{"token":"..."}` -> include in Authorization header for subsequent requests.

**Basic auth.** Base64-encoded username:password. Only over HTTPS. `Authorization: Basic YWxpY2U6cGFzc3dvcmQ=`

**Where to put credentials.** Always the Authorization header. Never in URL query strings (they leak into logs). Do not use cookies for API auth unless the client is a same-origin browser app.

### API Documentation

OpenAPI (formerly Swagger) is the industry standard — machine-readable YAML/JSON describing every endpoint, parameter, response, and auth method. Enables interactive docs (Swagger UI), client SDK generation, request validation, and contract testing.

```python
from fastapi import FastAPI
from pydantic import BaseModel,Field
app=FastAPI(title="Users API",version="1.0.0")
class UserCreate(BaseModel):
    name:str=Field(...,min_length=1)
    email:str=Field(...,pattern=r"^[\w\.-]+@[\w\.-]+\.\w+$")
@app.post("/api/v1/users",status_code=201)
def create_user(user:UserCreate):
    """Create a new user."""
    return {"data":{"id":1,**user.dict()}}
```

FastAPI auto-generates OpenAPI docs. For Express.js:

```javascript
const swaggerJsdoc=require("swagger-jsdoc"),swaggerUi=require("swagger-ui-express");
app.use("/api-docs",swaggerUi.serve,swaggerUi.setup(swaggerJsdoc({
  definition:{openapi:"3.0.0",info:{title:"Users API",version:"1.0.0"}},apis:["./routes/*.js"]
})));
```

### Rate Limiting

Protects the API from abuse, ensures fair usage, and controls costs.

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 987
X-RateLimit-Reset: 1717203600
```

```python
import time
from collections import defaultdict
store=defaultdict(lambda:{"count":0,"reset_at":0})
@app.middleware("http")
async def rate_limit(request,call_next):
    ip=request.client.host; now=time.time()
    if store[ip]["reset_at"]<now: store[ip]={"count":0,"reset_at":now+60}
    store[ip]["count"]+=1; remaining=max(0,100-store[ip]["count"])
    if store[ip]["count"]>100:
        return JSONResponse(429,content={"error":{"code":"RATE_LIMIT_EXCEEDED","message":"Too many requests","details":[]}})
    r=await call_next(request)
    r.headers["X-RateLimit-Limit"]="100"; r.headers["X-RateLimit-Remaining"]=str(remaining)
    r.headers["X-RateLimit-Reset"]=str(int(store[ip]["reset_at"]))
    return r
```

```javascript
const rateLimit=require("express-rate-limit");
app.use("/api/",rateLimit({windowMs:60000,max:100,standardHeaders:true,
  handler:(req,res)=>res.status(429).json({error:{code:"RATE_LIMIT_EXCEEDED",message:"Too many requests",details:[]}})
}));
```

**429 response** includes `Retry-After`. Clients should back off:

```python
import time
def call_with_retry(url,token,retries=3):
    for _ in range(retries):
        r=requests.get(url,headers={"Authorization":f"Bearer {token}"})
        if r.status_code==429: time.sleep(int(r.headers.get("Retry-After",5))); continue
        return r.json()
    raise Exception("Max retries")
```

```javascript
async function callWithRetry(url,token,retries=3){
  for(let i=0;i<retries;i++){
    const r=await fetch(url,{headers:{Authorization:`Bearer ${token}`}});
    if(r.status===429){await new Promise(r=>setTimeout(r,parseInt(r.headers.get("Retry-After")||"5")*1000));continue;}
    return r.json();
  } throw new Error("Max retries");
}
```

### Best Practices

**Consistent naming.** Use `snake_case` or `camelCase` everywhere. snake_case is common in Python/Django/Rails APIs; camelCase in JavaScript/Node.js APIs.

**Always return arrays as arrays.** Return `[]`, not `null`. `{"data":[]}` not `{"data":null}`.

**Use HTTPS.** Redirect HTTP to HTTPS. Use HSTS headers.

**Version your API.** Support old versions for 6-12 months during deprecation.

**Document breaking changes.** Removing/renaming fields, changing types, removing endpoints, changing auth. Process: add new version, set deprecation headers, announce, provide migration guide, sunset.

```python
@app.get("/api/v1/users/{user_id}")
def get_user_v1(user_id:int):
    r=JSONResponse({"data":old_format(user_id)})
    r.headers["Deprecation"]="true"
    r.headers["Sunset"]="Sat, 01 Jun 2025 00:00:00 GMT"
    r.headers["Link"]='</api/v2/users>; rel="successor-version"'
    return r
```

## Glossary

| Term | Definition |
|------|------------|
| REST | Representational State Transfer — architectural style for networked applications using HTTP |
| Resource | Any piece of information addressable via a URL (user, product, order) |
| Endpoint | URL path combined with an HTTP method performing an operation |
| CRUD | Create, Read, Update, Delete — mapped to POST, GET, PUT/PATCH, DELETE |
| Versioning | Assigning version identifiers to manage breaking changes |
| Pagination | Dividing large result sets into pages |
| Cursor | Opaque pointer to a dataset position for stable pagination |
| Envelope | Outer wrapper of an API response containing metadata alongside data |
| Rate limiting | Restricting requests within a time window |
| Throttle | Slowing or rejecting requests when rate limited |
| Quota | Hard limit on resource usage over a longer period |
| OpenAPI | Specification for describing REST APIs in machine-readable YAML/JSON |
| Swagger | Original name for OpenAPI; also refers to Swagger UI and Codegen |
| Bearer token | Security token granting access to the bearer |
| JWT | JSON Web Token — self-contained signed token for authentication |
| API key | Static token identifying a client application |
| Breaking change | Modification breaking existing client integrations |
| Deprecation | Marking a feature obsolete while keeping it functional during migration |
| HATEOAS | Hypermedia as the Engine of Application State — responses include navigational links |
| Idempotent | Operation producing same result whether executed once or multiple times |
| Safe method | HTTP method not modifying server state (GET, HEAD, OPTIONS) |
| 429 Too Many Requests | HTTP status returned when rate limit is exceeded |

## Quick References

- [RESTful Web Services (O'Reilly)](https://www.oreilly.com/library/view/restful-web-services/9780596529260/) — classic reference by Leonard Richardson and Sam Ruby
- [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines) — versioning, pagination, naming, and error handling conventions
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) — official specification for describing HTTP APIs
- [JSON API Specification](https://jsonapi.org/) — conventions for resource relationships, pagination, error handling, and sparse fieldsets
- [Stripe API Reference](https://stripe.com/docs/api) — practical example of naming, pagination, idempotency, and error conventions

## Next Steps

- [Headers, Body, Content Types, and JSON](headers-body-and-json.md) — deeper look at request/response mechanics underlying REST APIs
- [HTTP API module index](index.md) — back to the HTTP API module overview
