### 前言
在之前的[JavaScript数据结构之栈](https://blog.darwin.fun/posts/23)的博客中，我们了解栈这种数据结构，接下来我们来学习队列。队列和栈非常相似，但是使用了后进先出不同的原则，接下来让我们学习关于队列的相关知识。

### 队列数据结构
队列是遵循**先进先出**（FIFO，也称为先来先服务）原则的一组有序的项。队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。
 
在现实中，最常见的队列的例子就是排队。在电影院、自助餐厅、杂货店收银台，我们都会排队，排在第一位的人会先接受服务。
![img_01](https://api.darwin.fun/images/serve?name=BLOG_17_IMG_01.png)

#### 创建队列
了解了队列的原则后，我们来创建一个Queue类来表示队列。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }
}
```
首先需要一个用于存储队列中元素的数据结构。和栈一样，我们同样可以使用数组来完成，但是为了写出一个在获取元素时更高效的数据结构，我们将使用一个对象来存储我们的元素。你会发现其实Queue类和Stack类非常相似，只是添加和移除元素的原则不同。
 
需要声明一个count属性来帮助我们控制队列的大小。此外，由于我们将要从队列的前端移除元素，同样需要一个变量来帮助我们追踪第一个元素，还需声明一个lowestCount变量。
 
接下来需要声明一些队列可用的方法。
> * enqueue(element(s))：向队列尾部添加一个（或多个）新的项。
> * dequeue()：移除队列的第一项（即排在队列最前面的项）并返回被移除的元素。
> * peek()：返回队列中第一个元素————最先被添加，也将是最先被移除的元素。队列不做任何变动（不移除元素，只返回元素信息————与Stack类的peek方法非常类似）。该方法在其他语言中也可以叫作front方法。
> * isEmpty()：如果队列中不包含任何元素，返回true，否则返回false。
> * size()：返回队列包含的元素个数，与数组的length属性相似。

1.向队列添加元素
首先要实现的**enqueue**方法。该方法负责向队列添加新元素。此处一个非常重要的细节是新的项只能添加到队列末尾。
**enqueue**方法和Stack类中的**push**方法的实现方式相同。由于items属性是一个JavaScript对象，它是一个**键值对**的集合。要向队列中加入一个元素的话，我们要把 count 变量作为 items 对象中的值，对应的元素作为它的值。将元素加入队列后，我们将 count 变量加1。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }
}
```
 
2.检查队列是否为空并获取它的长度
下一个是**isEmpty**方法和一个**size**方法。如果队列为空，它会返回true，否则非返回false。要计算队列中有多少元素，我们只需要计算count和lowestCount之间的差值即可。
假设 count 属性的值为2，lowestCount 的值为0。这表示在队列中有两个元素。然后，我们从队列中移除一个元素，lowestCount 的值会变为1，count 的值仍然为2。现在队列中只有一个元素了，以此类推。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }
}
```
 
3.从队列移除元素
接下来要实现**dequeue**方法，该方法负责从队列移除项。由于队列遵循先进先出原则，最先添加的项也是最先被移除的。
首先，我们需要检验队列手否为空，如果为空，我们返回 undefined 值。如果队列不为空，我们将暂存队列的头部的值，以便该元素被移除后将它返回，再将lowestCount属性加1。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  dequeue(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    const result = this.items[this.lowestCount];
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return result;
  }
}
```
只有**enqueue**方法和**dequeue**方法可以添加和移除元素，这样就确保了 Queue 类遵循**先进先出原则**。

4.查看队列头元素
现在来为我们的类实现一些额外的辅助方法。如果想知道队列最前面的项是什么，可以用**peek**方法。该方法会返回队列最前面的项（把 lowestCount 作为键名来获取元素值）。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  dequeue(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    const result = this.items[this.lowestCount];
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return result;
  }

  peek(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    return this.items[this.lowestCount];
  }
}
```

5.清空队列
要清空队列中的所有元素，我们可以调用**dequeue**方法直到它的返回undefined，也可以简单地将队列中的属性值重设为和构造函数中的一样。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  dequeue(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    const result = this.items[this.lowestCount];
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return result;
  }

  peek(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    return this.items[this.lowestCount];
  }

  clear() {
    this.items = {};
    this.count = 0;
    this.lowestCount = 0;
  }
}
```

6.创建 toString 方法
最后，我们可以增加一个**toString**方法。
```ts
class Queue<T> {
  constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
  }

  enqueue(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  dequeue(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    const result = this.items[this.lowestCount];
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return result;
  }

  peek(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    return this.items[this.lowestCount];
  }

  clear() {
    this.items = {};
    this.count = 0;
    this.lowestCount = 0;
  }

  toString(): string {
    if(this.isEmpty()) {
      return '';
    }

    let objString = `${this.items[this.lowestCount]}`;
    for(let i = this.lowestCount + 1;i < this.count;i++) {
      objString = `${objString},${this.items[i]}`;
    }

    return objString;
  }
}
```

