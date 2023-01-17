---
title: "JS: Data Structures1"
categories:
  - "CS"
toc: true
toc_label: "JS로 보는 Data Structures"
toc_sticky: true
last_modified_at: 2023-01-17
---

## Data Structures

### Arrays

- Insert / remove from end: O(1)
- Insert / remove from beginning: O(n)
- Access: O(1)
- Search: O(n)
- push / pop: O(1)
- shift / unshift / concat / slice / splice: O(n)
- forEach / map / filter / reduce: O(n)

```js
const arr = [1, 2, 3, "Kaye"];
arr.push(4);
arr.unshift(0);
arr.pop();
arr.shift();

// method: map, filter, reduce, concat, slice, splice
```

### Objects

- Insert: O(1)
- Remove: O(1)
- Access: O(1)
- Search: O(n)
- Object.keys(): O(n)
- Object.values(): O(n)
- Object.entries(): O(n)

```js
const obj = {
  name: "Kaye",
  age: 23,
  "key-three": true,
  sayMyName: () => {
    console.log(this.name);
  },
};
obj.hobby = "Game";
delete obj.hobby;

// method: Object.keys(), .values(), .entries()
```

### Sets

```js
const set = new Set([1, 2, 3]);
set.add(4);
set.has(4);
set.delete(3);
set.size;
set.clear();
```

### Maps

```js
const map = new Map([
  ["a", 1],
  ["b", 2],
]);
map.set("c", 3);
map.delete("c");
map.has("a");
map.size;
map.clear();
```

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
