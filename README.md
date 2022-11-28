# `_posts` form

- file name : `YYYY-MM-DD-name-of-post.ext`
- front matter :
  ```YAML
  ---
  title: "실제 화면에 보이는 제목"
  categories:
    - 카테고리
  tags:
    - 태그 1
    - 태그 2
  toc: true (table of content, 목차)
  toc_label: "toc의 이름"
  toc_icon: "toc의 아이콘"
  toc_sticky: true (toc의 고정유무, 고정(true)시 스크롤에도 우측상단에 고정되게 보인다.)
  last_modified_at: 2016-03-09 (게시글 마지막 수정일)
  ---
  ```

## 명령어

- 수정할 때마다 새로고침 : `bundle exec jekyll serve --livereload`
