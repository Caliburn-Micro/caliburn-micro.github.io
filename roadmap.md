---
layout: page
title: Roadmap
---

Below is a rough roadmap for the future direction of Caliburn.Micro. This will be a living document that is routinely updated as time and priorities change.

The list has been broken down into sections, some of which can be done in parallel and some sequentially. Each will have a rough overview of what's covered and any development notes around approach.

## Templates

The setup samples above can potentially be used as the basis for project templates, allowing the removal the rather useless `Caliburn.Micro.Start` package.

Ideally there would be some transformation script possible to take the samples and build project templates from them rather than maintain two different versions of a basic setup project.

With the [dotnet/templating](templating) project that sits behind `dotet new` shows some promise as a way forward rather than using the current template projects and extensions in order to generate these templates from the projects the existing samples

Also adding a Caliburn.Micro extension to the [Windows Template Studio](wts) would be a welcome addition.

## 4.0.0

`4.0.0` is shaping up to be a major release with a [lot of features][4.0.0] each building on top of the next. These include:

Dropping support for Siverlight 5, .NET 4.0 and Windows Phone 8 (Silveright) and potentially Windows Phone 8 (Windows Runtime). By dropping these underused platforms the code base can be streamlined to help support effort in the features below.

Updating the solution to use the new project build system where the core (currently PCL) can switch to targeting a .NET Standard contract. The individual platform projects may be able to be combined to a single multitarged project ensuring easier maitenance and nuget package creation.

Move the frameworks lifecycle interfaces such as `IActivate` and `IGuardClose` to be asynchronous. The goal will be to have the framework be *async first* everywhere it makes sense. This will be a major breaking change.

`4.0.0` will be done on a new branch given there will probably be an extended beta for it.

## Intrastructure

Also required for the long term health of the project is some investment in the general infrastrucutre that makes an open source project healthy. These inlucde

- Automated CI on all commits and PR's.
- Automated UI tests on the features projects.
- More exhaustive unit tests.
- Stickers!

## Website and documentation overhaul

A lot of the documentation was written by Rob quite a while ago as a series of blog posts with new pages added as other platforms came onboard.  It badly needs a restrucure to be more consistent.

[3.1.0]: https://github.com/Caliburn-Micro/Caliburn.Micro/milestones/v3.1.0
[4.0.0]: https://github.com/Caliburn-Micro/Caliburn.Micro/milestones/v4.0.0
[templating]: https://github.com/dotnet/templating/
[wts]: https://github.com/Microsoft/WindowsTemplateStudio