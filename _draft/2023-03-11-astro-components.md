---
title: "컴포넌트 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Components"
toc_sticky: true
---

Astro Components는 Astro 프로젝트의 기본 구성 요소입니다. client-side 런타임 없는 HTML 전용 템플릿 components입니다. 파일 확장자로 Astro Component를 찾을 수 있습니다: `.astro`

Astro Components는 매우 유연성이 있습니다. 종종, Astro component는 header나 프로필 card 같은 재사용 가능한 UI를 페이지에 포함합니다. 다른 경우, Astro component는 SEO를 쉽게 사용할 수 있는 일반적인 `<meta>` 태그 모음과 같이 HTML의 작은 조각을 포함할 수 있습니다. Astro components는 전체 페이지 레이아웃을 포함할 수도 있습니다.

Astro components에 대해 알아야 할 가장 중요한 것은 client에서 렌더링 하지 않는다는 점입니다. [server 측 렌더링 (SSR)](https://docs.astro.build/en/guides/server-side-rendering/)을 이용하여 빌드 시간이나 요청 시 둘 다 HTML을 렌더링 합니다. component frontmatter에 JavaScript 코드를 포함할 수 있으며 사용자의 브라우저로 전송된 최종 페이지에서 모두 제거됩니다. 그 결과 기본적으로 자바스크립트 footprint가 추가되지 않은 더 빠른 사이트가 됩니다.

## Component 구조

Astro component는 두 주요 부분으로 구성됩니다: Component Script, Component Template. 각 부분은 서로 다른 일을 수행하지만 함께 사용하기 쉽고 빌드 하기 원하는 게 무엇이든지 충분히 표현하는 프레임워크를 제공하는 것을 목표로 합니다.

```html
---
// Component Script (JavaScript)
---

<!-- Component Template (HTML + JS Expressions) -->
```

### Component Script

Astro는 코드 펜스(code fence)(`---`)를 사용해 Astro component의 component script를 식별합니다. 이전에 Markdown을 써본 적이 있다면, 이미 *frontmatter*라고 불리는 비슷한 개념에 익숙할 것입니다. component script에 대한 Astro의 아이디어는 이 개념에서 직접적으로 영감을 받았습니다.

component script를 사용하여 template 렌더링 하는 데 필요한 JavaScript 코드를 작성할 수 있습니다. 이는 다음을 포함합니다:

- 다른 Astro components 가져오기
- React 같은 다른 프레임워크 component 가져오기
- JSON 파일 같은 데이터 가져오기
- API 또는 데이터베이스에서 content fetch 하기
- template에서 참조할 변수 만들기

```jsx
---
import SomeAstroComponent from '../components/SomeAstroComponent.astro';
import SomeReactComponent from '../components/SomeReactComponent.jsx';
import someData from '../data/pokemon.json';

// `<X title="Hello, World" />`와 같은 전달된 component props에 접근하기
const {title} = Astro.props;
// 개인 API이나 데이터베이스에서도 외부 데이터를 fetch하기
const data = await fetch('SOME_SECRET_API_URL/users').then(r => r.json());
---
<!-- 여기에 template -->
```

코드 펜스는 당신이 작성한 JavaScript가 "펜스 된" 것을 보장하도록 설계되었습니다. frontend 애플리케이션으로 빠져나가거나 사용자의 손에 들어가지 않을 것입니다. 사용자의 브라우저에서 끝날까 봐 걱정하지 않고 비싸거나 민감한(개인적인 데이터베이스에서 요청한 것 같이) 코드를 안전하게 작성할 수 있습니다.

> **TIP**  
> component script에 TypeScript을 작성할 수도 있습니다.

### Component Template

component template은 코드 펜스 아래에 있으며 component의 HTML 출력을 결정합니다.

여기에 일반 HTML을 작성하면 import 되고 사용되는 Astro 페이지에서 HTML을 component가 렌더링 합니다.

그러나 [Astro의 component template 문법](https://docs.astro.build/en/core-concepts/astro-syntax/)은 JavaScript 표현식, Astro [`<style>`](https://docs.astro.build/en/guides/styling/#styling-in-astro)과 [`<script>`](https://docs.astro.build/en/guides/client-side-scripts/#using-script-in-astro) 태그, import된 component 및 [특별한 Astro 지시어](https://docs.astro.build/en/reference/directives-reference/)도 지원합니다. component script에서 (페이지 빌드 시간에) 정의된 데이터와 값은 동적으로 만들어진 HTML을 생성하기 위해 component template에서 사용될 수 있습니다.

```jsx
---
// component script은 여기에 있습니다!
import Banner from '../components/Banner.astro';
import ReactPokemonComponent from '../components/ReactPokemonComponent.jsx';
const myFavoritePokemon = [/* ... */];
const { title } = Astro.props;
---
<!-- HTML 코멘트 지원! -->
{/* JS 코멘트도 유효합니다! */}

<Banner />
<h1>Hello, world!</h1>

<!-- component script에서 온 props와 다른 변수들 사용: -->
<p>{title}</p>

<!-- hydrate에 대한 `client:` 지시어와 함께 다른 components 포함: -->
<ReactPokemonComponent client:visible />

<!-- JSX와 유사한, HTMl와 JavaScript 표현식 혼합: -->
<ul>
  {myFavoritePokemon.map((data) => <li>{data.name}</li>)}
</ul>

<!-- template 지시어를 사용하여 여러 문자열 또는 objects에서 클래스 이름 작성! -->
<p class:list={["add", "dynamic", {classNames: true}]} />
```

## Component 기반 설계

Components는 **재사용 가능**하고 **구성 가능**하도록 설계되었습니다. 다른 components 안에 components를 사용하여 점점 더 고급 UI를 빌드 할 수 있습니다. 예를 들어, `Button` component는 아래와 같이 `ButtonGroup` component를 생성하는 데 사용될 수 있습니다.

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

## Component Props

Astro component는 props를 정의하고 받아들일 수 있습니다. 그런 다음 이러한 props는 HTML 렌더링을 위해 component template에서 사용할 수 있게 됩니다. Props는 frontmatter 스크립트의 `Astro.props`로 전역에서 사용할 수 있습니다.

여기에 `greeting` prop과 `name` prop을 받는 component의 예시가 있습니다. 받을 props는 전역 `Astro.props` object에서 분해된다는 점에 유의하세요.

```jsx
---
// Usage: <GreetingHeadline greeting="Howdy" name="Partner" />
const { greeting, name } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

이 component가 다른 Astro components, 레이아웃 또는 페이지에서 import 되고 렌더링 할 때 이 props를 속성으로 전달할 수 있습니다:

```jsx
---
import GreetingHeadline from './GreetingHeadline.astro';
const name = "Astro"
---
<h1>Greeting Card</h1>
<GreetingHeadline greeting="Hi" name={name} />
<p>I hope you have a wonderful day!</p>
```

`Props` 유형 인터페이스를 사용해 TypeScript로 props를 정의할 수도 있습니다. Astro는 자동으로 frontmatter에서 `Props` 인터페이스를 선택하고 유형 경고/에러를 제공합니다. 이러한 props는 `Astro.props`에서 분해될 때 기본 값을 부여할 수도 있습니다.

```jsx
---
interface Props {
  name: string;
  greeting?: string;
}

const { greeting = "Hello", name } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

none이 제공될 때 사용하기 위한 기본 값을 component props에 지정할 수 있습니다.

```jsx
---
const { greeting = "Hello", name = "Astronaut" } = Astro.props;
---
<h2>{greeting}, {name}!</h2>
```

## Slots

`<slot />` 요소는 다른 파일의 자식 요소를 component template에 주입(또는 "slot") 할 수 있는 외부 HTML content의 placeholder입니다.

기본적으로, component에 전달된 모든 자식 요소들은 `<slot />`에서 렌더링 됩니다.

> **NOTE**  
> `Astro.props`를 사용해 component 전체에서 사용할 수 있는 Astro component에 전달된 속성인 props와 달리, slots은 자식 HTML 요소가 작성된 위치에서 렌더링 합니다.

```jsx
import Header from "./Header.astro";
import Logo from "./Logo.astro";
import Footer from "./Footer.astro";
```

```jsx
<div id="content-wrapper">
  <Header />
  <Logo />
  <h1>{title}</h1>
  <slot />  <!-- 자식이 여기에 갈 것 -->
  <Footer />
</div>
```

```jsx
---
import Wrapper from '../components/Wrapper.astro';
---
<Wrapper title="Fred's Page">
  <h2>All about Fred</h2>
  <p>Here is some stuff about Fred.</p>
</Wrapper>
```

이 패턴은 [Astro 레이아웃 component](https://docs.astro.build/en/core-concepts/layouts/)의 기본입니다: HTML content의 전체 페이지를 `<SomeLayoutComponent></SomeLayoutComponent>` 태그로 "wrapped" 하고 component에 보내 정의된 공통 페이지 요소 안에 렌더링 할 수 있습니다.

### 명명된 슬롯 (Named Slots)

아스트로 컴포넌트에는 명명된 슬롯(named slots)도 있을 수 있습니다. 이렇게 하면 해당 슬롯 이름과 일치하는 HTML 요소만 슬롯의 위치로 전달할 수 있습니다.

슬롯 이름은 `name` 속성을 사용해 지정됩니다:

```jsx
---
import Header from './Header.astro';
import Logo from './Logo.astro';
import Footer from './Footer.astro';

const { title } = Astro.props
---
<div id="content-wrapper">
  <Header />
  <slot name="after-header"/>  <!--  `slot="after-header"` 속성을 가진 자식들은 여기 -->
  <Logo />
  <h1>{title}</h1>
  <slot />  <!--  `slot` 없이 또는 `slot="default"` 속성을 가진 자식들은 여기 -->
  <Footer />
  <slot name="after-footer"/>  <!--  `slot="after-footer"` 속성을 가진 자식들은 여기 -->
</div>
```

특정 슬롯에 HTML 콘텐츠를 삽입하려면, 자식 요소에서 `slot` 속성을 사용해 슬롯의 이름을 지정하세요. 컴포넌트의 다른 모든 자식 요소들은 기본(명명되지 않은) `<slot />`에 삽입될 것입니다.

```jsx
---
import Wrapper from '../components/Wrapper.astro';
---
<Wrapper title="Fred's Page">
  <img src="https://my.photo/fred.jpg" slot="after-header">
  <h2>All about Fred</h2>
  <p>Here is some stuff about Fred.</p>
  <p slot="after-footer">Copyright 2022</p>
</Wrapper>
```

컴포넌트에서 일치하는 `<slot name="my-slot" />` 자리 표시자(placeholder)로 전달하길 원하는 자식 요소에서 `slot="my-slot"` 속성을 사용하세요.

> **TIP**
> 명명된 슬롯은 [UI 프레임워크 컴포넌트](https://docs.astro.build/en/core-concepts/framework-components/)에도 전달할 수도 있습니다!

### 슬롯의 대체 콘텐츠 (Fallback Content for Slots)

슬롯은 대체 콘텐츠를 렌더링 할 수도 있습니다. 슬롯에 전달된 일치하는 자식들이 없다면, `<slot />` 요소는 자체 자리 표시자(placeholder) 자식을 렌더링 합니다.

```jsx
---
import Header from './Header.astro';
import Logo from './Logo.astro';
import Footer from './Footer.astro';

const { title } = Astro.props
---
<div id="content-wrapper">
  <Header />
  <Logo />
  <h1>{title}</h1>
  <slot>
    <p>This is my fallback content, if there is no child passed into slot</p>
  </slot>
  <Footer />
</div>
```

## HTML Components

## Next Steps

---

- [Astro DOCS - Astro Components](https://docs.astro.build/en/core-concepts/astro-components/)
