# Đăng nhập nhanh bằng FaceBook

vip https://developers.facebook.com/docs/android/getting-started/#ad-id-permissions

https://www.npmjs.com/package/react-native-fbsdk-next

https://youtu.be/fDK7N82Szho

vip: https://aboutreact.com/react-native-facebook-login/

1. **npm i react-native-fbsdk-next** : thư viện giúp đăng nhập nhanh với facebook
2.

- Go to **Android Studio | New Project | Minimum SDK**.

- Select API 15: **Android 4.0.3**(IceCreamSandwich) or higher and create your new project.

Hoặc: vào **Android SDK** trong phần **tools** đảm bảm đã cài **Android 4.0.3**(IceCreamSandwich)

3. Vào **android\app\build.gradle** thêm 1 dòng code này vào:

```js
  dependencies {

  ......

    implementation 'com.facebook.android:facebook-android-sdk:latest.release' //login with facebook fbsdk

   .......


  }

```

4. Lấy **app id, client token**

- Vào trang https://developers.facebook.com/apps/525090575748257/settings/basic/ để lấy **app id**
  - Điền những trường còn thiếu:
    - Privacy Policy URL: https://api.jasacadamy.com/privacy
    - Terms of Service URL: https://api.jasacadamy.com/terms
    - Key hashes: Chạy cmd chứa thư mục của dự án đang dev:
      **keytool -exportcert -alias androiddebugkey -keystore ~/.android/debug.keystore | openssl sha1 -binary | openssl base64**. Đảm bảo cài đặt **openssl-0.9.8k_X64** vào setup môi trường **path: C:\Program Files\openssl-0.9.8k_X64\bin**
- Vào trang https://developers.facebook.com/apps/525090575748257/settings/advanced/ để lấy **client token**

5. Vào: **android\app\src\main\res\values\strings.xml**

- Dán 2 dòng này thêm vào:

```js
<string name="facebook_app_id">app id</string>
<string name="facebook_client_token">client token</string>
```

- Lưu ý **app id, client token** được lấy từ bước 4

6. Mở **/app/manifests/AndroidManifest.xml**

- Dán 2 dòng code này vào file:

```js
<application>
    ....
       <!-- sign in with facebook -->
    <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id"/>
    <meta-data android:name="com.facebook.sdk.ClientToken" android:value="@string/facebook_client_token"/>
    <!-- sign in with facebook -->
    ...

</application>

```

7. Sử dụng trong **LoginScreen.js**

```js
import React, { useEffect } from "react";
import { SafeAreaView, StyleSheet, Text } from "react-native";

import { LoginButton, AccessToken } from "react-native-fbsdk-next";

const LoginScreen = ({ navigation }) => {
  useEffect(() => {
    AccessToken.getCurrentAccessToken().then((data) => {
      console.log(data.accessToken.toString());
      if (data) {
        navigation.navigate("Home");
      }
    });
  });

  return (
    <SafeAreaView
      style={{ alignItems: "center", justifyContent: "center", flex: 1 }}
    >
      <Text>Login With Facebook1</Text>
      <LoginButton
        onLoginFinished={(error, result) => {
          console.log(result);
          navigation.navigate("Home");
        }}
      />
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({});

export default LoginScreen;
```

8. Sử dụng trong **HomeScreen.js** để lấy data khi đã đăng nhập

```js
import { StyleSheet, Text, View, Image } from "react-native";
import React, { useEffect, useState } from "react";
import { Profile, LoginButton } from "react-native-fbsdk-next"; // import để

const HomeScreen = ({ navigation }) => {
  const [name, setName] = useState("");
  const [image, setImage] = useState("");

  useEffect(() => {
    Profile.getCurrentProfile().then(function (currentProfile) {
      console.log(currentProfile);
      setName(currentProfile.lastName + " " + currentProfile.firstName);
      setImage(currentProfile.imageURL);
    });
  }, []);

  return (
    <View style={{ justifyContent: "center", alignItems: "center", flex: 1 }}>
      <Text style={{ fontSize: 25, fontWeight: "bold" }}>HomeScreen</Text>
      <Image
        source={{ uri: image || null }}
        style={{ width: 100, height: 100 }}
      />
      <Text style={{ fontSize: 20, fontWeight: "bold" }}>{name}</Text>
      <LoginButton
        onLoginFinished={(error, result) => {
          if (error) {
            alert("login has error: " + result.error);
          } else {
            alert("login is cancelled.");
          }
        }}
        onLogoutFinished={() => navigation.navigate("Login")}
      /> //Thực hiện xong sẽ chạy cái này
    </View>
  );
};

export default HomeScreen;

const styles = StyleSheet.create({});
```
## Dùng import { LoginManager } from 'react-native-fbsdk-next';

