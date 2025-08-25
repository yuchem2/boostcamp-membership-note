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
### 이벤트 공통 속성
| 속성                 | 설명                                                      |
| ------------------ | ------------------------------------------------------- |
| `type`             | **이벤트 종류** (`"click"`, `"keydown"` 등)                   |
| `target`           | **이벤트가 실제 발생한 요소**                                      |
| `currentTarget`    | 현재 실행 중인 핸들러가 바인딩된 요소                                   |
| `srcElement`       | (옛날 IE) → `target`과 동일                                  |
| `composed`         | 이벤트가 Shadow DOM 경계를 넘어 전파되는지 여부                         |
| `bubbles`          | 이벤트가 버블링되는지 여부 (`true`/`false`)                         |
| `cancelable`       | `preventDefault()`로 기본 동작을 막을 수 있는지 여부                  |
| `defaultPrevented` | `preventDefault()` 호출 여부                                |
| `eventPhase`       | 현재 이벤트 흐름 단계 (1: Capturing, 2: At target, 3: Bubbling)  |
| `isTrusted`        | 사용자가 실제 발생시킨 이벤트인지(`true`), JS로 생성된 이벤트인지(`false`)      |
| `timeStamp`        | 이벤트가 발생한 시점 (ms 단위, 문서 로드 기준)                           |
| `view`             | 이벤트와 연결된 윈도우 객체 (보통 `window`)                           |
| `returnValue`      | (구식) `false`로 설정하면 기본 동작 취소 → 지금은 `preventDefault()` 사용 |
| `cancelBubble`     | (구식) `true`로 설정하면 버블링 중단 → 지금은 `stopPropagation()` 사용   |
| `detail`           | 이벤트에 대한 추가 정보 (예: 클릭 횟수: `dblclick`이면 2)                |
### 마우스 이벤트 관련 (`MouseEvent`)
| 속성                                         | 설명                                             |
| ------------------------------------------ | ---------------------------------------------- |
| `altKey`, `ctrlKey`, `shiftKey`, `metaKey` | 해당 키가 눌려 있는지 여부 (`true`/`false`)               |
| `button`                                   | 어떤 마우스 버튼이 눌렸는지 (0: 왼쪽, 1: 가운데, 2: 오른쪽)        |
| `buttons`                                  | 동시에 눌린 버튼 비트필드 (예: 왼쪽+오른쪽 동시에 누름)              |
| `clientX`, `clientY`                       | 브라우저 **뷰포트 기준 좌표**                             |
| `pageX`, `pageY`                           | 문서 전체 기준 좌표 (스크롤 포함)                           |
| `screenX`, `screenY`                       | 모니터 화면 기준 좌표                                   |
| `offsetX`, `offsetY`                       | 이벤트 대상 요소(`target`)의 좌상단 기준 좌표                 |
| `layerX`, `layerY`                         | 레이어 기준 좌표 (표준 아님, 브라우저마다 다름)                   |
| `movementX`, `movementY`                   | 직전 이벤트와의 상대 좌표 변화량                             |
| `x`, `y`                                   | `clientX`, `clientY`와 동일 (별칭)                  |
| `fromElement`                              | (구식) 마우스가 나간 요소 (→ `relatedTarget`)            |
| `toElement`                                | (구식) 마우스가 들어간 요소 (→ `relatedTarget`)           |
| `relatedTarget`                            | `mouseover`/`mouseout` 시 상대 요소 (ex. 나간/들어온 요소) |
| `which`                                    | 눌린 버튼 (옛날 방식, 지금은 `button` 사용)                 |
### 포인터 이벤트 관련 (`PointerEvent`)
|속성|설명|
|---|---|
|`pointerId`|포인터 식별자 (터치 멀티포인트 구분용)|
|`pointerType`|입력 장치 타입 (`"mouse"`, `"pen"`, `"touch"`)|
|`isPrimary`|주요 포인터인지 여부 (멀티 터치 중 첫 번째 터치 등)|
|`width`, `height`|포인터 접촉 영역의 직사각형 크기|
|`pressure`|압력 정도 (0~1, 마우스는 0/0.5/1 정도만)|
|`tangentialPressure`|펜 옆면 압력 (일부 디바이스만 지원)|
|`tiltX`, `tiltY`|펜이 기울어진 각도 (x, y 축 기준)|
|`altitudeAngle`|펜이 표면과 이루는 각도 (0 = 평행, π/2 = 수직)|
|`azimuthAngle`|펜이 기울어진 방향(각도)|
|`twist`|펜 회전(트위스트) 각도|
|`persistentDeviceId`|장치를 식별하는 ID (실험적)|

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