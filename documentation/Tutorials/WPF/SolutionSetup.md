---
layout: page
title: Solution Setup
---

[Contents](Contents) [Previous](Introduction_MVVM) [Next](Introduce_Caliburn)

## Solution Setup

### Create a WPF solution

The first step for this tutorial is to create a new C# WPF Solution. Make sure to select a .Net solution type. In the code samples .Net 5.0 is used, but .Net Core3.1 should be OK as well.

![New project dialog](/public/images/documentation/Tutorials/WPF/ProjectSelection.jpg)

Next, you can give your project and solution a name. At this moment we only need the UI project for WPF. To promote some consistency, the naming convention is used to name the project with a suffix .WPF. The solution gets the suffix .app

![Project settings dialog](/public/images/documentation/Tutorials/WPF/ProjectConfiguration.jpg)

Finally, you can set the .Net version:

![Set .Net version](/public/images/documentation/Tutorials/WPF/ProjectSelectionDotNet.jpg)

You should have a working project now.

### Add the Caliburn.Micro Nuget Package

Before we can adapt the project to a Caliburn.Micro project, it is necessary to install the Caliburn.Micro Nuget package.

![Get Caliburn.Micro Nuget package](/public/images/documentation/Tutorials/WPF/GetNugetPackage.jpg)

You need the Caliburn.Micro package, which contains the adapter for WPF.

You may notice a huge number of other packages are listed. They look useful, but unfortunately most of them are outdated.

### Create the MVVM folders

The last step to prepare the project is to create three folders in the ``Caliburn.Micro.Tutorial.Wpf`` project. Make sure to name them exactly as instructed. Caliburn.Micro uses naming conventions that enable it to search these folders automatically.

1. Models
2. Views
3. ViewModels

The solution explorer tab should loook like shown below:

![Adding folders](/public/images/documentation/Tutorials/WPF/AddingFolders.jpg)

[Contents](Contents) [Previous](Introduction_MVVM) [Next](Introduce_Caliburn)
