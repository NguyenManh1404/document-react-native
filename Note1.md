# Flexbox trong react native 

- Giúp xây dựng bố cục, thu nhỏ tự động dựa vào kích thước của màn hình thiết bị.
<details>
    <summary><b>DEMO</b></summary>

```js
import React from 'react';

import {Text, View, StyleSheet, Dimensions} from 'react-native';

const {height, width} = Dimensions.get('window');

function App() {
return (
    <View style={styles.container}>
    <View style={styles.top}>
        <View style={styles.topLeft}>
        <View style={styles.topLeftTop}></View>

        <View style={styles.topLeftBottom}></View>
        </View>

        <View style={styles.topRight}>
        <Text
            style={[styles.textTopRight, {fontWeight: 'bold', color: 'white'}]}>
            HELLO
        </Text>
        </View>
    </View>

    <View style={styles.bottom}>
        <View style={styles.bottomLeft}></View>

        <View style={styles.bottomCenter}></View>
        <View style={styles.bottomRight}></View>
    </View>
    </View>
);
}

export default App;

const styles = StyleSheet.create({
container: {
    flex: 1,
},
top: {
    backgroundColor: 'red',
    flex: 6,
    flexDirection: 'row',
},
bottom: {
    backgroundColor: 'white',
    flex: 3,
    flexDirection: 'row',
},
topLeft: {
    backgroundColor: 'white',
    flex: 2,
},
topRight: {
    flex: 6,
    alignItems: 'center',
    justifyContent: 'center',
},
textTopRight: {
    fontSize: 30,
},
topLeftTop: {
    borderWidth: 3,
    flex: 2,
},
topLeftBottom: {
    borderWidth: 3,
    flex: 8,
},

bottomLeft: {
    flex: 2,
    backgroundColor: 'blue',
},
bottomCenter: {
    flex: 4,
},
bottomRight: {
    flex: 2,
    borderWidth: 3,
},
});
```
</details>

ffff