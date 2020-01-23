# Beeline navigation SDK for iOS

Integrate Beeline navigation into your iOS app

![Image of Beeline navigation](/documentation/images/Landscape.png)

You may also be interested in [Beeline routing SDK for iOS]()

## Requirements
- Compatible with iOS 9+
- Integrate with CocoaPods or Carthage

See [installation guide](#installation)

## Integration

General approach
- Create an instance of `BeelineNavigationViewController`
- Register `onRideFinished` callback
- Present `BeelineNavigationViewController` in whatever way best suits your application
- Dismiss `BeelineNavigationViewController` when ride is finished

### Approach 1: Beeline navigation to a destination

Recommended if:
- your app has no route planner
- your app does not use the `Beeline routing SDK for iOS` to display a route to a user before starting navigation

When you start Beeline navigation with a destination Beeline will fetch a route from the user's current location to the destination.

An error will be displayed to the user if:
- no internet connection is available
- no route is available between the user's location and destination
- the destination is not a valid coordinate

```swift
// At the top of your file
import BeelineNavigation

// use a search or map to allow your user to choose a destination
let destination = CLLocationCoordinate2D(latitude: 51.504527, -longitude: 0.086500)

// provide a `CLLocationCoordinate2D` representing the destination
// you should verify the location is logically valid before passing it to Beeline
// (see the error handling section for more info)
let beelineNavigationViewController = BeelineNavigationViewController(destination: destination)

// omitted: display beelineNavigationViewController

beelineNavigationViewController.onRideFinished = { ride in
  // omitted: dismiss beelineNavigationViewController
}
```

### Approach 2: Beeline navigation with a route

Recommended if:
- your app uses the `Beeline routing SDK for iOS` to display a route to a user before navigation is started

```swift
// At the top of your file
import BeelineNavigation

// ... provide the `BeelineRoute`
let beelineNavigationViewController = BeelineNavigationViewController(route: route)

// omitted: display beelineNavigationViewController

beelineNavigationViewController.onRideFinished = { ride in
  // omitted: dismiss beelineNavigationViewController
}
```

## How to stop navigation programatically

Beeline will stop the ride and invoke the `onRideFinished` callback when the user presses the stop button.
If you want to end the ride programatically you can call the `endRide()` function.

```swift
// This will also invoke the BeelineNavigationViewController.onRideFinished callback
beelineNavigationViewController.endRide()
```

## How to update battery, range and cost details

By default these additional details will be hidden, to display them simply call the appropriate setter.

```swift
beelineNavigationViewController.set(batteryLevel: 0.5) // 50%
beelineNavigationViewController.set(range: 25_100.0) // 25.1km
beelineNavigationViewController.set(cost: 1.2, currency: "£") // £1.20
```

## How to change the the measurement unit

By default Beeline will use the user's default measurement preference. However you can override this behaviour by setting the measurement unit.

```swift
beelineNavigationViewController.set(measurementUnit: .metric) // km
```

## FAQ

> **What happens if the user goes off route?**
> <br>
> - It will automatically reroute the user

> **What happens if the phone loses a data connection?**
> <br>
> - A data connection is required to fetch an initial route if a destination was provided. In the case of no data connection Beeline will display an error and retry once a data connection is available
> - If the user goes off route and no data connection is available Beeline will display its off route arrow and try to reroute once a data connection is available

<a name="installation"/>

## Installation

### Install SDK
- [CocoaPods install guide](#cocoapods)
- [Carthage install guide](#carthage)

### Configure your Xcode project for location updates
- [x] Specify a *NSLocationWhenInUseUsageDescription* in your app's `Info.plist`
- [x] Enable *Location updates* in your app's *Background modes*


> The Beeline navigation SDK will automatically request permission when a `BeelineNavigationViewController` is presented if permission has yet to be requested.

<a name="cocoapods"/>

### CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

> CocoaPods 1.1+ is required to build BeelineNavigation.

To integrate **BeelineNavigation** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '9.0'
use_frameworks!

target '<Your Target Name>' do
pod 'BeelineNavigation', '~> 1.0'
end
```

Then run the following command:

```bash
$ pod install
```

<a name="carthage"/>

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate **BeelineNavigation** into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "RideBeeline/beeline-ios-navigation" ~> 1.0
```

Run `carthage update` to build the framework and drag the built `BeelineNavigation.framework` into your Xcode project.
