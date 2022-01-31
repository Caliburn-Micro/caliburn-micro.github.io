---
layout: page
title: Add a menu
---
## Create a menu

The previous parts did provide the techniques to create a simple MVVM application. In the next parts more functions will be added. Before we do so, a menu will be added to allow access to these new functions and to demonstrate ways to interact between these functions.

Add following code in the ``ShellView.xaml`` file

```XML
  <Menu>
   <MenuItem x:Name="FileMenu" Header="File" IsEnabled="{Binding CanFileMenu}"/>
   <MenuItem Header="Edit"/>
   <MenuItem Header="Settings">
     <MenuItem x:Name="EditCategories" Header="Edit Categories"/>
   </MenuItem>
   <MenuItem Header="Help">
     <MenuItem Header="Manual"/>
     <MenuItem x:Name="About" Header="About"/>
   </MenuItem>
 </Menu>
```

MenuItems seem not to support naming conventions like ``CanFileMenu`` directly. So, in order to enable or disable a menu item you need to add a binding to the ``IsEnabled`` dependency property. In the ViewModel add code like this:

```C#
  public bool CanFileMenu 
    { 
    get
      {
      return false;
      }
    }
    ```
Of course you need to add propertychanged notifications where you change the enabled status of this MenuItem. In the next parts an About Dialog Form will be added. In order to do so an few steps are needed to enable the appication to show Dialog Forms:

* Add an Inverion of control container
* Add a WindowManager interface.
* Create the dialog form and its ViewManager
* Hook it all up into the ShellViewModel
