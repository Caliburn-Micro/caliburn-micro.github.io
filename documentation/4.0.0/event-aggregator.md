---
layout: page
title: The Event Aggregator
---

## Handle Interfaces
The use of async/await allows the combining of IHandleWithTask, IHandleWithCoroutine and IHandle into a single interface to provide consistent behaviour. 

``` csharp
public interface IHandle<TMessage>
{
    Task HandleAsync(TMessage message, CancellationToken cancellationToken);
}
```

This does mean that every class using one of these interfaces will require some minor modifications.
Changes in how a class subscribes to events are detailed in a separate section in this document.

### IHandle implementations

3.2.0
``` csharp
public void Handle(MyNotification message)
{
    ... do something ...
}
```

4.0.0 (equivalent behaviour)
``` csharp
public async Task HandleAsync(MyNotification message, CancellationToken cancellationToken)
{
    ... do something ...
    return Task.CompleteTask;
}
```
### IHandleWithTask implementations

3.2.0
``` csharp
public async Task Handle(MyNotification message)
{
   await Task.Delay(3000);
}
```

4.0.0
``` csharp
public Task HandleAsync(MyNotification message, CancellationToken cancellationToken)
{
    await Task.Delay(3000, cancellationToken);
}
```

### IHandleWithCoroutine implementations

3.2.0
``` csharp
public IEnumerable<IResult> Handle(ProductSelectedMessage message)
{
    yield return new LoadingResult();
    yield return new GetProductResult(message.ProductId);
}
```

4.0.0
``` csharp
public Task HandleAsync(ProductSelectedMessage message, CancellationToken cancellationToken);
{
    var context = new CoroutineExecutionContext { Target = this, View = GetView() };
    
    await new LoadingResult().ExecuteAsync(context);
    await new GetProductResult(message.ProductId).ExecuteAsync(context);
}
```

## Publish
The publish method (and all overloads) now returns a `Task` that completes when all handling methods have finished.

## Subscription marshalling
Previously all thread marshalling was handled by publishing and the following methods.

1. `PublishOnCurrentThreadAsync`.
2. `PublishOnBackgroundThreadAsync`.
3. `PublishOnUIThreadAsync`.

We're now adding similar functionality to subscriptions and the following methods.

1. `SubscribeOnPublishedThread`.
2. `SubscribeOnBackgroundThread`.
2. `SubscribeOnUIThread`.

It's advised that you decide on marshalling at one end and follow that consistently through the app.
