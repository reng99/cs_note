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

### 3、輸出是什麼？

```javascript
function compareMembers(person1, person2 = person) {
  if (person1 !== person2) {
    console.log("Not the same!")
  } else {
    console.log("They are the same!")
  }
}

const person = { name: "Lydia" }

compareMembers(person)
```

- A: `Not the same!`
- B: `They are the same!`
- C: `ReferenceError`
- D: `SyntaxError`

<details>
<summary>答案</summary>

**答案：B**

對象通過引用傳遞。當我們檢查對象的嚴格相等性（===）時，我們正在比較他們的引用。

我們將`person2`的默認值設置為`person`對象，並將`person`對象作為`person1`的值傳遞。

這意味著兩個值都引用內存中的同一位置，因此它們是相等的。

運行`else`語句中的代碼塊，並記錄`They are the same!`。

</details>
<hr/>

### 4、輸出是什麼？

```javascript
function getFine(speed, amount) {
  const formattedSpeed = new Intl.NumberFormat({
    'en-US',
    { style: 'unit', unit: 'mile-per-hour' }
  }).format(speed)

  const formattedAmount = new Intl.NumberFormat({
    'en-US',
    { style: 'currency', currency: 'USD' }
  }).format(amount)

  return `The driver drove ${formattedSpeed} and has to pay ${formattedAmount}`
}

console.log(getFine(130, 300))
```

- A: The driver drove 130 and has to pay 300
- B: The driver drove 130 mph and has to pay $300.00
- C: The driver drove undefined and has to pay undefined
- D: The driver drove 130.00 and has to pay 300.00

<details>
<summary>答案</summary>

**答案：B**

通過方法`Intl.NumberFormat`，我們可以格式化任意區域數字值。我們對數字值`130`進行`mile-per-hour`作為`unit`的`en-US`區域格式化，結果為`130mph`。對數字值`300`進行`USD`作為`currentcy`的`en-US`區域格式化，結果為`$300.00`。
</details>
<hr/>