#### 使用 Queue 类
首先我们要做的是实例化我们刚刚创建的 Queue 类，然后就可以验证为空（输出为true，因为我们还没有向队列中添加任何元素）。
```ts
const queue = new Queue();
console.log(queue.isEmpty()); // 输出为true
```
接下来我们可以添加一些元素。
```ts
queue.enqueue('李白');
queue.enqueue('杜甫');
queue.enqueue('白居易');

console.log(queue.toString()); // 输出为 李白，杜甫，白居易
console.log(queue.size()); // 输出为 3
console.log(queue.isEmpty()); // false
queue.dequeue(); // 移除李白
queue.dequeue(); // 移除杜甫
console.log(queue.toString()); // 输出为白居易
```

### 双端队列数据结构
**双端队列**是一种允许我们同时从前端和后端添加和移除元素的特殊队列。
 
双端队列在现实生活中的例子有电影院、餐厅中排队的队伍等。举个例子，一个刚买了票的人如果只是还需要再问一些简单的信息，他就可以直接回到队伍的头部。另外，在队伍末尾的人如果赶时间，他可以离开队伍。
 
在计算机科学中，双端队列的一个常见应用是存储一系列的撤销操作。每当用户在软件中进行了一个操作，该操作就会被存储在一个双端队列中（就像是一个栈里）。当用户点击撤销按钮时，该操作会被双端队列中弹出，表示它被从后面移除了。在进行了预先定义的一定数量的操作后，最先进行的操作会被双端队列的前端移除。由于双端队列同时遵守了**先进先出**和**后进先出**原则，可以说是它把队列和栈组合的一种数据结构。

#### 创建 Deque 类
和队列一样，我们先声明一个 Deque 类及其构造函数。既然双端队列是一种特殊的队列，所以具有部分相同的属性和方法。
```ts
class Deque<T> {
   constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
   }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  clear() {
    this.items = {};
    this.count = 0;
    this.lowestCount = 0;
  }

  toString(): string {
    if(this.isEmpty()) {
      return '';
    }

    let objString = `${this.items[this.lowestCount]}`;
    for(let i = this.lowestCount + 1;i < this.count;i++) {
      objString = `${objString},${this.items[i]}`;
    }

    return objString;
  }
}
```
由于双端队列允许在两端添加和移除元素，还会有以下几个方法。
> * addFront(element)：该方法在双端队列前端添加新的元素。
> * addBack(element)：该方法在双端队列后端添加新的元素（实现方法和Queue类中的enqueue相同）。
> * removeFront()：该方法会从双端队列前端移除第一个元素（实现方法和Queue类中的dequeue方法相同）。
> * removeBack()：该方法会从双端队列后端移除第一个元素（实现方法和Stack类中的pop方法相同）。
> * peekFront()：该方法返回双端队列前端的第一个元素（实现方法和Queue类中的peek方法相同）。
> * peekBack()：该方法返回双端队列后端的第一个元素（实现方法和Stack类中的peek方法相同）。

由于其他的方法都可以在之前的博客中找到方法，所以不在进行说明，我们只专注于实现addFront方法的逻辑。
```ts
class Deque<T> {
   constructor() {
    this.count = 0;
    this.lowestCount = 0;
    this.items = {};
   }

  size(): number {
    return this.count - this.lowestCount;
  }

  isEmpty(): boolean {
    return this.size() === 0;
  }

  clear() {
    this.items = {};
    this.count = 0;
    this.lowestCount = 0;
  }

  toString(): string {
    if(this.isEmpty()) {
      return '';
    }
    let objString = `${this.items[this.lowestCount]}`;
    for(let i = this.lowestCount + 1;i < this.count;i++) {
      objString = `${objString},${this.items[i]}`;
    }
    return objString;
  }

  addBack(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  removeFront(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    const result = this.items[this.lowestCount];
    delete this.items[this.lowestCount];
    this.lowestCount++;
    return result;
  }

  removeBack(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    this.count--;
    const result = this.items[this.count];
    delete this.items[this.count];
    return result;
  }

  peekFront(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    return this.items[this.lowestCount];
  }

  peekBack(): T | undefined {
    if(this.isEmpty()) {
      return undefined;
    }
    return this.items[this.count - 1];
  }

  addFront(element: T) {
    if(this.isEmpty()) {
      // 第一种情况，双端队列为空
      this.addBack(element);
    }else if(this.lowestCount > 0) {
      // 第二种情况，双端队列第一个元素从前端被移除
      this.lowestCount--;
      this.items[this.lowestCount] = element;
    }else {
      // 第三种情况，lowestCount为0时
      for(let i = this.count;i > 0;i--) {
        this.items[i] = this.items[i - 1];
      }
      this.count++;
      this.lowestCount = 0;
      this.items[0] = element;
    }
  }
}
```

