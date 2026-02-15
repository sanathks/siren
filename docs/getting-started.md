# Getting Started with Draph

Draph is a visual diagram editor that runs entirely in your browser. No account needed, no installation required.

## Opening Draph

**Online:** Visit [draph.sanath.dev](https://draph.sanath.dev)

**Locally:** 
```bash
git clone https://github.com/sanathks/draph.git
cd draph
npx serve
# Or just open index.html directly
```

## The Interface

<!-- TODO: Add screenshot of main interface -->

### Top Toolbar
- **Logo** - Click to reset/new diagram
- **Line Style** - Toggle between rounded and straight connectors
- **Export** - Download as PNG or SVG

### Left Toolbar
The vertical toolbar on the left contains your drawing tools:

| Tool | Key | Description |
|------|-----|-------------|
| Select | `V` | Select, move, and edit nodes |
| Rectangle | `1` | Standard rectangular node |
| Pill | `2` | Rounded rectangle |
| Diamond | `3` | Decision/condition shape |
| Circle | `4` | Terminal/event shape |
| Container | `5` | Dashed group box for organizing |
| Text | `6` | Free text annotation |
| Arrow | `7` | Standalone arrow/line |
| Auto-arrange | `A` | Automatically layout nodes |
| Mermaid | `M` | Import from Mermaid syntax |
| Grid | `G` | Toggle snap-to-grid |

### Canvas
The main drawing area. You can:
- **Pan** - Click and drag on empty space (or middle-mouse)
- **Zoom** - Scroll wheel
- **Select** - Click on nodes or drag a selection box

## Creating Your First Diagram

### 1. Add Nodes
Click a shape tool (or press `1`-`7`), then click and drag on the canvas to create a node.

### 2. Connect Nodes
1. Switch to Select mode (`V`)
2. Hover over a node to see connection points (small circles on edges)
3. Click and drag from a connection point to another node
4. Or drag to empty space - a picker appears to choose the target node type

### 3. Edit Labels
Double-click any node to edit its label. Press Enter or click away to confirm.

### 4. Style Your Diagram
Select a node to see the floating toolbar with options for:
- Shape type
- Fill style (outline, solid, striped, grid)
- Fill color
- Border color

### 5. Share or Export
- **Share URL** - Your diagram is encoded in the URL. Just copy and share it.
- **Export** - Click Export in the top toolbar to download PNG or SVG.

## Tips

- **Undo/Redo** - `Ctrl+Z` / `Ctrl+Y`
- **Copy/Paste** - `Ctrl+C` / `Ctrl+V` (works across browser tabs!)
- **Delete** - Select and press `Delete` or `Backspace`
- **Multi-select** - Drag a selection box, or `Ctrl+Click` to add to selection
- **Quick connect** - Drop a connection on empty canvas to pick node type instantly

## Next Steps

- [Nodes and Shapes](nodes-and-shapes.md) - Deep dive into all node types
- [Connections](connections.md) - Advanced connection options
- [Keyboard Shortcuts](keyboard-shortcuts.md) - Full reference
- [Mermaid Import](mermaid.md) - Use Mermaid syntax
