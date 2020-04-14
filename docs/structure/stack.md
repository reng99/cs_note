# 栈

![stack](./imgs/stack.jpg "border_img_stack")

栈是一种后进先出(LIFO)线性表，是一种基于数组的数据结构。

- **LIFO(Last In First Out)**表示后进先出，后进来的元素第一个被弹出栈空间。类似于自动餐托盘，最后放上去的托盘，往往先被拿出来使用。

- 仅允许在表的一端插入和移除数据。这一端被称为**栈顶**，相对地，把另一端称为**栈底**。

- 向一个栈插入新数据称为**进栈、入栈或压栈**，这是将新元素放在栈顶元素上面，使之成为新的栈顶元素。

- 从一个栈删除元素又称为**出栈或退栈**，它是把栈顶元素删除掉，使其相邻的元素成为新的栈顶元素。

代码操作熟悉下：

```javascript
class Stack {
  constructor(){
    this.items = [];
  }
  // 入栈操作
  push(element = ''){
    if(!element) return;
    this.items.push(element);
    return this;
  }
  // 出栈操作
  pop(){
    this.items.pop();
    return this;
  }
  // 栈顶元素
  peek(){
    return this.items[this.size() - 1];
  }
  // 输出
  print(){
    return this.items.join(' ');
  }
  // 是否空栈
  isEmpty(){
    return this.items.length == 0;
  }
  // 栈的大小
  size(){
    return this.items.length;
  }
}

let stack = new Stack(),
  arr = ['鼠', '牛', '虎', '兔', '龙', '蛇', '马', '羊', '猴', '鸡', '狗', '猪'];
arr.forEach(item => {
  stack.push(item);
});

console.log(stack.print()); // 鼠 牛 虎 兔 龙 蛇 马 羊 猴 鸡 狗 猪
console.log(stack.peek()); // 猪

stack.pop().pop().pop().pop();
console.log(stack.print()); // 鼠 牛 虎 兔 龙 蛇 马 羊
console.log(stack.isEmpty()); // false
console.log(stack.size()); // 8
```

> :warning: 栈这裡的`push`和`pop`方法要和数组裡面的`push`和`pop`方法区分下。

