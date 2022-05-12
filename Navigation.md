# Navigation Top Bar

1. Đảm bảo đã Cài đặt: **npm i @react-navigation/native**
2. Cài : **npm install @react-navigation/material-top-tabs**: Thư viện Top Bar https://reactnavigation.org/docs/material-top-tab-navigator

3. Cài: **npm i react-native-pager-view**

4. Kiểm tra giống như này chưa ở **package.json**

```
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
               tabBarLabel: 'Home',
               tabBarIcon: ({}) => (
               <MaterialCommunityIcons name="home" color={'blue'} size={25} />
               ),//thêm icon
           }}

   ```

   <details>
