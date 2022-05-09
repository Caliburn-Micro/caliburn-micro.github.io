---
layout: page
title: The Bootstrapper class
---

[Contents](Contents) [Previous](ShellView) [Next](App_Xaml)

## The Bootstrapper class

### Create the class

Create the Bootstrapper class, make sure it is public and inherit from ``Bootstrapperbase``

 ```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Caliburn.Micro;

namespace Caliburn.Micro.Tutorial.Wpf
  {
  public class Bootstrapper: BootstrapperBase
    {
    }
  }

  ```

Note: due to the chosen namespace, the using statement ``using Caliburn.Micro;`` is not needed. In general, you need to include this using statement.

### Create the constructor

Once you created this, you need to add a number of methods. First add a constructor calling the ``Initialize()`` method

```csharp
public Bootstrapper()
  {
  Initialize();
  }
 ```

### Override the OnStartup event

One more thing to do, here. The Bootstrapper needs to know where to start. The next method will do this. This version works asynchronously. In the Caliburn.Micro version 4.0 this is used very common. It makes the application more responsive. It is worth the trouble to find out how to use asynchronous ways of working, if you are not aware of them.

```csharp
    protected override async void OnStartup(object sender, StartupEventArgs e)
      {
      await DisplayRootViewForAsync(typeof(ShellViewModel));
      }
```

You also need to include a using statement: ``using Caliburn.Micro.Tutorial.Wpf.ViewModels;''
This is why we created the [ShellViewModel](ShellViewModel) in the previous chapter. Alternatively you can create an exception to make your code compile.

```csharp
throw new NotImplementedException();
```

For the time being. ``DisplayRootViewFor<ShellViewModel>();`` will select the ViewModel that starts the application. Shortly it will be clear the **App.xaml** will invoke the Bootstrapper and the Bootstrapper controls what will be shown first.

### Summary

To get this all compile neatly, you may need to add some using statements. The code as shown below should work, if you use .Net 5.0. For .NetCore 3.1 you need to add a using statement to Caliburn.Micro

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Caliburn.Micro.Tutorial.Wpf.ViewModels;
using System.Windows;

namespace Caliburn.Micro.Tutorial.Wpf
  {
  public class Bootstrapper: BootstrapperBase
    {
    public Bootstrapper()
      {
      Initialize();
      }

    protected override async void OnStartup(object sender, StartupEventArgs e)
      {
      await DisplayRootViewForAsync(typeof(ShellViewModel));
      }
    }
  }

```

Bootstrapper.cs will be visited again later in this tutorial. There is a lot more to say about it.

[Contents](Contents) [Previous](ShellView) [Next](App_Xaml)
