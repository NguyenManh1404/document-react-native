# Permission select image android 13

-  "react-native-permissions": "3.8.0",
-   "react-native-image-crop-picker": "^0.40.2",



```js
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES"/>



export const galleryOption: any = {
  android: PERMISSIONS.ANDROID.READ_EXTERNAL_STORAGE,
  android:
    Platform.Version >= 33
      ? PERMISSIONS.ANDROID.READ_MEDIA_IMAGES
      : PERMISSIONS.ANDROID.READ_EXTERNAL_STORAGE,
  ios: PERMISSIONS.IOS.PHOTO_LIBRARY,
};
```



<details>
    <summary><b> Allow Access Notification and location</b></summary>


```js


import React, { useEffect } from 'react';
import {
  BackHandler,
  Image,
  Platform,
  Linking,
  SafeAreaView,
  StyleSheet,
  AppState,
} from 'react-native';

import { PERMISSIONS, RESULTS, check, request } from 'react-native-permissions';

import { Screen } from '../../constants';

import Utils from '../../utils';
import navigation from '../../navigation';
import { Colors } from '../../themes';
import { translate } from '../../i18n';

const { allowPermissionNotification, optionLocation } = Utils;

interface AllowPermissionProps {
  componentId: string;
  pageId: string;
  source: any;
  title: string;
  message: string;
  onPress: () => void;
  dispatch?: () => void;
}

const SHOULD_REQUEST_NOTIFICATION_ANDROID = Platform.OS === 'android' && Platform.Version >= 33;

const AllowPermission = ({
  componentId,
  source,
  title,
  pageId,
  message,
  onPress,
  dispatch,
}: AllowPermissionProps) => {
  const onDismiss = (): void => {
    navigation.dismissModal({ componentId });
    onPress && onPress();
  };

  const onAllowAccess = async () => {
    try {
      switch (pageId) {
        case 'NotificationPage':
          if (SHOULD_REQUEST_NOTIFICATION_ANDROID) {
            const requestAndroidResult = await request(PERMISSIONS.ANDROID.POST_NOTIFICATIONS);
            if (requestAndroidResult === RESULTS.GRANTED) {
              onPress && onPress();
            } else {
              Linking.openSettings();
            }
          }
          break;
        case 'LocationPage':
          const requestAndroidResult = await request(PERMISSIONS.ANDROID.ACCESS_FINE_LOCATION);
          console.log('requestAndroidResult', requestAndroidResult);
          if (requestAndroidResult === RESULTS.GRANTED) {
            onPress && onPress();
          } else if (requestAndroidResult === RESULTS.BLOCKED) {
            Linking.openSettings();
          } else {
            Linking.openSettings();
          }
          break;
        default:
          break;
      }
    } catch (error) {}
  };

  const onCheckStateLocation = async () => {
    const result = await check(Platform.select(optionLocation));
    if (result === RESULTS.GRANTED) {
      onDismiss();
    }
  };

  const handleAppStateChange = (nextAppState: string) => {
    if (nextAppState === 'active') {
      switch (pageId) {
        case 'NotificationPage':
          allowPermissionNotification({ onDismiss: () => onDismiss() });
          break;
        case 'LocationPage':
          onCheckStateLocation();
          break;
        default:
          break;
      }
    }
  };

  useEffect(() => {
    const backAction = () => true;
    const backHandler = BackHandler.addEventListener('hardwareBackPress', backAction);
    return () => backHandler.remove();
  }, []);

  useEffect(() => {
    AppState.addEventListener('change', handleAppStateChange);
    return () => {
      AppState.removeEventListener('change', handleAppStateChange);
    };
  }, []);

  useEffect(() => {
    if (pageId === 'LocationPage') {
      dispatch && dispatch();
    }
  }, [pageId]);

  return (
    <SafeAreaView style={styles.container}>
      <WView fill pHoz={16}>
        <Image source={source} style={styles.image} resizeMode="contain" />
        <WText type="medium20" textAlign="center" color={Colors.black} mTop={64}>
          {title}
        </WText>
        <WText
          type="regular14"
          color={Colors.blackB40}
          textAlign="center"
          mHoz={16}
          mTop={8}
          lineHeight={20}
        >
          {message}
        </WText>
        <WView alignCenter justifyCenter>
          <WButton
            title={translate('allowAccess')}
            onPress={onAllowAccess}
            disabled={false}
            mTop={40}
            w="65%"
          />
          {pageId === 'NotificationPage' && (
            <WButton color={'transparent'} onPress={onDismiss} disabled={false} mTop={16}>
              <WText type="regular16" color={Colors.primary}>
                {translate('notNow')}
              </WText>
            </WButton>
          )}
        </WView>
      </WView>
    </SafeAreaView>
  );
};

export default AllowPermission;

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: Colors.pageBackground,
  },
  image: {
    height: Screen.WIDTH / (3 / 2),
    marginTop: 84,
    width: '100%',
  },
});

```


</details>




<details>
    <summary><b> permission react native on IOS and android</b></summary>


