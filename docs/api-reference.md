# API Reference

Draph provides a REST API for programmatic diagram creation. Perfect for automation, CI/CD pipelines, and AI integrations.

**Base URL:** `https://draph.sanath.dev`

## Endpoints

### POST /api/diagram

Create a diagram and get shareable URLs.

**Request Body:**

Either Mermaid code:
```json
{
  "mermaid": "flowchart TD\n    A[Start] --> B[End]"
}
```

Or JSON nodes/connections:
```json
{
  "nodes": [...],
  "connections": [...]
}
```

**Response:**
```json
{
  "url": "https://draph.sanath.dev/d/abc123",
  "editUrl": "https://draph.sanath.dev/#eyJuIjpb..."
}
```

| Field | Description |
|-------|-------------|
| `url` | Short shareable URL (redirects to editUrl) |
| `editUrl` | Direct editor URL with encoded diagram data |

**Example:**
```bash
curl -X POST https://draph.sanath.dev/api/diagram \
  -H "Content-Type: application/json" \
  -d '{
    "nodes": [
      {"id": "n1", "type": "rect", "label": "Start", "x": 100, "y": 100},
      {"id": "n2", "type": "rect", "label": "End", "x": 100, "y": 200}
    ],
    "connections": [
      {"id": "c1", "from": "n1", "to": "n2"}
    ]
  }'
```

---

### POST /api/diagram/image *(experimental)*

Export a diagram as a PNG image.

**Request Body:**
```json
{
  "nodes": [...],
  "connections": [...],
  "width": 1200,
  "height": 800,
  "scale": 2
}
```

Or with Mermaid:
```json
{
  "mermaid": "flowchart TD\n    A --> B",
  "width": 1200,
  "height": 800,
  "scale": 2
}
```

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `width` | number | 1200 | Viewport width in pixels |
| `height` | number | 800 | Viewport height in pixels |
| `scale` | number | 2 | Resolution multiplier |

**Response:** Binary PNG image

**Example:**
```bash
curl -X POST https://draph.sanath.dev/api/diagram/image \
  -H "Content-Type: application/json" \
  -d '{"mermaid": "flowchart LR\n    A --> B --> C"}' \
  --output diagram.png
```

---

### GET /d/:id

Load a diagram from a short URL. Redirects to the full editor URL.

---

## JSON Schema

### Node Object

Nodes are the shapes in your diagram.

```json
{
  "id": "n1",
  "type": "rect",
  "label": "My Node",
  "x": 100,
  "y": 100,
  "width": 120,
  "height": 44,
  "color": "#7aa2f7",
  "outlineColor": "#414868",
  "fillStyle": "infill"
}
```

#### Required Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier for this node |
| `type` | string | Shape type (see below) |
| `x` | number | X position in pixels |
| `y` | number | Y position in pixels |

#### Optional Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `label` | string | `""` | Display text |
| `width` | number | varies | Width in pixels |
| `height` | number | varies | Height in pixels |
| `color` | hex | varies | Fill color. **Omit for no fill** |
| `outlineColor` | hex | `#414868` | Border/stroke color |
| `fillStyle` | string | `infill` | Fill pattern |

#### Node Types

| Type | Description | Default Size | Mermaid |
|------|-------------|--------------|---------|
| `rect` | Rectangle | 120 × 44 | `A[Label]` |
| `pill` | Rounded rectangle | 120 × 44 | `A(Label)` |
| `diamond` | Decision/condition | 80 × 60 | `A{Label}` |
| `circle` | Terminal/event | 80 × 80 | `A((Label))` |
| `container` | Group box (dashed) | 280 × 180 | `A[["Label"]]` |
| `text` | Free text | 100 × 24 | - |
| `line` | Standalone arrow | 100 × 0 | - |

#### Fill Styles

| Value | Description |
|-------|-------------|
| `outline` | Transparent fill, border only |
| `infill` | Solid color fill (default) |
| `stripe` | Diagonal stripe pattern |
| `grid` | Dot grid pattern |

**Important:** Do NOT use `"transparent"` as a color. Omit the `color` property instead.

#### Container Nodes

Containers create visual group boxes with dashed borders.

```json
{
  "id": "g1",
  "type": "container",
  "label": "My Group",
  "x": 50,
  "y": 50,
  "width": 300,
  "height": 200,
  "outlineColor": "#9ece6a"
}
```

