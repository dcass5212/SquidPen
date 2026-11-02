# SquidPen

Windows screen-annotation overlay (Epic Pen style) in WPF / .NET 8. See `docs/PRD.md`, `docs/ARCHITECTURE.md`, `docs/ROADMAP.md`.

## Commands
- Build: `dotnet build SquidPen.sln`
- Run: `dotnet run --project src/SquidPen`
- Test: `dotnet test`

## Conventions
- C# 12, nullable enabled, file-scoped namespaces.
- MVVM with `CommunityToolkit.Mvvm` (`[ObservableProperty]`, `[RelayCommand]`). Keep code-behind to view-only concerns (window styles, input plumbing).
- All Win32 P/Invoke lives in `src/SquidPen/Native/`. No P/Invoke elsewhere.
- No extra UI libraries; plain WPF controls and styles.
- Unit-test view models and services; don't try to unit-test windows.

## Gotchas
- The overlay uses `AllowsTransparency=True`: keep its visual tree tiny for drawing latency.
- Click-through is `WS_EX_TRANSPARENT` toggled at runtime; the toolbar is a separate window so it stays clickable.
- App must declare PerMonitorV2 DPI awareness; verify coordinates on mixed-DPI setups.
- Platform is Windows only (`net8.0-windows`); shell is PowerShell.

## Workflow
Work milestone by milestone from `docs/ROADMAP.md`; tick items off as they land and update the docs when design decisions change.
