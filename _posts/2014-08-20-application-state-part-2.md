---
layout: post
title: Application State, part 2
---

In the [previous post][part1] on application state we defined a whole of terms on how we can consider state and some of the user expectations how how our applications should respect that state. This this post I want to discuss what's already in Caliburn.Micro for these problems in particular for the **Windows Phone (Silverlight) platform**.

## Phone Application Frame

Applications in Windows Phone (Silverlight) are primarily **view first**, the root content of the app's Window is a `PhoneApplicationFrame` that Caliburn.Micro wraps in an `INavigationService`. As the app navigates to new `PhoneApplicationPage`'s our navigation service locates the appropriate view model for the view and activates it.

The `PhoneApplicationFrame` maintains an internal cache of the views that lines up with the back and forward stacks. This means as you navigate back you're returning the exact same view that you left earlier.

This behaviour gives us our desired outcomes for View State out of the box as that state is automatically preserved on the forward and back stacks. You can see this behaviour in existing apps by scrolling down a list, navigating forward and then back. All View State such as list scroll position should be maintained correctly.

The `INavigationService` implementation `FrameAdapter` takes care not to overwrite existing view models. Because views retain a reference to their view model (through DataContext) then these cached views act as a cache for the view model as well. Navigation back to a cached view causes an activation of the existing view model rather than creation.

In summary the view caching behaviour of `PhoneApplicationFrame` coupled with Caliburn.Micro's `INavigationService` give us the desired behaviour right out of the box.

## App Activation

[App activation and deactivation][acc] has changed a bit over the course of Windows Phone versions with the introduction of [fast app resume][far], but the basics remain the same.

Caliburn.Micro has some built in features to let you preserve some parts of a view model during the *tombstoning* of an application through the `StorageHandler<T>` class. This implements the desired behaviour from [part 1][part1], you can read more about it in the [documentation][docs].

## Where to from here?
In the next post I'll discuss how we can go about addressing these same behaviours in the Windows RT platform.

[part1]: /announcements/application-state-part-1/
[acc]: http://msdn.microsoft.com/en-us/library/windows/apps/ff817008(v=vs.105).aspx
[far]: http://msdn.microsoft.com/en-us/library/windows/apps/jj735579(v=vs.105).aspx
[docs]: /documentation/windows-phone