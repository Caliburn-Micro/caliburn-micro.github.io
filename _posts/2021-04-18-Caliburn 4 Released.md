# Caliburn Micro 4 is Released 

I am pleased to anounce that Caliburn.Micro 4 is released.  

### Additional Platforms supported
* .NET Core 3.1 WPF
* .NET 5 WPF

### Support for the following platforms has been removed.

* Silverlight 5
* Windows Phone 8 (Silverlight)
* Windows Phone 8 (Windows Runtime)
* Windows 8.1
* We’ll also be discontinued support for .NET 4.0 for WPF. The minimum version of .NET for WPF is now 4.6.1

All of these platforms are still supported in the 3.x releases.

## Changes
Below is the changes included in 4.0.0 release.

### Assemblies
The platforms targeted have been changed to support the new .NET ecosystem. These include:

* Caliburn.Micro.Core targets .NET Standard 2.0.
* Caliburn.Micro.Platform.Xamarin.Forms targets .NET Standard 2.0.
* Caliburn.Micro.Platform is consistently named across all platforms (previously UWP included the platform name).
* Event Aggregator

The Event Aggregator has some major breaking changes, that bring it into an async implementation approach that is the main reason for 4.0.0.

The full changes are available in the event aggregator migration documentation.

### Screens and view model lifecyle
* All the interfaces that support view model lifecycle such as IActivate and IGuardClose now support an async implementation.

## I would like to thank everyone who has contributed to this release (Hope I did not miss anyone)
@nigel-sampson, @KasperSK, @BartoszPierzchlewiczMacrix, @pableess, @mbreckon, @willson556, @beachwalker, @MafuJosh, @imba-tjd, @davidhenley, @@gpetrou, @louistio, @Rmurray0809, @qtuu, @mgnsm, @CaulyKan, @babackman, @chrisstaley, @bryanbcook, @llifon, @tziemek, @Stannieman, @jonstelly, @rossvegas, @TheBigRic, @markybolton, @vb2ae, @CoreyVincent

I would expecially like to thank @nigel-sampson for his hard work and leadership over the years on Caliburn.Micro. Without his support this release would not have been possible.

## Future Plans

* AvaloniaUI support
   @megazyz has created a PR for initial support for AvaloniaUI  
 * WinUI support
 * MAUI support
 * Updated templates for Windows Template Studio

If there are any additional changes you would like to see please open a support ticket so we can discuss them

 







