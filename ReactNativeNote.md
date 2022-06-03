# Những kiến thức cần nhớ về REACT NATIVE

## Cài đặt môi trường chạy:

1. Cài đặt máy ảo(Có thể dùng android studio hoặc genymotion);
2. Tải SDk.
3. Cài đặt sdk môi trường cho máy trỏ đến sdk vừa tải:

   - Bỏ trong phần **path** với đường dẫn **C:\Users\admin\AppData\Local\Android\Sdk**

4. Cài đặt **react native client**:

   - Với câu lệnh: **npm i -g react-native-cli**

5. Khởi tạo project thôi:
   - Với câu lệnh: **npx react-native init NameProject**
   - Theo phiên bản chỉ định: **npx react-native init AwesomeProject --version X.XX.X**
   - Theo templay TypeScript: **npx react-native init AwesomeTSProject --template react-native-template-typescript**
6. Chạy thử:
   - Step 1: Start Metro: **npx react-native start**
   - Step 2: Start your application: **npx react-native run-android**

## Chuyển React REACT NATIVE JS sang TS

1. Cài đặt thành công project với js
2. **npm i --save-dev typescript** : Cài đặt ts
3. **npm i --save-dev react-native-typescript-transformer**: Thêm React Native TypeScript Transformer vào dự án
4. **tsc --init --pretty --jsx react**: tạo tsconfig.json
5. **npm install touch-cli -g** : cài cái này vào và sau đó chạy lệnh **touch rn-cli.config.js**
6. **npm i --save-dev @types/react @types/react-native**: Thêm typings cho React và React Native
7. trong file **rn-cli.config.js**

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-typescript-transformer");
  },
  getSourceExts() {
    return ["ts", "tsx"];
  },
};
```

8. Đổi **App.js => App.tsx** Tất cả các file nào mà thêm vào sau thì sửa thành đuôi **tsx**
9. Chạy lên kiểm tra

## Cài yarn cho project

1. **npm install -g yarn**
2. **yarn global add react-native-cli**
3. **react-native init sample**

## Hooks trong REACT NATIVE

### Định nghĩa:

- Hooks là một bổ sung mới trong React 16.8.
- Hooks là những hàm cho phép bạn “kết nối” React state và lifecycle vào các components sử dụng hàm.
- Cho phép sử dụng state và những tính năng khác của React mà không cần phải khai báo ES6 class.
- Hooks là những cái hàm được viết sẵn bởi thư viện React, tùy thuộc vào trường hợp cụ thể để sử dụng
<details>
    <summary><b>DEMO</b></summary>

```ts
import React, { useState } from "react";

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

</details>

### Tại sao lại dùng react hooks?

- Các component được lồng (nested) vào nhau nhiều tạo ra một DOM tree phức tạp.
- Component quá lớn.
- Sự rắc rối của Lifecycles trong class.

=> React Hooks được sinh ra với mong muốn giải quyết những vấn đề này.

### Lợi ít khi sử dụng React Hooks?

- Khiến các component trở nên gọn nhẹ hơn.
- Giảm đáng kể số lượng code, dễ tiếp cận.
- Cho phép chúng ta sử dụng state ngay trong function component.

### Custom Hooks

- Chúng ta tự tạo một hooks là những logic hoặc đoạn code lặp đi lặp lại.
- tái sử dụng code, viết code dễ nhìn hơn;

<details>
    <summary><b>DEMO</b></summary>

```js
// Hooks hiển thị giờ hiện tại
import { View, Text } from "react-native";
import React from "react";
const useClock = () => {
  const [time, setTime] = React.useState(0);

  const formatDate = (date) => {
    if (!date) return "";

    const hours = date.getHours();
    const minutes = date.getMinutes();
    const seconds = date.getSeconds();

    return `${hours} : ${minutes} : ${seconds}`;
  };

  React.useEffect(() => {
    const id = setInterval(() => {
      let times = new Date();
      setTime(formatDate(times));
    }, 1000);
    return () => {
      clearInterval(id);
    };
  }, []);

  return time;
};

export default useClock;

// import lại và dùng

import useClock from "./hooks/useClock";

const useClocks = useClock();
```

</details>

### Những Hooks cơ bản cần nắm:

