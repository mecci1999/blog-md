### 前言
我们之前学习了数组、栈、队列这三种数据结构，可以发现这三种数据结构有一个相似的地方，无论是先进先出原则还是后进先出原则，都是遵循着特定的原则进行元素的添加和移除。接下来，我们将学习如何实现和使用链表这种动态的数据结构，来实现随意添加和移除项。

### 链表数据结构
要存储多个元素，数组（或列表）可能是最常用的数据结构。然而，这种数据结构都有一个**缺点**：（在大多数语言中）数组的大小都是**固定**的，从数组的起点或中间插入或移除元素的成本很高，因为需要**移动元素**。（尽管我们已经学过，JavaScript 有来自 Array 类的方法可以帮助我们达到想要的结果，但背后的情况同样如此）。
 
链表存储有序的元素集合，但不同于数组，链表中的元素**在内存中并不是连续放置的**。每个元素由**一个存储元素本身的节点**和**一个指向下一个元素的引用**（也称**指针或链接**）组成。
 
下图展示了一个链表的数据结构：
![img_01](https://blog.darwin.fun/images/serve?name=BLOG_27_IMG_01.png)
 
相对于传统的数组，链表的好处在于，添加或移除元素的时候不需要移动其他元素。然而，链表需要使用指针，因此实现链表时需要额外注意。在数组中，我们可以直接访问任何位置的任何元素，而想访问链表中间的一个元素，则需要从起点（表头）开始迭代链表直到找到所需的元素。

### 创建链表
理解了链表是什么之后，现在我们就要开始实现我们的数据结构了。以下是 LinkedList 类的“骨架”。
```ts
// 比较函数
function defaultEquals(a, b) {
  return a === b;
}

// 节点类
export class Node<T> {
  constructor(element:T) {
    this.elment = element; // 元素
    this.next = undefined; // 指针
  }
}

// 链表类
export class LinkedList {
  constructor(equalsFn = defaultEquals) {
    this.count = 0; // 链表中的元素个数
    this.head = undefined; // 表头
  }
}
```

接着，就是LinkedList 类的方法，如下：
> * **push(element)**：向链表尾部添加一个新元素。
> * **insert(element, position)**：向链表的特定位置插入一个新元素。
> * **getElement(index)**：返回链表中特定位置的元素。如果链表中不存在这样的元素，则返回 undefined。
> * **remove(element)**：从链表中移除一个元素。
> * **indexOf(element)**：返回元素在链表中的索引。如果链表中没有该元素则返回-1。
> * **removeAt(position)**：从链表的特定位置移除一个元素。
> * **isEmpty()**：如果链表中不包含任何元素，返回 true，如果链表长度大于0，则返回 false。
> * **size()**：返回链表包含的元素个数，与数组 length 属性类似。
> * **toString()**：返回表示整个链表的字符串。由于列表项使用了 Node 类，就需要重写继承自 JavaScript 对象默认的 toString 方法，让其只输出元素的值。

#### 1.向链表尾部添加元素
向**LinkedList**对象尾部添加一个元素时，可能有两种场景：链表为空，添加的是第一个元素；链表不为空，向其追加元素。下面是实现**push**方法。
```ts
// 比较函数
function defaultEquals(a, b) {
  return a === b;
}

// 节点类
export class Node<T> {
  constructor(element:T) {
    this.elment = element; // 元素
    this.next = undefined; // 指针
  }
}

// 链表类
export class LinkedList {
  constructor(equalsFn = defaultEquals) {
    this.count = 0; // 链表中的元素个数
    this.head = undefined; // 表头
  }

  push(element: T) {
    const node = new Node(element); // 创建节点
    let current; // 用来存储指向的节点
    if(this.head == null) {
      // 链表为空
      this.head = node;
    }else {
      // 链表不为空
      current = this.head;
      // 从链表的头结点开始，循环遍历直到获得链表的尾部节点
      while(current.next != null) {
        current = current.next;
      }
      // 将链表尾结点的next指向node节点，即为插入成功
      current.next = node;
    }
    this.count++; // 链表的节点数量增加
  }
}
```
演示结构如下图所示：
![img_02](https://blog.darwin.fun/images/serve?name=BLOG_27_IMG_02.png)

#### 2. 从链表中移除元素
让我们看看如何从**LinkedList**对象中移除元素。这里有两种**remove**方法：第一种从特定位置移除一个元素，第二种是根据元素的值移除元素。和**push**方法一样，对于从链表中移除元素，也有两种场景，第一种场景是移除第一个元素，第二种场景则是移除第一个元素之外的元素。代码如下：
```ts
// 比较函数
function defaultEquals(a, b) {
  return a === b;
}

// 节点类
export class Node<T> {
  constructor(element:T) {
    this.elment = element; // 元素
    this.next = undefined; // 指针
  }
}

// 链表类
export class LinkedList {
  constructor(equalsFn = defaultEquals) {
    this.count = 0; // 链表中的元素个数
    this.head = undefined; // 表头
  }

  // 根据索引值移除元素
  removeAt(index: number) {
    // 检查越界值
    if(index >= 0 && index < this.count) {
      let current = this.head; // 指向头结点
      // 移除元素是否为第一项
      if(index === 0) {
        this.head = current.next;
      }else {
        let previous; // 暂存变量
        for(let i = 0;i < index;i++) {
          previous = current;
          current = current.next;
        }
        // 将previous与current的下一项连接起来，跳过current，从而移除它
        previous.next = current.next;
      }
      this.count--; // 链表数量-1
      return current.element; // 输出移除元素的值
    }
    return undefined;
  }
}
```
演示结构如下图所示：
![img_03](https://blog.darwin.fun/images/serve?name=BLOG_27_IMG_03.png)

#### 3. 循环迭代链表直到目标位置
在**removeAt**方法中，我们需要迭代整个链表直到我们的目标索引**index**。循环目标**index**的代码在**LinkedList**类方法中非常常见，我们可以将这部分逻辑独立为单独的方法，这样就可以在不同的地方复用它。具体方法代码如下：
```ts
// 比较函数
function defaultEquals(a, b) {
  return a === b;
}

// 节点类
export class Node<T> {
  constructor(element:T) {
    this.elment = element; // 元素
    this.next = undefined; // 指针
  }
}

// 链表类
export class LinkedList {
  constructor(equalsFn = defaultEquals) {
    this.count = 0; // 链表中的元素个数
    this.head = undefined; // 表头
  }

  // 循环列表到达指定索引位置
  getElementAt(index: number):Node {
    // 检查越界值
    if(index >=0 && index < this.count) {
      let node = this.head; // 指向头结点
      for(let i = 0;i < index && node != null;i++) {
        node = node.next;
      }
      return node;
    }
    return undefined;
  }

  // 根据索引值移除元素
  removeAt(index: number) {
    // 检查越界值
    if(index >=0 && index < this.count) {
      let current = this.head;
      if(index === 0) {
        this.head = current.next;
      }else {
        const previous = this.getElement(index - 1);
        current = previous.next;
        previous.next = current.next;
      }
      this.count--; // 链表数量-1
      return current.element; // 输出移除元素的值
    }
    return undefined;
  }
}
```
#### 4. 在任意位置插入元素
接下来，我们要实现**insert**方法，使用该方法可以在任意位置插入一个元素。具体实现代码如下：
```ts
// 比较函数
function defaultEquals(a, b) {
  return a === b;
}

// 节点类
export class Node<T> {
  constructor(element:T) {
    this.elment = element; // 元素
    this.next = undefined; // 指针
  }
}

// 链表类
export class LinkedList {
  constructor(equalsFn = defaultEquals) {
    this.count = 0; // 链表中的元素个数
    this.head = undefined; // 表头
  }

  // 循环列表到达指定索引位置
  getElementAt(index: number):Node {
    // 检查越界值
    if(index >=0 && index < this.count) {
      let node = this.head; // 指向头结点
      for(let i = 0;i < index && node != null;i++) {
        node = node.next;
      }
      return node;
    }
    return undefined;
  }

  insert(element: T, index: number) {
    // 检查越界值
    if(index >= 0 && index <= this.count) {
      // 创建要插入的节点
      const node = new Node(element);
      if(index === 0) {
        // 第一个位置添加元素
        const current = this.head; // 指向头结点
        node.next = current; // 插入节点的下一节点为头结点
        this.head = node; // 头结点指向插入节点
      } else {
        // 获取目标索引前一节点
        const previous = this.getElementAt(index - 1);
        const current = previous.next; // 获取到目标索引值节点
        node.next = current; // 插入元素的下一节点指向目标索引节点
        previous.next = node; // 目标索引的上一节点的下一节点指向插入节点
      }
      this.count++; // 链表元素数量加1
      return true;
    }
    return false;
  }
}
```