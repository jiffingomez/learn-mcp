# MCP Chat

MCP Chat is a command-line interface application that enables interactive chat capabilities with AI models through the Anthropic API. The application supports document retrieval, command-based prompts, and extensible tool integrations via the MCP (Model Control Protocol) architecture.

## Prerequisites

- **Python 3.12+** (Python 3.11 is not supported — see [Troubleshooting](#troubleshooting))
- Anthropic API Key

## Setup

### Step 1: Configure the environment variables

1. Create or edit the `.env` file in the project root and verify that the following variables are set correctly:

```
ANTHROPIC_API_KEY=""  # Enter your Anthropic API secret key
```

### Step 2: Install dependencies

#### Option 1: Setup with uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver.

1. Install uv, if not already installed:

```bash
pip install uv
```

2. Create and activate a virtual environment, **explicitly targeting Python 3.12**:

```bash
uv venv --python 3.12
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

> **Note:** A `.python-version` file is included in the project that pins the version to `3.12`.
> This ensures `uv` always picks the correct interpreter automatically.

3. Install dependencies:

```bash
uv pip install -e .
```

4. Run the project

```bash
uv run main.py
```

#### Option 2: Setup without uv

1. Create and activate a virtual environment using Python 3.12 explicitly:

```bash
python3.12 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

2. Install dependencies:

```bash
pip install anthropic python-dotenv prompt-toolkit "mcp[cli]>=1.8.0"
```

3. Run the project

```bash
python main.py
```

## Usage

### Basic Interaction

Simply type your message and press Enter to chat with the model.

### Document Retrieval

Use the @ symbol followed by a document ID to include document content in your query:

```
> Tell me about @deposition.md
```

### Commands

Use the / prefix to execute commands defined in the MCP server:

```
> /summarize deposition.md
```

Commands will auto-complete when you press Tab.

## Troubleshooting

### `ImportError: libssl.so.1.1: cannot open shared object file`

**Symptom:**

```
ImportError: libssl.so.1.1: cannot open shared object file: No such file or directory
```

**Cause:**

This error occurs when `uv` (or any tool) picks up **Python 3.11.0** compiled against OpenSSL 1.1.x
(usually located at `/usr/local/bin/python3`), but your system only ships OpenSSL 3.x.
If you have multiple Python versions installed, `uv` may resolve to this broken interpreter.

**Fix:**

Always specify Python 3.12 explicitly:

```bash
# Remove any existing broken venv
rm -rf .venv

# Re-create with the correct Python version
uv venv --python 3.12
source .venv/bin/activate
uv pip install -e .
```

Alternatively, verify which interpreter uv is using:

```bash
uv python list
```

Make sure the interpreter for `3.12` points to `/usr/bin/python3.12` (not `/usr/local/bin/python3`).

## Development

### Adding New Documents

Edit the `mcp_server.py` file to add new documents to the `docs` dictionary.

### Implementing MCP Features

To fully implement the MCP features:

1. Complete the TODOs in `mcp_server.py`
2. Implement the missing functionality in `mcp_client.py`

### Linting and Typing Check

There are no lint or type checks implemented.
