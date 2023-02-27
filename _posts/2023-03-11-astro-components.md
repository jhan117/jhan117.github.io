---
title: "컴포넌트 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Components"
toc_sticky: true
---

Astro Components는 Astro 프로젝트의 기본 구성 요소입니다. client-side 런타임 없는 HTML 전용 템플릿 components입니다.

**만약 HTML을 안다면, 당신은 이미 첫 Astro component를 작성할 수 있을 만큼 충분히 알고 있습니다.**

Astro component 문법은 HTML 상위 집합(superset)입니다. 문법은 [HTML이나 JSX를 작성한 경험이 있는 사람이라면 친숙하게 느껴지도록 설계되었고](#astro와-jsx-차이점), components나 JavaScript 확장을 포함할 수 있도록 지원합니다. 파일 확장자로 Astro Component를 찾을 수 있습니다. : `.astro`

Astro Components는 매우 유연성이 있습니다. 종종, Astro component는 header나 프로필 card 같은 재사용 가능한 UI를 페이지에 포함합니다. 다른 경우, Astro component는 SEO를 쉽게 사용할 수 있는 일반적인 `<meta>` 태그 모음과 같이 HTML의 작은 조각을 포함할 수 있습니다. Astro components는 전체 페이지 레이아웃을 포함할 수도 있습니다.

Astro components에 대해 알아야 할 가장 중요한 것은 빌드 중에 HTML로 렌더링 하는 것입니다. component 내부에서 JavaScript 코드를 실행해도 사용자에게 보내는 최종 페이지에서는 제거된 상태로 모두 미리 실행됩니다. 그 결과 기본적으로 자바스크립트 footprint없이 추가되어 더 빠른 사이트가 됩니다.

## Component 구조

Astro component는 두 주요 부분으로 구성됩니다. : Component Script, Component Template. 각 부분은 서로 다른 일을 수행하지만 함께 사용하기 쉽고 빌드 하기 원하는 게 무엇이든지 충분히 표현하는 프레임워크를 제공하는 것을 목표로 합니다.

```astro
---
// Component Script (JavaScript)
---
<!-- Component Template (HTML + JS Expressions) -->
```

다른 components 안에 components를 사용하여 점점 더 고급 UI를 빌드 할 수 있습니다. 예를 들어, `Button` component는 아래와 같이 `ButtonGroup` component를 생성하는 데 사용될 수 있습니다.

```astro
---
import Button from './Button.astro';
---
<div>
  <Button title="Button 1" />
  <Button title="Button 2" />
  <Button title="Button 3" />
</div>
```

### Astro와 JSX 차이점

---

- [Astro DOCS - Astro Components](https://docs.astro.build/en/core-concepts/astro-components/)
