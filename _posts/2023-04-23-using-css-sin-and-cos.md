---
title: "CSS의 sin()과 cos() 이용하기"
categories:
  - "CSS"
toc: true
toc_label: "sin(), cos()"
toc_sticky: true
use_math: true
---

[Creating a Clock with the New CSS sin() and cos() Trigonometry Functions - CSS-TRICKS](https://css-tricks.com/creating-a-clock-with-the-new-css-sin-and-cos-trigonometry-functions/)

위의 글을 보고 한 번도 써보지 않은 기능이라 사용해보고 싶었다. 윗글에서는 Firefox, Safari에서만 지원된다고 하는데 현재는 업데이트 되어서 많은 브라우저가 지원하고 있다. [sin()의 브라우저 호환성](https://developer.mozilla.org/en-US/docs/Web/CSS/sin#browser_compatibility)

## 들어가기 전에

[`sin()`](https://developer.mozilla.org/en-US/docs/Web/CSS/sin)과 [`cos()`](https://developer.mozilla.org/en-US/docs/Web/CSS/cos)은 CSS 함수이며 `sin()`은 `-1`과 `1` 사이의 값인 숫자의 사인을, `cos()`은 그 숫자의 코사인을 반환하는 삼각함수이다. 두 함수는 인수의 결과를 호도(radian)로 해석해 [`<number>`](https://developer.mozilla.org/en-US/docs/Web/CSS/number) 또는 [`<angle>`](https://developer.mozilla.org/en-US/docs/Web/CSS/angle)로 분해해야하는 단일 계산을 포함한다.

매개변수로 `<number>` 또는 `<angle>`로 분해되는 계산인 `angle`만 허용하며 항상 `-1`과 `1` 사이의 숫자 값을 반환한다. (`infinity`, `-infinity`, `NaN`은 `NaN`을 반환하며 `0⁻`은 `0⁻`을 반환함.)

`tan()`는 사용하지 않는 이유는 원의 가장자리에 문자를 놓기에 `sin()`과 `cos()`이 적절하기 때문이다. 두 함수는 반지름을 이용해 x, y 좌표를 구할 수 있기 때문이다.

## 적용해보기

1.  너비에서 반지름을 계산해 변수로 저장한다.
    ```css
    .circle {
      --_width: 300px;
      --_radius: calc(var(--_width) / 2);
    }
    ```
2.  넣을 위치에 각도를 설정해 변수로 저장하는데 1사분면의 x축에서 시작하므로 유의해야 한다. (예를 들어 시계의 경우 3시가 0도가 되며 6시가 90도가 됨.)
    ```css
    /* 3시에 놓일 text */
    .circle-text p:nth-child(1) {
      --_d: 0deg;
    }
    /* 4시에 놓일 text */
    .circle-text p:nth-child(2) {
      --_d: 30deg;
    }
    ```
3.  계산한 숫자 위치를 적용한다. $sinΘ = \frac{y}{r}$이기 때문에 $y = sinΘ \times r$를 이용해서 계산한다. 똑같은 방법으로 $cosΘ = \frac{x}{r}$, $x = cosΘ \times r$ 반지름을 또 더해주는 이유는 중앙에서 시작한 위치가 아니기 때문에 중앙까지 가기 위해 마지막에 반지름을 더해준다.

    ```css
    .circle-text p {
      --_x: calc(var(--_radius) + var(--_radius) * cos(var(--_d)));
      --_y: calc(var(--_radius) + var(--_radius) * sin(var(--_d)));

      position: absolute;
      left: var(--_x);
      top: var(--_y);
    }
    ```

4.  숫자의 왼쪽 상단 모서리를 기준으로 배치 되기 때문에 글자 위치를 조정해준다. 반지름을 축소하고 글자들 크기를 일정하게 설정한다.
    ```css
    .circle {
      --_sz: 20px;
      /* 반지름에 조정 위치 계산 추가 */
      --_radius: calc((var(--_width) - var(--_sz)) / 2);
    }
    .circle-text p {
      width: var(--_sz);
      height: var(--_sz);
    }
    ```

실제로 대충 만들어본 코드는 하단에 첨부해봤다. 최상단의 글 링크에서 더 많은 코드를 확인해 볼 수 있다.

<p class="codepen" data-height="400" data-theme-id="dark" data-default-tab="html,result" data-slug-hash="NWOdzpo" data-user="2001Kaye" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/2001Kaye/pen/NWOdzpo">
  css sin and cos</a> by Kaye Kwon (<a href="https://codepen.io/2001Kaye">@2001Kaye</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## 마무리

보통 일렬로 배열하는 코드를 작성하는 경우가 많은데 이렇게 원 형식으로 놓는 방법도 알게 되어서 흥미로웠다.
