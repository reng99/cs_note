# 二叉查找树

![tree](./imgs/tree.jpg "border_img_tree")

### 树

- 树的定义：
 - 树（Tree）：`n(n >= 0)`个节点构成的有限集合。
  - 当`n = 0`时，称为空树；
  - 对任意一棵非空树`(n > 0)`，具备以下性质：
  - 树中有各一个称为**根（Root）**的特殊节点，用`r(root)`表示；
  - 其馀节点可分为`m(m > 0)`个互不相交的有限集`T1, T2, ...Tm`，其中每个集合本身又是一棵树，称为原来树的**子树（SubTree）**

- 注意：
 - 子树之间`不可以相交`
 - 除了根节点以外，每个节点有且仅有一个父节点；
 - 一个`N`个节点的树有`N-1`条边。

- 树的术语：
 - 节点的度（Degree）：节点的子树个数。
 - 树的度：树的所有节点中最大的度数（树的度通常为节点个数的`N-1`）
 - 叶节点（Leaf）：度为`0`的节点（也称为叶子节点）
 - 父节点（Parent）：有子树的节点是其子树的父节点
 - 子节点（Child）：若`A`节点是`B`节点的父节点，则称`B`节点是`A`节点的子节点
 - 兄弟节点（Sibling）：具有同一个父节点的各节点彼此是兄弟节点
 - 路径和路径长度：从节点`n1`到`nk`的路径为一个节点序列`n1,n2,n3,...,nk`，`ni`是`ni+1`的父节点。路径所包含边的个数为路径的长度
 - 节点的层次（Level）：规定根节点在`第0层`，它的子节点是`第1层`，子节点的子节点是`第2层`，以此类推
 - 树的深度（Depth）：树中所有节点中的最大层次是这棵树的深度`（因为上面是从0层开始，深度 = 第最大层数 + 1）`

> :warning: 类比我们的家族谱就很容易理解了

### 二叉树

- 二叉树的定义：
 - 二叉树可以为空，也就是没有节点
 - 二叉树若不为空，则它是由根节点和称为其左子树`TL`和右子树`RT`的两个不相交的二叉树组成
 - 二叉树每个节点的子节点`不允许超过两个`

- 二叉树的五种形态
 - 空
 - 只有根节点
 - 只有左子树
 - 只有右子树
 - 左右子树都有

对应下面的图（从左到右）：

![bst](./imgs/bst.jpg "border_img_bst")

我们接下来要了解的是**二叉查找树（BST, Binary Search Tree）**。二叉查找树，也称为`二叉搜索树或二叉排序树`，是一种特殊的二叉树，相对值`小`的值保存在`左`节点中，`大`的值保存在`右`节点中。二叉查找树特殊的结构使得它能够快速地进行查找、插入和删除数据。

用代码实现理解下：

