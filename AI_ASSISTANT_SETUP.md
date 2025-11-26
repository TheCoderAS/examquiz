# AI Assistant Setup Guide

## Overview
The AI Assistant feature provides real-time doubt assistance to users during quiz sessions. It uses OpenAI's API to answer questions about quiz topics, concepts, and definitions without revealing correct answers.

## Setup Instructions

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

This will install the `openai` package along with other dependencies.

### 2. Get OpenAI API Key
1. Visit [OpenAI Platform](https://platform.openai.com/)
2. Sign up or log in to your account
3. Navigate to [API Keys](https://platform.openai.com/api-keys)
4. Create a new API key
5. Copy the API key (it will only be shown once)

### 3. Configure API Key

#### Option A: Environment Variable (Recommended for Production)
```bash
# Windows (PowerShell)
$env:OPENAI_API_KEY="your-api-key-here"

# Windows (Command Prompt)
set OPENAI_API_KEY=your-api-key-here

# macOS/Linux
export OPENAI_API_KEY="your-api-key-here"
```

#### Option B: Direct Configuration in settings.py
Edit `examquiz/settings.py` and add:
```python
OPENAI_API_KEY = 'your-api-key-here'
OPENAI_MODEL = 'gpt-3.5-turbo'  # or 'gpt-4' for better responses
```

**Note**: Never commit API keys to version control! Use environment variables in production.

### 4. Test the Feature
1. Start the Django development server:
   ```bash
   python manage.py runserver
   ```
2. Log in to your account
3. Start a quiz session
4. Click the "AI Assistant" button in the question header
5. Ask a question about the current question or topic

## How It Works

### Features
- **Contextual Assistance**: The AI knows about the current question, exam, category, and all choices
- **Educational Focus**: Provides explanations, definitions, and guidance without revealing answers
- **Real-time Chat**: Interactive chat interface with typing indicators
- **Secure**: Only authenticated users can access the assistant
- **No Answer Revealing**: AI is instructed not to reveal correct answers

### Usage Examples
- "What is [concept]?"
- "Can you explain [term]?"
- "What does [definition] mean?"
- "Help me understand this question better"

### Limitations
- Requires an active internet connection
- Uses OpenAI API credits (check pricing at [OpenAI Pricing](https://openai.com/pricing))
- Responses are limited to 300 tokens for cost control

## Configuration Options

In `examquiz/settings.py`:
- `OPENAI_API_KEY`: Your OpenAI API key (required)
- `OPENAI_MODEL`: Model to use (default: `gpt-3.5-turbo`)
  - `gpt-3.5-turbo`: Faster, cheaper, good for most cases
  - `gpt-4`: More accurate, slower, more expensive

## Troubleshooting

### Error: "OpenAI library not installed"
```bash
pip install openai>=1.0.0
```

### Error: "OpenAI API key not configured"
- Check that `OPENAI_API_KEY` is set in environment variables or settings.py
- Verify the API key is correct and active

### Error: "Error calling OpenAI API"
- Check your internet connection
- Verify your OpenAI account has available credits
- Check API key permissions in OpenAI dashboard

### Assistant not appearing
- Ensure you're logged in
- Check that you're in an active quiz session
- Look for the "AI Assistant" button in the question header

## Cost Management

- Each message uses ~300 tokens
- GPT-3.5-turbo costs approximately $0.0005 per request
- GPT-4 costs approximately $0.01-0.03 per request
- Monitor usage at [OpenAI Usage Dashboard](https://platform.openai.com/usage)

## Security Notes

1. **Never expose API keys** in client-side code
2. **Use environment variables** for API keys in production
3. **Rate limiting**: Consider adding rate limiting to prevent abuse
4. **Input validation**: All user messages are sanitized server-side

## Future Enhancements

Potential improvements:
- Chat history per session
- Rate limiting per user
- Custom prompts per exam/category
- Support for multiple AI providers
- Caching common questions

