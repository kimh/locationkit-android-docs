---
title: LocationKit iOS Docs

language_tabs:
    - java: Java

toc_footers:
    - <a href='https://locationkit.io'>Website</a>
    - <a href='https://dashboard.locationkit.io'>Dashboard</a>
    - <a href='https://community.locationkit.io'>Community</a>
    - <a href='https://docs.locationkit.io'>Documentation</a>
    - <a href='https://locationkit.io/developer/faqs'>FAQs</a>
    - <a href='https://locationkit.io/pricing'>Pricing</a>
    - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Overview

[![LocationKit](images/locationkit-textonly.png)](https://locationkit.io)

SocialRadar's [LocationKit](https://locationkit.io) SDK for Android provides
advanced location technology to offer developers higher accuracy location, lower
battery drain, automatic venue recognition and detailed location analytics.

LocationKit enables you to add enhanced location capabilities, and therefore
personalized experiences, to your mobile applications without the need for
location expertise.

Looking for our [iOS documentation](https://docs.locationkit.io/iOS)?

### How it works

LocationKit processes location signals through a private location manager
instance, analyzing activity and validating data within the private SocialRadar
cloud. It pairs this with other sensor information on the device such as the
compass, accelerometer, and others to more accurately determine the user's
location while minimizing battery drain. Location data is seamlessly paired with
SocialRadar's best of breed location database full of addresses, venues, events,
and more which it delivers to your app. Anonymized consumer insights may be
shared with app publishers and marketing firms.

### Operating Requirements

- Device capable of running Android 4.x+
- Device capable of properly returning location signals

### Battery Consumption

Battery consumption is extremely efficient – LocationKit averages low
battery consumption per hour under normal usage, depending on the type of device
used and user activity level.

# Quickstart

## 1. Obtain an API Key

The first thing you'll need to do is to
<a href="https://dashboard.locationkit.io">sign up for an account</a> on our
Developer Dashboard. You will create your account and receive your API token.

Within the Developer Dashboard, you will be able to find your API token, and
find insights into the location-based usage of your app with LocationKit.

We have seeded the Dashboard with some sample data so you can explore the data
and insights your app will get once you've integrated LocationKit.

## 2. Add to build.gradle

Add LocationKit to your project's build.gradle file as shown on the right

> build.gradle

```
repositories {
	mavenCentral()
	maven {
        url 'http://maven.socialradar.com/releases'
	}
}
dependencies {
	compile ('socialradar:locationkit:2.0.1+@aar') { transitive = true }
}
```

## 3. Connect to the LocationKit service

In your application, connect to the LocationKit service to start receiving
location updates.

See the example to the right. In that example we will establish a location
listener for the activity or service created.  You can also register a pending
intent or custom broadcast to handle location data (see alternate examples at
the end of this document).

<aside class="notice">
LocationKit registers a boot receiver and will automatically restart on device restart.
</aside>

```java
  private Boolean mBound = false;
  private LocationKit.LKBinder mLocationKit
  @Override
  protected void onResume() {
    Intent i = new Intent(this, LocationKit.class);
    bindService(i, connection, Context.BIND_AUTO_CREATE);
  }
  @Override
  protected void onDestroy() {
        if (mBound) {
            try {
                unbindService(connection);
                mBound = false;
            } catch (Exception e) {

            }
        }
        super.onDestroy();
  }

  protected ServiceConnection connection = new ServiceConnection() {
    @Override
    public void onServiceConnected(ComponentName name, IBinder service) {
      mBound = true;
      mLocationKit = (LocationKit.LKBinder)service;
      try {
          mLocationKit.startWithApiToken("f0838784beb72a13", mLocationListener);
      } catch (Exception e) {
        e.printStackTrace();
      }
  }
    @Override
    public void onServiceDisconnected(ComponentName name) {
        mLocationKit = null;
        mBound = false;
    }
 };
    private LKListener mLocationListener = new LKListener() {
        @Override
        public void onStartVisit(LKVisit visit) {

        }

        @Override
        public void onEndVisit(LKVisit visit) {

        }

        @Override
        public void onNetworkUnavailable() {

        }

        @Override
        public void onNetworkAvailable() {

        }

        @Override
        public void onLocationManagerDisabled() {

        }

        @Override
        public void onLocationManagerEnabled() {

        }

        @Override
        public void onError(Exception e, String message) {

        }

        @Override
        public void onUnbind() {

        }

        @Override
        public void onLocationChanged(Location location) {

        }

        @Override
        public void onStatusChanged(String provider, int status, Bundle extras) {

        }

        @Override
        public void onProviderEnabled(String provider) {

        }

        @Override
        public void onProviderDisabled(String provider) {

        }
    };
```

# Operation

## Pausing and Resuming

*Use:* Developers may stop LocationKit from running in the background by
invoking `pause()` on the service binder.

```java
mLocationKit.pause(); //stops LocationKit
mLocationKit.resume(); //restarts LocationKit
```

# Event Listening Options

In the example above LocationKit events will be sent to the provided listener as
underlying data changes (for example when the user moves to a new place or
location).  LocationKit provides additional options for receiving events and an
optional interval for location events to be sent continuously regardless of
position change.

1. LocationKit with Event Listener
	-`startWithApiToken(String apiToken, LKListener listener)` -- as shown above
	-`startWithApiToken(String apiToken, LKListener listener, Looper looper)` -- provide an optional looper to recive events off UX thread
	-`startWithApiToken(String apiToken, Long interval, LKListener listener)` -- receive events at a specified interval in milliseconds (e.g. 60*5*1000l is every five minutes)
	-`startWithApiToken(String apiToken, Long interval, LKListener listener, Looper looper)` include a loooper and interval
2. LocationKit with PendingIntent
	-`startWithApiToken(String apiToken, PendingIntent intent)`
	-`startWithApiToken(String apiToken, Long interval, PendingIntent intent)` call pending intent at specified interval (as defined above)

3. LocationKit with custom broadcast action
Developers may implement a custom broadcast receiver to handle these events.  
	-`startWithApiToken(String apiToken, String action)`
	-`startWithApiToken(String apiToken, Long interval, String action) ` 

# Real-time information

In addition to LocationKit events developers may also make requests through the
service binder for additional location related data.

# UserValues

`updateUserValues(LKUserValues values)`
Developers may elect to send additional information to the LocationKit
dashboard. The dashboard provides advanced reporting, analytics, and
management features related to location data about end users.

By sending user demographic values as shown below, you can slice and dice the
location data analytics by these values and create useful segments.

Of course this is entirely optional.

```java
  LKUserValues values = new LKUserValues();
  values.setGender("UNKNOWN");
  values.setAge("18-35");
  values.setIdentifier("Some_Unique_Customer_Id");
  values.setHasInAppPurchases(true);
  values.setInAppPurchases(">$100");
  values.setIncome("$25000-$5000");
  values.setMaritalStatus("Married");
  try {
      mLocationKit.updateUserValues(values);
  } catch (LKServiceNotConnectedException e) {
    //you must call startWithToken(...) above 
  } catch (IOException e) {
  
  }
  ```

# Query Methods
LocationKit enables the developer to query our backend services for additional
and current location data.  These follow a callback pattern so that the
Activity thread will not be blocked.  All query methods use a `LKCallback<T>`
where `T` is the type of data processed. 

> `LKCallback<T>` provides two methods 
```java
 @Override
 public void onError(Exception e, String errorMessage) {
 
 }
 /* invoked when operation is complete. */
 @Override
 public void onReceivedData(T data) {
              
 }
```

## Get the current `LKPlace`

`getCurrentPlace(final LKCallback<LKPlace> callback)`

*Use:* Returns the current place (if availble).

```java
mLocationKit.getCurrentPlace( new LKCallback<LKPlace>() {
    @Override
    public void onError(Exception e, String errorMessage) {
		//error 
    }

    @Override
    public void onReceivedData(final LKPlace data) {
        //do soemthing with data
    }
});
```

## Get the `LKPlace` for a location
`getPlaceForLocation(double latitude, double longitude, final LKCallback<LKPlace> callback)`

*Use:* Same As `getCurrentPlace` but allows a developer provided lat/lng

```java
mLocationKit.getPlaceForLocation( 38.904415, -77.043190, new LKCallback<LKPlace>() {
    @Override
    public void onError(Exception e, String errorMessage) {
		//error 
    }

    @Override
    public void onReceivedData(final LKPlace data) {
        //do something with data
    }
});
```

## Search for places
`searchForPlaces(final LKSearchRequest search, final LKCallback<List<LKPlace>> callback)`

*Use:* Search for types of places based on an `LKSearchRequest`. If
`LKSearchRequest` does not include lat/lng then search will default to the
current location of the device

```java
LKSearchRequest search = new LKSearchRequest();
search.setCategory("Restaurants");
search.setRadius(2000);
mLocationKit.searchForPlaces(search, new LKCallback<List<LKPlace>> () {
    @Override
    public void onError(Exception e, String errorMessage) {
	    //error 
    }

    @Override
    public void onReceivedData(final LKCallback<List<LKPlace>> data) {
        //do something with data
    }
});
```

## Get the current location
`getCurrentLocation(final LKCallback<Location> callback)`

*Use:* Get the current Location of the device.

```
mLocationKit.getCurrentLocation(new LKCallback<Location> callback() {
    @Override
    public void onError(Exception e, String errorMessage) {
	    //error 
    }

    @Override
    public void onReceivedData(final LKCallback<List<LKPlace>> data) {
        //do something with data
    }
});
```

# Change Log

## 2.0.1
<h3>July 27, 2015</h3>

* Initial release with feature parity for most LocationKit iOS functionality

# Downloads

## CocoaPods

LocationKit is primarily available for download on Maven

```
repositories {
	mavenCentral() 
	maven {
        url 'http://maven.socialradar.com/releases'
	}
}
dependencies {
	compile ('socialradar:locationkit:2.0.1+@aar') { transitive = true }
}
```

## Cordova

Cordova support for LocationKit Android coming soon!
