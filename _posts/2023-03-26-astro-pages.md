---
title: "페이지 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Pages"
toc_sticky: true
---

**페이지**는 아스트로 프로젝트의 `src/pages/` 하위 디렉터리에 있는 파일입니다. 이는 웹 사이트의 모든 페이지에 대한 라우팅, 데이터 로드 및 전체 페이지 레이아웃을 처리합니다.

## 지원되는 페이지 파일 (Supported page files)

아스트로는 `src/pages/` 디렉터리에서 다음 파일 형식을 지원합니다:

- [`.astro`](#아스트로-페이지-astro-pages)
- [`.md`](#markdownmdx-페이지pages)
- `.mdx` ([MDX Integration이 설치된](https://docs.astro.build/en/guides/integrations-guide/mdx/#installation) 상태)
- [`.html`](#html-페이지pages)
- `.js`/`.ts` ([엔드 포인트](https://docs.astro.build/en/core-concepts/endpoints/)로)

## 파일 기반 라우팅 (File-based routing)

아스트로는 **파일 기반 라우팅**이라는 라우팅 전략을 활용합니다. `src/pages/` 디렉터리에 있는 각 파일은 파일 경로에 따라 사이트의 엔드 포인트가 됩니다.

단일 파일은 여러 페이지를 [동적 라우팅](https://docs.astro.build/en/core-concepts/routing/#dynamic-routes)을 사용하여 생성할 수도 있습니다. 이렇게 하면 콘텐츠를 [콘텐츠 컬렉션](https://docs.astro.build/en/guides/content-collections/) 또는 [CMS](https://docs.astro.build/en/guides/cms/)와 같은 특수한 `/pages/`의 외부에 놓더라도 페이지를 생성할 수 있습니다.

📚 [아스트로의 라우팅](https://docs.astro.build/en/core-concepts/routing/)에 대해 자세히 알아보세요.

### 페이지 간 링크 (Link between pages)

사이트의 다른 페이지에 연결하기 위해 아스트로 페이지에서 표준 HTML [`<a>` 요소](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)를 작성하세요.

## 아스트로 페이지 (Astro Pages)

아스트로 페이지는 `.astro` 파일 확장자를 사용하며 [아스트로 컴포넌트](2023-03-11-astro-components.md)와 동일한 기능을 지원합니다.

```jsx
---
---
<html lang="en">
  <head>
    <title>My Homepage</title>
  </head>
  <body>
    <h1>Welcome to my website!</h1>
  </body>
</html>
```

모든 페이지에서 동일한 HTML 요소를 반복하지 않으려면, 자신의 [레이아웃 컴포넌트](https://docs.astro.build/en/core-concepts/layouts/)로 공통 `<head>` 및 `<body>` 요소를 이동할 수 있습니다. 원하는 만큼 레이아웃 컴포넌트를 많이 또는 적게 사용할 수 있습니다.

```jsx
---
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout>
  <p>My page content, wrapped in a layout!</p>
</MySiteLayout>
```

📚 아스트로의 [레이아웃 컴포넌트](https://docs.astro.build/en/core-concepts/layouts/)에 대해 자세히 알아보세요.

## Markdown/MDX 페이지(Pages)

아스트로는 `src/pages/`에 있는 모든 마크다운(`.md`) 파일을 최종 웹사이트의 페이지로 취급합니다. 만약 [MDX 통합이 설치되어](https://docs.astro.build/en/guides/integrations-guide/mdx/#installation) 있다면 같은 방식으로 MDX(`.mdx`) 파일도 처리됩니다. 이들은 일반적으로 블로그 게시물 및 문서같이 텍스트가 많은 페이지에 사용됩니다.

`src/content/`의 [마크다운이나 MDX 페이지 콘텐츠 모음](https://docs.astro.build/en/guides/content-collections/)은 [동적으로 페이지를 생성하는 데](https://docs.astro.build/en/core-concepts/routing/#dynamic-routes) 사용될 수 있습니다.

페이지 레이아웃은 특히 [마크다운 파일](#markdownmdx-페이지pages)에서 유용합니다. 마크다운 파일은 특수 `layout` 머리말(front matter) 속성을 사용하여 전체 `<html>...<html>` 페이지 문서에서 마크다운 콘텐츠를 감쌀 [레이아웃 컴포넌트](https://docs.astro.build/en/core-concepts/layouts/)를 지정할 수 있습니다.

```md
---
layout: "../layouts/MySiteLayout.astro"
title: "My Markdown page"
---

# Title

This is my page, written in **Markdown.**
```

📚 아스트로의 [마크다운](https://docs.astro.build/en/guides/markdown-content/)에 대해 자세히 알아보세요.

## HTML 페이지(Pages)

`.html` 파일 확장자를 가진 파일은 `src/pages/`에 놓고 사이트의 페이지로 직접적 사용할 수 있습니다. 일부 주요 아스트로 기능은 [HTML 컴포넌트](2023-03-11-astro-components.md#html-컴포넌트-html-components)에서 지원되지 않습니다.

## 사용자 지정 404 오류 페이지 (Custom 404 Error Page)

사용자 지정 404 오류 페이지의 경우 `src/pages`에서 `404.astro`나 `404.md` 파일을 생성할 수 있습니다.

이는 `404.html` 페이지로 빌드됩니다. 대부분 [배포 서비스](https://docs.astro.build/en/guides/deploy/)는 이를 찾아서 사용할 것입니다.

---

- [Astro DOCS - Astro Pages](https://docs.astro.build/en/core-concepts/astro-pages/)
