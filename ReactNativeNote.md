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

## Hooks trong REACT NATIVE

### Định nghĩa:
- Hooks là một bổ sung mới trong React 16.8.
- Hooks là những hàm cho phép bạn “kết nối” React state và lifecycle vào các components sử dụng hàm.
- Cho phép sử dụng state và những tính năng khác của React mà không cần phải khai báo ES6 class.
- Hooks là những cái hàm được viết sẵn bởi thư viện React, tùy thuộc vào trường hợp cụ thể để sử dụng
```ts
    import React, { useState } from 'react';

    function Example() {
    // Declare a new state variable, which we'll call "count"
    const [count, setCount] = useState(0);

    return (
        <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
            Click me
        </button>
        </div>
    );
    }
```

### Tại sao lại dùng react hooks?
- Các component được lồng (nested) vào nhau nhiều tạo ra một DOM tree phức tạp.
- Component quá lớn.
- Sự rắc rối của Lifecycles trong class.

=> React Hooks được sinh ra với mong muốn giải quyết những vấn đề này.

### Lợi ít khi sử dụng React Hooks?
- Khiến các component trở nên gọn nhẹ hơn.
- Giảm đáng kể số lượng code, dễ tiếp cận.
- Cho phép chúng ta sử dụng state ngay trong function component.

### Những Hooks cơ bản cần nắm:
1. **useState()**
- Đơn giản hóa quá trình hiển thị dữ liệu ra giao diện người dùng
- Kiểm soát state trong component
- demo:
```ts
    import React,{ useState } from 'react'; //import

    function Component(){
        const [state, setState]= useState(initState); //khởi tạo
    }

// Đếm khi click vào button
import React,{  useState, useEffect } from 'react';
import {
  SafeAreaView,
  StyleSheet,
  Text,
  TouchableOpacity
} from 'react-native';

const App = ()=>{
  const [count, setCount]= useState(0)
  return (
      <SafeAreaView>
        <Text>{count}</Text>
        <TouchableOpacity onPress={()=>setCount(count+1)}>
          <Text>Count</Text>
        </TouchableOpacity>
      </SafeAreaView>
    
  );
};

export default App;

```

2. **useEffect()**
- Dùng nó khi thực hiện các **side effect** (Khi một chương trình phần mềm có một tác động xảy ra và làm thay đổi dữ liệu phần mền như gọi API, Update DOM,....);
- Quá trình: 
    + b1: Cập nhật state
    + b2: Cập nhật DOM
    + b3: Render UI
    + b4: Gọi cleanup nếu des thay đổi
    + b5: Gọi useEffect()

- demo:

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
3. **useLayoutEffect()** link https://youtu.be/loSqbCbH2xo
- Hoạt động gần giống **useEffect()** chỉ khác là sẽ render UI xuống cuối cùng quá trình;
- Khi bị chớp nhoáng dữ liệu
- Quá trình: 
    + b1: Cập nhật state
    + b2: Cập nhật DOM
    + b3: Gọi cleanup nếu des thay đổi
    + b4: Gọi useEffect()
    + b5: Render UI

4. **useRef()** link https://youtu.be/qr1dQqRJRNo

- Lưu các giá trị qua một tham chiếu bên ngoài

- Khi tạo một biến với ***useRef()** thì nó sẽ trả về một object với thuộc tính là current