# LLM Integration

Draph is built with AI agents in mind. This guide covers how to integrate Draph into your LLM workflows.

## Quick Start for LLMs

**Step 1:** Fetch the context file
```
https://draph.sanath.dev/llm.txt
```

This contains everything an LLM needs: JSON schema, color palette, examples.

**Step 2:** Create a diagram via API
```bash
POST https://draph.sanath.dev/api/diagram
Content-Type: application/json

{"nodes": [...], "connections": [...]}
```

**Step 3:** Share the returned `url` (short link)

---

## llm.txt Context File

The `/llm.txt` endpoint provides a comprehensive reference optimized for LLM consumption:

- Complete JSON schema for nodes and connections
- All node types with default sizes
- Color palette (Tokyo Night Storm)
- Fill styles and line styles
- Sequence and class diagram extensions
- Multiple examples

**Best practice:** Fetch `llm.txt` at the start of a diagram-related conversation to ensure accurate schema knowledge.

---

## MCP Server (Claude Desktop)

Draph provides an MCP (Model Context Protocol) server for direct Claude Desktop integration.

### Setup

Add to your Claude Desktop config:

**macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
**Linux:** `~/.config/claude/claude_desktop_config.json`
**Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "draph": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://draph.sanath.dev/mcp"]
    }
  }
}
```

Restart Claude Desktop after adding.

### Available Tools

#### create_diagram
Create a diagram from Mermaid code.

**Input:**
```json
{
  "mermaid": "flowchart TD\n    A[Start] --> B{Decision}\n    B --> C[End]"
}
```

**Output:** Shareable URL

#### create_diagram_json
Create a diagram from JSON with precise control over positioning.

**Input:**
```json
{
  "nodes": [
    {"id": "n1", "type": "rect", "label": "Step 1", "x": 100, "y": 100},
    {"id": "n2", "type": "rect", "label": "Step 2", "x": 100, "y": 200}
  ],
  "connections": [
    {"id": "c1", "from": "n1", "to": "n2"}
  ]
}
```

**Output:** Shareable URL

#### export_diagram_image *(experimental)*
Export a diagram as a PNG image.

**Input:**
```json
{
  "mermaid": "flowchart LR\n    A --> B",
  "width": 1200,
  "height": 800
}
```

**Output:** Instructions for downloading the image

### Example Prompts

Once MCP is set up, try prompts like:

- "Create a flowchart showing user authentication flow"
- "Draw a sequence diagram for API request handling"
- "Make an architecture diagram with frontend, backend, and database layers"
- "Create a class diagram for a User and Admin relationship"

---

## Best Practices for LLMs

### 1. Always Fetch llm.txt First

The schema may evolve. Fetch `/llm.txt` before generating diagrams to ensure accuracy.

### 2. Use the Short URL

Always share the `url` field from the API response, not `editUrl`. The short URL is cleaner and redirects properly.

### 3. Respect Rate Limits

The API allows 30 requests per minute per IP. If you get a `429` response, wait for `retryAfter` seconds before retrying.

### 4. Position Nodes Thoughtfully

- Use consistent spacing (multiples of 20px work well with grid)
- Vertical flows: ~100px between rows
- Horizontal flows: ~180px between columns
- Leave room for labels

### 5. Color with Purpose

- **Green** (`#9ece6a`) - Start states, success
- **Red** (`#f7768e`) - Errors, end states, danger
- **Yellow** (`#e0af68`) - Decisions, warnings
- **Blue** (`#7aa2f7`) - Primary nodes, process steps
- **Magenta** (`#bb9af7`) - Special/highlighted nodes

### 6. Containers Go First

When using containers (group boxes), define them before the nodes they contain. This ensures proper render order.

### 7. Don't Use "transparent"

For outline-only nodes, omit the `color` property entirely. Never set `color: "transparent"`.

### 8. Match Node Sizes to Content

- Short labels (1-2 words): Default sizes work fine
- Long labels: Increase width to prevent overflow
- Circles: Keep width and height equal

---

## Integration Patterns

### Pattern 1: Direct API Call

For simple integrations, call the API directly:

```python
import requests

def create_diagram(mermaid_code):
    response = requests.post(
        'https://draph.sanath.dev/api/diagram',
        json={'mermaid': mermaid_code}
    )
    return response.json()['url']
```

### Pattern 2: Two-Step with Review

1. Generate diagram → get URL
2. Human reviews and tweaks in editor
3. Human shares final URL

This works well for complex diagrams that need visual refinement.

### Pattern 3: Template-Based

Pre-define JSON templates for common diagram types, then fill in labels:

```python
flowchart_template = {
    "nodes": [
        {"id": "start", "type": "pill", "label": "{START}", "x": 200, "y": 50, "color": "#9ece6a"},
        {"id": "step1", "type": "rect", "label": "{STEP1}", "x": 170, "y": 130},
        {"id": "end", "type": "pill", "label": "{END}", "x": 200, "y": 210}
    ],
    "connections": [
        {"id": "c1", "from": "start", "to": "step1"},
        {"id": "c2", "from": "step1", "to": "end"}
    ]
}
```

---

## Troubleshooting

### Diagram looks cramped
Increase spacing between nodes. Use 100-150px vertical gaps, 180-200px horizontal gaps.

### Colors not showing
Make sure you're using hex colors (e.g., `#7aa2f7`), not color names.

### Containers behind nodes
Define container nodes before the nodes they should contain in the JSON array.

### Connection labels overlapping
Keep labels short (1-3 words). Position decision labels on different edges if needed.

---

## See Also

- [API Reference](api-reference.md) - Complete API documentation
- [Examples](examples.md) - Common diagram patterns
- `/llm.txt` - LLM-optimized reference file