1. **useState()**

- Đơn giản hóa quá trình hiển thị dữ liệu ra giao diện người dùng
- Kiểm soát state trong component
<details>
    <summary><b>DEMO</b></summary>

```ts
import React, { useState } from "react"; //import

function Component() {
  const [state, setState] = useState(initState); //khởi tạo
}

// Đếm khi click vào button
import React, { useState, useEffect } from "react";
import { SafeAreaView, StyleSheet, Text, TouchableOpacity } from "react-native";

const App = () => {
  const [count, setCount] = useState(0);
  return (
    <SafeAreaView>
      <Text>{count}</Text>
      <TouchableOpacity onPress={() => setCount(count + 1)}>
        <Text>Count</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
};

export default App;
```

</details>

2. **useEffect()**

- Dùng nó khi thực hiện các **side effect** (Khi một chương trình phần mềm có một tác động xảy ra và làm thay đổi dữ liệu phần mền như gọi API, Update DOM,....);
- Quá trình:
  - b1: Cập nhật state
  - b2: Cập nhật DOM
  - b3: Render UI
  - b4: Gọi cleanup nếu des thay đổi
  - b5: Gọi useEffect()
- Nó giúp cho re-render UI ra trước, còn các logic tốn nhiều thời gian sẽ chạy ra đằng sau, giúp cho UI app ko bị đứng khi xử lý logic
<details>
    <summary><b>DEMO</b></summary>

```ts
    import React,{ useEffect } from 'react'; //import

    function Component(){
        //có 1 tham số:
        //  + Call back luôn được gọi sau khi mounted(render hiển thị ra màn hình)
        //  + Gọi Callback mỗi khi component re-render(Nghĩa là nó sẽ render trước khi gọn đên useEffect)

        useEffect(()=>{

        })


        //có 2 tham số
        //  + Call back luôn được gọi sau khi mounted(render hiển thị ra màn hình)
        //  + Chỉ được gọi lai khi des có sự thay đổi(ví dụ như khi gọi api thì mình có thể thay đổi đuôi của URL sẽ ra những dữ liệu khác nhau)

        useEffect(()=>{

        }[des])


        //có 2 tham số nhưng mảng để trống
        //  + Call back luôn được gọi sau khi mounted(render hiển thị ra màn hình)
        //  + Chỉ gọi call back 1 lần khi sau khi component mounted

        useEffect(()=>{

        }[])


    }
```

</details>

3. **useLayoutEffect()** link https://youtu.be/loSqbCbH2xo

- Hoạt động gần giống **useEffect()** chỉ khác là sẽ render UI xuống cuối cùng quá trình;
- Khi bị chớp nhoáng dữ liệu
- Quá trình:
  - b1: Cập nhật state
  - b2: Cập nhật DOM
  - b3: Gọi cleanup nếu des thay đổi
  - b4: Gọi useLayoutEffect()
  - b5: Render UI

4. **useRef()** link https://youtu.be/qr1dQqRJRNo

- Nó cũng giống như **useState** là có thể lưu giá trị,nhưng nó có sự khác biệt là khi thay đổi giá trị của biến **useRef()** thì app sẽ ko re-render, còn **useState** thì bị re-render khi thay đổi giá trị.

- Muốn lưu các giá trị không mất đi khi bị re-render.
- Lưu các giá trị qua một tham chiếu bên ngoài component nó đứng và mỗi lần render lại thì không ảnh hưởng đến biến tạo với **useRef**;

  - Như khi mình gán một giá trị mặc định cho biến useRef, trong lúc xử lý biến đó sẽ thay đổi giá trị. Mỗi lần render lại thì nó sẽ ko quay lại giá trị ban đầu.

- Khi tạo một biến với **useRef()** thì nó sẽ trả về một object với thuộc tính là current

- Sau mỗi làn re-render thì trả về object trước đó chứ không tại object mới.

```js
import { useRef } from "react";

function App() {
  const ref = useRef(init);

  console.log(ref);
  //{current:init}
}
```

- Xử lý với các thuộc tính thành phần trong DOM như: TextInput
<details>
    <summary><b>DEMO</b></summary>

