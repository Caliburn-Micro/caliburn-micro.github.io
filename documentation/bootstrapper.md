---
layout: page
title: Customizing The Bootstrapper
---

In the [last part](./configuration) we discussed the most basic configuration for a Caliburn.Micro WPF Application and demonstrated a couple of simple features related to Actions and Conventions. In this part, I would like to explore the Bootstrapper class a little more. Let’s begin by configuring our application to use an IoC container. We’ll use the built in container for this example, but Caliburn.Micro will work well with any container. First, go ahead and grab the code from Part 1. We are going to use that as our starting point. Now, let’s create a new Bootstrapper called SimpleBootstrapper. Use the following code:

``` csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Windows;

public class SimpleBootstrapper : BootstrapperBase
{
    private SimpleContainer container;

    public SimpleBootstrapper()
    {
        Initialize();
    }

    protected override void Configure()
    {
        container = new SimpleContainer();

        container.Singleton<IWindowManager, WindowManager>();
        container.Singleton<IEventAggregator, EventAggregator>();

        container.PerRequest<ShellViewModel>();
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

    protected override void OnStartup(object sender, StartupEventArgs e)
    {
        DisplayRootViewFor<ShellViewModel>();
    }

    protected override IEnumerable<Assembly> SelectAssemblies()
    {
        return new[] { Assembly.GetExecutingAssembly() };
    }
}
```

That’s all the code to use the built in container. First, we override the Configure method of the Bootstrapper class. This gives us an opportunity to set up our IoC container as well as perform any other framework configuration we may want to do, such as customizing conventions. Here we create the SimpleContainer and add the WindowManager and EventAggregator and ofcourse the ShellViewModel, but not the ShellView Becase we have Assembly.Source.Instance. So, what is AssemblySoure.Instance? This is the place that Caliburn.Micro looks for Views. You can add assemblies to this at any time during your application to make them available to the framework, but there is also a special place to do it in the Bootstrapper. Simply override SelectAssemblies like this:


``` csharp
protected override IEnumerable<Assembly> SelectAssemblies()
{
    return new[] {
        Assembly.GetExecutingAssembly()
    };
}
```

All you have to do is return a list of searchable assemblies. By default, the base class returns the assembly that your Application exists in. So, if all your views are in the same assembly as your application, you don’t even need to worry about this. If you have multiple referenced assemblies that contain views, this is an extension point you need to remember. Also, if you are dynamically loading modules, you’ll need to make sure they get registered with your IoC container and the AssemblySource.Instance when they are loaded. 

After creating the container and providing it with the catalogs, I make sure to add a few Caliburn.Micro-specific services. The framework provides default implementations of both IWindowManager and IEventAggregator. Those are pieces that I’m likely to take dependencies on elsewhere, so I want them to be available for injection. I also register the container with itself (just a personal preference).

After we configure the container, we need to tell Caliburn.Micro how to use it. That is the purpose of the three overrides that follow. “GetInstance” and “GetAllInstances” are required by the framework. “BuildUp” is optionally used to supply property dependencies to instances of IResult that are executed by the framework. 

Finally, make sure to update your App.xaml and change the HelloBootstrapper to SimpleBootstrapper. That’s it! Your up and running with MEF and you have a handle on some of the other key extension points of the Bootstrapper as well.

You can of course use any IoC container you desire as long as you provide implementations for “GetInstance” and “GetAllInstances”.

### Word to the Wise
While Caliburn.Micro does provide ServiceLocator functionality through the Bootstrapper’s overrides and the IoC class, you should avoid using this directly in your application code. ServiceLocator is considered by many to be an anti-pattern. Pulling from a container tends to obscure the intent of the dependent code and can make testing more complicated. 

Besides what is shown above, there are some other notable methods on the Bootstrapper. You can override OnStartup and OnExit to execute code when the application starts or shuts down respectively and OnUnhandledException to cleanup after any exception that wasn’t specifically handled by your application code. 

#### v4.0 Changes
In 4.0 the bootstrapper has seen some changes and that is the the DisplayRootViewFor methods return a task and they can be awaited. 

``` csharp
protected Task DisplayRootViewFor<TViewModel>(IDictionary<string, object> settings = null)
{
    return DisplayRootViewForAsync(typeof(TViewModel), settings);
}
```


### Using Caliburn.Micro in Office and WinForms Applications

Caliburn.Micro can be used from non-Xaml hosts. In order to accomplish this, you must follow a slightly different procedure, since your application does not initiate via the App.xaml. Instead, create a custom boostrapper by inheriting from BoostrapperBase (the non-generic version). When you inherit, you should pass "false" to the base constructor's "useApplication" parameter. This allows the bootstrapper to properly configure Caliburn.Micro without the presence of a Xaml application instance. All you need to do to start the framework is create an instance of your Bootstrapper and call the Initialize() method. Once the class is instantiated, you can use Caliburn.Micro like normal, probably by invoke the IWindowManager to display new UI.
