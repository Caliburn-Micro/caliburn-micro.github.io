---
layout: page
title: Working with .net Maui
---

To use Caliburn.Micro with Maui use Visual Studio 2022 version 17.3 and install the maui workload. Then you can create a new Maui app. In the platforms folder delete the tizen folder. It is currently not supported by Caliburn.Micro.

     dotnet workload install maui

or if you have an older version

    dotnet workload update
    dotnet workload restore

Add a Caliburn.Micro.Maui nuget package to your solution. From the terminal window you can use this command

dotnet add package Caliburn.Micro.Maui --version 5.0.183-beta --source "https://nuget.pkg.github.com/caliburn-micro/index.json"

In the platforms folder android modify the MainApplication class to this

[Application]
public class MainApplication : Caliburn.Micro.Maui.CaliburnApplication
{
   public MainApplication(IntPtr handle, JniHandleOwnership ownership)
    : base(handle, ownership)
  {
      Initialize();
  }

  protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();

  protected override void Configure()
  {
      base.Configure();
  }

  protected override IEnumerable<Assembly> SelectAssemblies()
  {
      return new List<Assembly>() { typeof(App).Assembly };
  }
}
In the iOS folder modify AppDelgate to this

[Register("AppDelegate")]
 public class AppDelegate : Caliburn.Micro.Maui.CaliburnApplicationDelegate
{
    public AppDelegate()
    {
        Initialize();
    }

    protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
}
In the MacCatalyst folder modify AppDelegate to this

[Register("AppDelegate")]
public class AppDelegate : Caliburn.Micro.Maui.CaliburnApplicationDelegate
 {
    public AppDelegate()
   {
        Initialize();
   }

    protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
}
In the Windows folder modify the App.Xaml to this

<cal:CaliburnApplication
    x:Class="MauiApp2.WinUI.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maui="using:Microsoft.Maui"
    xmlns:cal="using:Caliburn.Micro.Maui"
    xmlns:local="using:MauiApp2.WinUI">

</cal:CaliburnApplication>
Make sure you replace x:Class with your apps name

cs file

public partial class App : Caliburn.Micro.Maui.CaliburnApplication
{
    /// <summary>
    /// Initializes the singleton application object.  This is the first line of authored code
    /// executed, and as such is the logical equivalent of main() or WinMain().
    /// </summary>
    public App()
    {
	this.InitializeComponent();
	Initialize();
    }

     protected override MauiApp CreateMauiApp() => MauiProgram.CreateMauiApp();
 }
The App.Xaml in project root folder should be modified like this

<?xml version = "1.0" encoding = "UTF-8" ?>
<cal:MauiApplication xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
         xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
         xmlns:local="clr-namespace:MauiApp2"
         xmlns:cal="using:Caliburn.Micro.Maui"
         x:Class="MauiApp2.App">
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary Source="Resources/Styles/Colors.xaml" />
            <ResourceDictionary Source="Resources/Styles/Styles.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
</cal:MauiApplication>
Replace xmlns:Local with your app name

The App.Xamal.cs file

public partial class App : Caliburn.Micro.Maui.MauiApplication
{
    public App()
    {
       InitializeComponent();

       Initialize();

      DisplayRootViewForAsync<MainViewModel>();
    }
 }