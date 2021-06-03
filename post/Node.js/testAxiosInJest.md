# How to test axios by jest

---

最近剛好在寫單元測試,看了些東西,有些眉眉角角,像是有函式在 Node.js 下並不支援,導致單元測試過不了。
像是這篇 [Jest: TypeError: replaceAll is not a function
](https://stackoverflow.com/questions/65295584/jest-typeerror-replaceall-is-not-a-function)

---

那麼在寫單元測試時,有碰到 axios,所以就稍微查網路做功課,以下就列出看到的方法

1. 藉由 jest.mock 替換掉 axios
    > SUT: System Under Test 測試目標
    > DOC: Depended-on Component 依賴組件
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

// 假設 fakeUrl 伺服器響應時間很久, 不可能跑測試真的去打,所以用 Mock 繞過,驗證回傳資料是否符合預期,或接下來要拿這資料跑甚麼流程,就能再度規劃
```
