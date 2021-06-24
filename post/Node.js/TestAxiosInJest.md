## How to test axios by jest

最近剛好在寫單元測試,看了些東西,有些眉眉角角,像是有函式在 Node.js 下並不支援,導致單元測試過不了。<br>
像是這篇 [Jest: TypeError: replaceAll is not a function
](https://stackoverflow.com/questions/65295584/jest-typeerror-replaceall-is-not-a-function)

---

那麼在寫單元測試時,有碰到 axios,所以就稍微查網路做功課,以下就列出看到的方法

1. 藉由 jest.mock 替換掉 axios
    > SUT: System Under Test 測試目標<br>
    > DOC: Depended-on Component 依賴組件<br>
    > 情況是這樣,有時候 SUT 有太多的 DOC ,為了避免模糊焦點,讓測試邏輯相對單純,所以可以會把一些 相依的 DOC,採用 mock 替換掉, 讓測試不失焦。

```js
import axios from "axios"

jest.mock("axios")

describe("axios 流程", () => {
    test("success", () => {
        const data = {
            success: true
        }
        // 在axios.get上面用mock提供的函式 回傳一個 "一次性" function
        axios.get.mockImplementationOnce(() => Promise.resolve(data)

        expect(axios.get('fakeUrl')).resolves.toEqual(data);
    })
})

```

2. 引用
    > axios.interceptors 裡面也有邏輯要做測試,如果用上面那一招,會被蓋掉<br>
    > 蓋掉不用測這樣覆蓋率就 100%了 XD

```js
// 基於 axios 再次封裝的 request 有在 interceptors 寫些邏輯
import request from "@/utils/request"

const mockData = {
    config: {
        url: "/test",
    },
    data: "test",
}

describe("request interceptors flow ~", () => {
    // resolve 流程
    const fulfilled = request.interceptors.request.handlers[0].fulfilled
    test("resolve handle", () => {
        expect(fulfilled(mockData)).toEqual(mockData)
    })
    // resolve 流程
    const rejected = request.interceptors.request.handlers[0].rejected
    test("resolve handle", () => {
        rejected()
    })
})

describe("response interceptors flow ~", () => {
    // resolve 流程
    const fulfilled = request.interceptors.response.handlers[0].fulfilled
    test("resolve handle", () => {
        expect(fulfilled(mockData)).toEqual(mockData)
    })
    // resolve 流程
    const rejected = request.interceptors.response.handlers[0].rejected
    test("resolve handle", () => {
        rejected()
    })
})
```

以上就是 這次為了測試 axios 有用到的方法,繼續提升覆蓋率~

#####參考資料
[How to test Axios in Jest by Example](https://www.robinwieruch.de/axios-jest)<br>
[axios](https://github.com/axios/axios)<br>
[Jest-Mocking Modules](https://jestjs.io/docs/mock-functions)
