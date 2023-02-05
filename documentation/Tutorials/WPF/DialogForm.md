---
layout: page
title: Create a Dialog Form
---

[Contents](Contents) [Previous](SimpleContainer) [Next](AddDialogToShellView)

## Create a simple dialog form

As a very simple example of a Dialog Form, an About form is created, name it ``AboutView.xaml``

```XML
Window x:Class="Caliburn.Micro.Tutorial.Wpf.Views.AboutView"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" WindowStartupLocation="CenterScreen" SizeToContent="WidthAndHeight"
        Title="About">
  <StackPanel>
    <Grid Margin="20">
      <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="Auto" MinWidth="150"/>
        <ColumnDefinition Width="*"/>
      </Grid.ColumnDefinitions>
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <TextBlock Grid.Row="0" Grid.Column="0" Margin="5">Program name:</TextBlock>
      <TextBlock x:Name="AboutData_Title" Grid.Row="0" Grid.Column="1" Margin="5"/>
      <TextBlock Grid.Row="1" Grid.Column="0" Margin="5">Program version:</TextBlock>
      <TextBlock x:Name="AboutData_Version" Grid.Row="1" Grid.Column="1" Margin="5"/>
      <TextBlock Grid.Row="2" Grid.Column="0" Margin="5">Author:</TextBlock>
      <TextBlock Grid.Row="2" Grid.Column="1" Margin="5" Text="{Binding AboutData.Author}"/>
      <TextBlock Grid.Row="3" Grid.Column="0" Margin="5">More information:</TextBlock>
      <TextBlock Grid.Row="3" Grid.Column="1" Margin="5">
         <Hyperlink RequestNavigate="Hyperlink_RequestNavigate"
               NavigateUri="{Binding AboutData.Url}">
            <TextBlock Text="{Binding AboutData.Url}"/>
         </Hyperlink>
      </TextBlock>
    </Grid>

    <Button x:Name="CloseForm" Margin="5" HorizontalAlignment="Right" Width="80">Close</Button>
  </StackPanel>
</Window>
```

The form looks like this:

![About dialog form](/public/images/documentation/Tutorials/WPF/Aboutform.jpg)

The code behind in ``AboutView.cs`` looks like this:

```C#
using System.Diagnostics;
using System.Windows;

namespace Caliburn.Micro.Tutorial.Wpf.Views
  {
   public partial class AboutView : Window
    {
    public AboutView()
      {
      InitializeComponent();
      }

    private void Hyperlink_RequestNavigate(object sender, System.Windows.Navigation.RequestNavigateEventArgs e)
      {
      // You need a workaround here for .Net Core:
      //  https://github.com/dotnet/runtime/issues/28005
      var psi = new ProcessStartInfo
        {
        FileName = e.Uri.AbsoluteUri,
        UseShellExecute = true
        };
      Process.Start(psi);
      }
    }
  }
```

  Because a hyperlink is used, a workaround is needed to make it work properly. It is not related to Caliburn.Micro and one of the few exceptions where you may want to use some code in the code behind.

The ViewModel should go into ``AboutViewModel.cs`` and looks like this:

```C#
using Caliburn.Micro.Tutorial.Wpf.Models;
using System.Threading.Tasks;

namespace Caliburn.Micro.Tutorial.Wpf.ViewModels
  {
  public class AboutViewModel: Screen
    {
    private AboutModel  _aboutData = new AboutModel();

    public AboutModel AboutData
      {
      get { return _aboutData; }
      }

    public Task CloseForm()
      {
      return TryCloseAsync();
      }
    }
  }
```

The use of ``CloseForm`` may look a bit strange. You can use ``Close`` but that solution requires the new qualifier because it hides a Windows method.

Finally a Model is used to represent the data in the form. The filename is ``AboutModel.cs``

```C#
namespace Caliburn.Micro.Tutorial.Wpf.Models
  {
  public class AboutModel
    {
    public string Title { get; set; } ="Caliburn.Micro Tutorial";
    public string Version { get; set; } ="1.0";
    public string Author { get; set; } = "Rudolf";
    public string Url { get; set; } = "https://caliburnmicro.com/";
    }
  }
  ```
  
  In this particular case all data are effectively constants, so no need to implement ``NotifyOfPropertyChange``.
  
  [Contents](Contents) [Previous](SimpleContainer) [Next](AddDialogToShellView)
