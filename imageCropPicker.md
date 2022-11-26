# Library React-native-image-crop-picker
- https://github.com/ivpusic/react-native-image-crop-picker
- https://blog.logrocket.com/how-to-build-an-image-picker-using-react-native-image-crop-picker/
- Dùng để select photo/video/file ở thư viện, chụp ảnh


1. yarn add react-native-image-crop-picker : cài thư viện

### Setup on IOS
1. Add this line in to **./ios/Podfile**

```js
platform :ios, '13.0'
install! 'cocoapods', :deterministic_uuids => false

target 'mtk_user_app' do
 
// here
  pod 'RNImageCropPicker', :path =>  '../node_modules/react-native-image-crop-picker' 
// here

```
2.  Add this line into file **./ios/mtk_app/Info.plist**

```js
......
			</dict>
		</dict>
	</dict>

	// camera // here   
	<key>NSPhotoLibraryUsageDescription</key>
	<string>$(PRODUCT_NAME) would like to upload photos from your photo gallery</string>
	<key>NSCameraUsageDescription</key>
	<string>$(PRODUCT_NAME) requires to access camera for uploading photos to your profile or posts</string>
	<key>NSMicrophoneUsageDescription</key>
	<string>$(PRODUCT_NAME) requires to access Audio recording to record and uplod videos</string>
	//camera // here

	<key>NSLocationWhenInUseUsageDescription</key>
	<string></string>
	<key>UILaunchStoryboardName</key>
.....
```

### Use in Screen
<details>

```js
import {Image, StyleSheet, Text, TouchableOpacity, View} from 'react-native';
import React, {useState} from 'react';
import ImageCropPicker from 'react-native-image-crop-picker';

const App = () => {
  const [photoPath, setPhotoPath] = useState('');

  const SelectMedia = async () => {
    const DEFAULT_PICKER_OPTION: any = {
      width: 300,
      height: 300,
      cropping: false,
      compressImageQuality: 1,
      mediaType: 'photo', // chi chon photo ma thoi
    };
    const image = await ImageCropPicker.openPicker({
      ...DEFAULT_PICKER_OPTION,
    });
    if(image){
      setPhotoPath(image?.path);
    }
  }
  return (
    <View style={{alignItems:'center'}}>
      <Image source={{uri: photoPath}} style={{width: 300, height: 300}} />
      <Text>Home</Text>
      <TouchableOpacity onPress={SelectMedia}>
        <Text>Open Select</Text>
      </TouchableOpacity>
    </View>
  );
};

export default App;

```
</details>