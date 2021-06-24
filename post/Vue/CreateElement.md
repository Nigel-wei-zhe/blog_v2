## 關於 createElement

### Document.createElement()

> 創造一個元素,搭配其他 api 可以實現在指定的元素後,把創造出來的新元素添加上去

```js
const title = document.createElement("div")
title.innerText = "Hello world"
document.getElementsByTagName("body")[0].appendChild(title)
```

### vue createElement()

> vue 製作 Virtual DOM 的函式,透過 render function 呼叫 createElement 來創造,
> 內部含有實現進行 diff 算法比較新舊 Virtual DOM 的差異,進而觸發渲染

```js
Vue.component("hello-world", {
    render(createElement) {
        return createElement("h1", {
            domProps: {
                innerHTML: "Hello world",
            },
        })
    },
})
```

---

### vue createElement() 應用

> 參數相關設定可以直接參考 [官網](https://cn.vuejs.org/v2/guide/render-function.html#createElement-%E5%8F%82%E6%95%B0)。
> 直接舉個使用經驗,今天有一個需求是需要基於 vue 提供的 transition 的标签名進行加工
> 目的是想透過 transition 本身針對 enter 跟 leave 的相關事件,來實作收合

```js
const transitionStyle = "0.3s height ease-in-out"
const Transition = {
    beforeEnter(el) {
        el.style.transition = transitionStyle
        if (!el.dataset) el.dataset = {}
        el.style.height = 0
    },

    enter(el) {
        if (el.scrollHeight !== 0) {
            el.style.height = `${el.scrollHeight}px`
        } else {
            el.style.height = ""
        }
        el.style.overflow = "hidden"
    },

    afterEnter(el) {
        el.style.transition = ""
        el.style.height = ""
    },

    beforeLeave(el) {
        if (!el.dataset) el.dataset = {}
        el.style.height = `${el.scrollHeight}px`
        el.style.overflow = "hidden"
    },

    leave(el) {
        if (el.scrollHeight !== 0) {
            el.style.transition = transitionStyle
            el.style.height = 0
        }
    },

    afterLeave(el) {
        el.style.transition = ""
        el.style.height = ""
    },
}

export default {
    name: "CollapseTransition",
    functional: true,
    render(h, { children }) {
        const data = {
            on: Transition,
        }
        return h("transition", data, children)
    },
}
// 備註:

// 事件监听器在 `on` 内，
// 但不再支持如 `v-on:keyup.enter` 这样的修饰器。
// 需要在处理函数中手动检查 keyCode。
//  on: {
//    click: this.clickHandler
//  },

// render(h, { children })  的第二個參數
// slots() 和 children 对比 參考官網
// https://cn.vuejs.org/v2/guide/render-function.html#slots-%E5%92%8C-children-%E5%AF%B9%E6%AF%94
```

---

## 參考資料

[MDN-Document.createElement](https://developer.mozilla.org/zh-TW/docs/Web/API/Document/createElement)<br>
[VUE-渲染函数 & JSX](https://cn.vuejs.org/v2/guide/render-function.html)<br>
[React 拾遗：React.createElement 与 JSX](https://www.jianshu.com/p/42a3ec621e94)<br>
[學會使用 Vue JSX，一車老乾媽都是你的](https://www.mdeditor.tw/pl/pZdO/zh-tw)<br>
[Vue 函数式组件](https://juejin.cn/post/6867458052036624392)<br>
[Vue Render Function 到 Functional Component — 建構一彈性高、易封裝的 UI Component 吧](https://medium.com/@leo36094/vue-render-function-%E5%BB%BA%E6%A7%8B%E4%B8%80%E5%BD%88%E6%80%A7%E9%AB%98-%E6%98%93%E5%B0%81%E8%A3%9D%E7%9A%84%E7%9A%84-component-%E5%90%A7-bdb2bdee46f1)
