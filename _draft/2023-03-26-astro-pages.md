---
title: "페이지 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Pages"
toc_sticky: true
---

페이지는 아스트로 프로젝트의 `src/pages/` 하위 디렉터리에 있는 파일입니다. 이는 웹 사이트의 모든 페이지에 대한 라우팅, 데이터 로드 및 전체 페이지 레이아웃을 처리합니다.

## 지원되는 페이지 파일 (Supported page files)

아스트로는 `src/pages/` 디렉터리에서 다음 파일 형식을 지원합니다:

- [`.astro`](#astro-pages)
- [`.md`](#markdownmdx-pages)
- `.mdx` ([MDX Integration이 설치된](https://docs.astro.build/en/guides/integrations-guide/mdx/#installation) 상태)
- [`.html`](#html-pages)
- `.js`/`.ts` ([엔드 포인트](https://docs.astro.build/en/core-concepts/endpoints/)로)

## 파일 기반 라우팅 (File-based routing)

아스트로는 파일 기반 라우팅이라고 불리는 라우팅 전략을 레버리지한다. 각 파일은 `src/pages/` 디렉터리에이 있는 ~이 된다. 엔드포엔트 사이트에서 기반한 그 파일 경로에

하나의 파일은 다수의 페이지를 [동적 라우팅](https://docs.astro.build/en/core-concepts/routing/#dynamic-routes)을 사용하여 생성할 수도 있습니다. 이는 [콘텐츠 수집](https://docs.astro.build/en/guides/content-collections/) 또는 [CMS](https://docs.astro.build/en/guides/cms/)에서 처럼, 특별한 `/pages/`의 밖에 콘텐츠를 놓더라도 페이지를 생성하는 것을 허락합니다.

[아스트로에서 라우팅](https://docs.astro.build/en/core-concepts/routing/)에 대해 더 읽어보세요.

### Link between pages

## Astro Pages

## Markdown/MDX Pages

## HTML Pages

## Custom 404 Error Page

---

- [Astro DOCS - Astro Pages](https://docs.astro.build/en/core-concepts/astro-pages/)
