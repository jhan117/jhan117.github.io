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

```html
---
// Component Script (JavaScript)
---

<!-- Component Template (HTML + JS Expressions) -->
```

다른 components 안에 components를 사용하여 점점 더 고급 UI를 빌드 할 수 있습니다. 예를 들어, `Button` component는 아래와 같이 `ButtonGroup` component를 생성하는 데 사용될 수 있습니다.

```js
---
import Button from './Button.astro';
---

<div>
  <button title="Button 1" />
  <button title="Button 2" />
  <button title="Button 3" />
</div>
```

### Component Script

Astro는 Astro component에서 component script를 식별하기 위해 code fence (`---`)를 사용합니다. 만약 전에 Markdown으로 써본 적이 있다면 이미 *frontmatter*라고 불리는 비슷한 개념에 친숙할지도 모릅니다. 이 개념에서 component script에 대한 Astro의 생각이 직접적으로 영감 받았습니다.

템플릿을 렌더할 필요가 있는 어떤 JavaScript를 작성하기 위해 component script를 사용할 수 있습니다. 다음을 이를 포함합니다:

- 다른 Astro components 불러오기
- React 같은 다른 프레임워크 component 불러오기
- JSON 파일 같은 데이타 불러오기
- API 또는 데이터베이스로부터 content를 fetch하기
- 템플릿에서 참조할 변수들을 생성하기

```js
---
import SomeAstroComponent from '../components/SomeAstroComponent.astro';
import SomeReactComponent from '../components/SomeReactComponent.jsx';
import someData from '../data/pokemon.json';

// `<X title="Hello, World" />`처럼, component props를 넘겨서 접근하기
const {title} = Astro.props;
// 외부 데이터, private API나 데이터베이스로 부터 온 fetch
const data = await fetch('SOME_SECRET_API_URL/users').then(r => r.json());
---
<!-- Your template here! -->
```

code fence는 "울타맃를 칠" 곳에 작성한 JavaScript를 보장하기 위해 설계되었습니다. frontend 애플리케이션에서 탈출할 수 없거나 사용자 접근성에서 떨어질 수 없습니다. 안전하게 여기서 코드를 상서ㅓㅇ할 수 있습니다. 비싸거나 민감한(private 데이터베이스로 부르려고 하는 것 같은) 걱정 없이, 사용자 브라우저에 ending up.

> TIP
> component Script에 TypeScript도 사용할 수 있습니다.

### Component Template

아래의 componenet sciprt는 sits component template. component template은 component의 HTML 출력을 결정한다.

만약 플레인 HTML을 썼다면 component는 HTML로 렌더 될 것이다. 어떤 Astro 페이지에서 이는 import되고 사용된.

그러나, Astro의 component template 문법은 또한 지원한다. javaScript 표현식, import된 component 그리고 특별한 Astro 직접적. 데이터와 가치는 page 빌드 시간에 정의된다. component sciprt에서 정의된 것은 component template에서 사용될 수 있다. 동적 생성된 HTML을 생산하기 위해.

```jsx
---
// Your component script here!
import ReactPokemonComponent from '../components/ReactPokemonComponent.jsx';
const myFavoritePokemon = [/* ... */];
---
<!-- HTML comments supported! -->

<h1>Hello, world!</h1>

<!-- Use props and other variables from the component script: -->
<p>My favorite pokemon is: {Astro.props.title}</p>

<!-- Include other components with a `client:` directive to hydrate: -->
<ReactPokemonComponent client:visible />

<!-- Mix HTML with JavaScript expressions, similar to JSX: -->
<ul>
  {myFavoritePokemon.map((data) => <li>{data.name}</li>)}
</ul>

<!-- Use a template directive to build class names from multiple strings or even objects! -->
<p class:list={["add", "dynamic", {classNames: true}]} />
```

## JSX 같은 표현식

Astro component 안에 frontmatter component script의 안에서 지역 Javasciprt 변수를 정의할 수 있다. 당신은 이 변수들을 jsk 강ㅌ은 표현식을 사용해서 component html template에서 이 변수들을 검사할 수 있습니다.

> 동적 vs 반응적
> 목표를 위해 당신은 동적 값을 포함할 수 있다. 이 동저 값은 frontrmatter에서 계산된 것. 그러나 일단 포함되면 이 값들ㅇ은 반응적이지 않고 절대로 바뀌지 않는다. Astro component들은 템플릿이다. 한번 실행되는, 빌드 시간에.
>
> 여기서 더 많은 예시를 볼 수 있습니다. astro와 jsx 차이점

### 변수

### Astro와 JSX 차이점

---

- [Astro DOCS - Astro Components](https://docs.astro.build/en/core-concepts/astro-components/)
