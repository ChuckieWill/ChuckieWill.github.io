---
title: 数据结构与算法(JavaScript)
date: 2020-03-12 10:43:17
tags:
- 数据结构
- 算法
categories:
- [数据结构与算法, JavaScript]
---





# 数据结构与算法(JavaScript)

##  1 时间复杂度与空间复杂度

* 时间复杂度：定性描述一个算法的运行时间
* 空间复杂度： 算法在运行过程中临时占用存储空间大小的量度

# 一、数据结构

## 1 栈（stack）

* 一种**后进先出**的数据结构

### 1.1 栈的应用场景

#####  十进制转二进制

![image-20200803172255803](数据结构与算法(JavaScript)/image-20200803172255803.png)

#####  有效的括号

![image-20200803172408631](数据结构与算法(JavaScript)/image-20200803172408631.png)

#####  函数调用堆栈

![image-20200803172448069](数据结构与算法(JavaScript)/image-20200803172448069.png)



##  2  队列（queue）

###  2.1 队列的应用场景

####  js异步中的任务队列

* js是单线程的，无法同时处理异步中的并发任务

####  计算最近请求次数

![image-20201002175325094](数据结构与算法(JavaScript)/image-20201002175325094.png)



##  3 链表

###  3.1 链表遍历、节点插入与删除

```js
const a = {val: "a"}
const b = {val: "b"}
const c = {val: "c"}
const d = {val: "d"}
a.next = b
b.next = c
c.next = d

//遍历链表
let p = a
while(p){
  console.log(p.val)
  p = p.next
}

//插入(e)
const e = {val: "e"}
c.next = e
e.next = d

//删除(e)
c.next = d
```



###  3.2 使用链表指针获取JSON节点值

```js
const json = {
  a : { b : { c : 1 } },
  d : { e : 2}
}

const path = ['d', 'e']

let p = json
path.forEach(k => {
  p = p[k]
})

//结果
p = 2
```



##  4 集合

**一种无序且唯一的数据结构**

###  4.1 集合基本用法

**通过集合实现数组去重**

* 利用了集合元素唯一的特性

```js
//去重
const arr = [1, 1, 1, 2, 3, 3, 4]
const arr2 = [...new Set(arr)]
```

![image-20201003175422004](数据结构与算法(JavaScript)/image-20201003175422004.png)

**判断元素是否在集合中**

* `.has()`方法判断，在集合中则返回true，否则返回false

```js
//判断元素是否在集合中
const set = new Set([1,2,3,4])
const has = set.has(0)
```

**求交集**

* 先将一个集合转化为数组，遍历数组看是否有元素在另一集合中，将符合条件的元素放在一个数组中
* 将交集元素的数组转化为集合，则求出了交集

```js
//求交集
const set1 = new Set([1,2,4,5])
const set2 = new Set([5,6,7,8])
const set3 = new Set([...set1].filter(item => set2.has(item)))

//执行结果
set3的值是5
```

###  4.2 ES6中set的使用

```js
//se6中set的使用

// 1 新建
let set = new Set()

// 2 添加元素
set.add(1)
set.add(5)
set.add(5) //此次不能添加， 因为集合元素无序且唯一
set.add('helo')
const obj = {a : 1, b :2}
set.add(obj)
set.add({a : 1, b :2}) //此次可以成功添加，因为对象内容虽然一样，但存储两个对象的地址不一样

// 3 集合中查找元素
const has = set.has(obj) //返回true
const has1 = set.has(5)  //返回true
const has2 = set.has({a : 1, b :2})  //返回false ,不知道问题在哪

// 4 删除集合中的元素
set.delete(5)     //可以正常删除
set.delete({a : 1, b :2})  //不能删除
set.delete(obj)   //可以正常删除


// 5 迭代，即循环集合
for(let item of set) console.log(item)
for(let item of set.values())  console.log(item)
for(let item of set.keys())  console.log(item)
  //以上三种方式输出结果一样，集合的values和keys属性一样
for(let [key, value] of set.entries())  console.log(key, value)
  //同时拿到集合的key和value值


// 6 数组与集合互转
// 6.1 集合转数组
const arr = [...set]
const arr1 = Array.from(set)
// 6.2 数组转集合
const set1 = new Set([1,2,3,4])


// 7 求集合的交集和差集
// 7.1 求集合交集
const intersection = new Set([...set].filter(item => set1.has(item)))
// 7.2 求差集
const difference = new Set([...set].filter(item => !set1.has(item)))

// 8 获取集合元素个数
console.log(set.size)
```