```javascript
// 辅助节点类
class Node {
  constructor(data, left, right){
    this.data = data;
    this.left = left;
    this.right = right;
  }
  // 展示节点信息
  show(){
    return this.data;
  }
}
class BST {
  constructor(){
      this.root = null;
  }
  // 插入数据
  insert(data){
    let n = new Node(data, null, null);
    if(this.root == null){
      this.root = n;
    }else{
      let current = this.root,
        parent = null;
      while(true){
        parent = current;
        if(data < current.data){
          current = current.left;
          if(current == null){
            parent.left = n;
            break;
          }
        }else{
          current = current.right;
          if(current == null){
            parent.right = n;
            break;
          }
        }
      }
    }
    return this;
  }
  // 中序遍历
  inOrder(node){
    if(!(node == null)){
      this.inOrder(node.left);
      console.log(node.show());
      this.inOrder(node.right);
    }
  }
  // 先序遍历
  preOrder(node){
    if(!(node == null)){
      console.log(node.show());
      this.preOrder(node.left);
      this.preOrder(node.right);
    }
  }
  // 后序遍历
  postOrder(node){
    if(!(node == null)){
      this.postOrder(node.left);
      this.postOrder(node.right);
      console.log(node.show());
    }
  }
  // 获取最小值
  getMin(){
    let current = this.root;
    while(!(current.left == null)){
      current = current.left;
    }
    return current.data;
  }
  // 获取最大值
  getMax(){
    let current = this.root;
    while(!(current.right == null)){
      current = current.right;
    }
    return current.data;
  }
  // 查找给定的值
  find(data){
    let current = this.root;
    while(current != null){
      if(current.data == data){
        return current;
      }else if(data < current.data){
        current = current.left;
      }else{
        current = current.right;
      }
    }
    return null;
  }
  // 移除给定的值
  remove(data){
    root = this.removeNode(this.root, data);
    return this;
  }
  // 移除给定值得辅助函数
  removeNode(node, data){
    if(node == null){
      return null;
    }
    if(data == node.data){
      // 叶子节点
      if(node.left == null && node.right == null){
        return null; // 此节点置空
      }
      // 没有左子树
      if(node.left == null){
        return node.right;
      }
      // 没有右子树
      if(node.right == null){
        return node.left;
      }
      // 有两个子节点的情况
      let tempNode = this.getSmallest(node.right); // 获取右子树
      node.data = tempNode.data; // 将其右子树的最小值赋值给删除的那个节点值
      node.right = this.removeNode(node.right, tempNode.data); // 删除指定节点下的最小值，也就是将其置空
      return node;
    }else if(data < node.data){
      node.left = this.removeNode(node.left, data);
      return node;
    }else{
      node.right = this.removeNode(node.right, data);
      return node;
    }
  }
  // 获取给定节点下的二叉树最小值的辅助函数
  getSmallest(node){
    if(node.left == null){
      return node;
    }else{
      return this.getSmallest(node.left);
    }
  }
}

let bst = new BST();
bst.insert(56).insert(22).insert(10).insert(30).insert(81).insert(77).insert(92);
bst.inOrder(bst.root); // 10, 22, 30, 56, 77, 81, 92
console.log('--中序和先序遍历分割线--');
bst.preOrder(bst.root); // 56, 22, 10, 30, 81, 77, 92
console.log('--先序和后序遍历分割线--');
bst.postOrder(bst.root); // 10, 30, 22, 77, 92, 81, 56
console.log('--后序遍历和获取最小值分割线--');
console.log(bst.getMin()); // 10
console.log(bst.getMax()); // 92
console.log(bst.find(22)); // Node { data: 22, left: Node { data: 10, left: null, right: null }, right: Node { data: 30, left: null, right: null } }
// 我们删除节点值为22，然后先序遍历的方法遍历，如下
console.log('--移除22的分割线--')
console.log(bst.remove(22).inOrder(bst.root)); // 10, 30, 56, 77, 81, 92
```

看了上面的代码之后，你是否感觉有些懵圈呢？

我们借助几张图来了解下，或许你就豁然开朗了。

在遍历的时候，我们分为三种遍历方法 -- **先序遍历，中序遍历和后序遍历**：

![bst_search](./imgs/bst_search.jpg "border_img_bst_search")

删除节点是一个比较複杂的操作，考虑的情况比较多：

- 该节点没有子节点的情况，直接将该节点置空
- 该节点只有左子树的情况，直接将该节点赋予左子树
- 该节点只有右子树的情况，直接将该节点赋予右子树
- 该节点左右子树都有的情况，有两种方法可以处理
 - 方案一：从待删除节点的`左`子树找到节点值`最大`的节点`A`，替换待删除节点值，并删除节点`A`
 - 方案二：从待删除节点的`右`子树找到节点值`最小`的节点`A`，替换待删除节点值，并删除节点`A`。`【上面代码演示的删除就是这种方案】`

删除两个节点的图解`（方案二）`如下：

![bst_remove](./imgs/bst_remove.jpg "border_img_bst_remove")