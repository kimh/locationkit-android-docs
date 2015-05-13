---
title: SocialRadar LocationKit - API Reference

language_tabs:
  - objective_c: iOS

toc_footers:
  - <a href='http://www.socialradar.com'>SocialRadar Website</a>
  - <a href='mailto:sdk@socialradar.com'>Email us for an API Token</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Overview

## SocialRadar LocationKit

LocationKit is designed to provide accurate location data to apps which require precise or continuous location services, passive venue check-in and detailed user insights based on the places your users go every day.

### How it works

LocationKit processes location signals through a private location manager instance, analyzing activity and validating data within the private SocialRadar cloud. Anonymized consumer insights may be shared with app publishers and marketing firms.

The LocationKit framework works independently of the host app, and other than the app providing match key data to the framework, no interaction between the app and framework is required to enable LocationKit to work properly.

### Operating Requirements

-	Device capable of running iOS 7.x or above (Android coming soon!)
-	Device capable of properly returning location signals

### Battery Consumption Safeguards

Battery consumption is extremely efficient – LocationKit averages 1.7% battery consumption per hour, depending on the type of device used. If remaining battery power dips below 21%, LocationKit pauses until the battery has been charged.

### Transparent operation

LocationKit never surfaces dialog boxes, errors or notifications directly to a consumer.

# Getting Started

## Obtain an API Key

For now, email [sdk@socialradar.com](mailto:sdk@socialradar.com) to request a key until our full developer site is ready.

## Retrieve and integrate the SDK

1. Our team will send you a file named **LocationKit.zip** when you request a key
1. Unzip the file and add the **LocationKit.framework** artifact to your project
1. Using Xcode, navigate to your project target's *General* settings; in the *Linked Frameworks and Libraries* section, add **LocationKit.framework**, as well as **AdSupport.framework**, **CoreLocation.framework** and **MapKit.framework**

## Configure your Project

In the project target's **Capabilities** section, enable **Background Modes** and enable **Location Updates** as shown in the following screenshot:

![Enable Location Updates](images/background_modes.png)

## Starting and Stopping LocationKit

### Initializing and Starting LocationKit

> For example, if you launch your Apple location manager services from within **AppDelegate.m**, add the following line above the **@implementation** section:

```objective_c
#import <LocationKit/LocationKit.h>
```

> From within appdelegate.m’s application:didFinishLaunchingWithOptions: method, add the following lines to initialize and launch LocationKit:

```objective_c
// Initialize and start LocationKit
[LocationKit startWithToken:@"<yourApiTokenHere>"];
```

SocialRadar LocationKit requires the use of the Apple location manager.

Start LocationKit at the point after your app normally begins collecting location data – ideally, it should be run immediately after requesting your user’s permission to access location data and while the app is still in the foreground.

Remember to replace `<yourAPITokenHere>` with the API token supplied to you by SocialRadar.

### Stopping LocationKit

> To stop LocationKit from within your code, use the following:

```objective_c
// Stop LocationKit
[LocationKit stop];
```

If you would like to stop LocationKit, you can do so using the class method `stop`.

## Configuring permissions

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

Using LocationKit requires the consumer to receive an informational message explaining how location services will be used within the app.

That informational message is contained in the Info.plist or the InfoPlist.strings file.

![Info plist for Location permission popup](images/info_plist.png)

If your app will run on iOS 7.x, you must add a `NSLocationUsageDescription` entry in addition to the `NSLocationlAlwaysUsageDescription` entry.

If successfully implemented, the consumer will receive a standard location services notification request dialog box presenting them with a request for location services and why the location services are being requested.

If the consumer chooses not to authorize location services, LocationKit will suspend activity until the consumer authorizes location services.

## Ensuring it works

> Place the following code immediately following the LocationKit initialization code to obtain some diagnostic information:

```objective_c
[LocatinKit showConfigurationStatus];
```

LocationKit operates in the background, and will not perform heavy operations, nor produce NSNotifications nor NSLogs which may interfere with the performance of your app.

If successful, you will see the following in your console:

![Developer Diagnostics printout](images/developer_diagnostics.png)

Remember to remove these debug lines prior to releasing your app to the App Store!

# Using LocationKit

Now that you've implemented LocationKit, you’re able to obtain location data on demand.

The location data you will receive is a CLLocation object returning a refined coordinate for the user’s current position.

There are two methods of returning data: single point request and continuous streaming.

## Single Location Point Request

> To quickly obtain the user’s most recent location point, execute the following:

```objective_c
[LocationKit getCurrentLocationWithHandler:^(CLLocation *location, NSError *error) {
    if (error == nil) {
        NSLog(@"%.6f, %.6f, %@", location.coordinate.latitude, location.coordinate.longitude, location.timestamp);
    } else {
        NSLog(@"Error: %@", error);
    }
}];
```

