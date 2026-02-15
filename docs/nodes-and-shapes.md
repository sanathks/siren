# Nodes and Shapes

Draph supports seven node types, each suited for different purposes.

## Node Types

### Rectangle (`rect`)
The standard node for most diagrams. Use for processes, steps, actions.

- **Default size:** 120 × 44 px
- **Keyboard:** `1`
- **Mermaid:** `A[Label]`

### Pill (`pill`)
Rounded rectangle. Often used for start/end states or softer visual style.

- **Default size:** 120 × 44 px
- **Keyboard:** `2`
- **Mermaid:** `A(Label)`

### Diamond (`diamond`)
Decision or condition node. Classic flowchart "if/else" shape.

- **Default size:** 80 × 60 px
- **Keyboard:** `3`
- **Mermaid:** `A{Label}`

### Circle (`circle`)
Terminal, event, or state node. Good for start/end points.

- **Default size:** 80 × 80 px
- **Keyboard:** `4`
- **Mermaid:** `A((Label))`

### Container (`container`)
A dashed-border group box for visually organizing related nodes. The label appears in the top-left corner.

- **Default size:** 280 × 180 px
- **Keyboard:** `5`
- **Mermaid:** `A[["Label"]]`

**Tip:** Create containers *before* the nodes they contain, so they render behind.

### Text (`text`)
Free-form text annotation. No border, no background - just text.

- **Default size:** 100 × 24 px
- **Keyboard:** `6`

### Arrow/Line (`line`)
Standalone arrow or line, not connected to nodes. Useful for annotations or custom layouts.

- **Keyboard:** `7`
- **Styles:** Solid or dashed

## Node Properties

Every node has these properties:

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier (auto-generated) |
| `type` | string | `rect`, `pill`, `diamond`, `circle`, `container`, `text`, `line` |
| `label` | string | Display text |
| `x` | number | X position (pixels) |
| `y` | number | Y position (pixels) |
| `width` | number | Width in pixels |
| `height` | number | Height in pixels |
| `color` | hex | Fill color (omit for no fill) |
| `outlineColor` | hex | Border/stroke color |
| `fillStyle` | string | Fill pattern |

### Fill Styles

| Style | Description |
|-------|-------------|
| `outline` | Transparent fill, border only |
| `infill` | Solid color fill (default) |
| `stripe` | Diagonal stripe pattern |
| `grid` | Dot grid pattern |

**Note:** Use `fillStyle: "outline"` for transparent nodes. Do NOT set `color: "transparent"`.

## Styling Nodes

### Using the Floating Toolbar
Select a node to see the floating toolbar:

<!-- TODO: Add screenshot of floating toolbar -->

From left to right:
1. **Shape changer** - Switch between node types
2. **Fill style** - Outline, infill, stripe, grid
3. **Fill color** - Color palette
4. **Border color** - Stroke color
5. **Delete** - Remove node
6. **Close** - Deselect

### Color Palette (Tokyo Night Storm)

| Color | Hex | Suggested Use |
|-------|-----|---------------|
| Blue | `#7aa2f7` | Primary accent |
| Cyan | `#7dcfff` | Secondary accent |
| Green | `#9ece6a` | Success, start states |
| Red | `#f7768e` | Error, danger |
| Yellow | `#e0af68` | Warning, decisions |
| Magenta | `#bb9af7` | Special, highlights |
| Orange | `#ff9e64` | Attention, output |
| Gray | `#414868` | Borders, muted |

## Special Node Features

### Sequence Diagram Participants
For sequence diagrams, nodes can have lifelines:

| Property | Type | Description |
|----------|------|-------------|
| `isParticipant` | boolean | Mark as sequence participant |
| `hasLifeline` | boolean | Show vertical dashed line below |
| `isActor` | boolean | Render as stick figure |

### Class Diagram Classes
For UML class diagrams:

| Property | Type | Description |
|----------|------|-------------|
| `isClass` | boolean | Render as UML class box |
| `properties` | string[] | List of properties |
| `methods` | string[] | List of methods |

Visibility prefixes: `+` public, `-` private, `#` protected, `~` package

Example:
```json
{
  "id": "user",
  "type": "rect",
  "label": "User",
  "x": 100,
  "y": 100,
  "width": 150,
  "height": 120,
  "isClass": true,
  "properties": ["+name: string", "-password: string"],
  "methods": ["+login()", "+logout()"]
}
```

## Editing Nodes

- **Move** - Drag the node
- **Resize** - Drag the corner handles
- **Edit label** - Double-click the node
- **Delete** - Select and press `Delete`
- **Duplicate** - `Ctrl+C`, `Ctrl+V`