```js

//login screen
  import React, { useEffect } from 'react';
import { SafeAreaView, StyleSheet, Text } from 'react-native';

import { LoginButton, AccessToken, LoginManager } from 'react-native-fbsdk-next';
import { TouchableOpacity } from 'react-native-gesture-handler';

import { GoogleSignin } from '@react-native-google-signin/google-signin';
import auth from '@react-native-firebase/auth';

const LoginScreen = ({ navigation }) => {
  useEffect(() => {
    GoogleSignin.configure({
      webClientId:
        '1019029921892-6er0fbsiphn5akq7dqkfq8ph0lb8ca1o.apps.googleusercontent.com',
    });
  }, []);

  const onFacebookButtonPress = () => {
    LoginManager.logInWithPermissions(['public_profile']).then(
      function (result) {
        if (result.isCancelled) {
          console.log('Login cancelled');
        } else {
          AccessToken.getCurrentAccessToken().then(data => {
            const { accessToken } = data;
            if (accessToken) {
              navigation.navigate('Home');
            }
            // login({
            //   token: accessToken,
            // });
          });
        }
      },
      function (error) {
        console.log('Login fail with error: ' + error);
      },
    );
  };

  const onGoogleButtonPress = async () => {
    // Get the users ID token
    const { idToken } = await GoogleSignin.signIn();

    // Create a Google credential with the token
    const googleCredential = auth.GoogleAuthProvider.credential(idToken);
    console.log(googleCredential);
    // // Sign-in the user with the credential
    const user_sign_in = auth().signInWithCredential(googleCredential);

    user_sign_in
      .then(user => {
        console.log(user.additionalUserInfo.profile);

        if (user) {
          navigation.navigate('Home', {
            userInfo: user.additionalUserInfo.profile,
          });
        }
      })
      .catch(error => {
        console.log(error);
      });
  };

  return (
    <SafeAreaView
      style={{ alignItems: 'center', justifyContent: 'center', flex: 1 }}>
      <Text style={{ fontWeight: 'bold', fontSize: 20 }}>Login Screens</Text>

      <TouchableOpacity
        style={{
          backgroundColor: 'blue',
          width: 180,
          height: 30,
          alignItems: 'center',
        }}
        onPress={onFacebookButtonPress}>
        <Text style={{ fontWeight: 'bold', fontSize: 18, color: 'white' }}>
          LoginButtonFacebook
        </Text>
      </TouchableOpacity>

      <Text></Text>

      <TouchableOpacity
        style={{
          backgroundColor: 'orange',
          width: 180,
          height: 30,
          alignItems: 'center',
        }}
        onPress={onGoogleButtonPress}>
        <Text style={{ fontWeight: 'bold', fontSize: 18, color: 'white' }}>
          LoginButton Google
        </Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
};

const styles = StyleSheet.create({});

export default LoginScreen;


//HomeScreen

import {StyleSheet, Text, View, Image, TouchableOpacity} from 'react-native';
import React, {useEffect, useState} from 'react';
import {Profile, LoginButton} from 'react-native-fbsdk-next'; //facebook login

import auth from '@react-native-firebase/auth'; //google-signin

const HomeScreen = ({route, navigation}) => {
  const [name, setName] = useState('');
  const [image, setImage] = useState('');

  const [userInfo, setUseInfo] = useState('');

  useEffect(() => {
    if (route.params) {
      const {userInfo} = route.params;
      setUseInfo(userInfo);
    } else {
      const idTimeout = setTimeout(() => {
        Profile.getCurrentProfile().then(function (currentProfile) {
          console.log(currentProfile);
          setName(currentProfile.lastName + ' ' + currentProfile.firstName);
          setImage(currentProfile.imageURL);
        });
      }, 5000);
      return () => {
        clearTimeout(idTimeout);
      };
    }
  }, []);

  const onLogoutGoogle = () => {
    auth()
      .signOut()
      .then(() => console.log('User signed out!'));

    navigation.navigate('Login');
  };

  return (
    <View style={{justifyContent: 'center', alignItems: 'center', flex: 1}}>
      {userInfo ? (
        <>
          <Text style={{fontSize: 25, fontWeight: 'bold'}}>
            Đăng nhập với Google
          </Text>
          <Image
            source={{uri: userInfo.picture}}
            style={{width: 100, height: 100}}
          />
          <Text style={{fontSize: 20, fontWeight: 'bold'}}>
            {userInfo.name}
          </Text>
          <Text style={{fontSize: 19, fontWeight: 'bold'}}>
            {userInfo.email}
          </Text>

          <TouchableOpacity
            style={{
              backgroundColor: 'orange',
              width: 180,
              height: 30,
              alignItems: 'center',
            }}
            onPress={onLogoutGoogle}>
            <Text>Log Out Google</Text>
          </TouchableOpacity>
        </>
      ) : (
        <>
          <Text style={{fontSize: 25, fontWeight: 'bold'}}>
            Đăng nhập với FaceBook
          </Text>
          <Image
            source={{uri: image || null}}
            style={{width: 100, height: 100}}
          />
          <Text style={{fontSize: 20, fontWeight: 'bold'}}>{name}</Text>
          <LoginButton
            onLoginFinished={(error, result) => {
              if (error) {
                alert('login has error: ' + result.error);
              } else {
                alert('login is cancelled.');
              }
            }}
            onLogoutFinished={() => navigation.navigate('Login')}
          />
        </>
      )}
    </View>
  );
};

export default HomeScreen;

const styles = StyleSheet.create({});

```