---
layout: page
title: Simple logging
---

[Contents](Contents) [Previous](App_Xaml) [Next](ExpensesLogbook)

## Simple logging

### Introduction

Before we continue to create a real application from this skeleton, there is one little topic to touch. Caliburn.Micro uses some magic and especially if you just started, it can be hard to find out why your application is not working. To help you out, there is an extremely simple logging class that shows at least how Caliburn.Micro tries to find the View and this really can be helpful. So, you may add this as an optional step to your solution.

### Activate the logger

In ``Bootstrapper`` the ``DebugLog`` is activated using this line of code:

```csharp
LogManager.GetLog = type => new DebugLog(type);
```

The location where it is activated is not very critical. You can include it in the constructor.

The logger will use the Output window in Visual Studio to display logging information it collects from Caliburn.Micro.  By default it uses the implementation DebugLog.cs in the Caliburn.Micro library. Alternatively, you can create your own version.

[Contents](Contents) [Previous](App_Xaml) [Next](ExpensesLogbook)
