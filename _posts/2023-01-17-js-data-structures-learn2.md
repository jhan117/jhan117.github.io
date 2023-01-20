---
title: "JS: Data Structures2"
categories:
  - "CS"
toc: true
toc_label: "JS로 보는 Data Structures"
toc_sticky: true
last_modified_at: 2023-01-20
---

## Data Structures

### Stacks

LIFO

- push(element)
- pop()
- peek()
- isEmpty()
- size()
- print()

```js
class Stack {
  constructor() {
    this.items = [];
  }

  push(element) {
    this.items.push(element);
  }

  pop() {
    return this.items.pop();
  }

  peek() {
    return this.items[this.items.length - 1];
  }

  isEmpty() {
    return this.items.length === 0;
  }

  size() {
    return this.items.length;
  }

  print() {
    console.log(this.items.toString());
  }
}

const stack = new Stack();
```

### Queues

FIFO

- enqueue(element)
- dequeue()
- peek()
- isEmpty()
- size()
- print()

```js
class Queue {
  constructor() {
    this.items = [];
  }

  enqueue(element) {
    this.items.push(element);
  }

  dequeue() {
    return this.items.shift();
  }

  isEmpty() {
    return this.items.length === 0;
  }

  peek() {
    if (!this.isEmpty()) {
      return this.items[0];
    }
    return null;
  }

  size() {
    return this.items.length;
  }

  print() {
    console.log(this.items.toString());
  }
}

const stack = new Stack();
```

#### 최적화된 Queue

```js
class Queue {
  constructor() {
    this.items = [];
    this.rear = 0;
    this.front = 0;
  }

  enqueue(element) {
    this.items[this.rear] = element;
    this.rear++;
  }

  dequeue() {
    const item = this.items[this.front];
    delete this.items[this.front];
    this.front++;
    return item;
  }

  isEmpty() {
    return this.items.length === 0;
  }

  peek() {
    return this.items[this.front];
  }

  size() {
    return this.rear - this.front;
  }

  print() {
    console.log(this.items);
  }
}

const stack = new Stack();
```

### Circular queues

- enqueue(element)
- dequeue()
- isFull()
- peek()
- isEmpty()
- size()
- print()

```js
class CircularQueue {
  constructor(capacity) {
    this.items = new Array(capacity);
    this.capacity = capacity;
    this.currentLength = 0;
    this.rear = -1;
    this.front = -1;
  }

  isFull() {
    return this.currentLength === this.capacity;
  }

  isEmpty() {
    return this.currentLength === 0;
  }

  enqueue(element) {
    if (!this.isFull()) {
      this.rear = this.rear + 1;
      this.items[this.rear] = element;
      this.currentLength += 1;
      if (this.front === -1) {
        this.front = this.rear;
      }
    }
  }

  dequeue() {
    if (this.isEmpty()) {
      return null;
    }
    const item = this.items[this.front];
    this.items[this.front] = null;
    this.front = (this.front + 1) % this.capacity;
    this.currentLength -= 1;
    if (this.isEmpty()) {
      this.front = -1;
      this.rear = -1;
    }
    return item;
  }

  peek() {
    if (!this.isEmpty()) {
      return this.items[this.front];
    }
    return null;
  }

  print() {
    if (this.isEmpty()) {
      console.log("Queue is empty");
    } else {
      let str = "";
      for (let i = this.front; i !== this.rear; i = (i + 1) % this.capacity) {
        str += this.items[i] + " ";
      }
      str += this.items[i];
      console.log(str);
    }
  }
}

const queue = new CircularQueue(5);
```

### Linked lists

#### class

```js
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.size = 0;
  }

  isEmpty() {
    return this.size === 0;
  }

  getSize() {
    return this.size;
  }
}

const list = new LinkedList();
```

#### prepend

```js
// O(1)
prepend(value) {
  const node = new Node(value);
  if (this.isEmpty()) {
    this.head = node;
  } else {
    node.next = this.head;
    this.head = node;
  }
  this.size++;
}
```

#### print

```js
print() {
  if (this.isEmpty()) {
    console.log("List is empty");
  } else {
    let curr = this.head;
    let listValues = "";
    while (curr) {
      listValues += `${curr.value}`;
      curr = curr.next;
    }
    console.log(listValues);
  }
}
```

