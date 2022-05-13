# Cấp quyền sử dụng camera android

link: https://react-native-camera.github.io/react-native-camera/docs/installation

1. Cài thư viện: **npm install react-native-camera --save**: Cài đặt thư viện
2. Link thư viện: **react-native link react-native-camera**
3. Vào **android/app/src/main/AndroidManifest.xml**: Thêm dòng code này vào:

```js
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Camera -->
    <uses-permission android:name="android.permission.CAMERA" />
    <!-- Camera -->
```

4.  Vào **android/app/build.gradle** dán code này vào

```js
    android {
    ...
        defaultConfig {
            ...
            missingDimensionStrategy 'react-native-camera', 'general' // <--- insert this line
        }
    }
```

5. Import và sử dụng:

```js



import {
  StyleSheet,
  Text,
  View,
  TouchableOpacity,
  TextInput,
  Image,
  ScrollView,
} from 'react-native';
import React from 'react';
import Icon from 'react-native-vector-icons/EvilIcons'; // import Icon

const HomeScreen = ({navigation}) => {
  return (
      <View style={styles.container}>
        <View style={styles.upLoadPost}>
          <View style={styles.upLoadPostImageUser}>
            <Image
              style={{width: '90%', height: '90%', borderRadius: 100}}
              source={require('../assets/images/anh1.jpg')}
            />
          </View>

          <TextInput
            style={styles.upLoadPostImageInput}
            placeholder="Bạn đang nghĩ gì ?"
          />

          <TouchableOpacity
            style={styles.upLoadPostImagePick}
            onPress={() => navigation.navigate('Camera')}>
            <Icon name="image" color="red" size={40} />
          </TouchableOpacity>
        </View>
      </View>
  );
};

export default HomeScreen;

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  upLoadPost: {
    flexDirection: 'row',
    height: 50,
    // flex: 10,
    alignItems: 'center',
  },
  upLoadPostImageUser: {
    alignItems: 'center',
    flex: 1.5,
    width: 100,
    marginLeft: 10,
    borderRadius: 100,
    backgroundColor: 'red',
  },

  upLoadPostImagePick: {
    flex: 1,
    margin: 5,
  },
  upLoadPostImageInput: {
    flex: 7,
    borderWidth: 1,
    margin: 5,
    borderRadius: 10,
    borderColor: 'grey',
  },

  recommendView: {
    backgroundColor: 'yellow',
    height: 40,
  },
  newsView: {
    backgroundColor: 'orange',
    height: 150,
  },
  postsView: {
    flex: 10,
    backgroundColor: 'pink',
  },
});



//Camera Screens
import {StyleSheet, Text, View, TouchableOpacity} from 'react-native';
import React, {useRef} from 'react';

import {RNCamera} from 'react-native-camera';

const CameraScreen = () => {
  const refCamera = useRef(null);

  const takePicture = async () => {
    try {
      const data = await this.camera.capture();
      console.log('Path to image: ' + data.path);
    } catch (err) {
      // console.log('err: ', err);
    }
  };

  return (
    <RNCamera
      ref={refCamera}
      style={{flex: 1}}
      type={RNCamera.Constants.Type.front}
      captureAudio={false}>
      <View style={{width: '100%'}}>
        <TouchableOpacity style={styles.capture} onPress={takePicture}>
          <Text>Take Photo</Text>
        </TouchableOpacity>
      </View>
    </RNCamera>
  );
};

export default CameraScreen;

const styles = StyleSheet.create({});

```
