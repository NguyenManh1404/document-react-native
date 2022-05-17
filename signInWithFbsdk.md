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
