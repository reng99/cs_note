# 圖

![graph](./imgs/graph.jpg "border_img_graph")

**圖**由邊的集合以及頂點的集合組成。

我們來了解下圖的相關術語：

- 頂點：圖中的一個節點
- 邊：表示頂點和頂點之間的連線
- 相鄰頂點：由一條邊連接在一個的頂點稱為相鄰頂點
- 度：一個頂點的度是相鄰頂點的數量。比如`0`頂點和其他兩個頂點相連，`0`頂點的度就是`2`
- 路徑：路徑是頂點`v1,v2,...,vn`的一個連續序列
 - 簡單路徑：簡單路徑要求不包含重複的頂點
 - 迴路：第一個頂點和最後一個頂點相同的路徑稱為迴路
- 有向圖和無向圖
 - 有向圖表示圖中的`邊`是`有`方向的
 - 無向圖表示圖中的`邊`是`無`方向的 
- 帶權圖和無權圖
 - 帶權圖表示圖中的`邊`是`有`權重的
 - 無權圖表示圖中的`邊`是`無`權重的

圖可以用於現實中的很多系統建模，比如：

- 對交通限流建模
 - 頂點可以表示街道的十字路口，邊表示街道
 - 加權的邊可以表示限速或者車道的數量或街道的距離
 - 建模人員可以用這個系統來判定最佳路線以及可能擁堵的街道

我們用代碼來加深理解：

```javascript
class Graph{
  constructor(v){
    this.vertices = v; // 頂點個數
    this.edges = 0; // 邊的個數
    this.adj = []; // 鄰接表或鄰接數組
    this.marked = []; // c存儲頂點是否被訪問過的標識
    this.init();
  }
  init(){
    for(let i = 0; i < this.vertices; i++){
      this.adj[i] = [];
      this.marked[i] = false;
    }
  }
  // 添加邊
  addEdge(v, w){
    this.adj[v].push(w);
    this.adj[w].push(v);
    this.edges++;
    return this;
  }
  // 展示圖
  showGraph(){
    for(let i = 0; i < this.vertices; i++){
      for(let j = 0; j < this.vertices; j++){
        if(this.adj[i][j] != undefined){
          console.log(i +' => ' + this.adj[i][j]);
        }
      }
    }
  }
  // 深度優先搜索
  dfs(v){
    this.marked[v] = true;
    if(this.adj[v] != undefined){
      console.log("visited vertex: " + v);
    }
    this.adj[v].forEach(w => {
      if(!this.marked[w]){
        this.dfs(w);
      }
    })
  }
  // 廣度優先搜索
  bfs(v){
    let queue = [];
    this.marked[v] = true;
    queue.push(v); // 添加到隊尾
    while(queue.length > 0){
      let v = queue.shift(); // 從隊首移除
      if(v != undefined){
        console.log("visited vertex: " + v);
      }
      this.adj[v].forEach(w => {
        if(!this.marked[w]){
          this.marked[w] = true;
          queue.push(w);
        }
      })
    }
  }
}

let graphFirstInstance = new Graph(5)
graphFirstInstance.addEdge(0, 1).addEdge(0, 2).addEdge(1, 3).addEdge(2, 4)
graphFirstInstance.showGraph()
// 0 => 1
// 0 => 2
// 1 => 0
// 1 => 3
// 2 => 0
// 2 => 4
// 3 => 1
// 4 => 2
// ❓為什麼會被出現這種數據呢？它對應的圖是什麼呢？可以思考🤔下，動手畫畫圖什麼的
console.log('--展示圖和深度優先搜索的分割圖--')
graphFirstInstance.dfs(0); // 從頂點 0 開始的深度搜索
// visited vertex: 0
// visited vertex: 1
// visited vertex: 3
// visited vertex: 2
// visited vertex: 4
console.log('--深度優先搜索和廣度優先搜索的分割線--')
let graphSecondInstance = new Graph(5)
graphSecondInstance.addEdge(0, 1).addEdge(0, 2).addEdge(1, 3).addEdge(2, 4)
graphSecondInstance.bfs(0) // 從頂點 0 開始的廣度搜索
// visited vertex: 0
// visited vertex: 1
// visited vertex: 2
// visited vertex: 3
// visited vertex: 4
```

對於搜索圖，在上面我們介紹了**深度優先搜索 - DFS（Depth First Search）和廣度優先搜索 - BFS（Breadth First Search）**，結合下面的圖再回頭看下上面的代碼，你會更加容易理解這兩種搜索圖的方式。

![graph_search](./imgs/graph_search.jpg "border_img_graph_search")

