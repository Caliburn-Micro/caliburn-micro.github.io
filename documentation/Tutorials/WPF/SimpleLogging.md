---
layout: page
title: Simple logging
---
[Contents](Contents) [Previous](App_Xaml) [Next](ExpensesLogbook)

## Simple logging

### Introduction

Before we continue to create a real application from this skeleton, there is one little topic to touch. Caliburn.Micro uses some magic and especially if you just started, it can be hard to find out why your application is not working. To help you out, there is an extremely simple logging class that shows at least how Caliburn.Micro tries to find the View and this really can be helpful. So, you may add this as an optional step to your solution.

### Create the logging class

Create a new class named ``DebugLogger`` which implements the ILog interface.

```csharp
using System;
using System.Diagnostics;

namespace Caliburn.Micro.Tutorial.Wpf
  {
  class DebugLogger : ILog

    {
    private readonly Type _type;

    public DebugLogger(Type type)
      {
      _type = type;
      }

    public void Info(string format, params object[] args)
      {
      if (format.StartsWith("No bindable"))
        return;
      if (format.StartsWith("Action Convention Not Applied"))
        return;
      Debug.WriteLine("INFO: " + format, args);
      }

    public void Warn(string format, params object[] args)
      {
      Debug.WriteLine("WARN: " + format, args);
      }

    public void Error(Exception exception)
      {
      Debug.WriteLine("ERROR: {0}\n{1}", _type.Name, exception);
      }
    }
  }
  ```

### Activate the logger

In ``Bootstrapper`` the ``DebugLogger`` is activated using this line of code:

```csharp
LogManager.GetLog = type => new DebugLogger(type);
```

The location where it is activated is not very critical. Probably the best location is in the overridden OnStartUp() handler, just before trying to display the ShellView.

The logger will use the Output window in Visual Studio to display logging information it collects from Caliburn.Micro.

[Contents](Contents) [Previous](App_Xaml) [Next](ExpensesLogbook)