##  5 字典

**存储唯一值的数据结构，以键值对的形式来存储**

###  5.1 ES6中Map(字典)的使用

```js
//ES6中Map(字典)的使用
const map = new Map()

//添加元素
map.set('a', 'aa') //键为a 值为aa
map.set('a', 'aa') //不能添加，因为字典元素唯一性
map.set('b', 'bb')

//获取元素
console.log(map.get('a')) 

//删除元素
map.delete('a')
map.clear() //全部删除

//修改元素
map.set('b', 'bbbb') //覆盖以前的元素即可

//字典中查找元素
map.has('b') //有该元素则返回true,否则返回false

//遍历字典
//获取的循序就是value:值在前， key:键在  这里的value、key只是形参  可自定义名
map.forEach((value,key) => {
    console.log(value,key)
})
```





##  6 树

**一种分层数据的抽象模型**

###  6.1 深度优先搜索

**案例：**

![image-20201004111046427](数据结构与算法(JavaScript)/image-20201004111046427.png)

```js
//树的深度优先搜索
const tree = {
  val : 'a',
  children : [
    {
      val : 'b',
      children : [
        {
          val : 'd',
          children : []
        },
        {
          val : 'e',
          children : []
        }
      ]
    },
    {
      val : 'c',
      children : [
        {
          val : 'f',
          children : []
        },
        {
          val : 'g',
          children : []
        }
      ]
    }
  ]
}

const dfs = (root) => {
  // 1 访问根节点
  console.log(root.val)
  // 2 对根节点的children挨个进行深度优先遍历
  root.children.forEach(item => {
    dfs(item)
  });
  //root.children.forEach(dfs) //简写方式，forEach支持直接传入函数
}

dfs(tree)
```



###  6.2 广度优先搜索

**案例：**

![image-20201004111153347](数据结构与算法(JavaScript)/image-20201004111153347.png)

```js
//树的广度优先搜索
const tree = {
  val : 'a',
  children : [
    {
      val : 'b',
      children : [
        {
          val : 'd',
          children : []
        },
        {
          val : 'e',
          children : []
        }
      ]
    },
    {
      val : 'c',
      children : [
        {
          val : 'f',
          children : []
        },
        {
          val : 'g',
          children : []
        }
      ]
    }
  ]
}

// 1 新建一个队列，把根节点入队
// 2 把队头出队并访问
// 3 把队头的children挨个入队
// 4 重复2、3步骤，直到队列为空
const bfs = (root) => {
  // 1 新建一个队列，把根节点入队
  const q = [root]
  // 4 重复2、3步骤，直到队列为空
  while(q.length > 0){
    // 2 把队头出队并访问
    let item = q.shift()  //队头出队
    console.log(item.val) //访问队头
    // 3 把队头的children挨个入队
    item.children.forEach(child => {
      q.push(child)
    })
  }
}

bfs(tree)
```

###  6.3 二叉树

**树中每个节点最多只能有两个子节点**

* js中通过对象构建二叉树

```js
//二叉树
const bt = {
  val : 1,
  left : {
    val : 2,
    left : {
      val : 4,
      left : null,
      right : null
    },
    right : {
      val : 5,
      left : null,
      right : null
    }
  },
  right : {
    val : 3,
    left : {
      val : 6,
      left : null,
      right : null
    },
    right : {
      val : 7,
      left : null,
      right : null
    }
  }
}

module.exports = bt
```



