---
layout: page
title: Add the ShellViewModel
---

[Contents](Contents) [Previous](Introduce_Caliburn) [Next](ShellView)

## ShellViewModel

### Why do we need this?

The **ShellViewModel** is the central viewmodel that controls the application logic. It activates the other ViewModels and will contain some common logic. This also is the place where most events will be handled. For the moment, we just create a very empty ShellViewModel. In future chapters it will be revisited and more logic will be added.

### Create the ShellViewModel

Create a new class named ``ShellViewModel``. It is important to create it inside the ``ViewModels`` folder to make it work properly. In the next chapter you will create a Bootstrapper and it will search this folder for ViewModels.

Also make sure to let the namespace end with .ViewModels

Finally, make you class ``public`` and let it inherit from ``Conductor<object>``

The resulting code looks like this:

```csharp
namespace Caliburn.Micro.Tutorial.Wpf.ViewModels
  {
  public class ShellViewModel: Conductor<object>
    {
    }
  }
  ```

You solutions explorer should look like this:

![ShellViewModel added](/public/images/documentation/Tutorials/WPF/ShellViewModelAdded.jpg)

### About the Conductor

``Conductor<object>`` is one of the ways Caliburn.Micro interfaces to ViewModels. Later we will add more detail to this topic.

[Contents](Contents) [Previous](Introduce_Caliburn) [Next](ShellView)
