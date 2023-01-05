---
title: "JS: Algorithms"
categories:
  - "CS"
toc: true
toc_label: "JS로 보는 Algorithms"
toc_sticky: true
last_modified_at: 2023-01-05
---

## Big-O

Worst case complexity

### Objects

```js
const person = {
  firstName: "Kaye",
  lastName: "Kwon",
};
```

Insert - O(1)
Remove - O(1)
Access - O(1)
Search - O(n)
Object.keys() - O(n)
Object.values() - O(n)
Object.entries() - O(n)

### Array

```js
const odd = [1, 3, 4, 5, 7, 9];
```

Insert / remove at end - O(1)
Insert / remove at beginning - O(n)
Access - O(1)
Search - O(n)
Push / pop - O(1)
Shift / unshift / concat / slice / splice - O(n)
ForEach / map / filter / reduce - O(n)

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
