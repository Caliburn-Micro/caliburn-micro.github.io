# 🚀 Caliburn.Micro V5 Release Notes

## 🆕 New Platform Support
- **Avalonia UI (Beta)**
  - Introduced `Caliburn.Micro.Avalonia` adapters
  - Added extensive Avalonia sample files.

- **.NET MAUI (Beta)**
  - New `Caliburn.Micro.Maui` package for MAUI integration.

- **WinUI 3**
  - Added adapters and sample applications under the `Features.WinUI3` project.

---

## 📦 Project & Package Updates

- Published new NuGet packages for:
  - Avalonia
  - MAUI
  - WinUI 3
- Reorganized sample and project files to support new platforms.

---

## 🌐 Multi-Platform Sample & Feature Enhancements

- Expanded sample projects for:
  - WPF, UWP, Xamarin
  - .NET Core, .NET 5–9
  - Avalonia, WinUI 3
- Samples now demonstrate:
  - New features
  - Cross-platform patterns
  - Platform-specific implementations

---

## 🧹 Code Modernization & API Improvements

- Refactored view models and helpers:
  - Replaced public fields with private backing fields
  - Improved property change notifications
- Enhanced `async/await` support in:
  - Bootstrappers
  - View models
  - Navigation
  - Coroutine execution
- Updated project targets:
  - WPF: .NET 4.8 → .NET 9
  - UWP: Latest SDKs

---

## 🎨 Platform-Specific UI/UX Enhancements

- Updated XAML/UI files for:
  - Modern UI idioms
  - Better design-time data
  - Platform conventions
- Added new visual states:
  - `ShowLoading` vs `Loading` for coroutines/progress bars
- Improved navigation and menu views across platforms.

---

## 🔧 CI/CD & Build Infrastructure

- Addressed code quality issues:
  - Removed redundant assignments
  - Simplified boolean expressions
  - Fixed variable shadowing
- Refactored loops to concise LINQ expressions (e.g., `.Where()`).
- Resolved deprecated usage (e.g., `StackLayout` in MAUI).
- Fixed build warnings and updated dependencies.

---

I would like to thank @K4PS3, @Yinimi, @darxis, @ravibaghel, @Lehonti, @Stannieman, @erik-hooper, @danwalmsley, @KasperSK , @inforithmics, @emanonhero for your contributions to this release.
