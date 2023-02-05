---
layout: page
title: Change App.xaml
---

[Contents](Contents) [Previous](Bootstrapper) [Next](SimpleLogging)

## Change App.Xaml

### Introduction

At this point you should have created a number of new classes:

+ ShellViewModel
+ ShellView
+ Bootstrapper

In this step we connect all this together and we can say goodbye to  the ``MainWindow`` class. This is done in the class App, which is generated for you automatically in each WPF project.

### Adapt App.xaml

Open the App.Xaml file, which looks like this:

```xml
<Application x:Class="Caliburn.Micro.Tutorial.Wpf.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:Caliburn.Micro.Tutorial.Wpf"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

In this file you see the line ``StartupUri="MainWindow.xaml"``. This line of code makes MainWindow.xaml the window to display when starting the application. This is not what we need, since the Bootstrapper class will take care of starting the correct ViewModel. Therefore, remove this line from the code.

Instead, you need to add this code:

```xml

       <ResourceDictionary>
           <ResourceDictionary.MergedDictionaries>
               <ResourceDictionary>
                   <local:Bootstrapper x:Key="Bootstrapper" />
               </ResourceDictionary>
             </ResourceDictionary.MergedDictionaries>
       </ResourceDictionary>
```

This step adds the Bootstrapper class as a local resource

### Overview

The resulting code should look like this:

```xml
<Application x:Class="Caliburn.Micro.Tutorial.Wpf.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:Caliburn.Micro.Tutorial.Wpf">
    <Application.Resources>
      <ResourceDictionary>
           <ResourceDictionary.MergedDictionaries>
               <ResourceDictionary>
                   <local:Bootstrapper x:Key="Bootstrapper" />
               </ResourceDictionary>
             </ResourceDictionary.MergedDictionaries>
       </ResourceDictionary>
    </Application.Resources>
</Application>
```

[Contents](Contents) [Previous](Bootstrapper) [Next](SimpleLogging)
