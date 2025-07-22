# Design Document: Bedrock Integration

## Overview

This design outlines the approach for integrating Amazon Bedrock models into the existing LLM chat application. The integration will allow users to interact with foundation models available through Amazon Bedrock alongside the current Ollama and Groq options.

## Architecture

The application will maintain its current structure while adding a new module for Bedrock integration. The architecture will follow these principles:

1. **Modularity**: Each LLM provider (Ollama, Groq, Bedrock) will be encapsulated in its own initialization logic
2. **Configuration-driven**: Model selection and provider selection will be configurable via environment variables
3. **Error handling**: Robust error handling for API calls and authentication issues
4. **Consistent interface**: All LLM providers will expose a similar interface for generating responses

## Components and Interfaces

### 1. Bedrock Client Integration

We'll use the `langchain-aws` package to integrate with Amazon Bedrock. This provides a consistent interface similar to the existing Groq and Ollama integrations.

```python
from langchain_aws import Bedrock

# Initialize Bedrock client
bedrock_llm = Bedrock(
    model_id="anthropic.claude-v2",  # Default model, can be overridden
    region_name="us-west-2",         # Default region, can be overridden
)
```

### 2. Environment Configuration

We'll extend the environment variable handling to include Bedrock-specific configurations:

```python
# Environment variables for Bedrock
BEDROCK_MODEL_ID = os.getenv("BEDROCK_MODEL_ID", "anthropic.claude-v2")
AWS_REGION = os.getenv("AWS_REGION", "us-west-2")
```

### 3. Provider Selection Logic

The provider selection logic will be extended to include Bedrock:

```python
def get_llm_provider(provider_name):
    if provider_name.lower() == "ollama":
        return llm_ollama
    elif provider_name.lower() == "groq":
        return llm_groq
    elif provider_name.lower() == "bedrock":
        return llm_bedrock
    else:
        raise ValueError(f"Unsupported LLM provider: {provider_name}")
```

### 4. Response Handling

Each provider returns responses in slightly different formats. We'll standardize the response handling:

```python
def get_response(provider, prompt):
    try:
        if provider == llm_ollama:
            return provider.invoke(prompt)
        elif provider == llm_groq:
            return provider.invoke(prompt).content
        elif provider == llm_bedrock:
            return provider.invoke(prompt)
        else:
            raise ValueError("Unknown provider")
    except Exception as e:
        return f"Error: {str(e)}"
```

## Data Models

### Configuration Model

```python
class LLMConfig:
    provider: str  # "ollama", "groq", or "bedrock"
    model_id: str  # Model ID specific to the provider
    region: Optional[str]  # Only needed for Bedrock
```

## Error Handling

1. **Authentication Errors**: When AWS credentials are missing or invalid, provide clear error messages directing users to set up their credentials.

2. **Model Availability Errors**: Handle cases where the requested Bedrock model is not available in the specified region.

3. **API Rate Limiting**: Implement exponential backoff for rate limit errors.

4. **Network Errors**: Gracefully handle network connectivity issues with informative messages.

## Testing Strategy

1. **Unit Tests**: Test the provider selection logic and response handling with mocked LLM clients.

2. **Integration Tests**: Test the Bedrock integration with actual AWS credentials (using environment variables).

3. **Configuration Tests**: Verify that the application correctly reads and applies configuration from environment variables.

## Security Considerations

1. **Credential Management**: AWS credentials should be managed securely using environment variables or AWS credential providers.

2. **Input Validation**: All user inputs should be validated before being sent to the LLM providers.

3. **Error Message Security**: Ensure that error messages don't expose sensitive information like API keys or credentials.