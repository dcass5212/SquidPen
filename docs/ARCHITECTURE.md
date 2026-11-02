# Architecture

## Stack
- .NET 8, WPF, C# (nullable enabled)
- `CommunityToolkit.Mvvm` for view models
- `System.Text.Json` for settings
- Win32 interop via P/Invoke (no extra NuGet for it)

## Solution layout
```
SquidPen.sln
src/
  SquidPen/                 WPF app (net8.0-windows)
    App.xaml(.cs)           Startup, tray icon, single-instance mutex
    Overlay/                OverlayWindow (one per monitor), InkCanvas host
    Toolbar/                ToolbarWindow + ToolbarViewModel
    Services/
      HotkeyService         RegisterHotKey / WM_HOTKEY
      MonitorService        Enumerate screens, DPI per monitor
      SettingsService       Load/save JSON
      InputModeService      Draw vs click-through state
    Native/                 NativeMethods.cs (P/Invoke), constants
    Models/                 ToolKind, PenSettings, AppSettings
tests/
  SquidPen.Tests/           xUnit, view-model and service logic only
docs/
```

## Key mechanisms
- **Overlay window**: `WindowStyle=None`, `AllowsTransparency=True`, `Background=Transparent`, `Topmost=True`, `ShowInTaskbar=False`, sized to a monitor's bounds in device-independent units (respect per-monitor DPI via `PerMonitorV2` manifest).
- **Click-through toggle**: set/clear `WS_EX_TRANSPARENT` (plus `WS_EX_LAYERED`, `WS_EX_NOACTIVATE`) with `SetWindowLongPtr`. Draw mode clears `WS_EX_TRANSPARENT`.
- **Drawing**: `InkCanvas` with `DrawingAttributes`. Highlighter = `IsHighlighter=true`, flat tip. Eraser = `EditingMode.EraseByStroke`.
- **Hotkeys**: `RegisterHotKey` on a hidden message-only HWND source; works while another app has focus.
- **Toolbar**: separate topmost window so it stays interactive even in click-through mode; excluded from overlay input.
- **Undo/redo**: stack of stroke add/remove commands per overlay.

## Risks
- `AllowsTransparency=True` uses software rendering; keep the visual tree minimal to preserve latency.
- Mixed-DPI multi-monitor coordinates; test with different scale factors.
- Some elevated/fullscreen-exclusive apps can sit above the overlay.
