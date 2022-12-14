---
title: "변수 호이스팅이란?"
categories: JavaScript
---

```javascript
console.log(score); // undefined 출력

var score;
```

위 소스코드를 보면 console.log(score);가 실행되는 시점에는 아직 score 변수의 선언이 실행되지 않았으므로 참조 에러(ReferenceError)가 발생할 것처럼 보이지만 undefined가 출력된다.

그 이유는 변수 선언이 소스코드가 한 줄씩 순차적으로 실행되는 시점, 즉 런다임(runtime)이 아니라 그 이전 단계에서 먼저 실행되기 때문이다.

자바스크립트 엔진은 소스코드를 한 줄씩 순차적으로 실행하기에 앞서 먼저 소스코드의 평가 과정을 거치면서 소스코드를 실행하기 위한 준비를 한다. 이 때 소스코드 실행을 위한 준비 단계인 소스코드의 평가 과정에서 자바스크립트 엔진은 변수 선언을 포함한 모든 선언문(변수 선언문, 함수 선언문 등)을 소스코드에서 찾아내 먼저 실행한다. 그리고 소스코드의 평가 과정이 끝나면 비로소 변수 선언을 포함한 모든 선언문을 제외하고 소스코드를 한 줄씩 순차적으로 실행한다.

즉, 자바스크립트 엔진은 변수 선언이 소스코드의 어디에 있든 상관없이 다른 코드보다 먼저 실행한다.

아까의 소스코드 결과를 다시 생각해보면 변수 선언(선언 단계와 초기화 단계)이 소스코드가 순차적으로 실행되는 런타임 이전 단계에서 먼저 실행된다는 증거이다. 이처럼 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 **변수 호이스팅(variable hoisting)**이라 한다.

사실 변수 선언뿐 아니라 var, let, const, function, function\*, class 키워드를 사용해서 선언하는 모든 식별자(변수, 함수, 클래스 등)는 호이스팅 된다. 모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문이다.

출처 :

- [모던 자바스크립트 Deep Dive 42p](http://www.yes24.com/Product/Goods/92742567) - 4.4 변수 선언의 실행 시점과 변수 호이스팅
