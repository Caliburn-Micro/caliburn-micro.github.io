---
layout: page
title: Introduction MVVM
---

[Contents](Contents) [Previous](Introduction) [Next](SolutionSetup)

## Introduction MVVM

### What is MVVM

MVVM stands for Model, View, View Model. The basic idea is to create a three layer user interface. It's primary use is to separate the visual part of the UI (forms, buttons and other controls) an well as possible from the logic that processes the UI commands. For WPF applications, the View is represented by the XAML code and the code behind, which essentially is way to write XAML code in C#. This layer should not do any processing. The View Model is the functional UI layer, which accepts commands from the UI and handles those commands. The third layer is the Model, which holds the data models.

for example, If the user presses a button in the View, a command is sent to the View Model, which will determine what to do and execute the command. You will see a lot more of this later in this tutorial. The Model should be used to represent (custom) data models for objects.

Please keep in mind, this all is a UI layer. You can keep these three layers separated from the program logic (functional layer) and the data access layer, which will separate your functional layer from the data access. In the tutorial, this will be used, but for the sake of simplicity databases will not be used. Instead, simple .csv files may be used for data storage.

### When to use MVVM

If you create a very simple application you probably do not need to use MVVM. MVVM will create some overhead on your application and you need some time to learn how to apply MVVM. If you want to use automated testing, you soon will find that testing through the UI is hard and because UI usually is subject to frequent changes, it is hard to maintain the tests. In this case, the View Model is a much better access point for testing. You can use the regular unit testing approach.

A second reason to have a look at MVVM is that it makes it easier to add other user interfaces like a console interface or an API. The core feature is using a View Model.

## Do I need Caliburn.Micro?

You can start using MVVM principles quite wlee without using Caliburn.Micro. It is less work to use Caliburn.Micro for a new project from day 1, but if you already use the View- ViewModel separation you can migrate to Caliburn.Micro quite fast in many cases. Even if you do not yet use Caliburn.Micro, you can start using Caliburn.Micro. It will not affect existing code in most cases. You also do not need to adopt all Caliburn.Micro features. It is flexible.

## What is the key advantage of Caliburn.Micro for a starting user?

It may depend a bit on what you like. For, it helps me to develop the UI command logic separate from the UI. Also, you do no longer control the availability of a command in the UI layer. Enabling controls can be done from the ViewModel, which is very powerful. On the other hand this involves some "magic" that may be hard to debug if it is not working as anticipated.
In IT the silver bullet does not exist and for MVVM it entirely depends on the problem you need to solve what is a good solution.
There are other MVVM Frameworks as well. You need to consider carefully which one is most suitable for your specific situation. You may want to watch this video: [Picking a new MVVM Framework](https://www.youtube.com/watch?v=8E000zu8UhQ) It was created when support for Caliburn.Micro was discontinued, but it is worth to see some options and some considerations.

[Contents](Contents) [Previous](Introduction) [Next](SolutionSetup)
