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