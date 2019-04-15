---
layout: page
title: Screen's and view model lifecycle
---

## Screen
- `OnInitialize` becomes `OnInitializeAsync`
- `OnActivate` becomes `OnActivateAsync`
- `OnDeactivate` becomes `OnDeactivateAsync` 

When they are called in view model life cycle has not changes. 

If you current implementation doesn't use an any async code then simply adding the following to each will suffice.

``` csharp
return Task.CompletedTask;
```

If your methods were `async void` then simply fixing the method signature will be enough. Be aware that this fixes the bug where `OnActivate` would be called as soon as you hit the `await` in `OnInitialize` where now it will wait till it is fully completed.

## IGuardClose

`CanClose` no longer has a `bool` callback, but returns a `Task<bool>` that better enables modern UI frameworks that use async dialogs. Now instead of calling the callback with whether the view model should close we can how simply return it from the method.