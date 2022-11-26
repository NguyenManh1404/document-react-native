# Những thư viện hữu dụng

## Slide carousel

1. https://www.npmjs.com/package/react-native-snap-carousel

2. Custom: https://youtu.be/B5WU6qSGw9o

## Image hiển thị hình ảnh

1. https://www.npmjs.com/package/react-native-fast-image

## Redux

1. https://youtu.be/aBqJjI-c-As

2. https://youtu.be/iBUJVy8phqw

## MapView

1. yarn add react-native-maps 
2. podInstall

### Set up cho android
1. `For Android: add the following line in your AndroidManifest.xml`
```js


<uses-permission android:name="android.permission.INTERNET" />


 <application
        android:usesCleartextTraffic="true"
        tools:targetApi="28"
        tools:ignore="GoogleAppIndexingWarning">
        <!-- Add mapView -->
         <meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="API_KEY" />
    <!-- Add mapView -->
        <activity android:name="com.facebook.react.devsupport.DevSettingsActivity" android:exported="false" />
    </application>
```
2. Get API_KEY https://developers.google.com/maps/documentation/javascript/get-api-key
3. Use in Screen

<details>

```js
import {StyleSheet, Text, View} from 'react-native';
import React from 'react';

import MapView, {Callout, Marker, PROVIDER_GOOGLE} from 'react-native-maps';
const App = () => {
  return (
    <View>
      <Text>Mapdemo</Text>
      <View>
        <MapView
          provider={PROVIDER_GOOGLE}
          zoomControlEnabled={true}
          zoomEnabled={true}
          style={{
            height: 180,
          }}
          region={{
            latitude: 16.069560532577032,
            longitude: 108.23408695899637,
            latitudeDelta: 0.015,
            longitudeDelta: 0.0121,
          }}>
          <Marker
            coordinate={{
              latitude: 16.069560532577032,
              longitude: 108.23408695899637,
            }}>
            <Callout>
              <Text>Address</Text>
            </Callout>
          </Marker>
        </MapView>
      </View>
    </View>
  );
};

export default App;

const styles = StyleSheet.create({
  container: {
    marginTop: 10,
    paddingVertical: 10,
    borderRadius: 5,
    borderWidth: 1,
    paddingHorizontal: 15,
    justifyContent: 'center',
  },
});

```
</details>

### Setup cho ios
1. add this line in **./ios/mtk_user_app/AppDelegate.mm**
```js
#import <GoogleMaps/GoogleMaps.h> //add google map
#import "AppDelegate.h"

#import <React/RCTBridge.h>
.....

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  RCTAppSetupPrepareApp(application);

  // //add google map
+  [GMSServices provideAPIKey:@"AIzaSyDswlxkWl-Wxyitg4T1-WXytnofuUUMNhM"]; 
  // //add google map

  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];

#if RCT_NEW_ARCH_ENABLED
.....
}
```
2. Add this line in to **./ios/Podfile**

```js
platform :ios, '13.0' //ios chỉ dùng 13.0
install! 'cocoapods', :deterministic_uuids => false

target 'mtk_user_app' do

  // //add google map
  rn_maps_path = '../node_modules/react-native-maps'
  pod 'react-native-google-maps', :path => rn_maps_path
  pod 'GoogleMaps'
  pod 'Google-Maps-iOS-Utils'
  // //add google map

  config = use_native_modules!
  ......
```
3. Yarn pod và cài lại