```ts
import React, { useRef } from "react";

import { SafeAreaView, Text, TextInput, TouchableOpacity } from "react-native";

function App() {
  type TypeRef = {
    current: any;
  };
  const refs: TypeRef = useRef();

  const handlefocus = () => {
    refs.current.focus();
  };

  return (
    <SafeAreaView>
      <TextInput placeholder="name" />
      <TextInput placeholder="name" ref={refs} /> // Khi click thì nó sẽ focus vào
      TextInput này
      <TextInput placeholder="name" />
      <TouchableOpacity onPress={handlefocus}>
        <Text>Click</Text>
      </TouchableOpacity>
    </SafeAreaView>
  );
}

export default App;
```

</details>

5. **React.memo()** link https://youtu.be/pPoKG_l3UFQ

- Giải quyết vấn đề re-render không cần thiết. Ví dụ component cha re-render thì kéo theo componet con cũng bị re-render không cần thiết.
- Dùng trong các component con có nhiều prop.
- Tối ưu hóa về performance.
- Prop thay đổi thì nó mới re-render lớp con, con nếu không thay đổi thì **React.memo()** ngăn cản re-render .

```js
//lớp cha

import { StyleSheet, Text, View,TouchableOpacity } from 'react-native'
import React,{ useState} from 'react'
import TextNumber from './components/TextNumber';

const App = () => {
 const [counter, setCounter]=useState(0);

 const handleClick =()=>{
      setCounter(counter+1) // cái này tăng dẫn đến state counter tăng và khiến prop của lớp con
 }

  return (
    <View style={{alignItems: 'center', justifyContent: 'center', flex:1}}>
        <TouchableOpacity onPress={handleClick}>
          <TextNumber counter={counter}/>
          {/* props lớp con thay đổi thì bị re-render  */}
          <Text>Click</Text>
        </TouchableOpacity>
    </View>

  )
}

export default App



//lớp con dùng memo

import { StyleSheet, Text, View, } from 'react-native'
import React,{ memo } from 'react'

const TextNumber = ({counter}) => {

  console.log("re-render");
  return (
    <View>
      <Text>{counter}</Text>
    </View>
  )
}

export default memo(TextNumber)
```

6. **useCallback(()=>{},[des])**

- Trả về hàm
- Tránh tạo ra những hàm mới không cần thiết trong **function component**;
- Phải dùng **useCallback()** kết hợp với **React.memo()** thì nó mới hoạt đông.
- Nó chỉ tạo ra các **callback** mới khi **des** thay đổi
- Nếu dùng empty **des** thì chỉ gọi **callback** 1 lần

<details>
    <summary><b>DEMO</b></summary>

```js

// cha
import { StyleSheet, Text, View,TouchableOpacity } from 'react-native'
import React,{ useState,useCallback} from 'react'
import TextNumber from './components/TextNumber';

const App = () => {
 const [counter, setCounter]=useState(0);

 const handleClick = useCallback(()=>{
      setCounter(counter+1)
 },[]) // chỉ có thể chạy 1 lần,

  return (
    <View style={{alignItems: 'center', justifyContent: 'center', flex:1}}>

      <TextNumber onCounter={handleClick}/>
      <Text>{counter}</Text>

    </View>

  )
}

export default App


// con  TextNumber.js

import { StyleSheet, Text, View,TouchableOpacity } from 'react-native'
import React, {memo } from 'react'

const TextNumber = ({ onCounter}) => {

  console.log("re-render");
  return (
    <View>
     <TouchableOpacity onPress={onCounter}>


         <Text>Click</Text>
       </TouchableOpacity>
    </View>
  )
}

export default memo(TextNumber)

```

</details>

7. **useMemo**

- Trả về giá trị
- Giải quyết tình trạng re-render đối với những **props** là tham chiếu(object, array, function), bởi vì khi cha re-render thì những biến tham chiếu sẽ tạo ra một địa chỉ mới, dẫn đến thằng **React.memo()** hiểu nhầm là props thay đổi.

- Kết hợp **useMemo** với **React.memo()** thì nó mới hoạt động
<details>
    <summary><b>DEMO</b></summary>

