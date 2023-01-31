---
title: "Astro 튜토리얼"
categories:
  - "Astro"
toc: true
toc_label: "블로그 만들기"
toc_sticky: true
last_modified_at: 2023-01-25
---

> 출처 : https://docs.astro.build/en/tutorial/0-introduction/

## 당신만의 Astro Blog를 만들어 봅시다.

이 과정을 따라가면 할 수 있는 것들

- 개발 환경 설정
- 웹사이트에 페이지와 블로그 포스트 생성
- Astro 컴포넌트로 구축
- 로컬 파일 쿼리 및 작업
- 사이트에 상호작용 추가
- 웹에 사이트 deploy

시작하기 전에 알아야 할 것들

- HTML, Markdown, CSS, JavaScript
- 웹에 publishing하기 위한 GitHub 계정

### 개발 환경 준비하기

설치해야 할것들

- Node.js (v16.12.0 or higher)

### 첫 프로젝트 설치하기

```shell
npm create astro@latest
```

2. `create-astro`를 설치하기 위해 y 눌러 확인
3. "Where would you like to create your new project?"라고 묻는다면 비어있는 폴더 입력하거나 만들 폴더명 입력하기
4. 시작 템플릿을 물어본다면 "Empty Project" 선택하기
5. “Would you like to install dependencies?”라고 묻는 다면 y 누르기
6. “Would you like to initialize a new git repository?”라고 묻는 다면 y누르기
7. TypeScript 옵션에서는 "Relaxed" 선택하기

### VS Code에서 dev mode 시작하기

[Astro language support extension](https://marketplace.visualstudio.com/items?itemName=astro-build.astro-vscode) 추가하는 걸 추천한다.

생성한 폴더로 가서 아래 명령어를 실행한다.

```shell
npm run dev
```

`http://localhost:3000`에서 확인 할 수 있다.
