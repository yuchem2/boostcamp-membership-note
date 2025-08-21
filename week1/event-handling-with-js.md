# Event Handling with JavaScript
## 이벤트란?

이벤트는 사용자 또는 브라우저가 발생시키는 동작이나 신호를 의미합니다. 예: 클릭, 입력, 마우스 이동, 키보드 입력 등
## 자주 사용되는 이벤트 유형
|유형|이벤트 이름 예시|
|---|---|
|마우스 이벤트|click, dblclick, mouseover, mouseout, mousedown, mouseup|
|키보드 이벤트|keydown, keyup, keypress|
|폼 이벤트|submit, change, input, focus, blur|
|문서/윈도우 이벤트|load, resize, scroll, DOMContentLoaded
## 커스텀 이벤트
- 기본 이벤트 외에 `CustomEvent` 객체를 이용해 개발자가 직접 정의한 이벤트를 생성할 수 있습니다.

```js
const myEvent = new CustomEvent("my-event", { detail: { foo: "bar" } });
document.dispatchEvent(myEvent);

document.addEventListener("my-event", (e) => {
  console.log(e.detail.foo); // "bar"
});
```

## 이벤트 등록 방법

### HTML 속성 방식
```html
<button onclick="alert('Clicked!')">Click Me</button>
```
- 장점: 간단, 바로 사용 가능
- 단점: JS와 HTML이 섞여 유지보수가 어렵고, 여러 다중 이벤트 등록 불가
### DOM 속성 방식
```js
const btn = document.getElementById("btn");
btn.onclick = () => {
  alert("Clicked!");
};
```
- 단점: 기존 핸들러 덮어쓰기 가능
### addEventListener 방식(권장)
```js
const btn = document.getElementById("btn");
btn.addEventListener("click", () => {
  console.log("Button clicked");
});
```
- 장점: 다중 이벤트 등록 가능
- 옵션
	- `capture`: true → 캡처링 단계에서 이벤트 처리
	- `once`: true → 한 번만 이벤트 처리 후 제거
	- `passive`: true → 스크롤 이벤트 성능 최적화
## 이벤트 객체
- 이벤트 핸들러 함수에 전달되는 객체
- 주요 속성/메서드
	- `event.type`: 이벤트 종류
	- `event.target`: 이벤트 발생 요소
	- `event.preventDefault()`: 기본 동작 취소
	- `event.stopPropagation()`: 이벤트 버블링/캡처링 중단
## 이벤트 버블링과 캡처링

### 이벤트 흐름
브라우저에서 이벤트가 발생하면 **세 단계**로 처리됩니다.
1. **캡처링 단계(Capturing phase)**
    - 최상위 요소(`window` 또는 `document`) → 이벤트가 발생한 대상 요소까지 내려오는 단계
    - `addEventListener`에서 `capture: true` 설정 시 캡처링 단계에서 이벤트가 먼저 처리됨
2. **타깃 단계(Target phase)**
    - 실제 이벤트가 발생한 요소에서 이벤트 처리
    - 이벤트 핸들러는 기본적으로 이 단계에서 실행
3. **버블링 단계(Bubbling phase)**
    - 이벤트가 발생한 요소 → 최상위 요소 방향으로 전파
    - 대부분의 이벤트는 버블링 가능(`focus` 등 일부 이벤트 제외)

```text
document
 └── <body>
      └── <div id="parent">
           └── <button id="child">Click</button>
```
1. Capturing: document → body → parent → child
2. Target: child
3. Bubbling: child → parent → body → document
### 이벤트 중단
- `event.stopPropagation()`: 이벤트 버블링 또는 캡처링 중 전파를 중단
- `event.stopImmediatePropagation()`: 같은 요소의 다른 핸들러까지 실행하지 않고 즉시 중단

```js
const btn = document.getElementById("child");

btn.addEventListener("click", (e) => {
  console.log("첫 번째 핸들러 실행");
  e.stopImmediatePropagation();
});

btn.addEventListener("click", () => {
  console.log("이건 실행되지 않음");
});
```
### 이벤트 위임
- 부모 요소에 이벤트를 등록하고 자식 요소 이벤트를 처리하는 패턴
- 장점
	- 성능 최적화 (많은 자식에 각각 리스너 등록 불필요)
	- 동적으로 생성되는 요소에도 이벤트 적용 가능
```js
document.getElementById("list").addEventListener("click", (event) => {
  if(event.target.tagName === "LI") {
    console.log("Clicked item:", event.target.textContent);
  }
});
```

## 출처
- [MDN Web Docs - DOM Events](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events)
- [MDN Web Docs - Events](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events#Event_handler_properties)