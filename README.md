Localytics for PhoneGap/Cordova
========

## Version

This version of the PhoneGap/Cordova SDK wraps v3.8.0 of the Localytics Andriod

## Supported Versions

The PhoneGap/Cordova SDK was tested on Cordova v6.1.1 and 6.2.0

## Installation

	cordova plugin add localytics-cordova --variable YOUR_APP_KEY=value --variable PACKAGE_NAME=value --variable YOUR_PUSH_ID=value

## Integration

To install Localytics for Phonegap, you'll need to take three basic steps.

1. Download after_platform_add source from https://github.com/JasonElbert/Hooks.git and add to Hook folder in your phonegap/cordova project.

2. Set up your android project and Integrate your app with Localytics.

3. Set up and register for Push notifications, if necessary.


### 1. Set up app keys

App keys allow you to separate data. Create one set of app keys for each app, so you can focus on one app at a time in the Dashboard (https://www.localytics.com/). 

You’ll need an app key for each device platform, such as iOS, Android, Windows, or web. Separate test data from production data by using separate app keys.

When you release your app to the app store, make sure your production app key is in it! You can feel free to delete your test app keys and make new ones whenever you want.

#### Android

Please download hook source from https://github.com/JasonElbert/Hooks.git.

Add this source to your project's hook folder.

Set up your android platform.

### 2. Automatic or manual integration?

With automatic integration, the plugin automatically opens, closes, and uploads sessions when the app goes into the background and foreground.

With manual integration, you have full control of open, close, and upload events, but you'll need to listen for pause and resume events and handle opens, closes, and uploading sessions manually.

Regardless of the integration method you choose, start by adding the following listener:

	document.addEventListener('deviceready', this.onDeviceReady, false);        


#### Automatic integration

Add the following for automatic integration.

	onDeviceReady: function() {
		Localytics.autoIntegrate();
		Localytics.openSession(); // For Android, we might have missed the call to open a session by the time autoIntegrate is called. Don't worry, calling this will not open a second session.
	}

#### Manual Integration

Add the following for manual integration.

	onDeviceReady: function() {
		document.addEventListener("resume", app.onResume, false);
        document.addEventListener("pause", app.onPause, false);
        Localytics.integrate();
        Localytics.openSession();                
        Localytics.upload();
	}
	onResume: function () {
        Localytics.openSession();
        Localytics.upload();
    },
    onPause: function () {
        Localytics.closeSession();
        Localytics.upload();
    },


Additionally, for each platform:

### 3. Set up and register for push notifications

iOS uses Apple Push Notification (APN) while Android uses Google Cloud Messaging (GCM). Follow the instructions for each respective push notification service to set up the necessary configurations and upload the certificate to the Localytics Dashboard before continuing with these instructions.

>*Note*: In-App messaging is not supported on Android at this time. The native Android implementation to handle in-app messaging makes use of the FragmentActivity class that is incompatible with the primary CordovaActivity class that holds the Webview.


#### Android

[Follow these instructions to set up push notifications for your app.](http://docs.localytics.com/dev/android.html#push-messaging-android)

Next, copy the Google Play Services library and add it as a dependency to your project:

1. Copy the folder \<ANDROID_SDK_DIR\>/extras/google/google\_play\_services\_lib/ to \<YOUR_PROJECT\>/platforms/android/
2. Add an extra line to \<YOUR_PROJECT\>/platforms/android/project.properties: "android.library.reference.2=google-play-services_lib


Afterwards, simply call the following function after the integration code in the previous step.

	Localytics.registerPush();



After integrating, tagging events and any further instrumentation should be done inside the web app.

### Event tagging

Anywhere in your application where an interesting event occurs, you can tag it by adding the following line of code, where “Options Saved” is a string describing the event:

	Localytics.tagEvent("Options Saved", null,  0);

Sometimes you might want to collect additional data about the event, like how many lives the player has, or what the last action the user took was before clicking on an ad. Use the second parameter of tagEvent to do this. It takes a hash of attributes and values.

	Localytics.tagEvent("Options Saved", { "Display Units" : "MPH", "Age Range" : "18-25" },  0);

The third parameter of tagEvent is used to increase customer lifetime value (CLV). If the user makes a purchase, you might specify the price of the purchase in cents.

	Localytics.tagEvent("Purchase Completed", { "Item Name" : "Power Up" }, 499);

## SampleApp
The Cordova app found under SampleApp demonstrates a list of functional APIs that can be called via the JavaScript interface. Update "LocalyticsAppKey" and "LOCALYTICS_APP_KEY" for their respective platforms to easily test out the calls.
