### 前言
栈是一种遵从**后进先出(LIFO)**原则的有序集合。新添加或待删除的元素都保存在栈的同一端，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。栈也被用在编程语言的编译器和内存中存储保存变量、方法调用等，也被用于浏览器历史记录，浏览器路由的存储模式就是一种栈结构。在现实生活中也能发现很多栈的例子。例如，下图里的一摞书或者餐厅里叠放的盘子。
![一摞书](https://api.darwin.fun/images/serve?name=BLOG_15_IMG_01.jpg)

### 创建一个基于数组的栈
接下来，我们将创建一个类来表示栈，来慢慢了解一下数据结构栈。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }
}
```
我们需要一种数据结构来保存栈中的元素。可以选择数组，如上例子。数组允许我们在任何位置或添加删除元素。由于栈遵循LIFO原则，需要对元素的插入和删除功能进行限制。接下来，要为栈声明一些方法。
* push(element(s))：添加一个或几个新元素到栈顶。
* pop()：移除栈顶的元素，同时返回被移除的元素。
* peek()：返回栈顶的元素，不对栈做任何修改（该方法不会移除栈顶的元素，仅仅返回它）。
* isEmpty()：如果栈里没有任何元素就返回true，否则返回false。
* clear()：移除栈里的所有元素。
* size()：返回栈里的元素个数。该方法和数组的length属性很类似。

#### 向栈添加元素
我们实现的第一个方法是push。该方法负责往栈里添加新元素，有一点很重要：该方法只添加元素到栈顶，也就是栈的末尾。因为我们使用的是数组来作为栈的存储结构，所以我们可以使用数组中的push方法来完成栈的添加元素方法。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }

  push(element:T) {
    this.items.push(element);
  }
}
```

#### 从栈顶移除元素
接着，我们来实现pop方法。该方法主要用来移除栈顶的元素。我们可以使用数组中的pop()方法来移除栈顶的元素。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }

  push(element:T) {
    this.items.push(element);
  }

  pop():T {
    return this.items.pop();
  }
}
```

#### 查看栈顶元素
实现栈的主要方法后，接下来为我们的类实现实现一些额外的辅助方法。如果想知道栈里最后添加的元素是什么，可以用peek方法。该方法将返回栈顶的元素。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }

  push(element:T) {
    this.items.push(element);
  }

  pop():T {
    return this.items.pop();
  }

  peek():T {
    return this.items[this.items.length - 1];
  }
}
```

#### 检查栈是否为空
下一个要实现的方法是isEmpty，如果栈为空的话将返回true，否则返回false。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }

  push(element:T) {
    this.items.push(element);
  }

  pop():T {
    return this.items.pop();
  }

  peek():T {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }
}
```

#### 清空栈元素
最后，我们来实现clear方法。clear方法用来移除栈里所有的元素，把栈清空。实现该方法最简单的方式如下。
``` typescript
class Stack<T> {
  private items: Array<T> = [];

  constructor() {
    this.items = [];
  }

  push(element:T) {
    this.items.push(element);
  }

  pop():T {
    return this.items.pop();
  }

  peek():T {
    return this.items[this.items.length - 1];
  }

  isEmpty(): boolean {
    return this.items.length === 0;
  }

  clear() {
    this.items = [];
  }
}
```

#### 使用Stack类
在深入了解栈的应用前，我们先来学习如何使用stack类。首先，需要初始化Stack类，然后验证一下栈是否为空。请看下面的例子：
![img_02](https://api.darwin.fun/images/serve?name=BLOG_15_IMG_02.png)
``` typescript
const stack = new Stack();
console.log(stack.isEmpty()); // true

stack.push(5); // 推入元素
stack.push(8); // 推入元素
console.log(stack.peek()); // 栈顶元素为8

stack.push(11); // 推入元素
console.log(stack.size()); // 3
console.log(stack.isEmpty()); // false

stack.pop(); // 移除栈顶元素
stack.pop(); // 移除栈顶元素
console.log(stack.size()) // 1
```

### 用栈解决问题
栈的实际应用非常广泛。在回溯问题中，它可以存储访问过的任务或路径、撤销的操作。下面我们用栈解决一些问题。

#### 从十进制到二进制
现实生活中，我们主要使用十进制。但在计算机科学中，二进制非常重要，因为计算机里的所有内容都是用二进制数字表示的（0和1）。没有十进制和二进制相互转化的能力，与计算机交流就很困难。
 
要把十进制转化二进制，我们可以将该十进制除以2（二进制是满二进一）并对商取整，直到结果为0为止。举个例子，如下图所示：
![img_01](https://api.darwin.fun/images/serve?name=BLOG_15_IMG_01.png)

```ts
function decimalToBinary(decNumber) {
  const remStack = new Stack();
  let number = decNumber; // 要转化的十进制数
  let rem; // 余数
  let binartString = ''; // 输出的二级制结果

  while(number > 0) {
    rem = Math.floor(number % 2);
    remStack.push(rem);
    number = Math.floor(number / 2);
  }

  while(!remStack.isEmpty()) {
    binaryString += `${remStack.pop()}`;
  }

  return binaryString;
}
```

#### 进制转换算法
我们可以修改之前的算法，使之能把十进制转化为基数为2~36的任意进制。除了把十进制数除以2转化成二进制数，还可以传入任意进制的基数为参数，具体代码实现如下：
```ts
/**
 * @params decNumber 要转化的数字
 * @params base 进制基数
 */
function baseConverter(decNumber, base) {
  const remStack = new Stack();
  const digits = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
  let number = decNumber;
  let rem;
  let baseString = '';

  if(!(base >= 2 && base <= 36)) {
    return '';
  }

  while(number > 0) {
    rem = Math.floor(number % base);
    remStack.push(rem);
    number = Math.floor(number / base);
  }

  while(!remStack.isEmpty()) {
    baseString += `${digits[remStack.pop()]}`;
  }

  return baseString;
}
```

### 结尾
我们学习了栈这一数据结构的相关知识。我们使用了一个数组自己实现了栈，还讲解了如何使用push和pop往栈里添加和移除元素，还学习了使用栈来解决计算机科学中经典的问题，希望大家看完后能有所收获。
