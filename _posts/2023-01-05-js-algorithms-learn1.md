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

### Big-O guide

- Calculation not dependent on input size - O(1)
- 1 loop - O(n)
- 2 nested loops - O(n^2)
- Input size reduced by half - O(logn)

## Math Algorithms

### Fibonacci Sequence

- Big-O : O(n)

```js
function fibonacci(n) {
  const fib = [0, 1];
  // 1 loop
  for (let i = 2; i < n; i++) {
    fin[i] = fib[i - 1] + fib[i - 2];
  }
  return fib;
}

console.log(fibonacci(2)); // [0, 1]
console.log(fibonacci(3)); // [0, 1, 1]
console.log(fibonacci(7)); // [0, 1, 1, 2, 3, 5, 8]
```

### Factorial of a number

- Big-O: O(n)

```js
function factorial(n) {
  let result = 1;
  // 1 loop
  for (let i = 2; i <= n; i++) {
    result = result * i;
  }
  return result;
}

console.log(factorial(0)); // 1
console.log(factorial(1)); // 1
console.log(factorial(5)); // 128
```

### Prime Number

- Big-O: O(n)

```js
function isPrime(n) {
  if (n < 2) {
    return false;
  }
  // 1 loop
  for (let i = 2; i < n; i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return true;
}

console.log(isPrime(1)); // false
console.log(isPrime(5)); // true
console.log(isPrime(4)); // false
```

최적화하기

항상 n에 루트를 씌운 것보다 두 숫자 중 작은 숫자가 더 작다. 그렇기에 루트 씌운 n보다 큰지 확인할 필요가 없다. 이를 이용해서 루프 횟수를 줄일 수 있다.

- n=24, a=4, b=6 → sqrt(n)=4.89, a < sqrt(n)
- n=35, a=5, b=7 → sqrt(n)=5.91, a < sqrt(n)

- Big-O: O(sqrt(n))

```js
function isPrime(n) {
  if (n < 2) {
    return false;
  }
  // 1 loop
  for (let i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return true;
}

console.log(isPrime(1)); // false
console.log(isPrime(5)); // true
console.log(isPrime(4)); // false
```

### Power of Two

- Big-O: O(logn)

```js
function isPowerOfTwo(n) {
  if (n < 1) {
    return false;
  }
  // 1 loop
  while (n > 1) {
    if (n % 2 !== 0) {
      return false;
    }
    // reduced by half
    n = n / 2;
  }
  return true;
}

console.log(isPowerOfTwo(1)); // true
console.log(isPowerOfTwo(2)); // true
console.log(isPowerOfTwo(5)); // false
```

최적화하기

2의 거듭제곱은 항상 이전 숫자와 비트 연산자의 합 연산을 하면 0이 나온다는 것을 이용해서 시간 복잡도를 1로 낮출 수 있다.

- 1 → 1
- 2 → 10
- 4 → 100
- 8 → 1000

|          |          |          |          |          |
| -------- | -------- | -------- | -------- | -------- |
| 1 - 0001 | 2 - 0010 | 3 - 0011 | 5 - 0101 | 8 - 1000 |
| 0 - 0000 | 1 - 0001 | 2 - 0010 | 4 - 0100 | 7 - 0111 |
| 0 - 0000 | 0 - 0000 | 2 - 0010 | 4 - 0100 | 0 - 0000 |

- Big-O: O(1)

```js
function isPowerOfTwoBitWise(n) {
  if (n < 1) {
    return false;
  }
  return (n & (n - 1)) === 0;
}

console.log(isPowerOfTwoBitWise(1)); // true
console.log(isPowerOfTwoBitWise(2)); // true
console.log(isPowerOfTwoBitWise(5)); // false
```

---

강의명

- Codevolution([Youtube](https://youtube.com/playlist?list=PLC3y8-rFHvwjPxNAKvZpdnsr41E0fCMMP)): JavaScript Algorithms and Data Structures
