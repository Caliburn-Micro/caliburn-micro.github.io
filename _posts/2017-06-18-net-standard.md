---
layout: post
title: Caliburn.Micro 4.0.0 and .NET Standard
---

I'm pleased to say that development on Caliburn.Micro 4.0.0 has started on the [branch `dev/4.0.0`][branch]. One of the first things I've done is transition the whole solution across to Visual Studio 2017, .NET Standard and making use of multi-targeting.

As it stands right now `Caliburn.Micro.Core` which was previously a PCL will now target .NET Standard 1.0 which gives a great range of supported platforms. `Caliburn.Micro.Xamarin.Forms` targets the .NET Standard 1.6.

I've also taken advantage of the new multi-targeting approach allowed in Visual Studio 2017. Previously for all the `Caliburn.Micro.Platform` projects I had all the source in one folder along with around six to eight project files. Each project simply included or excluded the files it needed with compiler directives if it needed to exlude only a little code.

Switching to multi-targeting doesn't really change this approach except we can shift from all those project files to simply one. Along with wild card inclusions platform specific code only needs to be dropped in the right folder for it to be used correctly.

There's a lot of work to be done before I consider doing a release but if you're looking to try it out or see how a project that targets **a lot** of platforms looks when making this change feel free to check out [the code][branch].

[branch]: https://github.com/Caliburn-Micro/Caliburn.Micro/tree/dev/4.0.0