#### append

```js
// O(n)
append(value) {
  const node = new Node(value);
  if (this.isEmpty()) {
    this.head = node;
  } else {
    let prev = this.head;
    while (prev.next) {
      prev = prev.next;
    }
    prev.next = node;
  }
  this.size++;
}
```

#### insert

```js
insert(value, index) {
  if (index < 0 || index > this.size) {
    return;
  }
  if (index === 0) {
    this.prepend(value);
  } else {
    const node = new Node(value);
    let prev = this.head;
    for (let i = 0; i < index - 1; i++) {
      prev = prev.next;
    }
    node.next = prev.next;
    prev.next = node;
    this.size++;
  }
}
```

#### remove

```js
removeFrom(index) {
  if (index < 0 || index > this.size) {
    return;
  }
  let removedNode;
  if (index === 0) {
    removedNode = this.head;
    this.head = this.head.next;
  } else {
    let prev = this.head;
    for (let i = 0; i < index - 1; i++) {
      prev = prev.next;
    }
    removedNode = prev.next;
    prev.next = removedNode.next;
  }
  this.size--;
  return removedNode.value;
}
```

#### remove value

```js
removeValue(value) {
  if (this.isEmpty()) {
    return null;
  }
  if (this.head.value === value) {
    this.head = this.head.next;
    this.size--;
    return value;
  } else {
    let prev = this.head;
    while (prev.next && prev.next.value !== value) {
      prev = prev.next;
    }
    if (prev.next) {
      removedNode = prev.next;
      prev.next = removedNode.next;
      this.size--;
      return value;
    }
    return null;
  }
}
```

#### search

```js
search(value) {
  if (this.isEmpty()) {
    return -1;
  }
  let i = 0;
  let curr = this.head;
  while (curr) {
    if (curr.value === value) {
      return i;
    }
    curr = curr.next;
    i++;
  }
  return -1;
}
```

#### reverse

```js
reverse() {
  let prev = null;
  let curr = this.head;
  while (curr) {
    let next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }
  this.head = prev;
}
```

#### with Tail

```js
class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  prepend(value) {
    const node = new Node();
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      node.next = this.head;
      this.head = node;
    }
    this.size++;
  }
  append(value) {
    const node = new Node(value);
    if (this.isEmpty()) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      this.tail = node;
    }
    this.size++;
  }
  removeFromFront() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.head.value;
    this.head = this.head.next;
    this.size--;
    return value;
  }
  removeFromEnd() {
    if (this.isEmpty()) {
      return null;
    }
    const value = this.tail.value;
    if (this.size === 1) {
      this.head = null;
      this.tail = null;
    } else {
      let prev = this.head;
      while (prev.next !== this.tail) {
        prev = prev.next;
      }
      prev.next = null;
      this.tail = prev;
    }
    this.size--;
    return value;
  }
}

const list = new LinkedList();
```

#### Linked List Stack

```js
class LinkedListStack {
  constructor() {
    this.list = new LinkedList();
  }

  push(value) {
    this.list.prepend(value);
  }
  pop() {
    return this.list.removeFromFront();
  }
  peek() {
    return this.list.head.value;
  }
  isEmpty() {
    return this.list.isEmpty();
  }
  getSize() {
    return this.list.getSize();
  }
  print() {
    return this.list.print();
  }
}

const stack = new LinkedListStack();
```

#### Linked List Queue

```js
class LinkedListQueue {
  constructor() {
    this.list = new LinkedList();
  }

  enqueue(value) {
    this.list.append(value);
  }
  dequeue() {
    return this.list.removeFromFront();
  }
  peek() {
    return this.list.head.value;
  }
  isEmpty() {
    return this.list.isEmpty();
  }
  getSize() {
    return this.list.getSize();
  }
  print() {
    return this.list.print();
  }
}

const queue = new LinkedListQueue();
```

### Hash tables