**Tips:**
- Define containers BEFORE the nodes they contain (render order)
- Set `outlineColor` for visible border and label
- Omit `color` property (containers don't need fill)

#### Sequence Diagram Nodes

For sequence diagrams, nodes can have lifelines:

| Property | Type | Description |
|----------|------|-------------|
| `isParticipant` | boolean | Mark as sequence participant |
| `hasLifeline` | boolean | Show vertical dashed lifeline |
| `isActor` | boolean | Render as stick figure |

#### Class Diagram Nodes

For UML class diagrams:

| Property | Type | Description |
|----------|------|-------------|
| `isClass` | boolean | Render as UML class box |
| `properties` | string[] | Class properties (e.g., `["+name: string"]`) |
| `methods` | string[] | Class methods (e.g., `["+getName()"]`) |

---

### Connection Object

Connections are the arrows/lines between nodes.

```json
{
  "id": "c1",
  "from": "n1",
  "to": "n2",
  "label": "next",
  "color": "#414868",
  "lineStyle": "solid"
}
```

#### Required Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier |
| `from` | string | Source node ID |
| `to` | string | Target node ID |

#### Optional Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `label` | string | `""` | Edge label text |
| `color` | hex | `#414868` | Line color |
| `lineStyle` | string | `solid` | `solid` or `dashed` |
| `fromSide` | string | auto | Source edge: `top`, `right`, `bottom`, `left` |
| `toSide` | string | auto | Target edge |
| `fromIdx` | number | 1 | Connection point index on source side |
| `toIdx` | number | 1 | Connection point index on target side |

#### Sequence Diagram Messages

For sequence diagrams, connections become horizontal messages:

| Property | Type | Description |
|----------|------|-------------|
| `isSequenceMessage` | boolean | Render as horizontal message |
| `messageY` | number | Y position of the message line |
| `index` | number | Message order (vertical positioning) |
| `arrow` | string | Arrow style: `->>`, `-->>`, `-)`, `--)` |

---

## Color Palette

Tokyo Night Storm colors recommended:

| Name | Hex | Usage |
|------|-----|-------|
| Blue | `#7aa2f7` | Primary accent |
| Cyan | `#7dcfff` | Secondary accent |
| Green | `#9ece6a` | Success, start states |
| Red | `#f7768e` | Error, danger |
| Yellow | `#e0af68` | Warning, decisions |
| Magenta | `#bb9af7` | Special, highlights |
| Orange | `#ff9e64` | Attention, output |
| Border | `#414868` | Default borders |
| Background | `#24283b` | Canvas background |

---

## Complete Examples

### Simple Flowchart

```json
{
  "nodes": [
    {"id": "start", "type": "pill", "label": "Start", "x": 200, "y": 50, "color": "#9ece6a"},
    {"id": "process", "type": "rect", "label": "Process Data", "x": 170, "y": 130},
    {"id": "decision", "type": "diamond", "label": "Valid?", "x": 180, "y": 220, "color": "#e0af68"},
    {"id": "success", "type": "rect", "label": "Save", "x": 100, "y": 320},
    {"id": "error", "type": "rect", "label": "Log Error", "x": 260, "y": 320, "color": "#f7768e"},
    {"id": "end", "type": "pill", "label": "End", "x": 200, "y": 420}
  ],
  "connections": [
    {"id": "c1", "from": "start", "to": "process"},
    {"id": "c2", "from": "process", "to": "decision"},
    {"id": "c3", "from": "decision", "to": "success", "label": "Yes"},
    {"id": "c4", "from": "decision", "to": "error", "label": "No"},
    {"id": "c5", "from": "success", "to": "end"},
    {"id": "c6", "from": "error", "to": "end"}
  ]
}
```

### Architecture with Groups

```json
{
  "nodes": [
    {"id": "g1", "type": "container", "label": "Frontend", "x": 50, "y": 50, "width": 300, "height": 120, "outlineColor": "#9ece6a"},
    {"id": "n1", "type": "rect", "label": "React App", "x": 70, "y": 90},
    {"id": "n2", "type": "rect", "label": "Next.js", "x": 210, "y": 90},
    
    {"id": "g2", "type": "container", "label": "Backend", "x": 50, "y": 200, "width": 300, "height": 120, "outlineColor": "#7aa2f7"},
    {"id": "n3", "type": "rect", "label": "API Server", "x": 70, "y": 240},
    {"id": "n4", "type": "pill", "label": "Auth", "x": 210, "y": 240},
    
    {"id": "n5", "type": "circle", "label": "DB", "x": 400, "y": 240, "width": 60, "height": 60, "color": "#bb9af7"}
  ],
  "connections": [
    {"id": "c1", "from": "n1", "to": "n3"},
    {"id": "c2", "from": "n2", "to": "n3"},
    {"id": "c3", "from": "n3", "to": "n4"},
    {"id": "c4", "from": "n4", "to": "n5"}
  ]
}
```

### Sequence Diagram

```json
{
  "nodes": [
    {"id": "a", "type": "rect", "label": "Client", "x": 100, "y": 50, "isParticipant": true, "hasLifeline": true},
    {"id": "b", "type": "rect", "label": "Server", "x": 300, "y": 50, "isParticipant": true, "hasLifeline": true},
    {"id": "c", "type": "rect", "label": "Database", "x": 500, "y": 50, "isParticipant": true, "hasLifeline": true}
  ],
  "connections": [
    {"id": "m1", "from": "a", "to": "b", "label": "POST /login", "isSequenceMessage": true, "messageY": 150, "index": 0, "arrow": "->>"},
    {"id": "m2", "from": "b", "to": "c", "label": "SELECT user", "isSequenceMessage": true, "messageY": 200, "index": 1, "arrow": "->>"},
    {"id": "m3", "from": "c", "to": "b", "label": "user data", "isSequenceMessage": true, "messageY": 250, "index": 2, "arrow": "-->>"},
    {"id": "m4", "from": "b", "to": "a", "label": "200 OK + token", "isSequenceMessage": true, "messageY": 300, "index": 3, "arrow": "-->>"}
  ]
}
```

---

## Error Handling

The API returns standard HTTP status codes:

| Code | Description |
|------|-------------|
| 200 | Success |
| 400 | Bad request (invalid JSON or missing required fields) |
| 429 | Rate limit exceeded |
| 500 | Server error |

Error response format:
```json
{
  "error": "Description of what went wrong"
}
```

---

## Rate Limits

API requests are rate limited to **30 requests per minute per IP** using a sliding window.

When exceeded, you'll receive a `429` response:
```json
{
  "error": "Rate limit exceeded. Please try again later.",
  "retryAfter": 21
}
```

**Response headers:**
| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Maximum requests per window |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | Unix timestamp when the limit resets |
| `Retry-After` | Seconds to wait before retrying |

---

## See Also

- [LLM Integration](llm-integration.md) - AI/LLM-specific usage
- [Mermaid Import](mermaid.md) - Mermaid syntax reference
- [Examples](examples.md) - More diagram patterns
