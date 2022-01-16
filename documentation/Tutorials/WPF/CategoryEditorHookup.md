---
layout: page
title: Hook up the CategoryViewModel in the Shell
---

[Contents](Contents) [Previous](Categories1) [Next](CategoryView)

## Hook up the CategoryViewModel in the Shell

This is a very short step, but we now can hook the new Category UserControl into the Shell. We need two things to do:

+ Create a method to invoke the category editor.
+ On application start activate the CategoryViewModel and CategoryView in  the shell.

### Invoke the category editor

In the ``ShellViewModel`` we create a new method:

```csharp
    public Task EditCategories()
      {
      var viewmodel = IoC.Get<CategoryViewModel>();
      return ActivateItemAsync(viewmodel, new CancellationToken());
      }
```

**IoC** stands for Inversion of Control. In a later section, you will see how you can set up a container. Here the default implementation is used. If you have a close look at the internals of the ``BootstrapperBase`` class, you may see it does not use a container, but in the end calls the .Net  [Activator](https://docs.microsoft.com/en-us/dotnet/api/system.activator?view=net-5.0#examples) class.

The second line calls ``ActivateItemAsync``, which is a Caliburn.Micro method that activates a user control in a contentcontrol ``<ContentControl x:Name="ActiveItem" Margin="20"/>`` in the ``ShellView`` Make sure to use ``ActiveItem`` as the name for the ``ContentControl`` because Caliburn.Micro needs this naming convention.

A final comment, note an asynchronous invocation is used.

### Activate ``EditCategories()`` on application startup

Once the ``ShellView`` is loaded, it is possible to show the contents of the contentcontrol. Thus, you can do it by adding this asynchronous method th the ``ShellViewModel``

```csharp
    protected async override void OnViewLoaded(object view)
      {
      base.OnViewLoaded(view);
      await EditCategories();
      }

```

One thing you need to remember: Do NOT use ``protected async override Task OnViewLoaded(object view)``
as you might think. The ``Task`` as return type will not work in this situation.

### Summary

The ShellViewModel may now look like this:

```csharp
using System.Threading;
using System.Threading.Tasks;

namespace Caliburn.Micro.Tutorial.Wpf.ViewModels
  {
  public class ShellViewModel : Conductor<object>
    {

    protected async override void OnViewLoaded(object view)
      {
      base.OnViewLoaded(view);
      await EditCategories();
      }


    public async Task EditCategories()
      {
      var viewmodel = IoC.Get<CategoryViewModel>();
      await ActivateItemAsync(viewmodel, new CancellationToken());
      }
    }
  }
```

[Contents](Contents) [Previous](Categories1) [Next](CategoryView)
