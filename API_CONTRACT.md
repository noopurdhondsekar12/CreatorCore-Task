# CreatorCore Backend API Contract

## Overview
The CreatorCore Backend provides RESTful APIs for content generation with contextual memory and reinforced feedback. It supports generating stories, ad scripts, and podcast scripts while maintaining context through embeddings and learning from user feedback.

## Base URL
```
http://localhost:5000
```

## Endpoints

### 1. Generate Content
**POST** `/generate`

Generates content based on topic and goal, returning related context from previous generations.

**Request Body:**
```json
{
  "topic": "string (required)",
  "goal": "string (required)",
  "type": "string (optional, default: 'story') - 'story' | 'ad' | 'podcast'"
}
```

**Response (200 OK):**
```json
{
  "id": "string - generation ID",
  "topic": "string",
  "output_text": "string - generated content",
  "tokens_used": "number - estimated token count",
  "related_context": [
    {
      "id": "string",
      "topic": "string",
      "output_text": "string",
      "similarity": "number (0-1)",
      "feedback_score": "number",
      "combined_score": "number"
    }
  ]
}
```

**Error Responses:**
- `400 Bad Request`: Missing topic or goal, or invalid type
- `500 Internal Server Error`: Generation failed

### 2. Submit Feedback
**POST** `/feedback`

Updates feedback for a generation and adjusts its score for future retrieval weighting.

**Request Body:**
```json
{
  "id": "string (required) - generation ID",
  "feedback": "string (required) - user feedback text"
}
```

**Response (200 OK):**
```json
{
  "message": "Feedback updated successfully"
}
```

**Error Responses:**
- `400 Bad Request`: Missing id or feedback
- `500 Internal Server Error`: Update failed

### 3. Get History
**GET** `/history/{topic}`

Retrieves the latest generation for a specific topic.

**Path Parameters:**
- `topic`: URL-encoded topic string

**Response (200 OK):**
```json
{
  "topic": "string",
  "goal": "string",
  "output_text": "string",
  "timestamp": "string (ISO format)",
  "feedback_score": "number",
  "iteration": "string"
}
```

**Error Responses:**
- `404 Not Found`: No generations found for topic

## Data Models

### Generation
```json
{
  "_id": "ObjectId",
  "topic": "string",
  "goal": "string",
  "output_text": "string",
  "tokens_used": "number",
  "timestamp": "string (ISO 8601)",
  "iteration": "string",
  "feedback": "string (optional)",
  "feedback_score": "number (default: 0.0)",
  "embedding": "array of numbers (optional)",
  "embedding_updated": "string (ISO 8601, optional)"
}
```

## Sequence Diagrams

### Content Generation Flow
```
Client -> Backend: POST /generate {topic, goal, type}
Backend -> DB: Check for similar generations
DB -> Backend: Return similar docs
Backend -> AI Model: Generate content
AI Model -> Backend: Return generated text
Backend -> Embedding Service: Generate embedding
Embedding Service -> Backend: Return vector
Backend -> DB: Store generation + embedding
DB -> Backend: Confirm storage
Backend -> Client: Return generation + related_context
```

### Feedback Loop Flow
```
Client -> Backend: POST /feedback {id, feedback}
Backend -> Scoring Engine: Analyze feedback text
Scoring Engine -> Backend: Return score change
Backend -> DB: Update feedback + score
DB -> Backend: Confirm update
Backend -> Client: Success message
```

## Error Handling
All endpoints return JSON error responses with appropriate HTTP status codes. Error messages are descriptive for debugging.

## Rate Limiting
No rate limiting implemented in current version.

## Authentication
No authentication required in current version.

## Versioning
API version not specified in URLs. Breaking changes will be communicated separately.