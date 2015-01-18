---
layout: page
title: Migrating Projects to 2.0.0
---

### Why are there changes?

Caliburn.Micro 2.0.0 reflects a large change to the internal structure of the project. By splitting the code into two components (a portable assembly, and a number of platform specific assemblies) we better allow you to create code that's dependent upon Caliburn.Micro in Portable Class libraries.

This change does create some breaking API changes, and as well we used the move 2.0.0 to clean up some of API to bring it more into line.

This document will try to outline all the changes you may see updating a project to 2.0.0. It's currently not exhaustive and will be amended as required.

### Breaking Changes

### All Platforms

 - `EventAggregator.Publish` now takes an action to marshal the event. Use `EventAggregator.PublishOnUIThread` for the existing behaviour.
 - XAML namespace references to Caliburn.Micro for functionality such as `Message.Attach` should be renamed from `clr-namespace:Caliburn.Micro;assembly=Caliburn.Micro` to `clr-namespace:Caliburn.Micro;assembly=Caliburn.Micro.Platform`. On WPF if you're using the `xmlns:cm="http://www.caliburnproject.org"` syntax then there's no change. On WinRT platforms (Windows 8, Windows 8.1 and Windows Phone 8.1) the ` xmlns:cm="using:Caliburn.Micro"` syntax will continue to work.
 - `IResult` uses `CoroutineExecutionContext` instead of `ActionExecutionContext`.
 - `BootstrapperBase.Start` is now called `BootstrapperBase.Initialize`
 - `BootstrapperBase.Initialize` and `CaliburnApplication.Initialize` is now NOT automatically called before the `Display*` methods. If you need it to be then call `Initialise` from the constructor. 
 - `BindableCollection<T>` move operations aren't automatically dispatched to the UI thread.
 - `Bind.Model="some-key-string"` is obsolete and should not be used.

### WPF/SL5

 - `Bootstrapper<T>` has been removed. Use `BootstrapperBase` and override `OnStartup` with a call to `DisplayRootViewFor<T>()` instead.

#### Windows Phone

 - `PhoneBootstrapper` is now called `PhoneBootstrapperBase`.



