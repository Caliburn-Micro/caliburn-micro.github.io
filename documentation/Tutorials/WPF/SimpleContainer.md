---
layout: page
title: Add a simple container
---

[Contents](Contents) [Previous](Menu) [Next](DialogForm)

## Dependency control container

In this part of the WPF Tutorial an Inversion of Control Container will be added. This container is a kind of dispenser for class objects. You also will find the term Dependency Injection used often. You can do without and use new statements to instantiate class objects, but for complex applications using a container is considered best practice for good reasons. For more background, one of your options is to watch this video: [Tim Corey Dependency Inversion tutorial](https://www.youtube.com/watch?v=NnZZMkwI6KI)

For this tutorial a Caliburn.Micro ``SimpleContainer`` will be used. Make following changes t ``Bootstrapper.cs``:

Add an interface to the container:

```C#
private readonly SimpleContainer _container = new SimpleContainer();
```

There are quite a lot of different libraries to do similar things, and you can combine those libraries with Caliburn.Micro but this is a Caliburn.Micro tutorial and in many cases this will do the job.

We need to setup the contents of the container by overriding the ``Configure`` method:

```C#
   protected override void Configure()
     {
     _container.Instance(_container);
     _container
       .Singleton<IWindowManager, WindowManager>()
       .Singleton<IEventAggregator, EventAggregator>();

     foreach(var assembly in SelectAssemblies())
       {
          assembly.GetTypes()
         .Where(type => type.IsClass)
         .Where(type => type.Name.EndsWith("ViewModel"))
         .ToList()
         .ForEach(viewModelType => _container.RegisterPerRequest(
             viewModelType, viewModelType.ToString(), viewModelType));
       }
     }
```
### Setup of the container

The first line adds the container itself to the container.

```C#
_container.Instance(_container);
```

If you need it can do something like this to get access to the container:

```C#
var container = IoC.Get<SimpleContainer>();
```

Next, two interfaces are added tot the container as ``Singleton`` which means only one instance will be created and the instance will be used repeatedly. You should consider carefully if you really need to use a single instance container.

```C#
 _container
       .Singleton<IWindowManager, WindowManager>()
       .Singleton<IEventAggregator, EventAggregator>()
```

The use for those two classes will be explained later, but it is convenient to add them right now.


### Add the viewmodels to the container

Now you need to add all viewmodels to the container. For each one, you can add a line of code that looks like this:

```c#
 __container
           .PerRequest<ShellViewModel>()
           .PerRequest<CategoryViewModel>()
           ...

```
This works fine and gives you detailed control on the process. You also can chose other ways to add viewmodels to the container, like the Singleton method discussed before. The disadvantage is that if you forget to do this step, you will get error messages because the application cannot find the viewmodel. 

Another way of working is to automate this, using reflection. The code below will d the job and will retrieve all ViewModels in the executing assembly:

```C#
foreach(var assembly in SelectAssemblies())
       {
          assembly.GetTypes()
         .Where(type => type.IsClass)
         .Where(type => type.Name.EndsWith("ViewModel"))
         .ToList()
         .ForEach(viewModelType => _container.RegisterPerRequest(
             viewModelType, viewModelType.ToString(), viewModelType));
       }
```

For these ViewModels each time you get a reference a new instance is created. The code relies heavily on Reflection and Linq. If you are not familiar with these aspects of .Net it is recommended to study them at some point.

``SelectAssemblies`` is defined in Caliburn.Micro. If you want to add ViewModels that are created in library projects, you need to override this method. Here follows an example

```C#
protected override IEnumerable<Assembly> SelectAssemblies()
      {
      // https://www.jerriepelser.com/blog/split-views-and-viewmodels-in-caliburn-micro/

      var assemblies = base.SelectAssemblies().ToList();
      assemblies.Add(typeof(LoggingViewModel).GetTypeInfo().Assembly);
      return assemblies;
      }
```

In this example the ViewModels from a Logging library are added. It looks a bit strange, but you need one class in the assemby as an example to locate the assembly.

Finally three additional methods must be created to setup all stuff. You can create variants to do more fancy things, but this is outside the scope of this tutorial.

```C#
    protected override object GetInstance(Type service, string key)
      {
      return _container.GetInstance(service, key);
      }

    protected override IEnumerable<object> GetAllInstances(Type service)
      {
      return _container.GetAllInstances(service);
      }

    protected override void BuildUp(object instance)
      {
      _container.BuildUp(instance);
      }
```

[Contents](Contents) [Previous](Menu) [Next](DialogForm)