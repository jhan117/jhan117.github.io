---
title: "JS: Algorithms2"
categories:
  - "CS"
toc: true
toc_label: "JS로 보는 Algorithms"
toc_sticky: true
last_modified_at: 2023-01-16
---

## Sorting Algorithms

### Bubble Sort

| 원본 |   -6   |   20   |   8    |   -2   |   4   | Swap |
| :--: | :----: | :----: | :----: | :----: | :---: | :--: |
|      | **-6** | **20** |   8    |   -2   |   4   |  x   |
|      |   -6   | **20** | **8**  |   -2   |   4   |  o   |
|      |   -6   |   8    | **20** | **-2** |   4   |  o   |
|      |   -6   |   8    |   -2   | **20** | **4** |  o   |

위의 과정 반복

- Big-O: O(n^2)

```js
function bubbleSort(arr) {
  let swapped;
  do {
    swapped = false;
    for (let i = 0; i < arr.length - 1; i++) {
      if (arr[i] > arr[i + 1]) {
        let temp = arr[i];
        arr[i] = arr[i + 1];
        arr[i + 1] = temp;
        swapped = true;
      }
    }
  } while (swapped);
}

const arr = [8, 20, -2, 4, -6];
bubbleSort(arr);
console.log(arr); // [-6, -2, 4, 8, 20]
```

### Insertion Sort

| 원본 |   -6   |    20     |    8     |    -2     |    4     | Number To Insert | SE  |                            |
| :--: | :----: | :-------: | :------: | :-------: | :------: | :--------------: | :-: | :------------------------: |
|      | **-6** | <u>20</u> |    8     |    -2     |    4     |        20        | -6  |  Place to the right of -6  |
|      | **-6** |  **20**   | <u>8</u> |    -2     |    4     |        8         | 20  |   Shift 20 to the right    |
|      | **-6** |   **8**   |  **20**  |    -2     |    4     |        8         | -6  | Place 8 to the right of -6 |
|      | **-6** |   **8**   |  **20**  | <u>-2</u> |    4     |        -2        | 20  |           Shift            |
|      | **-6** |   **8**   |  **20**  |  **20**   |    4     |        -2        |  8  |           Shift            |
|      | **-6** |   **8**   |  **8**   |  **20**   |    4     |        -2        | -6  |           Place            |
|      | **-6** |  **-2**   |  **8**   |  **20**   | <u>4</u> |        4         | 20  |           Shift            |
|      | **-6** |  **-2**   |  **8**   |  **20**   |  **20**  |        4         |  8  |           Shift            |
|      | **-6** |  **-2**   |  **4**   |   **8**   |  **20**  |        4         | -2  |           Place            |

- Big-O: O(n^2)

```js
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let numberToInsert = arr[i];
    let j = i - 1;
    while (j >= 0 && arr[j] > numberToInsert) {
      arr[j + 1] = arr[j];
      j = j + 1;
    }
    arr[j + 1] = numberToInsert;
  }
}

const arr = [8, 20, -2, 4, -6];
insertionSort(arr);
console.log(arr); // [-6, -2, 4, 8, 20]
```

### Quick Sort

<img src="https://favtutor.com/resources/images/uploads/mceu_46432632011643441346270.png" alt="quick sort" width="1000"/>

- Worst case: O(n^2)
- Avg case: O(nlogn)

```js
function quickSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  let pivot = arr[arr.length - 1];
  let left = [];
  let right = [];
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }
  return [...quickSort(left), pivot, ...quickSort(right)];
}

const arr = [8, 20, -2, 4, -6];
console.log(quickSort(arr)); // [-6, -2, 4, 8, 20]
```

### Merge Sort

<img src="https://www.101computing.net/wp/wp-content/uploads/Merge-Sort-Algorithm.png" alt="merge sort" width="1000"/>

- Big-O: O(nlogn)

```js
function mergeSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  const mid = Math.floor(arr.length / 2);
  const leftArr = arr.slice(0, mid);
  const rightArr = arr.slice(mid);
  return merge(mergeSort(leftArr), mergeSort(rightArr));
}

function merge(leftArr, rightArr) {
  const sortedArr = [];
  while (leftArr.length && rightArr.length) {
    if (leftArr[0] <= rightArr[0]) {
      sortedArr.push(leftArr.shift());
    } else {
      sortedArr.push(rightArr.shift());
    }
  }
  return [...sortedArr, ...leftArr, ...rightArr];
}

const arr = [8, 20, -2, 4, -6];
console.log(mergeSort(arr)); // [-6, -2, 4, 8, 20]
```

## Algorithm design techniques

- Bruce force : Linear search
- Greedy : Dijkstra's algorithm, Prim's algorithm, Kruskal's algorithm
- Divide and Conquer : Binary search, Quick sort, Merge sort, Tower of Hanoi
- Dynamic Programming : Fibonacci numbers, climbing staircase
- Backtracking : N-Queens problem

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