####  6.3.1 先序遍历

**根节点先访问**

![image-20201004165702355](数据结构与算法(JavaScript)/image-20201004165702355.png)

**递归版**

```js
//二叉树的先序遍历
const bt = require('./bt') //导入二叉树


const preorder = (root) => {
  //访问到空节点，则返回
  if(!root) {
    return
  } 
  // 1 访问根节点
  console.log(root.val)
  // 2 先序遍历左节点
  preorder(root.left)
  // 3 先序遍历有右节点
  preorder(root.right)
}

preorder(bt)
```

**非递归版**

* 本质是仍然是递归的思想，借鉴了函数调用堆栈，自己模拟了一个栈来存储访问的节点

```js
//二叉树的先序遍历
const bt = require('./bt')

const preorder = (root) => {
  //访问到空节点，则返回
  if(!root) {return}
  // 自定义栈模拟函数访问堆栈，存储待访问节点
  let stack = [root]
  // 栈不为空时，循环访问
  while(stack.length){
    // 1 访问根节点
    const node = stack.pop() //出栈
    console.log(node.val)
    // 2 右节点压入栈 因为栈是先进后出，所以右节点先压入，才能后访问
    if(node.right) stack.push(node.right)
    // 3 左节点压入栈
    if(node.left) stack.push(node.left)
  }

}

preorder(bt)
```





####  6.3.2 中序遍历

**根节点在中间访问**

![image-20201004170102258](数据结构与算法(JavaScript)/image-20201004170102258.png)

**递归版**

```js
//二叉树中序遍历
const bt = require('./bt') //导入二叉树

const inorder = (root) => {
  //根节点为空，则返回
  if(!root) {return}
  // 1 中序遍历左节点
  inorder(root.left)
  // 2 访问根节点
  console.log(root.val)
  // 3 中序遍历右节点
  inorder(root.right)
}

inorder(bt)
```

**非递归版**

```js
//二叉树中序遍历
const bt = require('./bt')

const inorder = (root) => {
  //根节点为空，则返回
  if(!root) {return}
  const stack = []
  let p = root
  while(stack.length || p){
    // 左节点全部入栈
    while(p){
      stack.push(p)
      p = p.left
    }
    // 最左的节点出栈并访问  左节点的回退就包括了中节点的访问
    const node = stack.pop()
    console.log(node.val)
    // p指针指向最左节点的右子节点，便于下一次右子节点的左节点入栈
    p = node.right
  }

}

inorder(bt)

```



####  6.3.3 后序遍历

**根节点在最后访问**

![image-20201004171255922](数据结构与算法(JavaScript)/image-20201004171255922.png)

**递归版**

```js
//二叉树后序遍历
const bt = require('./bt')

const postorder = (root) => {
  //根节点为空，则返回
  if(!root) {return}
  // 1 后序遍历左节点
  postorder(root.left)
  // 2 后序遍历有节点
  postorder(root.right)
  // 3 访问根节点
  console.log(root.val)
}

postorder(bt)
```

**非递归版**

* 后序遍历压入栈的顺序正好是先序遍历输出的顺序，只是左右节点对调了一下，所以可以利用先序遍历输出算法来构建后续遍历的栈

```js
//二叉树后序遍历
const bt = require('./bt')

const postorder = (root) => {
  const outputStack = []
  const stack = [root]
  // 构建完整的后序遍历的栈
  while(stack.length){
    const node = stack.pop()
    outputStack.push(node)
    if(node.left) stack.push(node.left)
    if(node.right) stack.push(node.right)

  }
  // 后序遍历的栈元素依次出栈即是后序遍历的顺序
  while(outputStack.length){
    const node = outputStack.pop()
    console.log(node.val)
  }
}

postorder(bt)
```



