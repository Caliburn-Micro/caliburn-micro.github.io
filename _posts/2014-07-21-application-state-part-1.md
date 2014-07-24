---
layout: post
title: Application State, part 1
---

Application State and it's management is an incredibly important part of building rich interactive applications. It's also a broad topic that covers a lot of different scenarios and can mean different things to different people. The next major release of Caliburn.Micro will include a number of things to help address this in WinRT (Windows 8 and Windows Phone 8.1) and I want to start framing the conversation. 

I'll start with defining the different sorts of state, outline the solutions on existing platforms such as Windows Phone Silverlight, then talks about the differences in WinRT.

## What is state?

First I want to break state up into three broad buckets. There is some overlap between these buckets thanks to bindings but they're generally discrete. These buckets are:

 1. View State
 2. View Model State
 3. Frame State

### View State

View state are the states of the controls that make up the view, this would be things such as scroll position, selected items in list boxes, partially entered text etc.

### View Model State

This is often what developers think of as application state, this is the current state of the view model and includes child view models loaded from external services. There's an overlap with View State here with what's being bound between view and view model. When this occurs I include it in View Model state.

### Frame State

Almost all Windows Phone and most Windows applications are view first in that the root window content is a Frame that is wrapped in a navigation service. What I refer to as Frame state is the back and forward stacks of that Frame control.

## When should we consider state?

I believe there's two major times we need to consider the state of the application. They are:

 1. Frame navigation
 2. Application Suspension / Resumption

### Frame Navigation

Users expect that when that when they navigate back to a view within an application it will be in the same state it was when they navigated away. Often this can be a subtle thing but will frustrate the user ALOT if they're navigating back and forth between views frequently and it's state is not retained. View model state will also be something the user will expect to be retained including things such as partially entered text fields and data loaded from web services.

### Application / Suspension

In the more mobile xaml frameworks such as Windows Phone Silverlight and WinRT there are systems in place that in order to conserve resources an suspended application in the background may be terminated. When the user relaunches the application there will be an expectation that it resumes to the state they left the application.

## How does this come together?

Obviously there is a relationship between the different sorts of state and when we should consider them, almost all of it is driven by user expectation. This can be different for individual applications (banking software can for security reasons have a different idea about resuming).

Below is my general thoughts on user expectations and how they relate to the two above lists.

| &nbsp; | Frame Navigation | Suspension / Resumption |
|--------|------------------|-------------------------|
| View State | Views from back and forward stacks should retain state. | Views are generally not expected to retain state. |
| View Model State | View models corresponding to the back and forward stacks should retain state. | The view model for the "current" view would retain some of its state. |
| Frame State | N/A | The back and forward stacks of the Frame are retained. |

## Where to from here?

This post has been a lot of definitions to probably highlight what's usually obvious but defining the terms for later discussion helps quite a bit.

In the next post I'll outline how Caliburn.Micro deals with most of these problems on Windows Phone Silverlight.



