# SquidPen — Product Requirements

A Windows screen-annotation tool in the style of Epic Pen: draw over anything on screen, then click through to work normally.

## Goals
- Draw on top of any app, on any monitor, with near-zero latency.
- Instantly switch between **Draw mode** (overlay captures input) and **Click-through mode** (overlay is transparent to the mouse).
- Stay out of the way: tray icon, small floating toolbar, global hotkeys.

## v1 scope
| Feature | Notes |
|---|---|
| Pen | Freehand, adjustable size, color |
| Highlighter | Semi-transparent, wider, flat tip |
| Eraser | Stroke eraser |
| Colors / sizes | Small preset palette + size slider |
| Undo / Redo / Clear | Per-monitor stroke history |
| Draw / click-through toggle | Toolbar button and global hotkey |
| Floating toolbar | Draggable, always on top, collapsible |
| Multi-monitor | One overlay window per monitor |
| Global hotkeys | Toggle draw, clear, undo, show/hide, quit |
| Tray icon | Show/hide, settings, exit |
| Settings persistence | JSON in `%AppData%\SquidPen` |

## Non-goals (v1)
Shapes, text, arrows, screenshot export, whiteboard mode, cloud sync, touch/pen-pressure tuning. See [ROADMAP.md](ROADMAP.md).

## Success criteria
- Strokes render with no visible lag on a 144 Hz display.
- Click-through mode lets every click reach the app underneath.
- Toolbar never gets covered by the overlay or lost off-screen.
- Works across monitors with mixed DPI scaling.
