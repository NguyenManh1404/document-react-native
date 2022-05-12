# Thêm Icon cho App

1. Cài: **npm i react-native-vectors-icons** : Thư viện lấy Icon phổ biến nhất hiện nay
2. Cài: **react-native link** : react-native link là một cách tự động để cài đặt các phụ thuộc gốc. Nó là một giải pháp thay thế cho việc liên kết thủ công phần phụ thuộc trong dự án của bạn. Nó hoạt động cho cả Android và iOS.
3. Chọn icons ở đây: https://oblador.github.io/react-native-vector-icons/

4. Import và dùng:

```ts
import { StyleSheet, Text, View } from "react-native";
import React from "react";
import Icon from "react-native-vector-icons/FontAwesome"; // import Icon, *FontAwesome* là tên gói
const HomeScreen = () => {
  return (
    <View>
      <Text>HomeScreens</Text>
      <Icon name="rocket" color="red" size={40} /> //dùng
    </View>
  );
};

export default HomeScreen;

const styles = StyleSheet.create({});
```

5. Build lại app để kiểm tra

# Thêm Fonts chữ cho App

https://viblo.asia/p/them-fonts-va-fonticons-vao-ung-dung-react-native-1VgZvwD2lAw
