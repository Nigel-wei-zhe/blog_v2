# Array.prototype.reduce 使用

---

## 語法

```js
array.reduce(callback[(accumulator, currentValue, currentIndex, array)], initialValue)
```

---

## 使用

#### 物件陣列轉 mapping 表

```js
array.reduce((map, item) => Object.assign(map, { [item.id]: item.name }), {})
```

#### 二維陣列轉一維

```js
array.reduce((acc, cur) => {
    return acc.concat(cur)
}, [])
```

#### 統計陣列中元素出現次數

```js
array.reduce((acc, cur) => {
    if (cur in acc) {
        acc[cur] = 1
    } else {
        acc[cur] += 1
    }
    return acc
}, {})
```

#### 物件陣列依條件分類

```js
array.reduce((acc, cur) => {
    if (!acc[cur.type]) {
        acc[cur.type] = []
    }
    acc[cur.type].push(cur)
    return acc
}, {})
```

#### 陣列去重

```js
array.reduce((acc, cur) => {
    if (!acc.includes(cur)) {
        acc.push(cur)
    }
    return acc
}, [])
```

#### 求最大值或最小值

```js
array.reduce((acc, cur) => {
    if (!acc) {
        acc = cur
        return acc
    }
    if (acc.age < cur.age) {
        acc = cur
        return acc
    }
    return acc
}, 0)
```

---

## 引用

1. [Reduce - MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)<br>

2. [以前我没得选，现在我只想用 Array.prototype.reduce](https://juejin.cn/post/6916087983808626701)<br>

---

## 瀏覽器相容性

| [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png" alt="IE / Edge" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)IE / Edge | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png" alt="Firefox" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)Firefox | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png" alt="Chrome" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)Chrome | [<img src="https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png" alt="Safari" width="24px" height="24px" />](http://godban.github.io/browsers-support-badges/)Safari |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|                                                                                                   9 / 12                                                                                                   |                                                                                                      3                                                                                                       |                                                                                                    3                                                                                                     |                                                                                                    5                                                                                                     |
