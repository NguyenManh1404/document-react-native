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

# Flatlist

1. **horizontal**: hiển thị nằm ngang
2.

```ts
<FlatList
  data={data.getInboxes.items}
  renderItem={renderItem}
  keyExtractor={(item) => item.id}
  ListEmptyComponent={() => {
    return (
      <>
        <Text>Empty Post</Text>
      </>
    );
  }}
  refreshControl={
    <RefreshControl
      colors={["#9Bd35A", "#689F38"]}
      refreshing={refreshing}
      onRefresh={refetch}
    />
  }
/>
```

3. **showsHorizontalScrollIndicator={false}**: ẩn thanh trượt

# StatusBar:

1. StatusBar.setBackgroundColor('#242334'); // set màu background cho status
2. StatusBar.setBarStyle('light-content'); // set màu cho icon trong status