##  7 图

> LeetCode：65、417、133

**图是网络结构的抽象模型，是一组由边连接的节点**

###  7.1 图的表示法

####  7.1.1 邻接矩阵

![image-20201005115937136](数据结构与算法(JavaScript)/image-20201005115937136.png)

#### 7.1.2 邻接表

![image-20201005120048155](数据结构与算法(JavaScript)/image-20201005120048155.png)

###  7.2 图的常用操作

**案例：**

* 图

![image-20201005123652847](数据结构与算法(JavaScript)/image-20201005123652847.png)

* 邻接表

```js
//图： 邻接表 
const graph = {
  0 : [1, 2],
  1 : [2],
  2 : [0, 3],
  3 : [3]
}

module.exports = graph
```

####  7.2.1 深度优先遍历

**算法思路：**

* 访问根节点
* 对根节点的**没有访问过的相邻节点**挨个进行深度优先遍历

```js
//图的深度优先遍历
const graph = require('./graph')

const visited = new Set()  //记录已经访问过的节点
//node: 节点值，邻接表的键
const dfs = (node) => {
  // 1 访问根节点
  console.log(node)
  visited.add(node)
  // 2 对根节点的没有访问过的所有相邻节点挨个进行深度优先遍历
  graph[node].forEach(item => {
    if(!visited.has(item)){
      dfs(item)
    }
  });
}

dfs(2)  //以2节点为根节点开始遍历
```

####  7.2.2 广度优先遍历

**算法思路：**

* 1 新建一个队列，把根节点入队
* 2 把队头出列并访问
* 3 把队头的**没有访问过的相邻节点**全部入队
* 4 重复2、3步，直到队列为空

```js
const graph = require('./graph')

const visited = new Set()//记录已访问节点
//node: 节点值，邻接表的键
const bfs = (node) => {
  //1 新建一个队列，把根节点入队
  let q = [node]
  visited.add(node)
  while(q.length){
    //2 把队头出列并访问
    let temp = q.shift()
    console.log(temp)
    //3 把队头的没有访问过的相邻节点全部入队
    graph[temp].forEach(item => {
      if(!visited.has(item)){
        q.push(item)
        visited.add(item)//标记已访问    ----注意：入队即表示访问了-----
      }
    })
  }
}

bfs(2)
```

###  7.3 leetcode

####  7.3.1 65 有效数字

![image-20201006104038726](数据结构与算法(JavaScript)/image-20201006104038726.png)

```js
//状态图
    //blank:空白， sign:+、-号， digit:数字
    const graph = {
        0: { 'blank': 0, 'sign': 1, '.': 2, 'digit': 6 }, 
        1: {'digit': 6, '.': 2},
        2: {'digit': 3},
        3: {'digit': 3, 'e':4},
        4: {'digit': 5, 'sign':7},
        5: {'digit': 5},
        6: {'digit': 6, '.': 3, 'e':4},
        7: {'digit': 5}
    }
```





##  8 堆

> LeetCode：215、347、23

**一种特殊的完全二叉树**

* 每一层节点都完全填满，最后一层如果不是满的，则只缺少右侧的若干节点

**最大堆：**

* 所有节点都大于等于它的子节点

**最小堆：**

* 所有节点都小于等于它的子节点

###  8.1 JS中的堆

![image-20201005211828741](数据结构与算法(JavaScript)/image-20201005211828741.png)

* JS中通常用数组来表示堆，以**广度优先遍历**顺序存储到数组中
* 左侧子节点的位置(数组中的索引值)：`2*index+1`
* 右侧子节点的位置(数组中的索引值)：`2*index+2`
* 父节点的位置： `Math.floor((index-1)/2)` 向下取整

###  8.2 堆的应用

####  8.2.1 求最大（小）值

**堆能高效地、快速地找出最大值或最小值、时间复杂度：O(1)**

