# API Documentation - Amen Bank AI Solution

**Version**: 1.0.0  
**Base URL**: `http://localhost:8000`  
**Status**: Implementation-aligned (current backend behavior)

## What this document covers

This document reflects the current FastAPI backend implementation in the chatbot service. It is intended for developers and coding agents who need to understand the actual endpoints, payloads, and behavior that are present today.

### Current implementation notes
- The backend is a FastAPI app serving chatbot, contact, FAQ, health, and ingestion endpoints.
- The chat workflow uses a local RAG-style retrieval flow backed by ChromaDB and a Groq-powered response path when configured.
- The current implementation uses a fallback response flow when no keyword is detected, when no relevant FAQ context is found, or when the LLM is unavailable.
- Contact submissions are appended to a JSONL file rather than a database.
- FAQ data is currently read from the repository JSON knowledge base and ingestion state files.

---

## Table of Contents

1. [Overview](#overview)
2. [Authentication and CORS](#authentication-and-cors)
3. [Error handling](#error-handling)
4. [Endpoints](#endpoints)
   - [GET /health](#get-health)
   - [POST /chat](#post-chat)
   - [POST /contact](#post-contact)
   - [GET /faq](#get-faq)
   - [POST /ingest](#post-ingest)
5. [Data files and integrations](#data-files-and-integrations)
6. [Testing examples](#testing-examples)

---

## Overview

The API currently exposes five main routes:
- **Health monitoring** for service checks
- **Chat interaction** with multilingual FAQ assistance
- **Contact submission** for website inquiries
- **FAQ retrieval** from the project knowledge base
- **FAQ ingestion** into the local ChromaDB-backed retrieval store

### Current capabilities
- ✅ Multilingual support for `fr`, `ar`, and `en`
- ✅ CORS enabled for local frontend development and the public site origin
- ✅ Structured JSON responses for successful requests
- ✅ Local fallback behavior when no relevant context or no LLM is configured
- ✅ Contact logging to a file-based submission log

---

## Authentication and CORS

### Authentication
There is no authentication layer on the current API endpoints. The public routes are currently open and intended for local development or internal use.

### Rate limiting
The server has optional slowapi integration, but the current route implementation does not apply explicit per-endpoint throttling decorators. In practice, the API should be treated as currently unthrottled unless the rate-limit middleware is added later.

### CORS
Allowed origins are configured for:
- `http://localhost:3000`
- `http://localhost:3001`
- `https://amen-bank.tn`

Allowed methods and headers are currently wide-open for compatibility.

---

## Error handling

The backend uses FastAPI `HTTPException` responses for operational failures. In the current implementation, error responses typically follow this shape:

```json
{
  "detail": "Human-readable error message"
}
```

Common failure cases include:
- invalid or missing request fields
- unsupported language values
- missing FAQ context or keyword extraction failure
- ingestion or ChromaDB initialization issues
- unexpected internal server errors

---

## Endpoints

### GET /health

**Purpose**: Check whether the backend is running and whether the main database dependency is available.

**Method**: `GET`

**Response shape**:

```json
{
  "status": "healthy",
  "version": "1.0.0",
  "database": "chromadb",
  "language_support": ["fr", "ar", "en"],
  "timestamp": "2026-07-04T10:30:45.123456"
}
```

**Example**:

```bash
curl http://localhost:8000/health
```

---

### POST /chat

**Purpose**: Submit a user message and receive a multilingual chatbot response.

**Method**: `POST`

**Content-Type**: `application/json`

#### Request body

```json
{
  "message": "How do I open an account?",
  "language": "en",
  "user_id": "optional-user-id",
  "conversation_id": "optional-conversation-id"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | string | Yes | User prompt or question |
| `language` | string | No | One of `fr`, `ar`, or `en` |
| `user_id` | string | No | Optional user identifier |
| `conversation_id` | string | No | Optional conversation identifier |

#### Supported languages
- `fr`
- `ar`
- `en`

#### Response shape

```json
{
  "status": "success",
  "message": "A generated or fallback answer",
  "language": "en",
  "sources": [
    {
      "text": "FAQ snippet preview",
      "relevance": 0.91
    }
  ],
  "confidence": 0.75,
  "timestamp": "2026-07-04T10:30:45.123456"
}
```

#### Behavioral notes
- The endpoint extracts a keyword from the incoming message before retrieval.
- If no keyword is detected, it returns a fallback support message.
- If no relevant FAQ chunk is found, it returns a fallback support message instead of a generic hallucinated answer.
- When Groq is configured, the response can be polished with the LLM; otherwise the system uses template-based fallback wording.

**Example**:

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "How do I open an account?",
    "language": "en"
  }'
```

---

### POST /contact

**Purpose**: Submit a contact inquiry from the website.

**Method**: `POST`

**Content-Type**: `application/json`

#### Request body

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "+216 21 123 456",
  "subject": "Account inquiry",
  "message": "I would like to open a new account.",
  "language": "fr"
}
```

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Contact name |
| `email` | string | Yes | Valid email address |
| `phone` | string | Yes | Contact phone number |
| `subject` | string | Yes | Inquiry subject |
| `message` | string | Yes | Inquiry details |
| `language` | string | No | One of `fr`, `ar`, or `en` |

#### Response shape

```json
{
  "status": "success",
  "message": "Contact submission received",
  "ticket_id": "TKT-20260704103045-1234",
  "timestamp": "2026-07-04T10:30:45.123456"
}
```

#### Behavioral notes
- A ticket ID is generated automatically.
- Each submission is appended to the file `chatbot/contact_submissions.jsonl`.

**Example**:

```bash
curl -X POST http://localhost:8000/contact \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+216 21 123 456",
    "subject": "Account inquiry",
    "message": "I would like to open a new account.",
    "language": "en"
  }'
```

---

### GET /faq

**Purpose**: Retrieve FAQs from the repository knowledge base.

**Method**: `GET`

#### Query parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `language` | string | No | `fr`, `ar`, or `en` |
| `category` | string | No | Filters FAQ entries by category |

#### Response shape

```json
{
  "status": "success",
  "language": "fr",
  "total": 12,
  "faqs": [
    {
      "id": "faq_001",
      "question": "Question text",
      "answer": "Answer text",
      "language": "fr",
      "category": "particuliers"
    }
  ]
}
```

**Example**:

```bash
curl 'http://localhost:8000/faq?language=fr&category=particuliers'
```

---

### POST /ingest

**Purpose**: Trigger FAQ ingestion and initialize the local ChromaDB-backed retrieval store.

**Method**: `POST`

#### Current behavior
This endpoint does not require a request body. It:
1. Loads the FAQ knowledge base from the repository.
2. Builds chunks and ingestion state.
3. Initializes or refreshes the ChromaDB collection if needed.

#### Response shape

```json
{
  "status": "success",
  "message": "FAQ ingestion complete",
  "chunks_processed": 75,
  "languages": ["fr", "ar", "en"],
  "timestamp": "2026-07-04T10:30:45.123456"
}
```

**Example**:

```bash
curl -X POST http://localhost:8000/ingest
```

---

## Data files and integrations

The current backend relies on these local resources:
- `chatbot/faq_ingestion_state.json` for ingestion state
- `chatbot/chroma_data/` for persisted local vector storage
- `chatbot/contact_submissions.jsonl` for contact logs
- `faq_knowledge_base.json` for FAQ source content

The RAG pipeline uses:
- ChromaDB for retrieval
- Groq for optional LLM generation when `GROQ_API_KEY` is configured
- local template fallbacks when the LLM or retriever is unavailable

---

## Testing examples

### Using curl

```bash
curl http://localhost:8000/health

curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "How do I open an account?", "language": "en"}'

curl -X POST http://localhost:8000/contact \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com", "phone": "+216 21 123 456", "subject": "Question", "message": "Hello", "language": "en"}'

curl 'http://localhost:8000/faq?language=fr'

curl -X POST http://localhost:8000/ingest
```

### Using Python

```python
import requests

base = "http://localhost:8000"
print(requests.get(f"{base}/health").json())
print(requests.post(f"{base}/chat", json={"message": "Hello", "language": "en"}).json())
```

---

**Last Updated**: July 4, 2026  
**Maintained By**: Amen Bank AI Team
