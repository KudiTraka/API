# KudiTraka AI API Documentation

Welcome to the KudiTraka AI API. This API serves as an interface for interacting with "MISA," an AI assistant designed to help with bookkeeping and customer interaction at KudiTraka Technologies.

## Table of Contents

1. [Base URL](#base-url)
2. [Authentication](#authentication)
3. [Endpoints](#endpoints)
   - [/docs](#get-docs)
   - [/kudi](#get-kudi)
   - [/userinput](#post-userinput)
   - [/chat_history](#get-chat_history)
   - [/feedback](#post-feedback)
   - [/reset_chat_history](#post-reset_chat_history)
4. [Error Handling](#error-handling)
5. [Logging](#logging)

## Base URL

The API is hosted at:

```
provided in dm for security purposes
```

All endpoints are accessible via this base URL.

## Authentication

Currently, no authentication is required to access the API. However, it is recommended to implement API key-based authentication in production.

## Endpoints

### GET /docs

**Description:**

Renders the API documentation page.

**Request:**

```http
GET /docs
```

**Response:**

- **200 OK**: Returns the documentation HTML page.

**Example:**

```bash
curl -X GET http://<your-server-url>/docs
```

### GET /kudi

**Description:**

Returns a welcome message from KudiTraka Technologies.

**Request:**

```http
GET /kudi
```

**Response:**

- **200 OK**: A JSON object containing the welcome message.

**Example Response:**

```json
{
  "message": "KudiTraka Traka Technologies"
}
```

**Example:**

```bash
curl -X GET http://<your-server-url>/kudi
```

### POST /userinput

**Description:**

Processes user input to interact with the MISA assistant. The input is parsed and processed, and the AI's response is returned.

**Request:**

```http
POST /userinput
Content-Type: application/json
```

**Request Body:**

- `user_id` (required): A unique identifier for the user.
- `text` (required): The input text from the user.

**Example Request:**

```json
{
  "user_id": "123",
  "text": "Hello MISA, I want to record a new purchase. Abu bought 5 tubers of yam for 3000 naira, paid in cash."
}
```

**Response:**

- **200 OK**: Returns the AI response in JSON format.
- **400 Bad Request**: Missing `user_id` or `text` in the request body.
- **500 Internal Server Error**: An error occurred while processing the request.

**Example Response:**

```json
{
  "status": "success",
  "data": {
    "task": "record",
    "parameters": {
      "Date": "2023-01-01",
      "Time": "10:00 AM",
      "Item": "Yam",
      "Price": 3000.0,
      "Quantity": "5",
      "Measure": "Tuber",
      "PaymentMethod": "Cash",
      "Notes": "",
      "Countomers id": null
    },
    "voice_output": "Sales record for Abu is successful."
  }
}
```

**Example:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "Hello MISA, I want to record a new purchase. Abu bought 5 tubers of yam for 3000 naira, paid in cash."
         }'
```
##### More Examples
### Example 1: Recording a New Purchase

**User Input:**

> "Hello MISA, I want to record a new purchase. John bought 3 cups of beans for 750 naira, paid via transfer."

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "Hello MISA, I want to record a new purchase. John bought 3 cups of beans for 750 naira, paid via transfer."
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "record",
    "parameters": {
      "Date": "2023-01-01",
      "Time": "10:00 AM",
      "Item": "Beans",
      "Price": 750.0,
      "Quantity": "3",
      "Measure": "Cup",
      "PaymentMethod": "Transfer",
      "Notes": "",
      "Countomers id": null
    },
    "voice_output": "Sales record for John is successful."
  }
}
```

### Example 2: Searching for Purchases by Customer ID

**User Input:**

> "MISA, please show me all purchases made by customer ID 102."

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "MISA, please show me all purchases made by customer ID 102."
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "search",
    "parameters": {
      "Countomers id": 102
    },
    "voice_output": "Search results for customer ID 102 are displayed."
  }
}
```

### Example 3: General Inquiry

**User Input:**

> "Hello MISA, how are you today?"

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "Hello MISA, how are you today?"
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "chat",
    "parameters": {
      "text": "I'm doing well, thank you! How can I assist you today?"
    },
    "voice_output": "I'm doing well, thank you! How can I assist you today?"
  }
}
```

### Example 4: Recording a Purchase with Missing Information

**User Input:**

> "MISA, record a purchase. I bought some groceries."

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "MISA, record a purchase. I bought some groceries."
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "chat",
    "parameters": {
      "text": "Could you please specify the items you purchased, the quantity, and the payment method?"
    },
    "voice_output": "Could you please specify the items you purchased, the quantity, and the payment method?"
  }
}
```

### Example 5: Searching for Purchases on a Specific Date

**User Input:**

> "MISA, show me all purchases made on January 3, 2023."

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "MISA, show me all purchases made on January 3, 2023."
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "search",
    "parameters": {
      "Date": "2023-01-03"
    },
    "voice_output": "Search results for purchases made on January 3, 2023 are displayed."
  }
}
```

### Example 6: Feedback Submission

**User Input:**

> "MISA, I think the service could be improved by adding more payment methods."

**API Request:**

```bash
curl -X POST http://<your-server-url>/userinput \
     -H "Content-Type: application/json" \
     -d '{
           "user_id": "123",
           "text": "MISA, I think the service could be improved by adding more payment methods."
         }'
```

**Expected Response:**

```json
{
  "status": "success",
  "data": {
    "task": "chat",
    "parameters": {
      "text": "Thank you for your feedback! We'll consider it for future updates."
    },
    "voice_output": "Thank you for your feedback! We'll consider it for future updates."
  }
}
```

### Example 7: Resetting Chat History

**User Input:**

> "MISA, please reset my chat history."

**API Request:**

```bash
curl -X POST http://<your-server-url>/reset_chat_history?user_id=123
```

**Expected Response:**

```json
{
  "message": "Chat history reset successfully"
}
```

### GET /chat_history

**Description:**

Retrieves the chat history for a specific user.

**Request:**

```http
GET /chat_history
```

**Query Parameters:**

- `user_id` (required): The unique identifier for the user.

**Response:**

- **200 OK**: Returns the chat history in JSON format.
- **400 Bad Request**: Missing `user_id` in the query parameters.

**Example Response:**

```json
{
  "chat_history": [
    {
      "timestamp": "2024-09-04T10:00:00",
      "user": "Hello MISA, I want to record a new purchase.",
      "response": {
        "status": "success",
        "data": {
          "task": "chat",
          "parameters": {
            "text": "Sure! What items did you purchase and how much did you pay?"
          },
          "voice_output": "Sure! What items did you purchase and how much did you pay?"
        }
      }
    }
  ]
}
```

**Example:**

```bash
curl -X GET http://<your-server-url>/chat_history?user_id=123
```

### POST /feedback

**Description:**

Submits feedback about the service.

**Request:**

```http
POST /feedback
Content-Type: application/json
```

**Request Body:**

- `feedback` (required): The user's feedback text.

**Example Request:**

```json
{
  "feedback": "Great service, but it could be faster."
}
```

**Response:**

- **200 OK**: Returns a thank you message.
- **400 Bad Request**: Missing `feedback` in the request body.

**Example Response:**

```json
{
  "message": "Thank you for your feedback!"
}
```

**Example:**

```bash
curl -X POST http://<your-server-url>/feedback \
     -H "Content-Type: application/json" \
     -d '{"feedback": "Great service, but it could be faster."}'
```

### POST /reset_chat_history

**Description:**

Resets the chat history for a specific user.

**Request:**

```http
POST /reset_chat_history
```

**Query Parameters:**

- `user_id` (required): The unique identifier for the user.

**Response:**

- **200 OK**: Returns a success message.
- **400 Bad Request**: Missing `user_id` in the query parameters.
- **500 Internal Server Error**: An error occurred while resetting the chat history.

**Example Response:**

```json
{
  "message": "Chat history reset successfully"
}
```

**Example:**

```bash
curl -X POST http://<your-server-url>/reset_chat_history?user_id=123
```

## Error Handling

The API includes error handling for common issues:

- **400 Bad Request**: Returned when required parameters are missing or invalid.
- **404 Not Found**: Returned when an endpoint or resource is not found.
- **500 Internal Server Error**: Returned when an internal error occurs during request processing.

**Example 400 Error:**

```json
{
  "message": "Bad request"
}
```

**Example 500 Error:**

```json
{
  "message": "Internal server error occurred"
}
```

## Logging

The API logs all significant actions and errors to a rotating log file (`logs/app.log`). The logs are rotated when they reach 5 MB, with up to 3 backup files kept.

The log format is:

```
%(asctime)s - %(levelname)s - %(message)s
```

Log files can be found in the `logs/` directory.
