# Let's Rock AI

## Setting Up Your Environment

### Create a virtual environment
```bash
python -m venv venv
```

### Activate the virtual environment:
#### On macOS/Linux:
```bash
source venv/bin/activate
```
You'll know it's activated when you see (venv) at the beginning of your terminal prompt.

#### On Windows:
```cmd
venv\Scripts\activate
```

### Install requirements from requirements.txt:
```bash
pip install -r requirements.txt
```

### Verify installation:
```bash
pip list
```

### Deactivate the virtual environment when done:
```bash
deactivate
```

## API Configuration

This project uses a `.env` file to manage API keys and configuration settings. Follow these steps to set up your environment:

1. Copy the example environment file to create your own:
   ```bash
   cp .env.example .env
   ```

2. Edit the `.env` file with your preferred text editor:
   ```bash
   nano .env  # or use any editor you prefer
   ```

3. Update the following settings in your `.env` file:

### Groq API Configuration
```
# Get your API key from https://console.groq.com/keys
GROQ_API_KEY=your_groq_api_key_here
```

### AWS Bedrock Configuration
```
# AWS credentials with Bedrock access permissions
AWS_ACCESS_KEY_ID=your_aws_access_key_id_here
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key_here
AWS_SESSION_TOKEN=your_aws_session_token_here  # if using temporary credentials
AWS_REGION=us-west-2  # or your preferred region where Bedrock is available

# Bedrock model settings
BEDROCK_MODEL_ID=anthropic.claude-v2  # Default model ID
BEDROCK_MODEL_KWARGS={"temperature": 0.7, "max_tokens_to_sample": 500}
```

#### Getting AWS Credentials with Ada

If you're using the Ada CLI tool, you can easily get your AWS credentials with the following command:

```bash
ada credentials update --account=<accountId> --provider=conduit --role=IibsAdminAccess-DO-NOT-DELETE --once
ada credentials print --account=<accountId> --provider=conduit --role=IibsAdminAccess-DO-NOT-DELETE 
```

This will output your credentials in JSON format:

```json
{
  "Version": 1,
  "AccessKeyId": "ASIAXXXXXXXXXXXXXXX",
  "SecretAccessKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  "SessionToken": "xxxxxxxx...",
  "Expiration": "2025-07-22T22:14:28Z"
}
```

You can then copy these values to your `.env` file:
- `AccessKeyId` → `AWS_ACCESS_KEY_ID`
- `SecretAccessKey` → `AWS_SECRET_ACCESS_KEY`
- `SessionToken` → `AWS_SESSION_TOKEN`

Note that these credentials are temporary and will expire at the time shown in the `Expiration` field. You'll need to refresh them periodically.

### Model Selection
```
# Choose which LLM provider to use
LLM_PICKER=bedrock  # Options: ollama, groq, bedrock
```

### Available Bedrock Models

When using AWS Bedrock, you can choose from these models by updating the `BEDROCK_MODEL_ID` in your `.env` file:

- `anthropic.claude-v2` - Anthropic's Claude v2
- `anthropic.claude-v2:1` - Claude v2.1
- `anthropic.claude-instant-v1` - Claude Instant
- `amazon.titan-text-express-v1` - Amazon Titan Text
- `ai21.j2-ultra-v1` - AI21 Jurassic-2 Ultra
- `cohere.command-text-v14` - Cohere Command

## Installing and Running Ollama

### On macOS
#### Install Ollama
The easiest way to install Ollama on macOS is using Homebrew:
```bash
brew install ollama
```
Alternatively, you can download the macOS app from the official website: https://ollama.com/download

#### Start the Ollama service:
```bash
ollama serve
```
This will start the Ollama server in the foreground. You can open a new terminal tab/window to continue working.

#### Pull a model:
```bash
ollama pull gemma:2b
```
Replace llama3 with any model you want to use. Some popular options:

- llama3 (Meta's Llama 3 8B model)
- llama3:8b
- llama3:70b (larger, more capable)
- mistral (Mistral 7B)
- gemma:2b (Google's Gemma 2B)
- gemma:7b

#### Run a model:
```bash
ollama run gemma:2b
```
This will start an interactive chat session with the model.

#### Use Ollama API: 
Ollama also provides a local API endpoint at http://localhost:11434 that you can use in your Python code.

### On Windows
#### Install Ollama
Download and install Ollama for Windows from the official website: https://ollama.com/download

#### Start the Ollama service:
After installation, Ollama should start automatically. If not, you can start it from the Start menu or run:
```cmd
ollama serve
```

#### Pull a model:
```cmd
ollama pull gemma:2b
```

#### Run a model:
```cmd
ollama run gemma:2b
```

#### Use Ollama API:
Just like on macOS, Ollama provides a local API endpoint at http://localhost:11434 that you can use in your Python code.

## Troubleshooting

### Windows-specific issues:
- If you encounter "Command not found" errors, ensure that Ollama is properly installed and added to your PATH.
- Some Windows environments may require running the Command Prompt or PowerShell as Administrator.
- If you experience GPU-related issues, ensure you have the latest graphics drivers installed.

### General issues:
- If models fail to download, check your internet connection and firewall settings.
- For memory-related errors when running larger models, ensure your system has sufficient RAM available.
