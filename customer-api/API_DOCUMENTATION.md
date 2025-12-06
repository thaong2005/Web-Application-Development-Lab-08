# Customer API Documentation

## Base URL
`http://localhost:8080/api/customers`

## Overview
This is a RESTful API for managing customer data with full CRUD operations, pagination, sorting, searching, and filtering capabilities.

---

## Endpoints

### 1. Get All Customers
**GET** `/api/customers`

Get all customers with optional pagination and sorting.

**Query Parameters:**
- `page` (optional, default: 0) - Page number
- `size` (optional, default: 10) - Items per page
- `sortBy` (optional) - Field to sort by (fullName, email, createdAt, etc.)
- `sortDir` (optional, default: asc) - Sort direction (asc/desc)

**Response:** 200 OK
```json
{
    "customers": [
        {
            "id": 1,
            "customerCode": "C001",
            "fullName": "John Doe",
            "email": "john.doe@example.com",
            "phone": "+1234567890",
            "address": "123 Main Street",
            "status": "ACTIVE",
            "createdAt": "2025-12-06T10:00:00"
        }
    ],
    "currentPage": 0,
    "totalItems": 50,
    "totalPages": 5
}
```

**Examples:**
```bash
GET /api/customers
GET /api/customers?page=0&size=5
GET /api/customers?sortBy=fullName&sortDir=asc
GET /api/customers?page=1&size=10&sortBy=createdAt&sortDir=desc
```

---

### 2. Get Customer by ID
**GET** `/api/customers/{id}`

Retrieve a specific customer by their ID.

**Path Parameters:**
- `id` (required) - Customer ID

**Response:** 200 OK
```json
{
    "id": 1,
    "customerCode": "C001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "address": "123 Main Street",
    "status": "ACTIVE",
    "createdAt": "2025-12-06T10:00:00"
}
```

**Error Response:** 404 Not Found
```json
{
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 1",
    "path": "/api/customers/1"
}
```

---

### 3. Create Customer
**POST** `/api/customers`

Create a new customer.

**Request Body:**
```json
{
    "customerCode": "C001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "address": "123 Main Street"
}
```

**Validation Rules:**
- `customerCode`: Required, 3-20 characters, must start with 'C' followed by numbers
- `fullName`: Required, 2-100 characters
- `email`: Required, valid email format
- `phone`: Optional, valid phone format (10-20 digits)
- `address`: Optional, max 500 characters

**Response:** 201 Created
```json
{
    "id": 1,
    "customerCode": "C001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "address": "123 Main Street",
    "status": "ACTIVE",
    "createdAt": "2025-12-06T10:00:00"
}
```

**Error Response:** 409 Conflict
```json
{
    "status": 409,
    "error": "Conflict",
    "message": "Customer code already exists: C001",
    "path": "/api/customers"
}
```

---

### 4. Update Customer (Full Update)
**PUT** `/api/customers/{id}`

Perform a full update of a customer. All fields (except customerCode) are required.

**Path Parameters:**
- `id` (required) - Customer ID

**Request Body:**
```json
{
    "customerCode": "C001",
    "fullName": "John Updated",
    "email": "john.updated@example.com",
    "phone": "+1555999999",
    "address": "456 New Street"
}
```

**Response:** 200 OK
```json
{
    "id": 1,
    "customerCode": "C001",
    "fullName": "John Updated",
    "email": "john.updated@example.com",
    "phone": "+1555999999",
    "address": "456 New Street",
    "status": "ACTIVE",
    "createdAt": "2025-12-06T10:00:00"
}
```

**Note:** `customerCode` is immutable and cannot be changed after creation.

---

### 5. Partial Update Customer
**PATCH** `/api/customers/{id}`

Perform a partial update of a customer. Only specified fields will be updated.

**Path Parameters:**
- `id` (required) - Customer ID

**Request Body (all fields optional):**
```json
{
    "fullName": "John Partially Updated"
}
```

**Other examples:**
```json
{
    "email": "newemail@example.com",
    "phone": "+1555888888"
}
```

**Response:** 200 OK
```json
{
    "id": 1,
    "customerCode": "C001",
    "fullName": "John Partially Updated",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "address": "123 Main Street",
    "status": "ACTIVE",
    "createdAt": "2025-12-06T10:00:00"
}
```

---

### 6. Delete Customer
**DELETE** `/api/customers/{id}`

Delete a customer by ID.

**Path Parameters:**
- `id` (required) - Customer ID

**Response:** 200 OK
```json
{
    "message": "Customer deleted successfully"
}
```

**Error Response:** 404 Not Found
```json
{
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 999",
    "path": "/api/customers/999"
}
```

---

### 7. Search Customers
**GET** `/api/customers/search`

Search customers by keyword. Searches in fullName, email, and customerCode fields.

