Leku
====

<img src="media/leku_logo.png" align="left" height="168px"/>
<img align="left" width="0" height="168px" hspace="10"/>

*A Location Picker for Android*

![leku-v1](https://img.shields.io/badge/leku-v1.0.0-green.svg)

Location picker component for Android. It returns a latitude,longitude and an address based on the location picked in the LocationPickerActivity provided.

<br/>
<p align="center">
<b><a href="#features">Features</a></b>
|
<b><a href="#download">Download</a></b>
|
<b><a href="#permissions">Permissions</a></b>
|
<b><a href="#usage">Usage</a></b>
|
<b><a href="#localization">Localization</a></b>
|
<b><a href="#customization">Customization</a></b>
|
<b><a href="#tracking">Tracking</a></b>
|
<b><a href="#extra">Extra</a></b>
|
<b><a href="#who-made-this">Who Made This</a></b>
|
<b><a href="#apps-using-leku">Apps using Leku</a></b>
|
<b><a href="#contribute">Contribute</a></b>
|
<b><a href="#bugs-and-feedback">Bugs and Feedback</a></b>
|
<b><a href="#license">License</a></b>
</p>
<br/>

### Features

<img align="right" width="0" height="328px" hspace="20"/>
<img src="media/screenshot.gif" height="328px" align="right" />

* Search by voice
* Search by text
* Geo Location by GPS, network
* Pick locations using "touch" gestures on the map
* Customization (Theme and layout)
* Events Tracking
* Multi-language support (English and Spanish supported by default)
<br><br><br>


### Prerequisites

minSdkVersion >= 15

### Download

Include the dependency:

```groovy
dependencies {
    compile 'com.schibstedspain.android:leku:1.0.0'
}
```

### Permissions

You must add the following permissions in order to use the Google Maps Android API:

* **android.permission.INTERNET**   Used by the API to download map tiles from Google Maps servers.

* **android.permission.ACCESS_NETWORK_STATE**   Allows the API to check the connection status in order to determine whether data can be downloaded.

* **android.permission.WRITE_EXTERNAL_STORAGE**   Allows the API to cache map tile data in the device's external storage area.

The following two permissions are not required to use Google Maps Android API v2, but are recommended.

* **android.permission.ACCESS_COARSE_LOCATION**   Allows the API to use WiFi or mobile cell data (or both) to determine the device's location. The API returns the location with an accuracy approximately equivalent to a city block.

* **android.permission.ACCESS_FINE_LOCATION**   Allows the API to determine as precise a location as possible from the available location providers, including the Global Positioning System (GPS) as well as WiFi and mobile cell data.

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

### Usage

To use the LocationPickerActivity first you need to add these lines to your AndroidManifest file:

```xml
<activity
    android:name="com.schibstedspain.leku.LocationPickerActivity"
    android:label="@string/title_activity_location_picker"
    android:theme="@style/Theme.AppCompat.Light.NoActionBar"
    android:windowSoftInputMode="adjustPan"
    android:parentActivityName=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEARCH" />
    </intent-filter>
    <meta-data android:name="android.app.searchable"
        android:resource="@xml/searchable" />
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity" />
</activity>
```

Then you have setup the call to start this activity wherever you like, always as startActivityForResult:

```java
Intent i = new Intent(this, LocationPickerActivity.class);
startActivityForResult(i, 1);
```

And add the response code from that activity:

```java

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {

    if (requestCode == 1) {
        if(resultCode == RESULT_OK){
            double latitude = data.getDoubleExtra(LocationPickerActivity.LATITUDE, 0);
            Log.d("LATITUDE****", String.valueOf(latitude));
            double longitude = data.getDoubleExtra(LocationPickerActivity.LONGITUDE, 0);
            Log.d("LONGITUDE****", String.valueOf(longitude));
            String address = data.getStringExtra(LocationPickerActivity.LOCATION_ADDRESS);
            Log.d("ADDRESS****", String.valueOf(address));
            String postalcode = data.getStringExtra(LocationPickerActivity.ZIPCODE);
            Log.d("POSTALCODE****", String.valueOf(postalcode));
            Bundle bundle = data.getBundleExtra(LocationPickerActivity.TRANSITION_BUNDLE);
            Log.d("BUNDLE TEXT****", bundle.getString("test"));
            Address fullAddress = data.getParcelableExtra(LocationPickerActivity.ADDRESS);
            Log.d("FULL ADDRESS****", fullAddress.toString());
        }
        if (resultCode == RESULT_CANCELED) {
            //Write your code if there's no result
        }
    }
}
```

That's all folks!


#### Localization

If you would like to add more language translations the only thing you have to do is:

1. Crate a new strings resource folder and file for your language like "/values-ru".
2. Add all text translations for those strings:

```xml
<string name="title_activity_location_picker">Location Picker</string>
<string name="load_location_error">Something went wrong. Please try again.</string>
<string name="no_search_results">There are no results for your search</string>
<string name="unknown_location">unknown location</string>
<string name="voice_search_promp">Search by voice…</string>
<string name="voice_search_extra_language">en-EN</string>
<string name="toolbar_action_voice_title">Voice</string>
<string name="search_hint">Search</string>
```

Note that you have the **voice_search_extra_language** that is used for the language of the voice recognition.
Replace it with the allowed voice recognition locale for your language.

I encourage you to add these languages to this component, please fork this project and submit new languages with a PR. Thanks!


#### Transition Bundle

If you need to send and receive a param through the LocationPickerActivity you can do it.
Also you can add a default location to load when you start the activity.
You only need to add an "Extra" param to the intent like:

```java
Intent intent = new Intent(getApplicationContext(), LocationPickerActivity.class);
intent.putExtra(LocationPickerActivity.LATITUDE, 41.4036299);
intent.putExtra(LocationPickerActivity.LONGITUDE, 2.1743558);
intent.putExtra("test", "this is a test");
startActivityForResult(intent, 1);
```

And parse it on onActivityResult callback:

```java
Bundle bundle = data.getBundleExtra(LocationPickerActivity.TRANSITION_BUNDLE);
String test = bundle.getString("test");
```

#### Customization

##### Theming

This library uses AppCompat, so should use Theme.AppCompat or descendant in manifest.

```xml
<item name="colorPrimary">#E91E63</item>
<item name="colorPrimaryDark">#C51162</item>
<item name="colorAccent">#FBC02D</item>
<item name="colorControlActivated">#E91E63</item>
```

> `colorControlActivated` is used to colorize Street title, if not set, it uses colorAccent by default

##### Layout

It's possible to hide or show some of the information shown after selecting a location.
Using tha bundle parameter **LocationPickerActivity.LAYOUTS_TO_HIDE** you can change the visibility of the street, city or the zipcode.

```java
intent.putExtra(LocationPickerActivity.LAYOUTS_TO_HIDE, "street|city|zipcode");
```

##### Search Zone

By default the search will be restricted to a zone determined by your default locale. If you want to force the search zone you can do it by adding this line with the locale preferred:

```java
intent.putExtra(LocationPickerActivity.SEARCH_ZONE, "es_ES");
```

#### Tracking

Optionally, you can set a tracking events listener. Implement LocationPickerTracker interface, and set it in your Application class as follows:

```java
LocationPicker.setTracker(new <<YourOwnTracker implementing LocationPickerTracker>>());
```
Available tracking events are:

|TAG|Message|
|---|---|
|didLoadLocationPicker|Location Picker|
|didSearchLocations |Click on search for locations|
|didLocalizeMe|Click on localize me|
|didLocalizeByPoi|Long click on map|
|RESULT_OK|Return location|
|CANCEL|Return without location|


#### Extra

If you would like to use the Geocoder presenter (MVP) used for this use case you are free to use it!
GeocoderPresenter has three methods:

* ***getLastKnownLocation:***
Which obviously returns the last known user location as a ***Location*** object.

* ***getFromLocationName(String query):***
Returns a ***List`<`Address`>`*** for the text introduced.

* ***getFromLocationName(String query, LatLng lowerLeft, LatLng upperRight):***
Returns a ***List`<`Address`>`*** for the text and the Rectangle introduced.

* ***getInfoFromLocation(double latitude, double longitude):***
Returns a ***List`<`Address`>`*** based on a latitude and a longitude.


To use it first you need to implement the GeocoderViewInterface interface in your class like:

```java
public class LocationPickerActivity extends AppCompatActivity implements GeocoderViewInterface {
```

Then you need to setup the presenter:

```java
private GeocoderPresenter geocoderPresenter;

@Override protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  ***
  Geocoder geocoder = new Geocoder(this, Locale.getDefault());
    geocoderPresenter = new GeocoderPresenter(getApplicationContext(), new GeocoderInteractor(geocoder));
    geocoderPresenter.setUI(this);
  ***
}
```

And besides filling the interface methods you have to add some things to your activity/fragment lifecycle to ensure that there are no leaks.

```java

@Override
protected void onStart() {
    super.onStart();
    geocoderPresenter.setUI(this);
}

@Override
protected void onStop() {
    geocoderPresenter.stop();
    super.onStop();
}
```

##### Tests

Note: If you need to execute the Espresso test you will need to add the Google Maps Key into the Tests AndroidManifest.xml


Now you have all you need. :)