#### 使用 Deque 类
```ts
const deque = new Deque();
console.log(deque.isEmpty()); // true
deque.addBack('李白');
deque.addBack('杜甫');
console.log(deque.toString()); // 李白，杜甫
deque.addBack('白居易');
console.log(deque.toString()); // 李白，杜甫，白居易
console.log(deque.size()); // 3
console.log(deque.isEmpty()); // false
deque.removeFront();
console.log(deque.toString()); // 杜甫，白居易
deque.removeBack();
console.log(deque.toString()); // 杜甫
deque.addFront('李白');
console.log(deque.toString()); // 李白，杜甫
```

### 使用队列和双端队列来解决问题
现在我们知道怎么使用Queue类和Deque类，就用它们解决一些计算机科学的问题。例如使用队列模拟击鼓传花游戏，并使用双端队列来检查一个短语是否为回文。

#### 循环队列————击鼓传花游戏
由于队列经常被应用在计算机领域和我们的现实生活中，就出现了一些队列的修改版本，其中一种叫做**循环队列**。循环队列的一个例子就是击鼓传花游戏，在这个游戏中，孩子们围成一个圆圈，把花尽快地传递给旁边的人。某一时刻传花停止，这个时候花在手里，谁就退出圆圈，结束游戏。重复该过程，直至只剩下一个孩子即为胜者。
```ts
/**
 * @elementList 参与游戏的人
 * @num 花的传递次数
 */
function hotPotato(elementsList: Array<string>, num: number): { eleliminated: Array<string>, winner:string } {
  // 初始化一个队列
  const queue = new Queue();
  // 定义一个存储淘汰人的数组
  const eliminatedList: Array<string> = [];
  // 将参与的人依次放入队列中
  elementsList.forEach((element:string) => {
    queue.enqueue(element);
  })
  // 进行传花游戏
  while(queue.size() > 1) {
    for(let i = 0;i < num;i++) {
      queue.enqueue(queue.dequeue());
    }
    // 淘汰
    eleliminatedList.push(queue.dequeue());
  }
  return {
    eleliminated: eleliminatedList,
    winner: queue.toString();
  } 
}

// 验证
const player = ['李白','杜甫','白居易','杜牧','辛庆疾'];
const result = hotPotato(player,7);
result.eleliminated.forEach(item => console.log(`${item}在击鼓传花游戏中被淘汰。`));
console.log(`胜者：${result.winner}`);
// 白居易在击鼓传花游戏中被淘汰。
// 杜甫在击鼓传花游戏中被淘汰。
// 辛庆疾在击鼓传花游戏中被淘汰。
// 杜牧在击鼓传花游戏中被淘汰。
// 胜者：李白
```
具体的过程如下图所示：
![img_02](https://api.darwin.fun/images/serve?name=BLOG_17_IMG_02.png)

#### 回文检查器
什么是回文？回文是**正反都能读通的单词、词组、数或一系列字符的序列**，例如 madam 或 racecar。
 
有不同的算法可以检查一个词组或字符串是否为回文。最简单的方式是将字符串反向排列并检查它和原字符是否相同。如果相同，那么它就是一个回文。我们可以使用栈来完成，也可以使用队列来完成，但是利用数据结构来解决这个问题最简单的方法是使用双端队列。
```ts
function palidromeChecker(aString: string): boolean {
  // 排除特殊情况
  if(aString === undefined || aString === null || (aString !== null && aString.length === 0)) {
    return false;
  }
  // 初始化一个双端队列
  const deque = new Deque();
  // 将传入的字符串进行初始化
  const lowerString = aString.toLocalLowerCase().split(' ').join('');
  let isEqual = true;
  let firstChar, lastChar;
  for(let i = 0;i < lowerString.length;i++) {
    deque.addBack(lowerString.charAt(i));
  }
  while(deque.size() > 1 && isEqual) {
    firstChar = deque.removeFront();
    lastChar = deque.removeBack();
    if(firstChar !== lastChar) isEqual = false;
  }
  return isEqual;
}

// 验证
console.log(palidromeChecker('a')); // true
console.log(palidromeChecker('aa')); // true
console.log(palidromeChecker('kayak')); // true
console.log(palidromeChecker('level')); // true
console.log(palidromeChecker('asdas')); // false
console.log(palidromeChecker('Was it a car or a cat I saw')); // true
console.log(palidromeChecker('Step on no pets')); // true
```

### 结尾
本篇博客介绍了队列这种数据结构，我们实现了一个队列的类，并学习了如何通过 enqueue 方法和 dequeue 方法去实现一个遵循先进先出原则来添加和移除元素。同时我们还了解了一种特殊的队列，双端队列。同样实现了一个双端队列的类，并通过创建的类解决了击鼓传花游戏和回文检查的算法，希望大家看完能够有所收获。
![img_03](https://api.darwin.fun/images/serve?name=BLOG_17_IMG_03.jpg)