**Query Parameters:**
- `keyword` (required) - Search keyword

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1234567890",
        "address": "123 Main Street",
        "status": "ACTIVE",
        "createdAt": "2025-12-06T10:00:00"
    }
]
```

**Examples:**
```bash
GET /api/customers/search?keyword=john
GET /api/customers/search?keyword=gmail
GET /api/customers/search?keyword=C001
```

---

### 8. Filter by Status
**GET** `/api/customers/status/{status}`

Get customers filtered by status.

**Path Parameters:**
- `status` (required) - Customer status (ACTIVE or INACTIVE)

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1234567890",
        "address": "123 Main Street",
        "status": "ACTIVE",
        "createdAt": "2025-12-06T10:00:00"
    }
]
```

**Examples:**
```bash
GET /api/customers/status/ACTIVE
GET /api/customers/status/INACTIVE
```

---

### 9. Advanced Search
**GET** `/api/customers/advanced-search`

Search customers with multiple optional criteria.

**Query Parameters (all optional):**
- `name` - Filter by name (partial match)
- `email` - Filter by email (partial match)
- `status` - Filter by status (ACTIVE/INACTIVE)

**Response:** 200 OK
```json
[
    {
        "id": 1,
        "customerCode": "C001",
        "fullName": "John Doe",
        "email": "john.doe@gmail.com",
        "phone": "+1234567890",
        "address": "123 Main Street",
        "status": "ACTIVE",
        "createdAt": "2025-12-06T10:00:00"
    }
]
```

**Examples:**
```bash
GET /api/customers/advanced-search?name=john
GET /api/customers/advanced-search?email=gmail
GET /api/customers/advanced-search?status=ACTIVE
GET /api/customers/advanced-search?name=john&status=ACTIVE
GET /api/customers/advanced-search?name=john&email=gmail&status=ACTIVE
```

---

## Error Responses

### 400 Bad Request - Validation Error
```json
{
    "status": 400,
    "error": "Bad Request",
    "message": "Validation failed",
    "path": "/api/customers",
    "errors": [
        {
            "field": "email",
            "message": "Invalid email format"
        },
        {
            "field": "fullName",
            "message": "Full name is required"
        }
    ]
}
```

### 404 Not Found
```json
{
    "status": 404,
    "error": "Not Found",
    "message": "Customer not found with id: 1",
    "path": "/api/customers/1"
}
```

### 409 Conflict
```json
{
    "status": 409,
    "error": "Conflict",
    "message": "Email already exists: john@example.com",
    "path": "/api/customers"
}
```

### 500 Internal Server Error
```json
{
    "status": 500,
    "error": "Internal Server Error",
    "message": "An unexpected error occurred",
    "path": "/api/customers"
}
```

---

## Data Models

### Customer Entity
```java
{
    "id": Long,
    "customerCode": String,      // Unique, format: C\d{3,}
    "fullName": String,          // 2-100 characters
    "email": String,             // Unique, valid email
    "phone": String,             // Optional, 10-20 digits
    "address": String,           // Optional, max 500 chars
    "status": Enum,              // ACTIVE or INACTIVE
    "createdAt": DateTime,       // Auto-generated
    "updatedAt": DateTime        // Auto-updated
}
```

### Customer Status Enum
- `ACTIVE` - Customer is active
- `INACTIVE` - Customer is inactive

---

## Testing with cURL

### Create a Customer
```bash
curl -X POST http://localhost:8080/api/customers \
  -H "Content-Type: application/json" \
  -d '{
    "customerCode": "C001",
    "fullName": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1234567890",
    "address": "123 Main Street"
  }'
```

### Get All Customers
```bash
curl http://localhost:8080/api/customers
```

### Get Customer by ID
```bash
curl http://localhost:8080/api/customers/1
```

### Update Customer (PUT)
```bash
curl -X PUT http://localhost:8080/api/customers/1 \
  -H "Content-Type: application/json" \
  -d '{
    "customerCode": "C001",
    "fullName": "John Updated",
    "email": "john.updated@example.com",
    "phone": "+1555999999",
    "address": "456 New Street"
  }'
```

### Partial Update (PATCH)
```bash
curl -X PATCH http://localhost:8080/api/customers/1 \
  -H "Content-Type: application/json" \
  -d '{
    "fullName": "John Partially Updated"
  }'
```

### Delete Customer
```bash
curl -X DELETE http://localhost:8080/api/customers/1
```

### Search Customers
```bash
curl "http://localhost:8080/api/customers/search?keyword=john"
```

### Filter by Status
```bash
curl http://localhost:8080/api/customers/status/ACTIVE
```

---

## Notes

1. **CORS**: The API has CORS enabled for all origins (`*`)
2. **Validation**: All input data is validated using Bean Validation
3. **Pagination**: Default page size is 10, default page number is 0
4. **Sorting**: Available sort fields include all entity properties
5. **Status**: Default status for new customers is ACTIVE
6. **Customer Code**: Immutable after creation
7. **Timestamps**: `createdAt` and `updatedAt` are automatically managed

---

## Technology Stack

- **Framework**: Spring Boot 3.5.8
- **Java Version**: 17
- **Database**: MySQL 8.0
- **ORM**: Hibernate/JPA
- **Validation**: Jakarta Validation
- **Build Tool**: Maven

---

## Contact & Support

For issues or questions, please contact the development team or create an issue in the repository.
