---
layout: home
published: true
title: Caliburn.Micro &middot; 'Xaml made easy'
---

A small, yet powerful framework, designed for building applications across all XAML platforms (.NET 4.5+ and .NET Core 3.0+). Its strong support for MV* patterns will enable you to build your solution quickly, without the need to sacrifice code quality or testability.

### Works everywhere Xaml does
Supports WPF, UWP, and Xamarin.Forms apps.

#### and many more ...

## Get Started
The quickest way to get started is down grab the latest package from [Nuget][nuget] and browse the [documentation][docs]. If you're running into trouble checkout our [support][support] section.

## Features
---
#### Bind view model properties to your view based on convention


``` xml
<ListBox x:Name="Products" />
``` 

``` csharp
public BindableCollection<ProductViewModel> Products
{
    get; private set; 
}

public ProductViewModel SelectedProduct
{
    get { return _selectedProduct; }
    set
    {
        _selectedProduct = value;
        NotifyOfPropertyChange(() => SelectedProduct);
    }
}
```

>Even at design time!


#### Apply methods between your view and view model automatically with parameters and guard methods

``` xml
<StackPanel>
    <TextBox x:Name="Username" />
    <PasswordBox x:Name="Password" />
    <Button x:Name="Login" Content="Log in" />
</StackPanel>
```

``` csharp
public bool CanLogin(string username, string password)
{
    return !String.IsNullOrEmpty(username) && !String.IsNullOrEmpty(password);
}

public string Login(string username, string password)
{
    ...
}
```


#### Decouple view models with built in composition patterns and event aggregation 
``` csharp
public class DocumentTabsViewModel : Conductor<TabViewModel>.Collection.OneActive
{
	...
}
```

``` csharp
public class CartSummaryViewModel : IHandle<CartChangedMessage>
{
	...
}
```

## Who's Behind It
The core contributors to Caliburn.Micro are:

 - [Ken Tucker][kt] - New Maintainer, Microsoft MVP from 2003 to 2016, User group leader, Mobile Developer.
 - [KasperSK][ksk] - New Maintainer, mysterious.
 - [Corey Vincent][cv] - Documentation editor, dynamic full stack developer
 - [Nigel Sampson][nigel] - Previous Project coordinator and responsible for the ports to new Xaml platforms such as Windows 8 and Windows Phone 8.1 / Universal Apps.
 - [Rob Eisenberg][rob] - Original creator of Caliburn.Micro and still serves as advisor to the rest of the team.
 - [Thomas Ibel][thomas] - Portable king, single handedly moved Caliburn.Micro into the new era of Portable Class libraries and set the foundation for the future.

As is with any open source project there are many other contributors, you can see a full list on the [GitHub][contributors]. Apologies if your name was lost during the move between version control systems.


## Sponsorship

We'd like to thank the people at [Jetbrains][jb] for providing licenses for [Resharper][resharper] to the team.

[nuget]: https://www.nuget.org/packages/Caliburn.Micro
[docs]: /documentation
[support]: /support
[getting-started]: /documentation/getting-started
[rob]: http://robeisenberg.com
[bs]: http://www.bluespire.com
[kt]: https://github.com/vb2ae
[ksk]: https://github.com/KasperSK
[cv]: https://github.com/KasperSK
[nigel]: http://compiledexperience.com
[mm]: http://markermetro.com
[thomas]: https://twitter.com/thomasibel
[contributors]: https://github.com/Caliburn-Micro/Caliburn.Micro/graphs/contributors
[durandal]: http://durandaljs.com/
[jb]: https://www.jetbrains.com/
[resharper]: https://www.jetbrains.com/resharper/
