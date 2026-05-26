# 🧠 learn-mcp

A hands-on learning repository for exploring **Model Context Protocol (MCP)** — from building a CLI chat client to controlling a real browser with natural language.

---

## Projects

### 1 · [MCP Chat CLI](./mcp_cli_project/) — `mcp_cli_project/`

A command-line chat application powered by the **Anthropic API** and MCP architecture.

**Highlights**
- Interactive chat with Claude via the terminal
- Document retrieval using `@document-id` syntax
- Slash-command prompts (`/summarize`, etc.) with Tab auto-complete
- Extensible tool integrations via MCP server/client pattern

**Quick start**

```bash
cd mcp_cli_project

# Recommended: using uv
uv venv && source .venv/bin/activate
uv pip install -e .
uv run main.py

# Or with plain pip
python -m venv .venv && source .venv/bin/activate
pip install anthropic python-dotenv prompt-toolkit "mcp[cli]==1.8.0"
python main.py
```

> Requires `ANTHROPIC_API_KEY` set in a `.env` file.

→ [Full setup & usage guide](./mcp_cli_project/README.md)

---

### 2 · [Playwright MCP Demo](./playwright-demo/) — `playwright-demo/`

A browser automation sandbox that demonstrates the **Playwright MCP server** — drive a real browser from Claude Code using plain English.

```
You ──(natural language)──▶ Claude ──(MCP tools)──▶ Playwright ──▶ Browser
```

**Highlights**
- Interactive HTML sandbox page (counter, todo list, colour picker, contact form)
- No test scripts needed — describe what you want and Claude does it
- Sample prompts for navigation, clicks, form filling, screenshots, and more
- Iterate fast: edit HTML → reload → verify, all in one prompt

**Quick start**

```bash
# 1. Register the MCP server (one-time)
claude mcp add playwright -- npx @playwright/mcp@latest

# 2. Serve the demo page
cd playwright-demo && npx serve .
# → http://localhost:3000

# 3. Try your first prompt in Claude Code:
# "Navigate to http://localhost:3000 and take a screenshot"
```

→ [Full setup, sample prompts & tool reference](./playwright-demo/README.md)

---

## Repository Structure

```
learn-mcp/
├── mcp_cli_project/      ← MCP Chat CLI (Python, Anthropic SDK)
│   ├── main.py
│   ├── mcp_client.py
│   ├── mcp_server.py
│   └── README.md
├── playwright-demo/      ← Playwright MCP browser sandbox
│   ├── index.html
│   └── README.md
└── README.md             ← This file
```

---

## What is MCP?

The **Model Context Protocol** is an open standard that lets AI models interact with external tools and data sources through a uniform interface. Each MCP *server* exposes a set of tools; an MCP *client* (like Claude Code) discovers and calls those tools on your behalf.

| Concept | Description |
|---------|-------------|
| **MCP Server** | Exposes tools (functions) the model can call |
| **MCP Client** | Connects to servers and routes tool calls |
| **Tool** | A named, typed function with a description the model uses to decide when to call it |

Learn more → [modelcontextprotocol.io](https://modelcontextprotocol.io)