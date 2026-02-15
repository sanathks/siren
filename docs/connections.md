# Connections

Connections link nodes together with arrows. Draph supports smart routing, labels, and multiple line styles.

## Creating Connections

### Method 1: Drag from Connection Points
1. Switch to Select mode (`V`)
2. Hover over a node - connection points appear (small circles on each edge)
3. Click and drag from a connection point
4. Drop on another node's connection point

### Method 2: Quick Connect (New Node)
1. Drag from a connection point to empty canvas space
2. A picker appears with node type options
3. Click a shape - the new node is created and connected

<!-- TODO: Add screenshot of quick connect picker -->

## Connection Properties

| Property | Type | Description |
|----------|------|-------------|
| `id` | string | Unique identifier |
| `from` | string | Source node ID |
| `to` | string | Target node ID |
| `fromSide` | string | Source edge: `top`, `right`, `bottom`, `left` |
| `toSide` | string | Target edge |
| `label` | string | Text label on the connection |
| `color` | hex | Line color |
| `lineStyle` | string | `solid` or `dashed` |

## Line Routing

### Rounded (Default)
Connections curve smoothly around corners. Best for most diagrams.

Toggle in the top toolbar or set globally.

### Straight
Direct point-to-point connections with sharp corners.

## Styling Connections

Select a connection to see its toolbar:

<!-- TODO: Add screenshot of connection toolbar -->

Options:
- **Color** - Line color from palette
- **Solid/Dashed** - Line style toggle
- **Delete** - Remove connection

## Labels

Add labels to connections for flow descriptions, conditions, etc.

### Adding a Label
1. Select the connection
2. Double-click or use the label field in the toolbar
3. Type your label text

### Label Positioning
Labels appear at the midpoint of the connection. For decision flows, common labels include:
- `Yes` / `No`
- `True` / `False`
- `Success` / `Failure`

## Sequence Diagram Messages

For sequence diagrams, connections become horizontal messages between lifelines.

### Message Properties

| Property | Type | Description |
|----------|------|-------------|
| `isSequenceMessage` | boolean | Render as horizontal message |
| `messageY` | number | Y position of the message line |
| `index` | number | Message order (vertical positioning) |
| `arrow` | string | Arrow style |

### Arrow Styles

| Syntax | Description |
|--------|-------------|
| `->>` | Solid arrow, filled head (sync) |
| `-->>` | Dashed arrow, filled head (response) |
| `-)` | Solid arrow, open head (async) |
| `--)` | Dashed arrow, open head (async response) |

## Tips

- **Multi-point connections:** Connections automatically route around nodes
- **Reconnect:** Drag a connection endpoint to move it to a different node
- **Parallel connections:** Multiple connections between the same nodes are supported
- **Self-reference:** Connect a node to itself for loops/recursion
