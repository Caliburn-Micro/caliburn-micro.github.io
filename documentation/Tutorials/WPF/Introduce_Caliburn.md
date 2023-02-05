---
layout: page
title: Introduce Caliburn
---

[Contents](Contents) [Previous](SolutionSetup) [Next](ShellViewModel)

## Introduce Caliburn.Micro

### Overview

In this step, the standard WPF project will be transformed to support the Caliburn.Micro MVVM Framework. A WPF project uses **app.xaml** to start the application, while **MainWindow.xaml** is the startup window. For Caliburn.Micro, a class will be introduced that will setup the application. It then will invoke the startup ViewModel and View. In Caliburn.Micro you start the ViewModels. The framework will then try to find an appropriate view and activate the view.

So, in this chapter we do a few things:

1. Adapt the **App.xaml** class so it uses **Bootstrapper.cs** instead of **MainWindow.xaml** as string point for your code.
2. Create your first ViewModel, named **ShellViewModel.cs** as the startup ViewModel for your application.
3. Create the first View, named **ShellView.xaml**.
4. Create a class called **Bootstrapper.cs** which contains all startup logic.
5. Delete **MainWindow.xaml**, you no longer need this.

Alternatively, you can rename MainWindow.xaml to ShellView.xaml and move it to the Views folder. This may be a good strategy to migrate existing applications.

Essentially, you need to perform the same steps to migrate an existing WPF application to a Caliburn.Micro application. Of course, this does not make the application and MVVM application.

[Contents](Contents) [Previous](SolutionSetup) [Next](ShellViewModel)
