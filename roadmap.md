---
layout: page
title: Roadmap
---

Below is a rough roadmap for the future of Caliburn.Micro. This should be a living document that is routinely updated as priorities change.

It's been broken down into sections, some of which can be done in parallel and some sequentially. Each should have a rough overview of what's covered and any development notes around approach.

## Samples

There's a [`samples`][samples] branch which contains a restrucure of the samples projects. The main goals of these samples are:

1. Demonstrate initial setup on all supported platforms.
2. Demonstrate all major features on all supported platforms.
3. Show platform specific features on the framework such as *Key Binding in WPF*
4. Highlight *one off scenarios* such as *using a different IoC container* or *F#*.

What's left here is polish the new samples filling in the gaps and extracting the rough setup notes on the branch into usable documentation for the website.

## Templates

The setup samples above can potentially be used as the basis for project templates, allowing us to remove the rather useless `Caliburn.Micro.Start` package.

Ideally there would be some transformation script possible to take the samples and build project templates from them rather than maintain two different versions of a basic setup project.

From here a Visual Studio extension can be created to allow easy install of these templates.

## 3.1.0

`3.1.0` should be the next release of the framework which a rough list of features under the [milestone][3.1.0] on GitHub. This list needs to be revaluated to see whether they make sense giving the ongoing work in `4.0.0`.

## 4.0.0

`4.0.0` is shaping up to be a major release with a [lot of features][4.0.0] each building on top of the next. These include:

Dropping support for Siverlight 5, .NET 4.0 and Windows Phone 8 (Silveright) and potentially Windows Phone 8 (Windows Runtime). By dropping these underused platforms the code base can be streamlined to help support effort in the features below.

Updating the solution to use the new project build system where the core (currently PCL) can switch to targeting a .NET Standard contract. The individual platform projects may be able to be combined to a single multitarged project ensuring easier maitenance and nuget package creation.

Move the frameworks lifecycle interfaces such as `IActivate` and `IGuardClose` to be asynchronous. The goal will be to have the framework be *async first* everywhere it make sense. This will be a major breaking change.

## Website and documentation overhaul

A lot of the documentation was written by Rob quite a while ago as a series of blog posts with new pages added as other platforms came onboard.  It badly needs a restrucure to be more consistent.

[samples]: https://github.com/Caliburn-Micro/Caliburn.Micro/tree/samples
[3.1.0]: https://github.com/Caliburn-Micro/Caliburn.Micro/milestones/v3.1.0
[4.0.0]: https://github.com/Caliburn-Micro/Caliburn.Micro/milestones/v4.0.0