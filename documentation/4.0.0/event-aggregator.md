---
layout: page
title: The Event Aggregator
---

## Handle Interfaces
In order to simply things and provide some consistent behavior we've moved back to a single interface which is similar to the old `IHandleWithTask`.

``` csharp
public interface IHandle<TMessage>
{
    Task HandleAsync(TMessage message, CancellationToken cancellationToken);
}
```

Moving from the old `IHandle` interface should just involve adding

``` csharp
return Task.CompletedTask;
```

If you're migrating from `IHandleWithCoroutine` then you can move from something like

``` csharp
public IEnumerable<IResult> Handle(ProductSelectedMessage message)
{
    yield return new LoadingResult();
    yield return new GetProductResult(message.ProductId);
}
```

to

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