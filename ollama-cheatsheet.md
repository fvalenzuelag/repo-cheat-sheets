# Ollama Cheatsheet

## Basic Commands

### Installation
```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows
# Download from https://ollama.com/download/windows
```

### Managing Models
```bash
# List available models
ollama list

# Pull a model
ollama pull [model]
# Examples:
ollama pull llama2
ollama pull mistral
ollama pull llama2:13b

# Remove a model
ollama rm [model]

# Get model info
ollama show [model]
```

### Running Models
```bash
# Run model in interactive mode
ollama run [model]

# One-shot prompt
echo "Your prompt here" | ollama run [model]

# Generate with specific parameters
ollama run [model] --temperature 0.7 --top_p 0.9
```

## Creating Custom Models

### Custom Model with Modelfile
```bash
# Create a Modelfile
cat > Modelfile << EOF
FROM llama2
SYSTEM "You are a helpful assistant that specializes in Python programming."
PARAMETER temperature 0.7
EOF

# Create model from Modelfile
ollama create python-assistant -f Modelfile

# Run the custom model
ollama run python-assistant
```

### Modelfile Parameters

```
FROM [model]          # Base model
SYSTEM "..."          # System prompt
TEMPLATE "..."        # Custom prompt template
PARAMETER temp [0-1]  # Set default temperature
PARAMETER top_p [0-1] # Set default top_p
ADAPTER [path]        # Path to LoRA adapter
```

## API Usage

### Server Management
```bash
# Start Ollama server
ollama serve

# Check server status
curl http://localhost:11434/api/version
```

### API Examples
```bash
# Generate completion
curl -X POST http://localhost:11434/api/generate -d '{
  "model": "llama2",
  "prompt": "Write a Python function to calculate Fibonacci numbers",
  "stream": false
}'

# List models
curl http://localhost:11434/api/tags

# Create embedding
curl -X POST http://localhost:11434/api/embeddings -d '{
  "model": "llama2",
  "prompt": "The text to embed"
}'
```

## Advanced Usage

### Fine-tuning
```bash
# Basic fine-tuning (requires a Modelfile with training data)
cat > Modelfile << EOF
FROM llama2
ADAPTER ./path/to/adapter
EOF

ollama create my-tuned-model -f Modelfile
```

### Multi-Modal Models
```bash
# Run models that support image input
ollama run llava

# Pass image to model
ollama run llava -m "What's in this image?" < image.jpg
```

### Quantization Levels
```bash
# Pull different quantization levels
ollama pull llama2:7b-q4_0  # More compressed, faster, less accurate
ollama pull llama2:7b-q8_0  # Less compressed, slower, more accurate
```

### Context Window Management
```bash
# Set specific context window size
ollama run llama2 --num_ctx 8192
```
