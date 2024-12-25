---
layout: page
title: Roadmap
---

Below is a rough roadmap for the future direction of Caliburn.Micro. This will be a living document that is routinely updated as time and priorities change.

The list has been broken down into sections, some of which can be done in parallel and some sequentially. Each will have a rough overview of what's covered and any development notes around approach.


## 5.0.0

`5.0.0` will include support for .net Maui, .net 8 & 9, WinUI3, and AvaloniaUI
packages available on github nuget feed.

## 6.0.0

#### Upcoming Features
- **Support for Uno Platform:** V6 will introduce support for the Uno Platform, enhancing cross-platform development capabilities.
- **Xamarin Version to be Removed:** Due to Microsoft discontinuing support for Xamarin, V6 will no longer include a Xamarin Forms version.

#### Performance Improvements
- **Enhanced performance:** performance improvements for Caliburn Micro, optimizing both speed and efficiency.


### Caliburn.Micro Extensions Project

- **Project Overview:**
  - This project will provide templates for Caliburn.Micro.
  - The release will include a code generator to simplify `INotifyPropertyChanged`.

- **IValue Converters:**
  - Bool to Visibility converter for all platforms, not just UWP.
  - URL to Image converter.
  - Decimal to String converter for custom currency formatting.
  - DateTime to String converter for custom DateTime formatting.



## Website and documentation overhaul

A lot of the documentation was written by Rob quite a while ago as a series of blog posts with new pages added as other platforms came onboard. It badly needs a restructure to be more consistent.

[4.0.0]: https://github.com/Caliburn-Micro/Caliburn.Micro/milestones/v4.0.0
[templating]: https://github.com/dotnet/templating/
[wts]: https://github.com/Microsoft/WindowsTemplateStudio
