---
layout: page
title: Add a simple container
---
## Dependency control container

In this part of the WPF Tutorial an Inversion of Control Container will be added. This container is a kind of dispenser for class objects. You also will find the term Dependency Injection used often. You can do without and use new statements to instantiate class objects, but for complex applicationt using a container is considered best practice for good reasons. For more backgroud, one of your options is to watch this video: [Tim Corey Dependency Inversion tutorial](https://www.youtube.com/watch?v=NnZZMkwI6KI)

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
The first line adds the container itself to the container.

```C#
_container.Instance(_container);
```

If you need it can do something like this:

```C#
var c= IoC.Get<SimpleContainer>();
```

Next, two interfaces are added tot the container as ``SingleTon`` which means on lon one instance will be created and it will be used repeatedly.

```C#
 _container
       .Singleton<IWindowManager, WindowManager>()
       .Singleton<IEventAggregator, EventAggregator>()
 ```

The use for those two class will be explained later, but it is convenient to add them right now.

Finally a longer chunk of code is needed. It is a bit more complicated but this code will retrieve all ViewModels in the executing asembly:

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

For these ViewModels each time you get a reference a new instance is created. The code relies heavily on Reflexction and Linq. If you are not familiar with these aspects of .Net it is recommende to study them at some point.

``SelectAssemblies`` is defined in Caliburn.Micro. If wnat to add ViewModels that are created in library projects, you need to override this method. Here follows an example

```C#
protected override IEnumerable<Assembly> SelectAssemblies()
      {
      // https://www.jerriepelser.com/blog/split-views-and-viewmodels-in-caliburn-micro/

      var assemblies = base.SelectAssemblies().ToList();
      assemblies.Add(typeof(LoggingViewModel).GetTypeInfo().Assembly);
      return assemblies;
      }
```

In this example the ViewModels from a Logging library are added. It looks a bit curious, but you need one class in the assemby as an example to locate the assembly.

Finally some additional code must be created. It allows you to do fancy things, but it is outside the scope of this tutorial.

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