##### Important

Searching using the "SearchView" (geocoder) will be restricted to a zone if you are with a Locale from: US, UK, France, Italy and Spain. If not, the search will return results from all the world.


Sample usage
------------

We provide a sample project which provides runnable code samples that demonstrate their use in Android applications.
Note that you need to include your Google Play services key in the sample to be able to test it.


Who made this
--------------

| <a href="https://github.com/ferranpons"><img src="https://avatars2.githubusercontent.com/u/1225463?v=3&s=460" alt="Ferran Pons" align="left" height="100" width="100" /></a>
|---
| [Ferran Pons](https://github.com/ferranpons)


#### Contributors

<a href="https://github.com/DiegoMillanR"><img src="https://avatars0.githubusercontent.com/u/9133635?v=3&s=460" alt="Diego Millán" align="left" height="80" width="80" /></a> | <a href="https://github.com/gerardpedrenyscmspain"><img src="https://avatars0.githubusercontent.com/u/9216185?v=3&s=460" alt="Gerard Pedreny" align="left" height="80" width="80" /></a> | <a href="https://github.com/marcserrascmspain"><img src="https://avatars1.githubusercontent.com/u/8959720?v=3&s=460" alt="Marc Serra" align="left" height="80" width="80" /></a> | <a href="https://github.com/sergiocastilloscmspain"><img src="https://avatars2.githubusercontent.com/u/8904364?v=3&s=460" alt="Sergio Castillo" align="left" height="80" width="80" /></a> | <a href="https://github.com/alorma"><img src="https://avatars3.githubusercontent.com/u/887462?v=3&s=460" alt="Bernat Borras" align="left" height="80" width="80" /></a>
---|---|---|---|---|
[Diego Millán](https://github.com/DiegoMillanR) | [Gerard Pedreny](https://github.com/gerardpedrenyscmspain) | [Marc Serra](https://github.com/marcserrascmspain) | [Sergio Castillo](https://github.com/sergiocastilloscmspain) | [Bernat Borras](https://github.com/alorma)


Apps using Leku
---------------

The following is a list of some of the public apps using Leku and are published on the Google Play Store.

Want to add your app? Found an app that no longer works or no longer uses Leku? Please submit a pull request on GitHub to update this page!

| <a href="https://play.google.com/store/apps/details?id=com.anuntis.segundamano"><img src="media/vibbo_logo.png" align="left" width="68px" height="68px"/></a>
|---
| [vibbo](https://play.google.com/store/apps/details?id=com.anuntis.segundamano)


Contribute
----------

1. Create an issue to discuss about your idea
2. [Fork it] (https://github.com/SchibstedSpain/leku/fork)
3. Create your feature branch (`git checkout -b my-new-feature`)
4. Commit your changes (`git commit -am 'Add some feature'`)
5. Push to the branch (`git push origin my-new-feature`)
6. Create a new Pull Request
7. Profit! :white_check_mark:


Bugs and Feedback
-----------------

For bugs, questions and discussions please use the [Github Issues](https://github.com/SchibstedSpain/leku/issues).


License
-------

Copyright 2016 Schibsted Classified Media Spain S.L.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.