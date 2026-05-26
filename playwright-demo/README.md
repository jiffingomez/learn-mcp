# 🎭 Playwright MCP Demo

A hands-on sandbox for learning the **Playwright MCP server** — control a real browser from Claude Code using natural language.

---

## What is Playwright MCP?

The **Playwright MCP server** (`@playwright/mcp`) exposes browser automation tools (navigate, click, type, screenshot, etc.) directly to Claude via the **Model Context Protocol**. Instead of writing test scripts, you simply describe what you want and Claude drives the browser for you.

```
You ──(natural language)──▶ Claude ──(MCP tools)──▶ Playwright ──▶ Browser
```

---

## Setup

### 1 · Install the MCP server (once, globally)

```bash
npm install -g @playwright/mcp
```

> **Note:** No separate `playwright install` step is needed — `@playwright/mcp` bundles its own browser.

### 2 · Register the server in Claude Code

```bash
claude mcp add playwright -- npx @playwright/mcp@latest
```

This adds the server to your project's `.claude/mcp.json`. Verify it was added:

```bash
claude mcp list
```

You should see `playwright` in the list.

### 3 · Restart / open Claude Code

The MCP server starts automatically when Claude Code launches. You're ready to go!

---

## How to serve the demo page

The HTML file needs to be served over HTTP (not opened as a `file://` URL) for full browser behaviour.

```bash
# From the playwright-demo folder:
npx serve .
# → http://localhost:3000

# Or with Python:
python3 -m http.server 8080
# → http://localhost:8080
```

---

## Sample Prompts

Copy-paste these directly into Claude Code once the MCP server is running.

### 🔍 Navigation & Screenshots

```
Navigate to http://localhost:3000 and take a screenshot of the full page.
```

```
Go to http://localhost:3000, wait for the page to fully load, then take a screenshot.
```

```
Take a screenshot of just the "Counter" section on http://localhost:3000.
```

---

### 🔢 Counter Widget

```
On http://localhost:3000, click the Increment (+) button 5 times, then take a screenshot
showing the counter value.
```

```
Go to http://localhost:3000 and click Decrement 3 times, then click Reset, then take a screenshot.
```

```
Click Increment 10 times and tell me what colour the counter number turns.
```

---

### ✅ Todo List

```
On http://localhost:3000, add a new todo item "Learn Playwright MCP" using the input field,
then take a screenshot.
```

```
Navigate to http://localhost:3000, type "Write automation tests" in the todo input
and press Enter to add it.
```

```
On http://localhost:3000, check the "Take a screenshot" todo item as done,
then take a screenshot of the updated list.
```

```
Delete the first todo item on http://localhost:3000 and take a screenshot.
```

---

### 🎨 Color Picker

```
On http://localhost:3000, change the colour picker to #ff6b6b (coral red)
and take a screenshot of the colour preview.
```

```
Set the colour input on http://localhost:3000 to a deep purple (#7c3aed)
and describe what changes on the page.
```

---

### 📋 Contact Form

```
Fill in the contact form on http://localhost:3000 with:
  - Name: "Jane Doe"
  - Email: "jane@example.com"
  - Role: "QA Engineer"
  - Message: "Hello from Playwright MCP!"
Then click Submit and take a screenshot of the result.
```

```
Try submitting the contact form on http://localhost:3000 without filling
any fields and take a screenshot of the validation message.
```

```
Fill only the Name and Email fields on the contact form (leave Role empty),
submit it, and show me the error.
```

---

### 🔎 Accessibility & Page Info

```
Go to http://localhost:3000 and list all the interactive elements on the page
(buttons, inputs, selects).
```

```
Navigate to http://localhost:3000 and get the page title and heading text.
```

```
Take a full-page screenshot of http://localhost:3000 and describe the layout.
```

---

### 🛠️ Modify & Re-test Flow

Use this pattern to make a change to `index.html` then immediately verify it in the browser:

```
1. Change the "Increment" button colour to orange in index.html.
2. Reload http://localhost:3000 in the browser.
3. Take a screenshot to confirm the change.
```

```
Add a "Clear All" button to the todo list section in index.html,
then open http://localhost:3000 and verify the button appears.
```

---

## Available Playwright MCP Tools (reference)

| Tool | What it does |
|------|-------------|
| `browser_navigate` | Go to a URL |
| `browser_screenshot` | Capture the current viewport |
| `browser_click` | Click an element by selector or text |
| `browser_type` | Type text into a focused element |
| `browser_fill` | Fill an input field |
| `browser_select_option` | Choose a `<select>` option |
| `browser_check` / `browser_uncheck` | Toggle checkboxes |
| `browser_hover` | Hover over an element |
| `browser_scroll` | Scroll the page |
| `browser_wait_for` | Wait for an element / condition |
| `browser_evaluate` | Run JavaScript in the page |
| `browser_get_text` | Get visible text content |
| `browser_get_attribute` | Read an element attribute |
| `browser_close` | Close the browser |

> Full tool list: https://github.com/microsoft/playwright-mcp

---

## File Structure

```
playwright-demo/
├── index.html   ← The demo sandbox page
└── README.md    ← This file
```

---

## Tips

- **Serve first** — always start your HTTP server before asking Claude to navigate.
- **Be specific** — mention element labels or visible text; e.g. "the *Add Task* button".
- **Iterate fast** — edit `index.html`, reload the page (`browser_navigate` to the same URL), then verify.
- **Chain actions** — Playwright MCP handles multi-step instructions in a single prompt.