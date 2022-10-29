# ATTENTION WENT JOIN REAL PROJECT

1. Khi xử lý với display: flex nếu có text trong các view con thì để thuộc tính
fill

```js
<WView fill></WView>
```

2. **!!variable**: có nghĩa là phủ định của phủ định. Dùng để check crash app

3. Khi mà code ở nhánh của mình mà cần phải dùng code của nhánh khác, thì mình pull code về nhé.

4. Đọc kỹ yêu cầu trước khi làm nhé;

5. Sửa file **en.json** thì lưu ý nó nằm ở nhiều nơi khác nhé;

7. loading và useEffect nhớ và hiểu.

8.  Hàm xóa item theo id
```js
 const handleRemoveCentre = (id) => {
    const newData = compareCentres?.filter((item) => item.id !== id);
    setcompareCentres(newData);
  };
```

9. Hàm này dùng để giải mã hóa url.
```js
    console.log('url', decodeURIComponent(url));
```
 

10. Các Đặt tên biến và file: cú pháp: </br>
```js
{Tên/Mục đích sử dụng}{Loại (thường là danh từ)}
```
"Hành động + Type của biến/file sẽ đặt ở đằng sau"
ex: AppliedListView , StatusModal, OneClickApplyButton(Ko sợ dài)
+ em ơi mình đặt tên component cho đúng ngữ pháp và đồng bộ là như này nhé:
 VD: LocationContainer, JobTypeSelection, DateRangeInput...
Hiện tại e đặt CircleSelect về cơ bản không hiểu được nó là gì, ngoài ra select là động từ thì không thể đặt cho component name được.
Sau khi đối chiếu với Figma thì a thấy e đang làm 1 carousel gồm các circle selection, vậy nên em có thể. đặt tên là CircleSelectionCarousel
Nhìn vào rất dễ hiểu hì.
 sửa tên các component lại giúp a nhé.
cc @mobile-team cùng nắm để đặt tên cho phù hợp và review lẫn nhau giúp a nhé.
11. Không dùng function inline
12. Cho vào constant nếu là dữ liệu cứng
13. style={[styles.sectionContainer, style]}

14. ***Sử dùng git stash*** 
```js
git stash list //Xem tất cả stash
git stash push // tạo stash mới

git stash apply stash@{0} // apply code vào branch hiện tại
git stash clear //Xoá tất cả stash hiện tại
```
15. Log idButton in rightTabBar

16. Error Could not get BatchedBridge
https://stackoverflow.com/questions/38870710/error-could-not-get-batchedbridge-make-sure-your-bundle-is-packaged-properly?page=2&tab=scoredesc#tab-top
For me i just kill all the terminal inside visual code studio and open cmd where i run bellow two command
npx react-native start
npx react-native run-android

17. Bắt sự kiện ở Tab button right
```js
import { Navigation } from 'react-native-navigation';

useEffect(() => {
    const listener = Navigation.events().registerNavigationButtonPressedListener((event) => {
      if (event.buttonId === 'deleteBtn') {
        onShowOverlayDelete();
      }
    });
    return () => listener.remove();
  }, []);
```
18. Login api
https://rise.api-uat.kindicare.com/v1/client/auth/login

19. **git reset --merge** : sau khi pull ma muon back/reset lai
20. String(index) -> ${index}String(index) -> ${index}
21. Icon App in iOS https://techmaster.vn/posts/35900/tao-app-launcher-icon-cho-react-native-app-android-ios
22. Không checkout qua được remote branch thì dùng **git fecth**
23. Shadow cho componet:
```js
container : {
    shadowColor: Colors.black,
    shadowOffset: {
      width: 0,
      height: 1,
    },
    shadowOpacity: 0.18,
    shadowRadius: 1.0,
    elevation: 1,
}

```
24. Lưu ý để flex: 1 cho container

25. Khi lỡ push code lên mà muốn reves lại:
```js
git checkout uat// nhánh đã merge vào
git log 
git reset --hard commitcu 
git push -f 
```
26. https://reactjsexample.com/tag/admin-template/
```js
mẫu login: https://boss.ux-maestro.com/login
mẫu https://github.com/V-bhoy/Lyst-clone?ref=reactjsexample.com
```
27. dùng callback để render UI
```js
const renderHeadline = useCallback(() => {
    if (headline?.verticalLine) {
      return (
        <View>
          {headline?.content}
        </View>
      );
    }
    return (
      <View fill style={styles.heading}>
        <Text type="bold18" color={Colors.blackText}>
          {headline?.content || EMPTY_STRING} 
        </Text>
        {!!iconUrl && <FastImage source={{ uri: iconUrl }} style={styles.headingIcon} />}
      </View>
    );
  }, [headline, iconUrl]);
```
27. Đặt tên cho biến

```js
 idSelected => selectedId
 radioButtonStyle
 radioSelectedStyle
```

28. Add Listen event
```js
//create event
 DeviceEventEmitter.emit('BACK_MY_JOB_EVENT', { tabIndex: 2 });

//bat su kien
useEffect(() => {
    const listener = NativeAppEventEmitter.addListener(NAME_EVENT, (event) => {
      onSortChange?.(event);
    });

    return () => {
      listener.remove();
    };
  }, [onSortChange]);
``` 