# Cách thêm fonts cho app

1. Taỉ font về máy.
2. Tạo thư mục **../src/assets/fonts** sau đó bỏ fonts vừa tải về vào
3. Tạo file **react-native.config.js** cùng cấp với package.json

```js
module.exports = {
  assets: ['./src/assets/fonts'],
};
```


4. chạy lệnh **npx react-native-asset** để nó tự link vào ios và android

5. Tạo file **./src/themes/Fonts**

```js
export const type = {
  semiBold: 'Mulish-SemiBold',
  boldItalic: 'Mulish-BoldItalic',
  bold: 'Mulish-Bold',
  italic: 'Mulish-Italic',
  medium: 'Mulish-Medium',
  regular: 'Mulish-Regular',
  light: 'Mulish-Light',
  lightItalic: 'Mulish-LightItalic',
};

export const size = {
  S8: 8,
  S10: 10,
  S12: 12,
  S13: 13,
  S14: 14,
  S15: 15,
  S16: 16,
  S17: 17,
  S18: 18,
  S20: 20,
  S24: 24,
  S28: 28,
  S30: 30,
  S36: 36,
  S40: 40,
};

export default {
  type,
  size,
};

```
7. Dùng trong dự án bằng cách;
```js
  normal: {
    color: Colors.blackText,
    fontFamily: Fonts.type.regular,
    letterSpacing: 0.02,
  },
```