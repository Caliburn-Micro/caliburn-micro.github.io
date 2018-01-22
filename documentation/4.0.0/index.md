---
layout: page
title: Migrating to 4.0.0
---

## Supported Platforms

One of the guiding philosophies we've had about supported platforms for Caliburn.Micro is to support the platforms that are supported by the latest version of Visual Studio.

Given this we've dropped support for the following platforms.

- Silverlight 5
- Windows Phone 8 (Silverlight)
- Windows Phone 8 (Windows Runtime)
- Windows 8.1

We'll also be discontining support for .NET 4.0 for WPF.

All of these platforms are still supported in the 3.x releases.

## Changes

Below is the changes included in 4.0.0 release (so far).

### Assemblies

The platforms targeted have been changed to support the new .NET ecosystem. These include:

- Caliburn.Micro.Core targets .NET Standard 1.0.
- Caliburn.Micro.Platform.Xamarin.Forms targets .NET Standard 1.4.
- Caliburn.Micro.Platform is consistently named across all platforms (previosly UWP includes the platform name).


### Event Aggregator

The Event Aggregator is the first class to go through major breaking changes, this brings it into a "async first" approach that is the main reason for 4.0.0.

The full changes are available in the [event aggregator migration documentation](./documentation/4.0.0/event-aggregator).