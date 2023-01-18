---
title: "JS: Data Structures2"
categories:
  - "CS"
toc: true
toc_label: "JS로 보는 Data Structures"
toc_sticky: true
last_modified_at: 2023-01-18
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

### Hash tables

### Trees

### Graphs

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
