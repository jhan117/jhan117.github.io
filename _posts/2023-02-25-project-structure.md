---
title: "프로젝트 구조 [Astro 문서 번역]"
categories:
  - "Astro"
toc: true
toc_label: "프로젝트 구조"
toc_sticky: true
last_modified_at: 2023-02-25
---

`create astro` CLI 마법사로 생성된 새로운 Astro 프로젝트는 이미 몇 가지 파일들과 폴더들이 포함되어 있습니다. 그 외로, 직접 작성하여 Astro 기존 파일 구조에 추가할 수 있습니다.

여기에서 어떻게 Astro 프로젝트가 구성되어 있는지, 그리고 새 프로젝트에서 찾을 수 있는 몇 가지 파일들을 설명합니다.

## 디렉터리들와 파일들

Astro는 프로젝트에서 독자적인 폴더 구성을 활용합니다. 모든 Astro 프로젝트의 루트는 다음의 디렉터리들과 파일들을 포함해야 합니다.

- `src/*` - 프로젝트 소스 코드. (components, pages, styles, etc.)
- `public/*` - 코드가 없고 처리되지 않은 asset들. (fonts, icons, etc.)
- `package.json` - 프로젝트 정보(manifest).
- `astro.config.mjs` - Astro 환경 파일. (권장)
- `tsconfig.json` - TypeScript 환경 파일. (권장)

## 프로젝트 트리 예시

보통 Astro 프로젝트 디렉터리는 다음과 같습니다.

```bash
├─ 📁 public/
│   ├─ 📄 robots.txt
│   ├─ 📄 favicon.svg
│   └─ 📄 social-image.png
├─ 📁 src/
│   ├─ 📁 componnents/
│   │   ├─ 📄 Header.astro
│   │   └─ 📄 Buttom.jsx
│   ├─ 📁 layouts/
│   │   └─ 📄 PostLayout.astro
│   ├─ 📁 pages/
│   │   ├─ 📁 posts/
│   │   │   ├─ 📄 post1.md
│   │   │   ├─ 📄 post2.md
│   │   │   └─ 📄 post3.md
│   │   └─ 📄 index.astro
│   └─ 📁 styles/
│       └─ 📄 global.css
├─ 📄 astro.config.mjs
├─ 📄 pakage.json
└─ 📄 tsconfig.json
```

## `src/`

src/ 폴더는 대부분의 프로젝트 소스 코드가 있는 곳입니다. 이는 다음을 포함합니다.

