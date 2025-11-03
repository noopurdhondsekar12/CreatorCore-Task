# CreatorCore Task

A Flask-based backend application for content generation using AI prompts. This project provides APIs for generating stories, ad scripts, and podcast scripts with feedback tracking and history management.

## Features

- **Content Generation**: Generate stories, ad scripts, and podcast scripts using AI prompts
- **Feedback Loop**: Update and track feedback for generated content
- **History Tracking**: Retrieve past generations for specific topics
- **Database Integration**: MongoDB for storing generations and feedback
- **Mock Mode**: Fallback to mock data when database is unavailable

## Installation

1. Clone the repository:
```bash
git clone https://github.com/noopurdhondsekar12/CreatorCore-Task.git
cd CreatorCore-Task
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up environment variables (optional):
```bash
export MONGO_URI="your_mongodb_connection_string"
```

## Usage

### Running the Application

```bash
python backend/app.py
```

The server will start on `http://localhost:5000`

### API Endpoints

#### Generate Content
```http
POST /generate
Content-Type: application/json

{
  "topic": "Your topic here",
  "goal": "Your goal here",
  "type": "story|ad|podcast"
}
```

#### Update Feedback
```http
POST /feedback
Content-Type: application/json

{
  "id": "generation_id",
  "feedback": "Your feedback here"
}
```

#### Get History
```http
GET /history/{topic}
```

## Project Structure

```
CreatorCore-Task/
├── backend/
│   ├── app.py              # Main Flask application
│   ├── db_utils.py         # Database utilities
│   ├── prompts.py          # AI prompt templates
│   ├── utils/
│   │   └── schema.json     # Data schema
│   ├── test_*.json         # Test data files
├── requirements.txt        # Python dependencies
└── README.md              # This file
```

## Dependencies

- Flask: Web framework
- PyMongo: MongoDB driver
- LangChain Core: Prompt template management
- python-dotenv: Environment variable management

## Development

The application includes mock functions for testing without external dependencies. Set up a MongoDB instance for full functionality.

## License

This project is part of the CreatorCore Task.