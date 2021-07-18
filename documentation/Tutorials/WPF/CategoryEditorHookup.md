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
    public async Task EditCategories()
      {
      var viewmodel = IoC.Get<CategoryViewModel>();
      await ActivateItemAsync(viewmodel, new CancellationToken());
      }
```

**IoC** stands for Inversion of Control. As you can see it is static and it will retrieve the viewmodel of the specified type from the container that holds all viewmodels. Remember, this container is created in the ``BootstrapperBase`` using reflection.

You have now access to the viewmodel, which allows you to set class variables and properties, before opening the view.

The second line calls ``ActivateItemAsync``, which is a Caliburn.Micro method that can activate a user control in a contentcontrol ``<ContentControl x:Name="ActiveItem" Margin="20"/>`` in the ``ShellView``

Make sure to use ``ActiveItem`` as the name for the ``ContentControl`` because Caliburn.Micro needs this naming convention.

A final comment, note the asynchronous invocation is used. It is recommended to use this always.

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