* 最小堆的堆顶就是最小值
* 最大堆的堆顶就是最大值

####  8.2.2 求第K个最大（小）值

**找出第k个最大的元素**

* 1 构建一个最小堆，并将元素依次加入堆中
* 2  当堆的容量超过k，就删除堆顶
* 3 插入结束后，堆顶就是第K个最大的元素

**找出第K个最小元素**

* 使用找第K个最大元素的方法，只是换成构建一个最大堆

###  8.3 构建最小堆

**插入操作-算法思路**

* 1 将值插入堆的底部，即数组的尾部
* 2 然后上移：将这个值和它的父节点交换位置，直到父节点小于等于这个插入的值
* 插入操作时间复杂度：O(logN) 
  * 堆的大小为n(有n个节点)，插入的值最小的情况下，则大需要上移logN层

**删除堆顶-算法思路**

* 1 用数组尾部的元素替换堆顶（直接删除堆顶，会导致数组所有元素前移，破坏堆的结构）
* 2 然后下移：将堆顶的元素和它的子节点交换位置，直到它的子节点大于等于它
* 删除堆顶时间复杂度：O(logN)
  * 堆的大小为n(有n个节点)，队尾的值最大，则大需要下移logN层

```js
//最小堆
class MinHeap {
  constructor() {
    this.heap = [] //存储堆
  }
  //获取父节点
  getParent(index){
    return Math.floor((index-1)/2)
  }
  //获取左子节点
  getLeft(index){
    return index*2 + 1
  }
  //获取右子节点
  getRight(index){
    return index*2 + 2
  }
  //交换节点 
  swap(i1, i2){
    let temp = this.heap[i1]
    this.heap[i1] = this.heap[i2]
    this.heap[i2] = temp
  }
  //上移操作
  shiftUp(index){
    if(index === 0) return 
    let parentIndex = this.getParent(index)
    //父节点值大于子节点值则交换
    if(this.heap[parentIndex] > this.heap[index]){
      this.swap(parentIndex, index)
      this.shiftUp(parentIndex)
    }
  }
  //下移操作
  shiftDown(index){
    let leftIndex = this.getLeft(index)
    let rightIndex = this.getRight(index)
    //子节点值大于父节点值则交换
    if(this.heap[leftIndex] < this.heap[index]){
      this.swap(leftIndex, index)
      this.shiftDown(leftIndex)
    }
    if(this.heap[rightIndex] < this.heap[index]){
      this.swap(rightIndex, index)
      this.shiftDown(rightIndex)
    }
  }
  //插入
  insert(val){
    //1 插入数组尾部，堆底部
    this.heap.push(val)
    //2 上移
    this.shiftUp(this.heap.length-1)
  }
  //删除堆顶
  delTop(){
      //堆只有一个元素时，删除即可不用再执行下面的对调和下移，否则会报错
    if(this.getSize() === 1){
        this.heap.pop()
        return 
    }
    //1 用数组尾部的元素替换堆顶
    this.heap[0] = this.heap.pop()
    //2 下移
    this.shiftDown(0)
  }
  //获取堆顶
  getTop(){
    return this.heap[0]
  }
  //获取堆元素个数
  getSize(){
    return this.heap.length
  }
}

const h = new MinHeap()
h.insert(3)
h.insert(2)
h.insert(1)
h.insert(5)
h.insert(-1)
h.delTop()
```



#  二、结合前端

##  1 队列与前端

###   1.1 js异步中的任务队列

