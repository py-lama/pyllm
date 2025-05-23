# pyllm

pyllm is a CLI tool for managing LLM models with Ollama integration. It allows you to install, list, set the default model, and update the model list. Both standard CLI and interactive shell modes are available. PyLLM is part of the PyLama ecosystem and integrates with LogLama as the primary service for centralized logging and environment management.

## LogLama Integration

PyLLM integrates with LogLama as the primary service in the PyLama ecosystem. This integration provides:

- **Centralized Environment Management**: Environment variables are loaded from the central `.env` file in the `pylama` directory
- **Shared Configuration**: Model configurations are shared across all PyLama components
- **Dependency Management**: Dependencies are validated and installed by LogLama
- **Service Orchestration**: Services are started in the correct order using LogLama CLI
- **Centralized Logging**: All PyLLM operations are logged to the central LogLama system
- **Structured Logging**: Logs include component context for better filtering and analysis
- **Health Monitoring**: LogLama monitors PyLLM service health and availability

---

## General Diagram (Mermaid)
```mermaid
graph TD
    A[User] -->|CLI/Interactive| B[pyllm/cli.py]
    B --> C[models.py]
    B --> D[interactive_cli.py]
    C --> E[LogLama Central .env]
    C --> F[Ollama API]
    D --> B
    G[LogLama] --> E
```

---

## ASCII Diagram: CLI Command Flow
```
User
    |
    v
+-----------------+
|   pyllm CLI     |
+-----------------+
    |
    v
+-----------------+
|   models.py     |
+-----------------+
    |
+-----------------+
| LogLama Central |
|    .env File    |
+-----------------+
    |
+-----------------+
|  Ollama API     |
+-----------------+
```

---

## Modes

- **CLI**: `pyllm <command>`
- **Interactive**: `pyllm -i` or `pyllm interactive`

---

## Installation

```bash
# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install the package in development mode
pip install -e .  # This is important! Always install in development mode before starting
```

> **IMPORTANT**: Always run `pip install -e .` before starting the project to ensure all dependencies are properly installed and the package is available in development mode.

---

## Using the Makefile

PyLLM includes a Makefile to simplify common development tasks:

```bash
# Set up the project (creates a virtual environment and installs dependencies)
make setup

# Run the API server (default port 8001)
make run

# Run the API server on a custom port
make run PORT=8080

# The run-port command is also available for backward compatibility
make run-port PORT=8080

# Run tests
make test

# Format code with black
make format

# Lint code with flake8
make lint

# Clean up project (remove __pycache__, etc.)
make clean

# Show all available commands
make help
```

---

## Key Files

- `pyllm/cli.py` – main CLI
- `pyllm/interactive_cli.py` – interactive shell with menu and cursor selection
- `pyllm/models.py` – model logic, .env/env.example handling, Ollama integration
- `.env`/`env.example` – environment config and default model

---

## Example Usage

### List available models
```bash
pyllm list
```

### Install a model
```bash
pyllm install deepseek-coder:6.7b
```

### Set default model
```bash
pyllm set-default deepseek-coder:6.7b
```

### Show default model
```bash
pyllm default
```

### Update model list from Ollama
```bash
pyllm update
```

### Run interactive mode (menu, cursor selection)
```bash
pyllm -i
```

---

## set_default_model function flow (Mermaid)
```mermaid
flowchart TD
    S[Start] --> C{Does .env exist?}
    C -- Yes --> R[Update OLLAMA_MODEL in .env]
    C -- No --> K[Copy env.example to .env]
    K --> R
    R --> E[End]
```

---

## Interactive mode - menu (ASCII)
```
+--------------------------------+
|  pyllm - interactive mode       |
+--------------------------------+
| > List available models         |
|   Show default model           |
|   List installed models        |
|   Install model                |
|   Set default model            |
|   Update model list            |
|   Test default model           |
|   Exit                         |
+--------------------------------+
  (navigation: arrow keys + Enter)
```

---

## Installation

```bash
pip install pyllm
```

## Usage

```python
from pyllm import get_models, get_default_model, set_default_model, install_model

# Get available models
models = get_models()
for model in models:
    print(f"{model['name']} - {model.get('desc', '')}")

# Get the current default model
default_model = get_default_model()
print(f"Current default model: {default_model}")

# Set a new default model
set_default_model("codellama:7b")

# Install a model
install_model("deepseek-coder:6.7b")
```

## Environment Variables

The package uses the following environment variables:

- `OLLAMA_MODEL`: The default model to use with Ollama

## License
This project is licensed under the Apache 2.0 License (see LICENSE file).
