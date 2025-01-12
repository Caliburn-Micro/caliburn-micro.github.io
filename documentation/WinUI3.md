---
layout: page
title: Working with WinUI3
---

WinUI3 is Microsoft sucessor to Universal Windows Projects.  In this article I will show you how to add Caliburn.Micro to an existing WinUI3 project.  

Rather than creating a new Bootstrapper in WinUI3 we replace the existing Application with one of our own. In App.xaml replace the Application instance with CaliburnApplication.

``` xml
<cal:CaliburnApplication
    x:Class="Setup.WinUI3.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:cal="using:Caliburn.Micro"
    xmlns:local="using:Setup.WinUI3">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
                <!-- Other merged dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
            <!-- Other app resources here -->
        </ResourceDictionary>
    </Application.Resources>
</cal:CaliburnApplication>
```

The functionality of the CaliburnApplication is very similar to the Bootstrapper. Do expect some breaking changes regarding the functionality of handling the default view, launch arguments and types.

In App.xaml.cs you would typically start with the following code:

``` csharp
using System;
using System.Collections.Generic;
using Caliburn.Micro;
using Microsoft.UI.Xaml.Controls;
using Setup.WinUI3.ViewModels;
using Setup.WinUI3.Views;
using Windows.ApplicationModel.Activation;

// To learn more about WinUI, the WinUI project structure,
// and more about our project templates, see: http://aka.ms/winui-project-info.

namespace Setup.WinUI3
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    public partial class App : CaliburnApplication
    {
        private WinRTContainer container = new WinRTContainer();

        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Initialize();
            this.InitializeComponent();
        }

        protected override void Configure()
        {
            container.RegisterWinRTServices();

            container.PerRequest<HomeViewModel>();
        }

        protected override void PrepareViewFirst(Frame rootFrame)
        {
            container.RegisterNavigationService(rootFrame);
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="args">Details about the launch request and process.</param>
        protected override void OnLaunched(Microsoft.UI.Xaml.LaunchActivatedEventArgs args)
        {
            if (args.UWPLaunchActivatedEventArgs.PreviousExecutionState == ApplicationExecutionState.Running)
                return;

            DisplayRootView<HomeView>();
        }
        protected override object GetInstance(Type service, string key)
        {
            return container.GetInstance(service, key);
        }

        protected override IEnumerable<object> GetAllInstances(Type service)
        {
            return container.GetAllInstances(service);
        }

        protected override void BuildUp(object instance)
        {
            container.BuildUp(instance);
        }
    }
}

```

Caliburn Micro on its various platforms has usually supported either a View Model first or a View first approach, but not usually both at the same time. Typically Silverlight and WPF applications follow a View Model first approach, usually with a Shell View Model and using view model composition. Meanwhile Windows Phone applications due to having the navigation concept baked very close to the hardware (with the back button) typically follow a View first approach and expose a navigation service to move between pages.

WinUI3 apps sit somewhere in the middle, since there is no hardware back button we don’t have the same drive towards View first, however most apps follow a design where a root Frame control navigating between pages makes sense. However there is nothing stopping a developer using a View Model first approach and for certain apps and scenarios this makes sense. In fact I feel that fully featured apps will use both approaches.

Here’s some examples of how you’d run through each scenario.

### View First

This approach is what you’re used to if you’ve been using the WinRT version up until now, it’s fundamentally the same with a few minor changes in your App.xaml.cs. The first thing to note is that WinRTContainer.RegisterDefaultServices doesn’t register an instance of INavigationService as it wouldn’t make sense in View Model first scenarios. Instead we override a method named PrepareViewFirst that has a parameter of the root frame for the application (this is also accessible through the RootFrame property). We can then pass this to WinRTContainer.RegisterNavigationService, this creates the required FrameAdapter and registers it to the container as a navigation service. If you’re using a different container this is where you’d do the same with your own container.

``` csharp
protected override void PrepareViewFirst(Frame rootFrame)
{
   container.RegisterNavigationService(rootFrame);
}
```

Now instead of defining the default view we’ll override OnLaunched, this is the method called by Windows 8 on launch. Here we’ll call DisplayRootView with the type of the view we want our root frame to navigate to, in this case MenuView. This approach enables us to use things like the launch arguments and choose a different view to navigate to. Caliburn will ensure then ensure the Bootstrapper is initialized, set the root frame as the content of the window and navigate to the specified view.

``` csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
     DisplayRootView<MenuView>();
}
```

### View Model First

This approach takes advantage of the view model composition built into Caliburn Micro. It’s a little simpler to set up than view first. We don’t need to override PrepareViewFirst and in our OnLaunched we call DisplayRootViewFor with the view model as the type. Caliburn determines the view as per its conventions and sets that as the content of the window as well as binding the two together.

``` csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    DisplayRootViewFor<ShellViewModel>();
}
```

##### Things to watch out for

Due to the event model of a WinRT application there isn’t a suitable place to initialize the CaliburnApplication before OnLaunched, therefore both DisplayRootView and DisplayRootViewFor will Initialize the application (and ultimately call Configure). If you’re dependent upon the application already being configured then you can call Initialize yourself.

### Combining both approaches

Any significantly sized WinRT application will most likely use a combination of the two approaches. While you may use View first to quickly enable the navigation metaphor with a back button you may compose individual views with child view models. Also some launch scenarios such as Share Target give you a small window to work in where View Model first will be simpler. The sample WinRT application shows this approach.

Overall these changes should give you increased functionality and better control of your application using Caliburn Micro.


### Dealing with Fast App Resume

On WinUI3 OnLaunched will only be called when your app is launched, if your app is already running and the user relaunches then the app will be activated but not launched and opened where it currently is.
