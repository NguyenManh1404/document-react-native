# Image

1. Dùng khi muốn show ra những hình ảnh lên giao diện

- Dùng với các file đã download về source:

```js
<Image source={require("./assets/images/image.jpg")} />
```

- Dùng khi muốn với cái file trên internet:

```js
<Image source={{ uri: "https://anh1.png" }} />

//lưu ý phải setting width và height cho ảnh nó mới hiển thị
```

2. Các thuộc tính:

- **resizeMode**: cài đặt kích thước ảnh cho phù hợp với container chứ nó
-

# Flastlist