> [Javascript(0x06)-js异步中的任务队列)](https://chuckiewill.github.io/2020/10/02/Javascript/Javascript(0x06)-js%E5%BC%82%E6%AD%A5%E4%B8%AD%E7%9A%84%E4%BB%BB%E5%8A%A1%E9%98%9F%E5%88%97/)

##  2 链表与前端

###  2.1  使用链表指针获取JSON节点值

```js
const json = {
  a : { b : { c : 1 } },
  d : { e : 2}
}

const path = ['d', 'e']

let p = json
path.forEach(k => {
  p = p[k]
})

//结果
p = 2
```



##  3 树与前端

###  3.1 遍历JSON的所有节点值

```js
const json = {
  a : { b : { c : 1}},
  d : [110 , 112]
}

//root: json对象， path：访问节点的路径
const dfs = (root, path) => {
  console.log(root, path)
  Object.keys(root).forEach(item => {
    dfs(root[item], path.concat(item))
  })
}

dfs(json, [])

//输出结果
{ a: { b: { c: 1 } }, d: [ 110, 112 ] } ------ []
{ b: { c: 1 } } ------ [ 'a' ]
{ c: 1 } ------ [ 'a', 'b' ]
1 ------ [ 'a', 'b', 'c' ]
[ 110, 112 ] ------ [ 'd' ]
110 ------ [ 'd', '0' ]
112 ------ [ 'd', '1' ]
```





#  三、算法

> [算法可视化](https://visualgo.net/zh)

##  1 排序与搜索

> LeetCode： 21、374

###  1.1 排序

####  1.1.1  冒泡排序

* 时间复杂度：O(n*n)

```js
//冒泡排序  时间复杂度：O(n*n) 排序结果为升序
Array.prototype.bubbleSort = function () {
  for (let i = 0; i < this.length - 1; i++) {
    for (let j = 0; j < this.length - 1 - i; j++) {
      if (this[j] > this[j + 1]) {
        let temp = this[j]
        this[j] = this[j + 1]
        this[j + 1] = temp
      }
    }
  }
}

const arr = [5, 4, 3, 2, 1]
arr.bubbleSort()
```

####  1.1.2 选择排序

* 时间复杂度O(n*n)

```js
//选择排序  排序结果为升序
Array.prototype.selSort = function () {
  for(let i =0;i<this.length-1;i++){
    let sel = i
    for(let j = i;j<this.length;j++){
      if(this[j] < this[sel]){
        sel = j
      }
    }
    if(sel !== i){
      let temp = this[i]
      this[i] = this[sel]
      this[sel] = temp
    }
   
  }
}

const arr = [5, 4, 3, 2, 1]
arr.selSort()
console.log(arr)
```

####  1.1.3 插入排序

* 时间复杂度：O(n*n)

```js
//插入排序  排序结果为升序
Array.prototype.insertSort = function () {
  for (let i = 1; i < this.length; i++) {
    let temp = this[i]
    let j = i
    while(j > 0){
      if(this[j-1] > temp){
        this[j] = this[j-1]
      }else{
        break
      }
      j--
    }
    this[j] = temp
  }
}

const arr = [5, 4, 3, 2, 1]
arr.insertSort()
console.log(arr)
```

####  1.1.4 归并排序

* 时间复杂度：O(n*logn)
  * 分时间复杂度：O(logn)
  * 合时间复杂度：O(n)

```js
//归并排序  排序结果为升序
Array.prototype.mergeSort = function () {
  const dfs = (arr) => {
    //1 拆分
    //1.1 拆分终点（单个元素）
    if(arr.length === 1) return arr
    const mid = Math.floor(arr.length / 2)
    const left = arr.slice(0, mid)
    const right = arr.slice(mid, arr.length)
    //1.2 递归拆分 直到为单个元素
    const orderLeft = dfs(left)
    const orderRight = dfs(right)
    //2 合并
    const res = [] //用于存储合并后的有序数组元素
    while(orderLeft.length || orderRight.length){
      if(orderLeft.length && orderRight.length){
        res.push(orderLeft[0] < orderRight[0] ? orderLeft.shift() : orderRight.shift())
      }else if(orderLeft.length){
        res.push(orderLeft.shift())
      }else if(orderRight.length){
        res.push(orderRight.shift())
      }
    }
    return res
  }
  const res = dfs(this)
  res.forEach((val , i) => {
    this[i] = val
  })
}

const arr = [5, 4, 3, 2, 1]
arr.mergeSort()
console.log(arr)
```

####  1.1.5 快速排序

* 时间复杂度：O(n*logn)
  * 递归时间复杂度：O(logn)
  * 分区时间复杂度：O(n)

```js
//归并排序  排序结果为升序 时间复杂度：O(n*logn)
Array.prototype.quickSort = function () {
  const dfs = (arr) => {
    //数组一个元素或空数组时为递归终点 当输入为有序数组时可能才分的左右数组为空数组
    if(arr.length === 1 || arr.length === 0){
      return arr
    } 
    const left = [] //存储比选取元素小的值
    const right = [] //存储比选取元素大的值
    const mid = arr[0] //任意选取一个参考值
    //分区，比参考值大的在右，比参考值小的在左
    for(let i = 1;i<arr.length;i++){
      if(arr[i]<mid){
        left.push(arr[i])
      }else{
        right.push(arr[i])
      }
    }
    return [...dfs(left),mid,...dfs(right)]
  }
  const res = dfs(this)
  res.forEach((val , i) => {
    this[i] = val
  })
}

const arr = [5, 4, 3, 2, 1]
arr.quickSort()
console.log(arr)
```



###  1.2 搜索

####  1.2.1 顺序搜索

* 时间复杂度：O(n)

```js
//顺序搜索
Array.prototype.sequentialSearch = function (item) {
  for(let i=0;i<this.length;i++){
    if(this[i] === item){
      return i //找到结果则返回下标
    }
  }
  return -1 //没找到结果则返回-1

}

const res = [5,4,3,2,1].sequentialSearch(9)
console.log(res)
```

####  1.2.2 二分搜索

* 时间复杂度：O(logn)

```js
//二分搜索  ---前提：数组为有序数组  时间复杂度：O(logn)
Array.prototype.binarySearch = function (item) {
  let low = 0
  let high = this.length-1
  while(low <= high){
    let mid = Math.floor((low+high) / 2)
    if(item < this[mid]){
      high = mid-1
    }else if(item > this[mid]){
      low = mid+1
    }else{
      return mid //找到结果则返回下标
    }
  }
  return -1 //没找到结果则返回-1
}

const res = [1,2,3,4,5].binarySearch(1)
console.log(res)
```



##  2 分而治之

> 它将一个问题**分**成多个和原问题相似的小问题，**递归解决**小问题，再将小问题**合并**以解决原来的问题
>
> 
>
> LeetCode：374、226、100、101

* 归并排序
* 快速排序

##   3 动态规划

> 它将一个问题分解为**互相重叠**的子问题，通过反复求解子问题，来解决原来的问题
>
> 
>
> 动态规划 VS 分而治之
>
> * 子问题是相互重叠的（例：斐波拉切数列） 则是动态规划
> * 子问题是相互独立的（例：[翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)）则是分而治之
>
> 
>
> LeetCode： 70、198

* 斐波拉切数列

![image-20201008144728198](数据结构与算法(JavaScript)/image-20201008144728198.png)

##  4 贪心算法

> 期盼通过每个阶段的**局部最优**选择，从而达到全局的最优
>
> * 结果并**不一定最优**
>
> 
>
> leetcode: 455、122

**不是最优解案例：**

* 零钱兑换

  * 用最少的硬币数达到要求的兑换总额
  * coins: 可以使用的面额， amount: 兑换总额
  * 情况1是最优解，但是情况2可以是2个3，不是最优解

  ![image-20201008164156548](数据结构与算法(JavaScript)/image-20201008164156548.png)



##  5 回溯法

> LeetCode：46、78
>
> 回溯法是一种**渐进式**寻找并构建问题解决方案的策略
>
> 回溯法会先从一个可能的动作开始解决问题，如果不行，就回溯并选择另一个动作，直到解决问题

