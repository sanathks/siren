# Siren

<p align="center">
  <img src="resources/siren-logo.png" alt="Siren Logo" width="200">
</p>

<p align="center">
  <strong>The best of both worlds: Mermaid syntax + visual editing.</strong><br>
  <sub>Create, edit, and share diagrams instantly.</sub>
</p>

<p align="center">
  <a href="https://siren.sanath.dev"><strong>Try it now →</strong></a>
</p>

---

## Create Diagrams Fast

Open the browser. Start drawing. Share instantly.

No account. No install. No learning curve.

```bash
# Run locally in one command
npx serve
```

## Why Siren?

Most diagram tools are either:
- **Too heavy** - Login walls, slow load times, cluttered UI
- **Too limited** - Can't customize, can't export, can't share easily

Siren is different:

- ⚡ **Instant** - Opens in milliseconds, works offline
- 🎨 **Visual** - Drag, resize, style - everything is editable
- 🔗 **Shareable** - One URL contains your entire diagram
- 📦 **Portable** - Single HTML file, zero dependencies

## Features

- **All the shapes** - Rectangles, pills, diamonds, circles, containers, text, arrows
- **Smart connections** - Auto-routing with rounded corners, labels, colors
- **Sequence diagrams** - Lifelines, messages, self-referencing loops
- **Keyboard-first** - Fast workflow for power users
- **Export anywhere** - PNG, SVG, or shareable link

### Plus: Mermaid Support

Already have diagrams in Mermaid? Import them, edit visually, export back to code. Best of both worlds.

### Plus: AI-Native

Built for LLMs from day one. API endpoints, MCP server for Claude Desktop, and `/llm.txt` context file for custom integrations.

## Quick Start

**Online:** [siren.sanath.dev](https://siren.sanath.dev)

**Local:**
```bash
git clone https://github.com/user/siren.git
cd siren
npx serve
```

Or just download `index.html` and open it.

## AI & LLM Integration

Siren is built for AI agents. Generate diagrams programmatically via API or connect directly to Claude Desktop via MCP.

### API Endpoints

**Create shareable diagram:**
```bash
curl -X POST https://siren.sanath.dev/api/diagram \
  -H "Content-Type: application/json" \
  -d '{"mermaid": "flowchart TD\n  A[Start] --> B{Decision}\n  B -->|Yes| C[Done]"}'
```

Returns:
```json
{
  "url": "https://siren.sanath.dev/#eyJuIjpb...",
  "data": { "n": [...], "c": [...] }
}
```

**Export as PNG:**
```bash
curl -X POST https://siren.sanath.dev/api/diagram/image \
  -H "Content-Type: application/json" \
  -d '{"mermaid": "flowchart TD\n  A[Start] --> B[End]"}' \
  --output diagram.png
```

### MCP Server (Claude Desktop)

Connect Siren directly to Claude Desktop for natural language diagram creation.

**Setup:**

Add to your Claude Desktop config (`~/.config/claude/claude_desktop_config.json` on Linux/Mac):

```json
{
  "mcpServers": {
    "siren": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://siren.sanath.dev/mcp"]
    }
  }
}
```

**Available tools:**
- `create_diagram` - Create diagram from Mermaid code
- `create_diagram_json` - Create diagram from JSON nodes/connections
- `export_diagram_image` - Export diagram as PNG

**Example prompt:**
> "Create a flowchart showing user authentication flow with login, validation, and redirect steps"

Claude will generate the diagram and return a shareable URL.

### LLM Context File

For custom AI integrations, fetch the context file:
```
https://siren.sanath.dev/llm.txt
```

Contains color palette, JSON schema, and API documentation optimized for LLM consumption.

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1-6` | Shape tools |
| `V` | Select mode |
| `A` | Auto-arrange |
| `M` | Mermaid editor |
| `Ctrl+Z/Y` | Undo/Redo |
| `Delete` | Delete selected |

## License

MIT

---

<p align="center">
  Made by <a href="https://sanath.dev">Sanath</a>
</p>
