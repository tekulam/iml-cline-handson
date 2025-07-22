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
pip install -r practice/requirements.txt
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

### Add Groq API key
You can fetch the groq api key from here: https://console.groq.com/keys

#### On macOS/Linux:
1) Temporary (Current Terminal Session Only)
   ```bash
   export GROQ_API_KEY="your-api-key-here"
   ```
   This will only last for your current terminal session. Once you close the terminal, you'll need to set it again.

2) Semi-Permanent (For Your Current Shell)
   a) Add it to your shell profile file:
      For Zsh:
      ```bash
      echo 'export GROQ_API_KEY="your-api-key-here"' >> ~/.zshrc
      ```

   b) Then reload your profile:
      ```bash
      source ~/.zshrc
      ```

#### On Windows:
1) Temporary (Current Command Prompt Session Only)
   ```cmd
   set GROQ_API_KEY=your-api-key-here
   ```

2) Permanent (System-wide)
   ```cmd
   setx GROQ_API_KEY "your-api-key-here"
   ```
   Note: You'll need to restart your command prompt for this to take effect.

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
ollama pull llama3
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
ollama run llama3
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
ollama pull llama3
```

#### Run a model:
```cmd
ollama run llama3
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
