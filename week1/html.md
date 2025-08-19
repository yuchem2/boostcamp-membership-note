# HTML(Hyper Markup Language)
## 개요

HTML은 프로그래밍 언어는 아니고, 웹페이지가 어떻게 구조화되어 있는지 브라우저로 하여금 알 수 있도록 하는 마크업 언어.

HTML은 element로 구성되어 있으며, 이들을 적절한 방법으로 나타내고 실행하기 위해 각 컨텐츠의 여러 부분들을 감싸고 마크업합니다.

태그는 웹 상의 다른 페이지로 이동하게 하는 하이퍼링크 내용들을 생성하거나, 단어를 강조하는 등의 역할을 수행합니다.

## HTML Element

![element-img](https://developer.mozilla.org/ko/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax/grumpy-cat-small.png)

- Element: 여는 태그, 닫는 태그, 내용을 통틀어 일컫는 용어
- 여는 태그: element의 이름과 `<`, `>`로 구성. element의 시작을 의미.
- 닫는 태그: `<`뒤에 `/`가 있는 것을 제외하면 여는 태그와 동일. element의 끝을 의미
- 내용: 태그 사이에 들어가는 텍스트나 다른 요소
- 속성: element에 실제로 나타나진 않지만, 추가 정보를 담기 위해 사용
	- 이름=값 구조 (`class="title"`)
	- Boolean attribute는 값 없이도 존재 자체로 참(`disabled`, `checked` 등)
### 종류
- Block-level elements: 웹 페이지 상에 블록을 만드는 element.
	- 줄 바꿈 발생 
	- 문단, 목록, 네비게이션, footer 등
- Inline elements: 항상 Block-level element 내에 포함되는 element.
	- 문장 내에서 사용
	- `<a>`, `<em>`, `<strong>` 등
- Empty elements: 내용 없음, 단일 태그(self-closing tags)
	- `<img src="{link}" />`와 같은 형태로 표현
## HTML 문서 구조

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

-  `<!DOCTYPE html>`: 문서 형식 정의
- `<html>`: 최상위 루트 요소
- `<head>`: meta 정보, 검색 키워드, 스타일 등
- `<body>`: 사용자에게 보이는 콘텐츠

## DOM Tree

브라우저는 HTML 문서를 해석한 후 이를 트리 구조로 변환하여 메모리에 저장합니다. 이 구조를 DOM(Document Object Model) Tree라고 하며, 각 HTML 태그가 하나의 노드(Node)로 표현됩니다.

```css
Document
 └── html
      ├── head
      │     ├── meta
      │     └── title
      └── body
            └── p
```

- Document: 최상위 객체
- Element node: `html`, `head`, `body` 등 태그
- Text node: 태그 안에 들어가는 실제 텍스트

## HTML 태그

HTML 문서를 구성할 때 단순 `div`만 사용하지 않고, 의미에 맞는 태그를 사용하는 것이 중요합니다. 태그는 문서의 구조와  의미를 정의하며, 스타일과 스크립트를 적용할 때도 편리합니다.
### 의미 있는 태그
- `div` 태그: 블럭 단위 그룹화용
	- 스타일 적용이나 그룹화를 위해 사용 가능
	- 단, 문서의 구조적 의미를 나타내는 용도로는 적합하지 않음
- 의미있는 태그 사용: 문서 구조를 명확히 하고, SEO 접근성을 높임

### `<head>`

HTML의 `head`는 브라우저에는 표시되지 않지만, HTML에 필요한 메타데이터, CSS 설정 등의 내용을 포함하는 태그입니다. 

| 태그                          | 용도                                   |
| --------------------------- | ------------------------------------ |
| `<title>`                   | 문서 제목 정의, 브라우저 탭에 표시                 |
| `<meta>`                    | 문서 메타데이터 정의 (문자 인코딩, 설명, 키워드, 뷰포트 등) |
| `<link>`                    | 외부 CSS 파일 연결                         |
| `<style>`                   | 내부 CSS 작성                            |
| `<script>`                  | 자바스크립트 포함 또는 외부 JS 파일 연결             |
| `<base>`                    | 문서 내 상대경로 기준 URL 지정                  |
| `<meta name="description">` | 페이지 설명, SEO 최적화에 사용                  |
| `<meta charset="UTF-8">`    | 문자 인코딩 설정                            |
| `<meta name="viewport">`    | 모바일 반응형 웹을 위한 화면 크기 설정               |
### 레이아웃 관련 태그

| 태그          | 용도                        |
| ----------- | ------------------------- |
| `<header>`  | 문서나 섹션의 상단 영역, 로고·제목·메뉴 등 |
| `<section>` | 독립된 콘텐츠 영역 (주제별 구분)       |
| `<nav>`     | 내비게이션 메뉴 영역               |
| `<aside>`   | 본문과 관련되지만 부가적인 콘텐츠 (사이드바) |
| `<footer>`  | 문서나 섹션의 하단 영역, 저작권, 연락처 등 |
### 콘텐츠 관련 태그
| 태그                        | 용도                   |
| ------------------------- | -------------------- |
| `<h1>` ~ `<h6>`           | 제목 (대제목~소제목)         |
| `<p>`                     | 문단                   |
| `<img>`                   | 이미지 삽입               |
| `<a>`                     | 링크                   |
| `<ul>`, `<ol>`, `<li>`    | 목록 (순서 없음/순서 있음)     |
| `<table>`, `<tr>`, `<td>` | 표                    |
| `<strong>`, `<em>`        | 강조 (굵게/기울임)          |
| `<!---->`                 | 주석 태그, 브라우저에 표시되지 않음 |
- [html elements with practice](https://developer.mozilla.org/ko/docs/Web/HTML)
- [html element reference](https://www.w3schools.com/tags/default.asp)
### 태그 속성

| 속성       | 용도                                     |
| -------- | -------------------------------------- |
| `class`  | CSS 스타일링 및 JS 선택자 용도, 여러 요소에서 반복 사용 가능 |
| `id`     | 고유 식별자, 페이지 내 한 번만 사용 권장               |
| `data-*` | 커스텀 데이터 저장, JS에서 접근 가능                 |
| `style`  | 인라인 스타일 적용 (권장하지 않음, CSS 별도 관리 권장)     |
## 웹 접근성

> "웹은 사람들의 하드웨어, 소프트웨어, 언어, 문화, 장소, 혹은 신체적 및 물리적 능력과 관계없이 **모든 사람을 위해 작동하도록 설계되었습니다**. 웹의 이러한 목표가 달성될 때, 다양한 범위의 청각, 움직임, 시각, 그리고 인지 능력을 가진 사람들이 웹에 보다 쉽게 접근할 수 있습니다." ([W3C - 접근성](https://www.w3.org/standards/webdesign/accessibility))

가능한 많은 사람들이 웹사이트를 사용할 수 있도록 보장하는 것을 말합니다. 통상적으로 장애인만을 대상으로 생각하지만, 모바일 장치나 느린 네트워크 연결을 사용하는 사람들 등 다른 사용자들에게도 도움을 줍니다.

접근성을 갖춘 웹 사이트는 다음과 같은 장점이 있습니다.
- 접근성이 향상된 시멘틱 HTML은 SEO도 개선
- 접근성 고려는 윤리와 도덕 관념을 보여주어, 서비스의 대중적 이미지를 개선
- 서비스를 보다 많은 사용자가 사용할 수 있게 만듦

시멘틱 태그와 ARIA 속성을 통해 접근성을 향상할 수 있습니다.

- Semantic Tags: 스크린 리더가 페이지 구조를 이해하고 안내할 수 있습니다.
	- `<header>`, `<nav>`, `<main>`, `<footer>`, `<section>`, `<article>`, `<aside>`
- 이미지 접근성: 이미지에는 반드시 `alt` 속성을 제공하여, 시각 장애인이 내용을 이해할 수 있도록 구성
- 링크와 버튼: 링크나 버튼에 의미 있는 텍스트를 사용
- ARIA(Accessible Rich Internet Applications) 속성: 동적 UI 컴포넌트나 시멘틱 구조로는 표현하기 어려운 정보에 대해 보조 기술에 의미 전달
- 키보드 접근성: 모든 기능을 마우스 없이도 키보드로만도 작동이 가능하도록 구성
	- Tab 순서, focus 관리 등을 고려하여 개발
## 출처
- [MDM web Docs](https://developer.mozilla.org/en/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax)
- [Web Accessibility](https://developer.mozilla.org/ko/docs/Web/Accessibility)