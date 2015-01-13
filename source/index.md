---
title: API Reference

language_tabs:
  - objective_c: iOS
  - java: Android

toc_footers:
  - <a href='mailto:sdk@socialradar.com'>Contact us for an API Token</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Overview

## SocialRadar Location Manager

The SocialRadar Location Manager is designed to provide accurate location data to apps which require precise or continuous location services, passive venue check-in and detailed user insights based on the places your users go every day. 

### How it works

SocialRadar SDK processes location signals through a private location manager instance, analyzing activity and validating data within the private SocialRadar cloud. Anonymized consumer insights may be shared with app publishers and marketing firms.

The SocialRadar Location Manager framework works independently of the host app, and other than the app providing match key data to the SDK framework, no interaction between the app and framework is required to enable SocialRadar Location Manager to work properly.

### Operating Requirements

-	Device capable of running iOS 7.x or above (Android coming soon!)
-	Device capable of properly returning location signals

### Battery Consumption Safeguards

Battery consumption is extremely efficient – the SocialRadar SDK averages 1.7% battery consumption per hour, depending on the type of device used. If remaining battery power dips below 21%, the SDK pauses until the battery has been charged.

### Transparent operation

The SocialRadar Location Manager never surfaces dialog boxes, errors or notifications directly to a consumer. 

### User Insights

Consumer interest profiles are indexed with match keys, allowing data to be properly analyzed and cross-referenced by app-created user data. Match keys include:

 * Device ID (IDFA) – automatically obtained by SocialRadar SDK
 * Home IP address – automatically obtained by SocialRadar SDK
 * E-mail address – provided by you through an API interface
 * Proprietary user IDs – provided by you through an API Interface

Match key data is handled in compliance with advertising industry privacy and data handling/security practices in the US and EU.

# Getting Started (iOS)

Adding the SocialRadar SDK to an app is easy:

## 1. Obtain an API Key

For now, email [sdk@socialradar.com](mailto:sdk@socialradar.com) to request a key until our full developer site is ready.

## 2. Retrieve and integrate the SDK

You can either:

### Download Manually

