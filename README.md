# Draph

<p align="center">
  <img src="resources/draph-logo.png" alt="Draph Logo" width="200">
</p>

<p align="center">
  <strong>Visual diagram editor with Mermaid support.</strong><br>
  <sub>Draw, edit, share - no account required.</sub>
</p>

<p align="center">
  <a href="https://draph.sanath.dev"><strong>Try it now</strong></a>
</p>

---

## Why Draph?

- **Instant** - Opens in milliseconds, works offline
- **Visual** - Drag, connect, style - everything editable on canvas
- **Mermaid support** - Paste Mermaid code, get editable visual diagram
- **Shareable** - One URL contains your entire diagram
- **AI-native** - API + MCP server for LLM integrations

## Features

### Shapes & Connections
- **Node types** - Rectangle, pill, diamond, circle, container (grouping), text, arrow
- **Smart connections** - Drag from any edge, auto-routing with rounded/straight modes
- **Quick connect** - Drop a connection on empty canvas to choose node type
- **Fill styles** - Outline, infill, stripe, grid patterns
- **Colors** - Tokyo Night Storm palette built-in

### Diagram Types
- **Flowcharts** - Standard flow with decisions, loops, groups
- **Sequence diagrams** - Lifelines, messages, loop/alt/opt frames
- **Class diagrams** - UML classes with properties and methods
- **Architecture diagrams** - Containers for grouping related components

### Import/Export
- **Mermaid import** - Paste Mermaid code, get editable diagram
- **PNG/SVG export** - High-res images for docs
- **Share URLs** - URL contains full diagram state
- **JSON** - Full control over positioning

## Quick Start

**Online:** [draph.sanath.dev](https://draph.sanath.dev)

**Local:**
```bash
git clone https://github.com/sanathks/draph.git
cd draph
npx serve
```

Or just open `index.html` directly.

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `V` | Select tool |
| `1` | Rectangle |
| `2` | Pill |
| `3` | Diamond |
| `4` | Circle |
| `5` | Container |
| `6` | Text |
| `7` | Arrow/Line |
| `A` | Auto-arrange |
| `M` | Mermaid import |
| `G` | Toggle grid snap |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Ctrl+C/V` | Copy/Paste |
| `Delete` | Delete selected |
| `Escape` | Deselect |

## AI & LLM Integration

Draph is built for AI agents. Generate diagrams via API or connect to Claude Desktop via MCP.

### LLM Context

For AI integrations, fetch the context file:
```
https://draph.sanath.dev/llm.txt
```

Contains full JSON schema, color palette, and examples optimized for LLM consumption.

### API

**Create diagram:**
```bash
curl -X POST https://draph.sanath.dev/api/diagram \
  -H "Content-Type: application/json" \
  -d '{"mermaid": "flowchart TD\n  A[Start] --> B{Decision}\n  B -->|Yes| C[Done]"}'
```

Returns:
```json
{
  "url": "https://draph.sanath.dev/d/abc123",
  "editUrl": "https://draph.sanath.dev/#eyJuIjpb..."
}
```
- `url` - Short shareable link (for LLMs/sharing)
- `editUrl` - Direct editor link with full data

**Export as PNG** *(experimental)*:
```bash
curl -X POST https://draph.sanath.dev/api/diagram/image \
  -H "Content-Type: application/json" \
  -d '{"nodes": [...], "connections": [...]}' \
  --output diagram.png
```

### MCP Server (Claude Desktop)

Add to Claude Desktop config:

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

**Tools:** `create_diagram`, `create_diagram_json`, `export_diagram_image` *(experimental)*

## License

MIT

---

<p align="center">
  Made by <a href="https://sanath.dev">Sanath</a>
</p>