``` js
//ios
    "postinstall": "npx patch-package && yarn createIconFont && npx react-native setup-ios-permissions"

......
  "postinstall": "npx patch-package && yarn createIconFont && npx react-native setup-ios-permissions"
  },
  "reactNativePermissionsIOS": [
    "Camera",
    "Contacts",
    "LocationAlways",
    "LocationWhenInUse",
    "MediaLibrary",
    "Notifications",
    "PhotoLibrary",
    "PhotoLibraryAddOnly"
  ],
  "dependencies": {

    ...



//android


 <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES"/>
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    <uses-permission android:name="android.permission.WRITE_CONTACTS" />
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>

    <application
      android:name=".MainApplication"
      android:label="@string/APP_NAME"


//full js request permissions

import { checkNotifications, RESULTS, check, PERMISSIONS, request } from 'react-native-permissions';


export const optionLocation: any = {
  android:
    PERMISSIONS.ANDROID.ACCESS_FINE_LOCATION || PERMISSIONS.ANDROID.ACCESS_BACKGROUND_LOCATION,
  ios: PERMISSIONS.IOS.LOCATION_WHEN_IN_USE || PERMISSIONS.IOS.LOCATION_ALWAYS,
};

export const optionContacts: any = {
  android: PERMISSIONS.ANDROID.READ_CONTACTS,
  ios: PERMISSIONS.IOS.CONTACTS,
};

export const cameraOption: any = {
  android: PERMISSIONS.ANDROID.CAMERA,
  ios: PERMISSIONS.IOS.CAMERA,
};

export const galleryOption: any = {
  android:
    Platform.Version >= 33
      ? PERMISSIONS.ANDROID.READ_MEDIA_IMAGES
      : PERMISSIONS.ANDROID.READ_EXTERNAL_STORAGE,
  ios: PERMISSIONS.IOS.PHOTO_LIBRARY,
};

export const allowPermissionNotification = async ({
  onPress,
  onDismiss,
}: {
  onPress?: () => void;
  onDismiss?: () => void;
}): Promise<void> => {
  try {
    if (isIOS) {
      const authStatus = await messaging().requestPermission();
      const enabled =
        authStatus === messaging.AuthorizationStatus.AUTHORIZED ||
        authStatus === messaging.AuthorizationStatus.PROVISIONAL;
      if (enabled) {
        const isRegisteredForRemote = messaging().isDeviceRegisteredForRemoteMessages;
        if (!isRegisteredForRemote) {
          await messaging().registerDeviceForRemoteMessages();
        }
        onDismiss && onDismiss();
      } else if (authStatus === messaging.AuthorizationStatus.DENIED) {
        onPress && onPress();
      }
    } else {
      const result: any = await checkNotifications();
      if (result?.status === RESULTS.GRANTED) {
        onDismiss && onDismiss();
      } else if (result?.status !== RESULTS.GRANTED) {
        onPress && onPress();
      }
    }
  } catch (err) {
  }
};

export const requestPermission = async (onPress: () => void, options: any) => {
  const result = await request(Platform.select(options));
  if (result === RESULTS.GRANTED || result === RESULTS.LIMITED) {
    if (typeof onPress === 'function') {
      onPress();
    }
  }
  return result;
};

export const allowPermission = async ({
  onPress,
  onBlocked,
  options,
  title,
  message,
  confirmationText,
  dismissText,
  type,
}: PropsPermission) => {
  const result = await check(Platform.select(options));

  switch (result) {
    case RESULTS.GRANTED:
      onPress && onPress();
      return result;
    case RESULTS.DENIED:
      if (type === 'AllowLocation' && Utils.isAndroid) {
        onBlocked && onBlocked();
      } else {
        requestPermission(onPress, options);
      }
      break;
    case RESULTS.LIMITED:
      onPress && onPress();
      break;
    case RESULTS.BLOCKED:
      if (typeof onBlocked === 'function') {
        onBlocked && onBlocked();
      } else {
        showConfirmationAlert({
          title: `"${appName}" ${title}`,
          message,
          confirmationText: confirmationText || 'Allow',
          dismissText,
          onCancel: () => {
            // TODO: handle cancel
          },
          onPress: () => Linking.openSettings(),
        });
      }
      break;
    default:
      break;
  }
  return result;
};

export const allowPermissionLocation = async ({
  onGranted,
  onBlocked,
}: PermissionFunctionProps) => {
  allowPermission({
    onPress: () => onGranted && onGranted(),
    onBlocked: () => onBlocked && onBlocked(),
  });
};

export const allowPermissionContacts = async ({
  onGranted,
  onBlocked,
}: PermissionFunctionProps) => {
  allowPermission({
    options: optionContacts,
    onPress: () => onGranted && onGranted(),
    onBlocked: () => onBlocked && onBlocked(),
  });
};

export const allowPermissionCamera = ({ onGranted }: PermissionFunctionProps) => {
  allowPermission({
    onPress: () => onGranted && onGranted(),

  });
};

export const allowPermissionPhotos = ({ onGranted }: PermissionFunctionProps) => {
  allowPermission({
    onPress: () => onGranted && onGranted(),
    options: galleryOption,
  });
};

```