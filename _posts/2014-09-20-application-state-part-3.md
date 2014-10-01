---
layout: post
title: Application State, part 3
---

In my [previous post][part2] I talked about how Caliburn.Micro deals with state on Windows Phone (Silverlight). Today I want to deal with the differences in WinRT (Windows 8 and Windows Phone 8.1) and how it impacts things. I want to finish with some potential solutions and open it up to discussion.

## WinRT Frame View Caching

I've already talked about how the view caching behaviour of `PhoneApplicationFrame` on Windows Phone (Silverlight) enables almost all the view state and view model state outcomes we're expecting.

However the `Frame` control in WinRT has a largely different behaviour which can trip a lot of developers (including myself) up.

The `Page` control in WinRT has a property `NavigationCacheMode` which by default is set to `Disabled`. This means on every navigation a new view is created, including navigating back to a view you were previously on. This means we instantly lose any view and view model state upon navigation. You can see this happening in a brand new application where you scroll down a list, select and item to take you to another page. Upon navigating back you'll notice the `ScrollViewer` will be reset back to the start. Not desirable at all.

So what happens when we set `NavigationCacheMode` to `Enabled`?. Initially it looks like we get back to our desired behaviour, navigating back to our previous views restores a cached view rather than a new one. This view held our view model so view and view model state are retained.

The problems surface when you navigate forward again. For our first example lets consider an application with two views, Product List and Product Details. If the user taps a product and the `Frame` navigates from Product List to Product Details. A new Product Details view and view model are created as expected, navigating back to Product List returns to the cached view / view model pairing with retained state.

Where it all goes wrong however is when the user taps a second product and the `Frame` navigates to Product Details with a different parameter. The `Frame` will reuse the cached view from the previous product. This means you'll have the same view model from the previous product details as before as well. Unless you're careful with your views the user will see a flash of the previous product details before the new product data loads.

This gets even worse if your application can potentially have the same view in the stack multiple times (for instance a file browser application may have a Browse Folder view). In this case with caching on then navigating backward or forward will be to the view / view model you were previously on and by loading the new state means you're wiping out the state for the previous view so you get no benefit at all.

To summarise, **the caching behaviour of the `Frame` respects neither the type of navigation (Back, Forward, New) nor the parameter of the navigation**. This is no where near our desired behaviour and puts a lot more onus on us as application developers to retain the correct state.

## WinRT Frame State 

`Frame` does have one set of useful methods, `GetNavigationState` and `SetNavigationState`. `GetNavigationState` returns a serialized representation of the forward and back stacks at the time of calling. It's a custom format that you're not supposed to understand, although some people have posted articles on manipulating it. One caveat is that this custom format can't handle complex types when used as a navigation parameter and will throw an exception if they're used. Simple types such as string and integer are fine

If you're using `INavigationService.UriFor<>` with simple types then you shouldn't run into this problem.

We can use these methods to save frame state on suspension of the application, so if the application is terminated when can restore it to where the user was previously.

This functionality already exists in Caliburn.Micro and is usable through the methods `INavigationService.SuspendState` and `INavigationService.ResumeState`. These take the current frame state and store it in `ApplicationData.Current.LocalSettings` to be restored if required.

You can see a sample of this in the [Windows 8.1 Sample][frame-state].

## Potential Solutions

### Selective Caching

For the Product List / Product Details example above we can get our desired behaviour by only enabling `NavigationCacheMode` on the Product List view and not Product Details. This only works because we only have two levels of view, if for instance Product Details navigated forward to say Product Photo Gallery then this isn't a plausible solution as we'll want to enable caching on Product Details. We then run into the problems highlighted above.

For our File Browser example it makes sense to have `NavigationCacheMode` set to `Disabled` as we're unable to retain any state at all given that it's wiped out by the next view model on the stack. This doesn't mean we get any of our desired behaviour, just lessen the amount of work we need to do to avoid a sub-optimal user experience.

### Suspension Manager

The Visual Studio templates for Windows 8 and Windows Phone 8.1 come with a class `SuspensionManager` than can help with some of these problems. First it gives us the ability to save the frame state of an application to allow for suspension and resumption of the application. It also maintains a "stack" of state for you. Because the default templates have a rather weird implementation of MVVM then you can use this state bucket for both view and view model state. 

This avoids the requirement of enabling caching on the Frame, but pushes the responsibility back to you as a developer to determine which view / view model state your want to retain (including minor things like scroll position). This can get awkward when the `ScrollViewer` is built into a `GridView` and you can't restore the scroll position until the layout is updated and so on. It's a passable solution, but becomes onerous quickly and prone to mistakes.

### Custom Frame

It's occurred to me a number of times while ranting about this problem to create my own Frame control that acts in the same way as `PhoneApplicationFrame` from Windows Phone (Silverlight). It's certainly a possibility, but in my opinion on out of the scope of Caliburn.Micro in mandating control usage. Particular care would need to be paid around performance in caching views.

This is the best solution for retaining view state on `Frame` navigation.

I still have hope some of this will be resolved in Windows 10.

### Maintaining a View Model stack

An idea I've been experimenting with is having the Caliburn.Micro navigation service maintaining internal stacks of view models mapping one to one with the forward and back stacks of the `Frame`. On navigating backwards a new view is created but an existing view model is reused. This would give us view model state retained over navigation. It's working well in the experiments, but is running afoul of a [weird xaml bug and cached views][xaml] it would not help in the retaining of view state.

### StorageHandler<> for WinRT

As quickly discussed in the [previous post][part2] Caliburn.Micro has a solution to retain view model state on the current view model when the application is suspended through a class named `StorageHander<>` (you can read more about it in the [documentation][docs]). Porting this to WinRT is certainly a possibility, I have had some feedback the current API isn't to everyone's liking and it's hard to gauge demand for this feature.

What this would do is allow you to record parts of your current view model's state that would be stored somewhere in `ApplicationData.Current.LocalSettings` for when the application is resumed. This would most likely be wired into the current methods on Navigation Service.

## Get in touch

I've created an [issue][issue] on GitHub to facilitate discussion around some of these issues and would invite feedback. Let us know what you think.


[part1]: /announcements/application-state-part-1/
[part2]: /announcements/application-state-part-2/
[frame-state]: /announcements/application-state-part-2/
[xaml]: https://github.com/Caliburn-Micro/Caliburn.Micro/issues/1
[docs]: /documentation/windows-phone
[issue]: https://github.com/Caliburn-Micro/Caliburn.Micro/issues/95