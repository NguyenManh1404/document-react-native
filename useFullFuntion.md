# USEFUL FUNTION IN REACT NATIVE
1. Kiểm tra ký tự đặc biệt trong một string 
```js

const calculateActualHTMLLength = (htmlContent) => {
  if (htmlContent) {
    const regex = /[^(script|style)][^>]*>([^<>]*)</g;
    let match;
    let result = '';

    while ((match = regex.exec(htmlContent))) {
      result += decode(match[1]);
    }

    return result.length || 0;
  }
  return 0;
};
```

2. Lọc data ra từ một mảng:
- Ko nên sử dụng UseEffest để setState: nó là sai lầm

```js
const jobImages = useMemo(() => {
    if (data?.length > 0) {
      return data?.filter((image) => image.type !== 'video');
    }
    return [];
  }, [data]);

```