1. Download the latest version [here](#downloads)
1. Add the **SocialRadar.framework** file to your project
1. Using Xcode, navigate to your project target's "General" settings; in the "*Linked Frameworks and Libraries*" section, add **SocialRadar.framework**

### Add SocialRadar as a CocoaPod

The SocialRadar SDK is available as a [CocoaPod](http://cocoadocs.org/docsets/SocialRadarSDK) for ease of integration.

1. Add the following to your Podfile: `pod 'SocialRadarSDK'`
1. Close your project in Xcode and update it by running `pod install` from **Terminal** in your project directory
1. Open your project in Xcode and perform a Clean build

## 3. Configure your Project

In the project target's **Capabilities** section, enable **Background Modes** and enable **Location Updates** as shown in the following screenshot:

![Enable Location Updates](images/background_modes.png)

## 4. Initialize the SocialRadar Location Manager within your app

> For example, if you launch your Apple location manager services from within **AppDelegate.m**, add the following line above the **@implementation** section:

```objective_c
#import <SocialRadar/SocialRadar.h>
```

> From within appdelegate.m’s application:didFinishLaunchingWithOptions: method, add the following lines to initialize and launch SocialRadar Location Manager:

```objective_c
// Initialize
[SocialRadar initializeWithApiToken:@"<yourApiTokenHere>"];

// Start it
[[SocialRadar sharedInstance] startSocialRadarWithPersistenceEnabled: YES];
```

The SocialRadar Location Manager SDK requires the use of the Apple location manager.

Start the SocialRadar Location Manager at the point after your app normally begins collecting location data – ideally, it should be run immediately after requesting your user’s permission to access location data and while the app is still in the foreground.

Remember to replace `<yourAPITokenHere>` with the API token supplied to you by SocialRadar.

The return result of `[SocialRadar sharedInstance]` will be nil if called before the `[SocialRadar initializeWithApiToken:@"<yourApiTokenHere>"]` method is invoked.


## 5. Configuring location services permissions for your app

> We recommend the following InfoPlist.strings file configuration (adjust the language as required by your app):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>NSLocationAlwaysUsageDescription</key>
        <string>Change this line to inform consumer about how location is being used in the background</string>
    </dict>
</plist>
```


Using SocialRadar Location Manager requires the consumer to receive an informational message explaining how location services will be used within the app. 

That informational message is contained in the Info.plist or the InfoPlist.strings file. 

![Info plist for Location permission popup](images/info_plist.png)

If your app will run on iOS 7.x, you must add a `NSLocationUsageDescription` entry in addition to the `NSLocationlAlwaysUsageDescription` entry.

If successfully implemented, the consumer will receive a standard location services notification request dialog box presenting them with a request for location services and why the location services are being requested.

If the consumer chooses not to authorize location services, SocialRadar Location Manager will suspend activity until the consumer authorizes location services.


## 6. Test your implementation of the SDK

> Enable **Developer Diagnostics** by placing the following code immediately following the SDK initialization code:

```objective_c
#if DEBUG
    [[SocialRadar sharedInstance] printDeveloperDiagnostics];
#endif
```

SocialRadar Location Manager operates in the background, and will not perform heavy operations, nor produce NSNotifications nor NSLogs which may interfere with the performance of your app. 

The best way to determine a successful SocialRadar Location Manager implementation is to enable **Developer Diagnostics** when running the app in developement.

If successful, you will see the following in your console:

![Developer Diagnostics printout](images/developer_diagnostics.png)

Remember to remove these debug lines prior to releasing your app to the App Store!

# Getting Started (Android)

Unfortunately, at this time the SocialRadar SDK for Android is currently in internal beta only.

If you are interested in receiving early access, send an email to [sdk@socialradar.com](mailto:sdk@socialradar.com).

# Initialization

> To initialize the SocialRadar SDK, use the following code:

```objective_c
// The following should go in your AppDelegate.m above the @implementation section
#import <SocialRadar/SocialRadar.h>

// The following should go in your AppDelegate.m file in application:didFinishLaunchingWithOptions: method
[SocialRadar initializeWithApiToken:@"<yourApiTokenHere>"];

[[SocialRadar sharedInstance] startSocialRadarWithPersistenceEnabled: YES];
```

```java
import SocialRadar

// This is not real since the Android library has not yet been launched...
```

The SocialRadar SDK uses API tokens to allow access to the API.

Until our developer portal is finished (coming soon!), you can get an API token my emailing [sdk@socialradar.com](mailto:sdk@socialradar.com)

The SocialRadar SDK expects the API token to be included in all API requests to the server and without one, the server will reject requests.

<aside class="notice">
You must replace `<yourApiTokenHere>` with your app's API key.
</aside>

# Downloads

## iOS

### CocoaPod

The SocialRadar SDK (iOS) can be easily installed from CocoaPods.

1. Add the following to your Podfile: `pod 'SocialRadarSDK'`
1. Close your project in Xcode and update it by running `pod install` from **Terminal** in your project directory
1. Open your project in Xcode and perform a Clean build

### Manual Download

* SocialRadar SDK
  * [1.3.5](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.3.5.zip)
  * [1.3.2](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.3.2.zip)
  * [1.3.0](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.3.0.zip)
  * [1.2.8](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.2.8.zip)
  * [1.2.7](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.2.7.zip)
  * [1.2.6](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.2.6.zip)
  * [1.2.5](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-1.2.5.zip)
  * [0.8.3](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-0.8.3.zip)
  * [0.8.2](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-0.8.2.zip)
  * [0.8.1](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-0.8.1.zip)
  * [0.8.0](http://cdn.socialradar.com/sdk/SocialRadarSDK-ios-0.8.zip)
  
## Android

Currently in internal beta, coming soon!

### Maven

### Manual Download