Single point requests do not increase battery consumption rates.

You will quickly receive the user's most recent location point to the provided handler.

## Streaming Update Frequency

Selecting the right update frequency when initiating streaming location tracking ensures the right algorithm is used to refine the GPS data.

If you’re unsure which frequency to use, choose **SRFrequencyLevelMedium** – this will provide accurate GPS data useful for most tracking requirements.

The **SRFrequencyLevel** defines the frequency with which you wish to update. Each type is optimized for processing speed, battery life and accuracy. The update frequencies offered are:

* **SRFrequencyLevelLowest** – suitable for continuous very low power GPS signaling (3.3%/hour on average). New location signals will arrive every 1-2 seconds when moving, less frequently if not moving.
* **SRFrequencyLevelLow** — suitable for general fitness, sports, and walking <15mph.
* **SRFrequencyLevelMedium** – tracking for activity speeds <25mph.
* **SRFrequencyLevelHigh** – tracking for speeds <40mph on land and water.
* **SRFrequencyLevelHighest** – automotive tracking with ‘snap-to-road’.

The location object will update as new location data becomes available. This data will include both highly refined and lightly refined data points suitable for mapping an activity.

In order to minimize battery usage, it is recommended that you enable the continuous stream only for the period your app needs that type of GPS update and disable it when that is no longer needed.

For example, for an app that tracks runs, start continuous updates when the user starts their run and stop when the user indicates their run is finished.

## Start Streaming Location Data

> To start streaming location data, use the following (substitute the *SRFrequencyLevelMedium* with the activity tracking option you require):

```objective_c
[LocationKit startLocationUpdatesWithFrequencyLevel:SRFrequencyLevelMedium updateHandler:^(CLLocation *location, NSError *error) {
    if (error == nil) {
        NSLog(@"Activity location %@", location);
    } else {
        NSLog(@"Activity error %@", error);
    }
}];
```

Streaming location data is available when a continuous real-time feed of location data is required.

## Stop Streaming Location Data

> To stop streaming location data, use the following:

```objective_c
[LocationKit stopLocationUpdates];
```

Stopping activity tracking will not stop LocationKit -- it only stops the location tracking function that you have chosen. Note, only one location tracking function will operate at any time.

## Single Venue List Request

> To retrieve a list of venues at the current location, use the following:

```objective_c
[LocationKit getCurrentVenuesWithHandler:^(NSArray *venues, NSError *error) {
  if (error == nil) {
      if(venues.count > 0) {
          SRVenue *venue = venue[0];
          NSLog(@"First Venue name %@", venue.name);
      } else {
          NSLog(@"No Venues found");
      }
  } else {
      NSLog(@"Venues error %@", error);
  }
}];
```

`getCurrentVenuesWithHandler` will retrieve an array of SRVenue objects at the current location.

An SRVenue object contains the name, category and subcategory for a venue, in addition to its location and address information.  The address is represented as an NSDictionary.

## Start Receiving Venue Updates

> To start receiving venue updates, use the following:

```objective_c
[LocationKit startVenueUpdatesWithHandler:^(NSArray *venues, NSError *error) {
  if (error == nil) {
      if(venues.count > 0) {
          SRVenue *venue = venue[0];
          NSLog(@"First Venue name %@", venue.name);
      } else {
          NSLog(@"No Venues found");
      }
  } else {
      NSLog(@"Venues error %@", error);
  }
}];
```

Venue updates will be sent when LocationKit detects that a user has entered a new venue, so there is no frequency setting as with location updates.

## Stop Receiving Venue Updates

> To stop receiving venue updates, use the following:

```objective_c
[LocationKit stopVenueUpdates];
```

Stopping venue updates will not stop LocationKit -- it only stops receiving venue updates.

## Venue Object

Venue objects as returned with `getCurrentVenuesWithHandler` or `startVenueUpdatesWithHandler` have the following properties:

Property | Type | Description
--------- | --------- | ---------
locationCoordinate | CLLocationCoordinate2D | Coordinates of the venue
addressDictionary | NSDictionary | Address of the venue
name | NSString | Name of the venue
category | NSString | Category of the venue
subcategory | NSString | Subcategory of the venue

Below are the keys in `addressDictionary`:

Key | Type | Description
--------- | --------- | ---------
Street | kABPersonAddressStreetKey | Street address of the venue
City | kABPersonAddressCityKey | City of the venue address
State | kABPersonStateKey | State of the venue address
ZIP | kABPersonZIPKey | ZIP Code of the venue address
Country | kABPersonCountryKey | Country of the venue address

# Downloads

## iOS

### CocoaPods

Coming soon!

### Manual Download

Please contact the LocationKit team on [sdk@socialradar.com](mailto:sdk@socialradar.com) to obtain an API key and a copy of LocationKit.

## Android

Coming soon!