```js
class HashTable {
  constructor(size) {
    this.table = new Array(size);
    this.size = size;
  }

  hash(key) {
    let total = 0;
    for (let i = 0; i < key.length; i++) {
      total += key.charCodeAt(i);
    }
    return total % this.size;
  }

  set(key, value) {
    const index = this.hash(key);
    this.table[index] = value;
  }

  get(key) {
    const index = this.hash(key);
    return this.table[index];
  }

  remove(key) {
    const index = this.hash(key);
    this.table[index] = undefined;
  }

  display() {
    for (let i = 0; i < this.table.length; i++) {
      if (this.table[i]) {
        console.log(i, this.table[i]);
      }
    }
  }
}

const table = new HashTable(50);
```

위처럼 작성하면 충돌이 발생한다. 아래의 코드처럼 수정해주면 잘 작동한다.

```js
set(key, value) {
  const index = this.hash(key);
  const bucket = this.table[index];
  if (!bucket) {
    this.table[index] = [[key, value]];
  } else {
    const sameKeyItem = bucket.find((item) => item[0] === key);
    if (sameKeyItem) {
      sameKeyItem[1] = value;
    } else {
      bucket.push([key, value]);
    }
  }
}

get(key) {
  const index = this.hash(key);
  const bucket = this.table[index];
  if (bucket) {
    const sameKeyItem = bucket.find((item) => item[0] === key);
    if (sameKeyItem) {
      return sameKeyItem[1];
    }
  }
  return undefined;
}

remove(key) {
  const index = this.hash(key);
  const bucket = this.table[index];
  if (bucket) {
    const sameKeyItem = bucket.find((item) => item[0] === key);
    if (sameKeyItem) {
      bucket.splice(bucket.indexOf(sameKeyItem), 1);
    }
  }
}
```

### Trees

#### Binary Search Tree Class

```js
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  isEmpty() {
    return this.root === null;
  }
}

const bst = new BinarySearchTree();
```

#### Insert

```js
insert(value) {
  const newNode = new Node(value);
  if (this.isEmpty()) {
    this.root = newNode;
  } else {
    this.insertNode(this.root, newNode);
  }
}

insertNode(root, newNode) {
  if (newNode.value < root.value) {
    if (root.left === null) {
      root.left = newNode;
    } else {
      this.insertNode(root.left, newNode);
    }
  } else {
    if (root.right === null) {
      root.right = newNode;
    } else {
      this.insertNode(root.right, newNode);
    }
  }
}
```

#### Search

```js
search(root, value) {
  if (!root) {
    return false;
  }
  if (root.value === value) {
    return true;
  } else if (value < root.value) {
    return this.search(root.left, value);
  } else {
    return this.search(root.right, value);
  }
}
```

#### DFS

```js
// Root Node 부터
preOrder(root) {
  if (root) {
    console.log(root.value);
    this.preOrder(root.left);
    this.preOrder(root.right);
  }
}

// 자식 Node 부터
postOrder(root) {
  if (root) {
    this.postOrder(root.left);
    this.postOrder(root.right);
    console.log(root.value);
  }
}
```

#### BFS

```js
levelOrder() {
  const queue = [];
  queue.push(this.root);
  while (queue.length) {
    let curr = queue.shift();
    console.log(curr.value);
    if (curr.left) {
      queue.push(curr.left);
    }
    if (curr.right) {
      queue.push(curr.right);
    }
  }
}
```

#### Min Max

```js
min(root) {
  if (!root.left) {
    return root.value;
  } else {
    return this.min(root.left);
  }
}

max(root) {
  if (!root.right) {
    return root.value;
  } else {
    return this.max(root.right);
  }
}
```

#### Delete

```js
delete(value) {
  this.root = this.deleteNode(this.root, value);
}

deleteNode(root, value) {
  if (root === null) {
    return root;
  }
  if (value < root.value) {
    root.left = this.deleteNode(root.left, value);
  } else if (value > root.value) {
    root.right = this.deleteNode(root.right, value);
  } else {
    if (!root.left && !root.right) {
      return null;
    }
    if (!root.left) {
      return root.right;
    } else if (!root.right) {
      return root.left;
    }
    root.value = this.min(root.right);
    root.right = this.deleteNode(root.right, root.value);
  }
  return root;
}
```

### Graphs

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