```js
// cha
import { StyleSheet, Text, View,TouchableOpacity } from 'react-native'
import React,{ useState,useCallback,useMemo} from 'react'
import TextNumber from './components/TextNumber';

const App = () => {
 const [counter, setCounter]=useState(0);

 const handleClick =()=>{
      setCounter(counter+1)
 } // chỉ có thể chạy 1 lần,

const arrayDemo=useMemo(()=>[1,2,3,4],[])  // dung useMemo ở đây để khỏi bị re-render với biến tham chiếu được làm prop

  return (
    <View style={{alignItems: 'center', justifyContent: 'center', flex:1}}>

      <TextNumber arrayDemo={arrayDemo}/>
      <Text>{counter}</Text>

      <TouchableOpacity onPress={handleClick}>
        <Text>Click</Text>
      </TouchableOpacity>

    </View>

  )
}

export default App

//con

import { StyleSheet, Text, View,TouchableOpacity } from 'react-native'
import React, {memo } from 'react'

const TextNumber = ({ arrayDemo}) => {

  console.log("re-render");
  return (
    <View>
         <Text>{JSON.stringify(arrayDemo)}</Text>

    </View>
  )
}

export default TextNumber

```

</details>

8. **useReducer()**

- Được dùng thay **useState()** khi giá trị khởi tạo, logic phức tạp,...
- Quản lý những state phức tạp
- Có các tính trình sau: + 1: Init state; + 2: Actions + 3: Reducer + 4: Dispatch
<details>
    <summary><b>DEMO</b></summary>

```JS
 import React,{useReducer}  from 'react';

 import {
   SafeAreaView,
   ScrollView,
   StatusBar,
   StyleSheet,
   Text,
   TextInput,
   useColorScheme,
   View,
   TouchableOpacity,
   FlatList
 } from 'react-native';



 function App ()  {

//initState
 const initState = 0;

 //Actions
 const UP='up';
 const DOWN='down';
 // reducer
 const reducer = (state,action) =>{
    switch(action){
      case UP:{
        return state=state+1
      }
      case DOWN:{
        return state=state-1
      }
      default:{
        return state
      }
    }
 }

 const [state, dispatch] = useReducer(reducer,initState);


   return (
     <SafeAreaView style={{alignItems: 'center'}} >
       <Text style={{fontSize:20}}>
          {state}
       </Text>
       <TouchableOpacity  onPress={()=>{dispatch(UP)}}><Text >UP</Text></TouchableOpacity>
       <TouchableOpacity  onPress={()=>{dispatch(DOWN)}}><Text >DOWN</Text></TouchableOpacity>
      <View>
      </View>
     </SafeAreaView>
   );
 };


 export default App;

```

</details>

9. **React Context()** https://youtu.be/TENin-HxvRg

- Truyển dữ liều từ components cha sang con mà không cần sử dụng **Prop**
- Bất cứ componet con nào cũng nhận được dữ liệu từ cha
- Tiến trình:
  - 1: Create context
  - 2: Provider:
  - 3: Consumer: nhận dữ liệu từ cha

<details>
    <summary><b>DEMO</b></summary>

```js
//Cha
 import React,{ createContext,useState } from 'react'

 import {
   SafeAreaView,
   Text,
   View,
   TouchableOpacity,

 } from 'react-native';
import ChidComponent from './components/ChidComponent';

export const DemoContext = createContext(); //bỏ ở ngoài component cha

 function App ()  {

  const [counter, setCounter]= useState(0)

   return (
     <DemoContext.Provider value={counter}>
        <SafeAreaView style={{alignItems: 'center'}} >
          <ChidComponent/>
          <Text style={{fontSize:20}}>
              {counter}
          </Text>
          <TouchableOpacity onPress={()=>{setCounter(counter+1)}}>
            <Text>Click</Text>
          </TouchableOpacity>
          <View>
          </View>
        </SafeAreaView>
    </DemoContext.Provider>

   );
 };


 export default App;

 //Con
 import { StyleSheet, Text, View } from 'react-native'
import React,{ useContext } from 'react'

import { DemoContext } from '../App'

const ChidComponent = () => {
    const counter = useContext(DemoContext);
  return (
    <View>
      <Text>ChidComponent{counter}</Text>
    </View>
  )
}

export default ChidComponent

const styles = StyleSheet.create({})
```

</details>
