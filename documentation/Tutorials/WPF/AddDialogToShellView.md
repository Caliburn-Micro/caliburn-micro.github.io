---
layout: page
title: Add the About Dialog Form to the ShellViewModel
---

[Contents](Contents) [Previous](DialogForm)

## Add the Dialog Form to the ShellViewModel

In a previous part we have seen how to invoke user controls. To use Dialog Forms is slightly more complicated. In this part we will hook up the [About dialog](DialogForm) to the [ShellViewModel](ShellViewModel).  To support this an interface with a ``WindowManager`` is needed in the ``ShellViewModel``

First, declare a readonly class variable in ``ShellViewModel.cs``:

```C#
private readonly IWindowManager _windowManager;
```

Then adapt the constructor for the ``ShellViewModel`` to import the ``IWindowManager`` interface. The ``container`` we created in a [previous section](SimpleContainer) will handle this, while creating the ``ShellViewModel``.

```C#
public ShellViewModel(IWindowManager windowManager)
    {
    _windowManager = windowManager;
    }
```

Now, using the Caliburn.Micro naming conventions, a method can be created that is invoked when the menu item named "About" is clicked:

```C#
public Task About()
  {
  return _windowManager.ShowDialogAsync(IoC.Get<AboutViewModel>());
  }
```

``ShowDialogAsync`` will show a viewmodel as a modal dialog form. The DialogResult will be returned as ``bool?``. You can set the desired value in the ViewModel using:

```C#
TryClose(true); // for OK
TryClose(false); // for Cancel
```

  If you need a modeless form, this code can be used:

```C#
_windowManager.ShowWindowAsync(IoC.Get<AboutViewModel>());  
```

The third variant is to show a popup form at the current mouse position:

```C#
_windowManager.ShowPopupAsync(IoC.Get<AboutViewModel>());  
```

### More about the WindowManager

The WindowManager is specific for the platform your code runs at. It is not very well documented, but in its full form it has some additional parameters:

```C#
public virtual async Task<bool?> ShowDialogAsync(object rootModel, object context = null, IDictionary<string, object> settings = null)
```

* ``rootModel`` refers to your ``ViewModel``.
* ``context`` appears to refer to the ``View`` you want to associate with the ViewModel. In most case ``null`` is a good value. Caliburn.Micro will do the magic for you.
* ``settings`` allows to insert settings for the window. In case you want to experiment, have a look at this blog:
[System.Mike!](https://claytonone.wordpress.com/2015/01/16/caliburn-micro-part-6-the-window-manager/)

```C#
dynamic settings = new ExpandoObject();
settings.WindowStartupLocation = WindowStartupLocation.CenterOwner;
settings.ResizeMode = ResizeMode.NoResize;
settings.MinWidth = 450;
settings.Title = "My New Window";
settings.Icon = new BitmapImage( new Uri( "pack://application:,,,/MyApplication;component/Assets/myicon.ico" ) );
 
IWindowManager manager = new WindowManager();
manager.ShowDialog( myViewModel, null, settings );
```

Especially for library functions this may be interesting. In the tutorial example it is not used.

[Contents](Contents) [Previous](DialogForm)
