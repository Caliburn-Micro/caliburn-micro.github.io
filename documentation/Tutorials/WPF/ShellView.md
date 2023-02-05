---
layout: page
title: Add the ShellView
---

[Contents](Contents) [Previous](ShellViewModel) [Next](Bootstrapper)

## Add the ShellView

For the time being, each ViewModel should have a View with exactly the same name, but the name should end with View instead of ViewModel and it should be stored in the ``Views`` folder.

Caliburn.Micro promotes a programming style where you use one window and then show user controls inside this window. This works OK. In this tutorial it will be shown how this works, but it also will be shown how to use dialog windows and mode less windows. Fortunately this is fully supported as well.

### Create the ShellView class

Since we already created a class ``ShellViewModel`` we also need a WPF Window named ``ShellView``, so create a new WPF Window class in the View folder. You do not need any specific inheritance here.

### Add a content control

Though it is not yet, used, at this point add a ``ContentControl`` to the ShellView.

```csharp
<ContentControl x:Name="ActiveItem" Margin="20"/>
```

You should make sure to use the name ''ActiveItem``  here, because this is a Caliburn.Micro naming convention. This name will be used later to locate where the user controls should be displayed.

### Overview

The XAML code now looks like this:

```xml
<Window x:Class="Caliburn.Micro.Tutorial.Wpf.Views.ShellView"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:Caliburn.Micro.Tutorial.Wpf.Views"
        mc:Ignorable="d"
        Title="ShellView" Height="450" Width="800">
    <Grid>
    <ContentControl x:Name="ActiveItem" Margin="20"/>
  </Grid>
</Window>
```

### Delete ``MainWindow.xaml``

The final step is to delete ``MainWindow.xaml``. Alternatively you could have renamed it and moved it to the ViewModels  folder. This is a useful strategy if you want to migrate existing projects to a Caliburn.Micro based solution.

[Contents](Contents) [Previous](ShellViewModel) [Next](Bootstrapper)
