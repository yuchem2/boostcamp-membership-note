# DOM APIs
## DOM (Document Object Model)

브라우저가 HTML, XML 문서를 파싱해서 만든 트리 구조 객체 모델로, HTML 문서를 JS에서 조작할 수 있는 객체 형태로 표현한 것을 말합니다.

```html
<body>
  <h1>Hello</h1>
  <p>World</p>
</body>
```

```text
Document
 └── <html>
      └── <body>
           ├── <h1> → "Hello"
           └── <p> → "World"
```

## 노드 타입과 DOM 계층 구조
DOM에서는 모든 요소, 텍스트, 문서 등이 노드(Node)라는 단위로 표현됩니다. 각 노드는 특정 타입을 가지고 있으며, 계층 구조로 연결되어 있습니다.
### 주요 노드 타입
|nodeType|의미|예시|
|---|---|---|
|1|Element 노드|`<div>`, `<p>`|
|2|Attribute 노드|`class="test"`|
|3|Text 노드|`"Hello World"`|
|8|Comment 노드|`<!-- comment -->`|
|9|Document 노드|전체 HTML 문서|
|11|DocumentFragment|메모리 상의 임시 DOM 조각|
```js
const p = document.querySelector("p");
console.log(p.nodeType); // 1 → Element
console.log(p.firstChild.nodeType); // 3 → Text
```
### DOM 계층 구조
DOM은 모두 트리 구조를 가지는데, 모든 노드는 부모(`parentNode`), 자식(`childNodes`), 형제(`nextSibling`, `previousSibling`)을 가질 수 있습니다.

```text
Document (nodeType 9)
 └── <html> (nodeType 1)
      ├── <head> (1)
      └── <body> (1)
           ├── <h1> (1)
           │    └── "Hello" (3)
           └── <p> (1)
                └── "World" (3)
```
### 주요 속성
- `nodeName`: 노드 이름
- `nodeValue`: Text/Comment 노드의 값
- `childNodes`: 자식 노드 모두 접근 (Text 포함)
- `children`: 자식 요소만 접근 (Element만)
## DOM API

DOM API는 브라우저가 제공하는 인터페이스 모음으로, 이를 이용해 JS가 HTML 요소를 조회, 생성, 수정, 삭제, 이벤트 처리 등을 수행할 수 있습니다.

### 노드 탐색 & 선택
- `document.getElementById("id")`
- `document.getElementsByClassName("class")`
- `document.getElementsByTagName("p")`
- `document.querySelector(".class")` (단일)
- `document.querySelectorAll(".class")` (여러 개)
- `element.parentNode`: 부모 노드 접근
- `element.children`: 자식 요소들 접근 (HTMLCollection)
- `element.firstChild` / `element.lastChild`: 첫/마지막 자식 노드
- `element.nextSibling` / `element.previousSibling`: 형제 노드
- `element.closest("selector")`: 가장 가까운 조상 요소 찾기
- `element.querySelector` / `element.querySelectorAll`: 하위 요소 탐색
### 요소 조작
- `element.textContent = "새 텍스트"`: 텍스트 변경
- `element.innerHTML = "<b>굵게</b>"`: HTML 삽입
- `element.setAttribute("src", "image.png")`: 속성 변경
- `element.hasAttribute("attr")`: 특정 속성 존재 여부 확인
- `element.getAttribute("attr")`: 속성 값 읽기
- `element.removeAttribute("attr")` : 속성 제거
- `element.style.color = "red"`: CSS 스타일 변경
- `window.getComputedStyle(element)`: 실제 적용된 스타일 읽기
- `element.classList.add("active")`, `element.classList.remove("active")`: 클래스 제어
- `element.classList.toggle("class")`: 클래스 토글
### 요소 생성/삽입/삭제
- `document.createElement("div")`: 새 요소 만들기
- `parent.appendChild(child)`: 자식으로 추가
- `parent.insertBefore(newNode, referenceNode)` : 특정 위치에 삽입
- `element.remove()`: 요소 삭제
- `element.cloneNode(true|false)`: 노드 복제 (true: 자식 포함)
- `parent.replaceChild(newNode, oldNode)`: 기존 노드 교체
- `element.insertAdjacentHTML(position, htmlString)`: HTML 삽입
    - `beforebegin`, `afterbegin`, `beforeend`, `afterend` 위치 지정 가능
### 이벤트 관련
- `element.addEventListener("event", handler, { capture: true|false, once: true })`: 이벤트 등록
- `element.removeEventListener("click", handler)`: 이벤트 제거
- `element.onclick = handler`: 이벤트 핸들러 직접 연결
- `event.preventDefault()`: 기본 이벤트 동작 취소
- `event.stopPropagation()`: 이벤트 버블링/캡처링 중단
### 문서 정보
- `document.title`: 문서 제목
- `document.URL`: 현재 URL
- `document.cookie`: 쿠키 정보
- `document.body`:  `<body>` 요소
- `document.head`: `<head>` 요소
- `document.documentElement`: `<html>` 요소
- `document.forms`: 모든 폼 요소 접근
- `document.images`: 모든 이미지 요소 접근
- `document.links`: 모든 `<a>` 요소 접근
### 기타
- `element.scrollIntoView()`: 특정 요소 스크롤 이동
- `element.focus()`: 포커스 지정
- `element.setAttributeNS(namespace, attr, value)`: 네임스페이스가 있는 속성 지정
- `document.createTextNode("text")`: 텍스트 노드 생성
## 출처
- [MDN Web Docs - Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [MDN Web Docs - DOM API](https://developer.mozilla.org/en-US/docs/Web/API)