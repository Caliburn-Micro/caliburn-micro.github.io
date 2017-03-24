---
layout: post
title: New Samples
---

Recently I've gone through an effort to rebuild and reogranize the samples for Caliburn.Micro. There a number of goals:

1. Have consistent setup examples for all supported platforms.
2. Have examples of most framework features on all supported platforms.
3. Highlight novel scenarios for using Calbirn.Micro.

These projects can also form the foundation for anyone looking to repro a small issue with ease.

## [Setup](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup)

An example of a barebones setup for each supported platform, just beyond the bare minimum (includes a container and dependency injection.) These include:

- [Windows 10 / UWP](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.UWP)
- [WPF](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.WPF)
- [Xamarin.iOS](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.iOS)
- [Xamarin.Android](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samplessetup/Setup.Android)
- [Xamarin.Forms (iOS, Android, Windows Phone, Portable Class Library)](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.Forms)
- [Windows 8.1](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.Windows.Runtime)
- [Windows Phone 8.1 / Windows Runtime](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.WindowsPhone.Windows.Runtime)
- [Windows Phone 8.1 / Silverlight](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.WindowsPhone.Silverlight)
- [Silverlight](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/setup/Setup.Silverlight)

## [Features](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/features)

A solution that demonstrates the usage of most major framework features across all the supported platforms (showing some of the inevitable platform discrepancies). Also this solution provides an example of using a Shared project to share code (in this case the view models) across mulitple platforms. The features covered include:

- Binding Conventions
- Action Conventions
- Coroutines
- Dispatching to the UI thread
- Event Aggregation
- Design Time Conventions
- Conductors and Composition

## [Scenarios](https://github.com/Caliburn-Micro/Caliburn.Micro/tree/master/samples/scenarios)

A collection of solutions highlighting one off scenarios that may or not apply to multiple platforms. They're such that demonstrating them on all platforms would not add extra value. These include:

- Switching IoC containers to something like Autofac
- The use of F#
- Customising the framework
