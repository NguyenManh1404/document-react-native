# Cài đặt:

Link: https://reactnavigation.org/docs/getting-started

1. **npm install @react-navigation/native**: Thư viện của hắn
2. **npm install react-native-screens react-native-safe-area-context**: Cứ cài cái này để đảm bảo không lỗi

# Navigation Top Bar

1. Đảm bảo đã Cài đặt: **npm i @react-navigation/native**
2. Cài : **npm install @react-navigation/material-top-tabs**: Thư viện Top Bar https://reactnavigation.org/docs/material-top-tab-navigator

3. Cài: **npm i react-native-pager-view**

4. Kiểm tra đầy đủ giống như này chưa ở **package.json**

```js
"dependencies": {
    "@react-navigation/material-top-tabs": "^6.2.1",
    "@react-navigation/native": "^6.0.10",
    "react": "17.0.2",
    "react-native": "0.68.2",
    "react-native-pager-view": "^5.4.15",
    "react-native-splash-screen": "^3.3.0"
  },
```

4. Cấu hình các screen theo dạng sau:

```ts
import { NavigationContainer } from "@react-navigation/native";
import { createMaterialTopTabNavigator } from "@react-navigation/material-top-tabs";

import HomeScreen from "./screens/HomeScreen";
import FriendScreen from "./screens/FriendScreen";
import NotifyScreen from "./screens/NotifyScreen";
import UserScreen from "./screens/UserScreen";
import VideoScreen from "./screens/VideoScreen";

const Tab = createMaterialTopTabNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Tab.Navigator>
        <Tab.Screen name="HomeScreen" component={HomeScreen} />
        <Tab.Screen name="FriendScreen" component={FriendScreen} />
        <Tab.Screen name="NotifyScreen" component={NotifyScreen} />
        <Tab.Screen name="UserScreen" component={UserScreen} />
        <Tab.Screen name="VideoScreen" component={VideoScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
};

const styles = StyleSheet.create({});

export default App;
```

5. Build lại app và kiểm tra

6. Style the **Tab.Navigator**

   <details>

   ```ts
   <Tab.Navigator
     initialRouteName="HomeScreen"
     screenOptions={{
       headerShown: false,
       tabBarActiveTintColor: "black", //màu chữ của từng tap
       tabBarLabelStyle: { fontSize: 12 }, // kích cỡ chữ của từng tab
       tabBarStyle: {
         backgroundColor: "white",
         borderRadius: 5,
         padding: 1,
       }, //style của cả tabBar
       tabBarPressColor: "#968f8f", // ấn vào thì đổi màu
       tabBarIconStyle: {
         // backgroundColor: 'white',
         borderRadius: 1,
         shadowRadius: 5,
       }, //màu nền của icon
       // lazy: true, //
       tabBarContentContainerStyle: { backgroundColor: "white", height: 50 }, // style của cả tabBar
       tabBarInactiveTintColor: { color: "yellow" },
       // tabBarIndicator: {color: 'yellow'},
       // tabBarScrollEnabled: true, // làm nó trở thành scroll theo chiều ngang
       tabBarIndicatorStyle: {
         backgroundColor: "blue",
       }, //màu của thanh trượt bên dưới icon
       tabBarItemStyle: { marginLeft: 10 }, // style cho từng tab một
     }}
   ></Tab.Navigator>
   ```

   </details>

7. Option của từng **Tab.Screen**
   <details>

   ```js

           <Tab.Screen
           name="HomeScreen"
           component={HomeScreen}
           options={{
               tabBarShowLabel: false, //ẩn tên của tab
               tabBarLabel: 'Home', // tên của tab hiển thị lên ui
               tabBarIcon: ({}) => (
               <MaterialCommunityIcons name="home" color={'blue'} size={25} />
               ),//thêm icon
               tabBarLabelStyle: {color: '#edb021', fontSize: 10},// màu của tên label
           }}

   ```

   <details>

## Navigation Bottom Bar

1. Cài những dependencies sau:

```js
"dependencies": {
    "@react-navigation/bottom-tabs": "^6.3.1",
    "@react-navigation/native": "^6.0.10",
    "react": "17.0.2",
    "react-native": "0.68.2",
    "react-native-pager-view": "^5.4.15",
    "react-native-safe-area-context": "^4.2.5",
    "react-native-screens": "^3.13.1",
  },
```

2. Import và sử dụng như này:

<details>

```js
import * as React from "react";
import { Text, View } from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

function HomeScreen() {
  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      <Text>Home!</Text>
    </View>
  );
}

function SettingsScreen() {
  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      <Text>Settings!</Text>
    </View>
  );
}

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}

export default function App() {
  return (
    <NavigationContainer>
      <MyTabs />
    </NavigationContainer>
  );
}
```

</details>

## LỖI: Invariant Violation: requireNativeComponent: "RNSScreen" was not found in the UIManager.

### Sửa:

1. **npm install react-native-screens react-native-safe-area-context**: Thiếu cái này

2. Buil lại app;

# Navigation Topbar hoặc Bottombar kết hợp với Stack

Dùng trong trường hợp muốn chuyển từ Screen Topbar/Bottombar sang một screen bất kỳ.

1. Cài đặt **npm install @react-navigation/stack**
2.
