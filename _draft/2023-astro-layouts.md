---
title: "레이아웃 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "Layouts"
toc_sticky: true
---

레이아웃은 페이지 템플릿 처럼 재사용할 수 있는 UI 구조를 제공하고 있는 아스트로 컴포넌트입니다.

관습적으로

## Sample Layout

```jsx
---
import BaseHead from '../components/BaseHead.astro';
import Footer from '../components/Footer.astro';
const { title } = Astro.props
---
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <BaseHead title={title}/>
  </head>
  <body>
    <nav>
      <a href="#">Home</a>
      <a href="#">Posts</a>
      <a href="#">Contact</a>
    </nav>
    <h1>{title}</h1>
    <article>
      <slot /> <!-- your content is injected here -->
    </article>
    <Footer />
  </body>
</html>
```

```jsx
---
import MySiteLayout from '../layouts/MySiteLayout.astro';
---
<MySiteLayout title="Home Page">
  <p>My page content, wrapped in a layout!</p>
</MySiteLayout>
```

📚 [슬롯](2023-03-11-astro-components.md#slots)에 대해 자세히 알아보세요.

## Markdown/MDX Layouts

### Markdown Layout Props

### Importing Layouts Manually (MDX)

## Using one Layout for .md, .mdx, and .astro

## Nesting Layouts

---

- [Astro DOCS - Astro Layouts](https://docs.astro.build/en/core-concepts/layouts/)
