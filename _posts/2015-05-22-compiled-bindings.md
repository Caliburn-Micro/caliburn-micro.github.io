---
layout: post
title: Working with Compiled Bindings on Windows 10
---

I've had a few people ask me how Caliburn.Micro will work with compiled bindings in Windows 10. Here's a brief summary based on I know from chatting to some of the engineers at Microsoft.

### Overview

For those who aren't familiar with them (most people given it's a new technology), compiled bindings are a way to have the compiler generate static code for a binding rather than using a dynamic binding that depends on reflection. This increases performance quite a bit and can be used quite heavily, there's also some features where you can *bind* an event to a method.

What used to be:

``` xml
<TextBox Text="{Binding Username, Mode=TwoWay}" />
``` 

becomes (where ViewModel is a strongly typed property on the view)

``` xml
<TextBox Text="{x:Bind ViewModel.Username, Mode=TwoWay}" />
``` 

To get a better understanding I'd recommend watching Sam Spencer's Build talk [Data Binding: Boost Your Apps' Performance Through New Enhancements to XAML Data Binding](http://channel9.msdn.com/Events/Build/2015/3-635)

### Working with Caliburn.Micro

Out of the box Caliburn.Micro checks properties for existing bindings before applying conventions. For instance this stops the convention for `SomePropertyName` overwriting the existing `Binding`.

``` xml
<TextBox x:Name="SomePropertyName" Text="{Binding Username, Mode=TwoWay}" />
```

Unfortunately there is no programmatic way to test if a `DependencyProperty` has a compiled binding, which means that if you're mixing convention based binding and compiled bindings there is a chance they could collide.

``` xml
<TextBox x:Name="SomePropertyName" Text="{x:Bind ViewModel.SomeOtherProperty, Mode=TwoWay}" />
```

Interestingly because a compiled binding isn't a real binding then the convention doesn't overwrite the compiled binding but they exist in parallel which will most likely just confuse everyone.

I'd recommend using one or the other and not mixing them both. You can potentially save yourself some pain and get a performance increase by disabling conventions either on a per view basis

``` xml
<Page cm:View.ApplyConventions="false">
```

or on a global basis.

``` csharp
ViewModelBinder.ApplyConventionsByDefault = false;
```

Of course Windows 10 is still in preview and things could change.