# JAVASCRIPT問題

拋出容易忽略的`JAVASCRIPT`的問題。

### 1、下面哪些值是falsy?

```javascript
0
new Number(0)
('')
(' ')
new Boolean(false)
undefined
```

- A: `0`, `''`, `undefined`
- B: `0`, `new Number(0)`, `''`, `new Boolean(false)`, `undefined`
- C: `0`, `''`, `new Boolean(false)`, `undefined`
- D: All of them are falsy

<details>
<summary>答案</summary>

**答案：A**

只有6種 [false](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy) 值：
- `undefined`
- `null`
- `NaN`
- `0`
- `''` (empty string)
- `false`

`Function`構造函數，比如`new Number`和`new Boolean`，是[truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)
</details>
<hr/>

### 2、輸出是什麼？

```javascript
// counter.js
let counter = 10;
export default counter;
```

```javascript
// index.js
import myCounter from "./counter";

myCounter += 1;

console.log(myCounter);
```

- A: `10`
- B: `11`
- C: `Error`
- D: `NaN`

<details>
<summary>答案</summary>

**答案：C**

引入的模塊是**只讀的：你不能修改引入的模塊**。只有導出它們的模塊才能修改其值。

當我們給`myCounter`增加一個值得時候會拋出一個異常：`myCounter`是只讀的，不能被修改。
</details>
<hr/>