- [Pages](https://docs.astro.build/en/core-concepts/astro-pages/)
- [Layouts](https://docs.astro.build/en/core-concepts/layouts/)
- [Astro components](https://docs.astro.build/en/core-concepts/astro-components/)
- [UI framework components (React, etc.)](https://docs.astro.build/en/core-concepts/framework-components/)
- [Styles (CSS, Sass)](https://docs.astro.build/en/guides/styling/)
- [Markdown](https://docs.astro.build/en/guides/markdown-content/)

Astro는 `src/` 파일들을 가공하고 최적화하고 번들해 브라우저에 전달되는 최종 웹사이트를 생성합니다. 정적인 `public/` 디렉터리와 달리 `src/` 파일들은 Astro에 의해 빌드 되고 처리됩니다.

심지어 일부 파일들은(Astro components와 같은) 작성된 대로 브라우저에 보내지 않으며 대신에 정적 HTML로 렌더링 됩니다. 다른 파일들은(CSS처럼) 브라우저에 보내지지만 성능을 위해 다른 CSS 파일들과 함께 최적화되거나 번들 될 수도 있습니다.

## `src/components`

Compoennts는 HTML 페이지들에 대한 재사용 가능한 코드 단위입니다. 이들은 [Astro components](https://docs.astro.build/en/core-concepts/astro-components/)나 React, Vue 같은 [UI framework components](https://docs.astro.build/en/core-concepts/framework-components/) 일 수 있습니다. 이 폴더에서 모든 프로젝트 components를 서로 그룹화하고 구성하는 것이 일반적입니다.

이는 Astro 프로젝트에서 일반적인 관습이지만 필수는 아닙니다. 원하는 대로 components를 자유롭게 구성하세요!

## `src/layouts`

[Layouts](https://docs.astro.build/en/core-concepts/layouts/)은 일부 콘텐츠를 더 큰 페이지 레이아웃으로 감싸는 특수한 컴포넌트입니다. 이것들은 페이지의 레이아웃을 정의하기 위해 [Astro pages](https://docs.astro.build/en/core-concepts/astro-pages/) 와 [Markdown or MDX pages](https://docs.astro.build/en/guides/markdown-content/)에서 가장 자주 사용됩니다.

`src/components`와 마찬가지로, 이 디렉터리는 일반적인 관습이지만 필수는 아닙니다.

## `src/pages`

[Pages](https://docs.astro.build/en/core-concepts/astro-pages/)는 새로운 페이지를 만드는 데 사용되는 특수한 컴포넌트입니다. 페이지는 Astro component이거나 일부 콘텐츠 페이지를 보여주는 Markdown 파일일 수도 있습니다.

> ⚠️ **주의**  
> `src/pages`는 Astro 프로젝트에서 필수적인 하위 디렉터리입니다. 이 디렉터리가 없다면, 페이지나 경로를 가지지 못할 것입니다!

## `src/styles`

`src/styles` 에 CSS 또는 Sass 파일들을 저장하는 것은 일반적인 관습이지만 필수는 아닙니다. styles이 `src/` 디렉터리의 어딘가에 있고 올바르게 import 되어 있는 한, Astro는 이들을 처리하고 최적화할 것입니다.

## `public/`

`public/` 디렉터리는 Astro의 빌드 과정 중에 처리될 필요가 없는 파일들과 assets을 위한 것입니다. 이 파일들은 그대로 빌드 폴더로 복사될 것입니다.

이 동작은 이미지, fonts 같은 일반적인 assets 또는 `robots.txt` , `manifest.webmanifest` 같은 특수 파일에 대해 `public/` 을 이상적으로 만듭니다.

`public/` 디렉터리에 CSS와 JavaScript를 놓을 수 있지만 최종 빌드에서 이 파일들이 번들 되거나 최적화되지 않습니다.

> 🚀 **TIP**  
> 일반적으로, 직접 작성한 모든 CSS나 JavaScript는 `src/` 디렉터리에 있어야 합니다.

## `pakage.json`

이 파일은 JavaScript 패키지 매니저가 종속성을 관리하는 데 사용됩니다. 또한 Astro를 실행하는 데 일반적으로 사용되는 스크립트 (예: `npm start`, `npm run build`)를 정의합니다.

`package.json`에서 지정할 수 있는 [두 가지의 종속성](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file)이 있습니다. : `dependencies` , `devDependencies`. 대부분의 경우, 이 둘은 동일하게 작동합니다. : Astro는 빌드 시 모든 종속성이 필요하며, 패키지 매니저는 둘 다 설치합니다. 처음에는 모든 종속성을 `dependencies`에 두고, 특별한 경우가 아니면 `devDependencies`를 사용하지 않는 것이 좋습니다.

프로젝트에 대한 새로운 `package.json` 파일을 만드는 데 도움이 되는 정보는 [수동 설치](https://docs.astro.build/en/install/manual/) 지침을 확인하세요.

## `astro.config.mjs`

이 파일은 모든 시작 템플릿에서 생성되고 Astro 프로젝트에 대한 구성 옵션을 포함합니다. 사용할 통합, 빌드 옵션, 서버 옵션 등을 지정할 수 있습니다.

구성 설정에 대한 세부사항은 [Astro 구성 가이드](https://docs.astro.build/en/guides/configuring-astro/)에서 확인해 보세요.

## `tsconfig.json`

이 파일은 모든 시작 템플릿에서 생성되고 Astro 프로젝트에 대한 TypeScript 구성 옵션을 포함합니다. 일부 기능은(npm package 가져오기 같은) `tsconfig.json` 파일이 없으면 에디터에서 완전히 지원되지 않습니다.

구성 설정에 대한 세부사항은 [TypeScript 가이드](https://docs.astro.build/en/guides/typescript/)에서 확인해 보세요.

---

- [Astro DOCS - Project Structure](https://docs.astro.build/en/core-concepts/project-structure/)
