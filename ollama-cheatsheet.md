# Ollama Cheat Sheet

## Installation

```bash
# MacOS & Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# Download from https://ollama.com/download/windows
```

## Basic Commands

```bash
# Run a model (downloads it if not available)
ollama run llama2

# List all available models
ollama list

# Pull a specific model
ollama pull llama2

# Remove a model
ollama rm llama2

# Get information about a model
ollama show llama2
```

## Running Models

```bash
# Basic conversation
ollama run llama2

# Run with specific parameters
ollama run llama2 --temperature 0.7 --top_p 0.9

# One-shot query (non-interactive)
ollama run llama2 "Write a short poem about coding"

# Stream response to file
ollama run llama2 "Explain quantum computing" > response.txt
```

## Model Management

```bash
# Create a model tag from a Modelfile
ollama create mymodel -f Modelfile

# Push model to Ollama registry
ollama push username/mymodel:latest

# Pull model from Ollama registry
ollama pull username/mymodel:latest

# Copy a model
ollama cp mistral myownmistral
```

## API Usage

```bash
# Basic API request
curl -X POST http://localhost:11434/api/generate -d '{
  "model": "llama2",
  "prompt": "Why is the sky blue?"
}'

# Chat completion API
curl -X POST http://localhost:11434/api/chat -d '{
  "model": "llama2",
  "messages": [
    { "role": "user", "content": "Hello, who are you?" }
  ]
}'
```

## Server Management

```bash
# Start Ollama server
ollama serve

# Specify server address
OLLAMA_HOST=0.0.0.0:11434 ollama serve

# Use custom model library path
OLLAMA_MODELS=/path/to/models ollama serve
```

## Advanced Usage

```bash
# Create a custom model with a Modelfile
cat << EOF > Modelfile
FROM llama2
SYSTEM "You are a helpful assistant specialized in Python programming."
PARAMETER temperature 0.7
EOF
ollama create pyassistant -f Modelfile

# Embeddings API
curl -X POST http://localhost:11434/api/embeddings -d '{
  "model": "llama2",
  "prompt": "The quick brown fox jumps over the lazy dog"
}'
```

## Popular Models

- `llama2`, `llama2:13b`, `llama2:70b` - Meta's LLaMA 2 models
- `mistral`, `mistral:instruct` - Mistral AI models
- `codellama` - Specialized for code generation
- `gemma`, `gemma:7b`, `gemma:2b` - Google's lightweight models
- `phi` - Microsoft's small but capable models

## Python Integration

```python
# Install the official client
pip install ollama

# Basic usage
import ollama

# Generate text
response = ollama.generate(
    model='llama2',
    prompt='Explain asyncio in Python'
)
print(response['response'])

# Chat completion
response = ollama.chat(
    model='llama2',
    messages=[
        {'role': 'user', 'content': 'What are Python decorators?'}
    ]
)
print(response['message']['content'])

# Embeddings
embeddings = ollama.embeddings(
    model='llama2',
    prompt='Python is a programming language'
)
```

## Environment Variables

```bash
# Server address
OLLAMA_HOST=127.0.0.1:11434

# Model library location
OLLAMA_MODELS=/path/to/models

# GPU device selection
CUDA_VISIBLE_DEVICES=0

# VRAM usage control
OLLAMA_GPU_LAYERS=35
```

## Troubleshooting

- **Model download issues**: Check network connection and disk space
- **Out of memory errors**: Use a smaller model or adjust `OLLAMA_GPU_LAYERS`
- **Server not responding**: Verify Ollama is running with `ps aux | grep ollama`
- **Slow responses**: Consider using quantized models (e.g., `:q4_k_m`)
