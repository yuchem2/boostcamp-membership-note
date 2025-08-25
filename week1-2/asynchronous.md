# 비동기 프로그래밍

## 개요

비동기 프로그래밍은 작업이 완료될 때까지 기다리지 않고, 다른 작업을 동시에 진행할 수 있는 프로그래밍 방식을 의미합니다. 즉, 한 작업이 끝날 때까지 프로그램이 멈추지 않고 다른 일을 계속 수행할 수 있습니다.
### Synchronous 처리

동기 방식에서는 작업이 순차적으로 진행되며, 한 작업이 끝나야 다음 작업이 수행됩니다. 코드가 간단하고 이해하기 쉽지만, 시간이 오래 걸리는 작업에서 전체 프로그램이 멈추는 문제가 발생할 수 있습니다.

```js
console.log("작업 1 시작");
console.log("작업 2 시작"); // 작업 1이 끝난 뒤 실행
console.log("작업 3 시작");
```
### Asynchronous 처리

비동기 방식에서는 작업을 요청하고, 완료될 때까지 기다리지 않고 다음 코드를 실행합니다. 완료되면 콜백, Promise, async/await 등을 통해 결과를 처리합니다.

```js
console.log("작업 1 시작");
setTimeout(() => {
  console.log("작업 2 완료"); // 2초 후 실행
}, 2000);
console.log("작업 3 시작"); // 바로 실행
```
## Javascript 비동기 처리 방식

### `Callback`

콜백 함수는 비동기 처리의 가장 기본적인 형태입니다. 기본적으로 콜백 함수는 매개변수로 함수 객체를 전달하여, 호출 함수 내에서 매개변수 함수를 실행하는 것을 말합니다. 즉, 파라미터로 일반적인 변수나 값을 전달하는 것이 아니라 함수 자체를 전달하는 것을 말합니다.

비동기 방식은 요청과 응답의 순서를 보장하지 않습니다. 그러므로, 응답의 처리 결과에 의존하는 경우에는 콜백 함수를 이용하여 작업 순서를 간접적으로 끼워 맞출 수 있습니다. 콜백 함수 방식으로 비동기 연산을 수행한 뒤 그 결과를 처리하는 코드를 작성하는 예시는 다음과 같습니다.

```js
function getDB(callback) {
  setTimeout(() => {
      const value = 100;
      callback(value);
  }, 3000);
}

function main() {
  getDB(function(value) {
    let data = value * 2;
    console.log('data의 값 : ', data);
  });
}

main();
```

이러한 방법은 만약, 호출하는 콜백 함수들이 연속적으로 배치되면 깊이가 점점 증가하여 코드 가독성이 매우 나빠지는 문제가 있습니다.
### `Promise`

콜백 함수는 비동기를 순차적으로 처리하기 위한 일종의 편법으로, 정식으로 지원하는 비동기 전용 함수가 아닙니다. Javascript에서 제공하는 `Promise` 객체는 이러한 한계를 해결하기 위해 비동기 처리를 위한 전용 객체로서 탄생하였습니다. `Promise`는 비동기 작업의 성공 또는 실패와 그 결과를 나타내는 객체입니다.
#### 객체 생성

`new` 키워드와 `Promise` 생성자 함수를 활용하여 `Promise` 객체를 생성할 수 있습니다. 생성자 함수는 두 개의 매개변수를 갖습니다.
- `resolve`: 작업이 성공했을 때를 알려주는 객체
- `reject`: 작업이 실패했을 때를 알려주는 객체

```js
const myPromise = new Promise((resolve, reject) => {
  const data = fetch('서버로부터 요청할 URL');
  if(data) resolve(data);
  reject("Error");
});
```
#### 객체 처리

이렇게 만들어진 `Promise` 객체는 비동기 작업이 완료된 이후에 다음 작업을 연결시켜 진행할 수 있습니다. `then()`과 `catch()` 메서드 체이닝을 통해 성공과 실패에 대한 후속처리를 할 수 있습니다.
- `then`: `resolve(data)`가 호출된 경우 메서드에 전달된 콜백 함수를 수행합니다. 이때 `data`를 매개변수로 넘겨줍니다.
- `catch`: `reject('Error')`가 호출된 경우 메서드에 전달된 콜백 함수를 수행합니다. 이때 `'Error'`를 매개변수로 넘겨줍니다.
- `finally`: 항상 호출되는 코드입니다.
#### `Promise`의 상태

비동기 작업의 상태를 나타내는 것을 `Promise`의 상태라고 합니다. 3가지 상태를 가지고 있습니다.
- `Pending(대기)`: 처리가 완료되지 않은 상태(코드 실행중)
- `Fulfilled(이행)`: 성공적으로 처리가 된 상태
- `Rejected(거부)`: 처리가 실패한 상태
### `async` / `await`

서비스 규모가 커질수록, 코드가 복잡해질 수록 콜백과 `Promise`를 중첩 사용하다 보면은 가독성이 떨어지고, 유지보수가 어려워지는 문제가 있습니다. 이를 `Caallback Hell`, `Promise Hell`이라고 합니다. Javascript의 `async` 와 `await`은 이러한 문제들을 해결하기 위해 등장하였고, 문법에 있어도 훨씬 단순해져 가독성과 유지보수성을 향상 시켜줍니다.

`async`와 `await`은 ES2017에 도입된 문법으로, `Promise` 로직을 더 쉽고 간결하게 사용할 수 있게 해줍니다. 이 기능을 사용하더라도, 기존 `Promise`가 대체되는 것이 아니라 내부적으로는 여전히 `Promise`를 사용해 비동기를 처리하고, 단지 내부 코드 작성 부분을 편하게 보여주는 기능이라고 할 수 있습니다. 마치, `prototype`과 `class`문법의 차이와 같습니다.

비동기 구문을 호출하는 함수를 정의할 때 `async` 키워드를 추가하고, 그 함수를 호출할 때 `await` 키워드를 붙여주면 간단하게 함수를 비동기로 호출할 수 있습니다.

```js
// 프로미스 객체 반환 함수
function delay(ms) {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log(`${ms} 밀리초가 지났습니다.`);
      resolve();
    }, ms);
  });
}

// 기존 Promise.then() 형식
function main() {
  delay(1000)
    .then(() => {
      return delay(2000);
    })
    .then(() => {
      return Promise.resolve('끝');
    })
    .then(result => {
      console.log(result);
    });
}

// async/await 방식
async function main() {
  await delay(1000);
  await delay(2000);
  const result = await Promise.resolve('끝');
  console.log(result);
}
```

`async` 키워드를 붙인 함수에서 값을 리턴하면 항상 `promise` 객체로 감싸져 이행(fulfilled) 상태의 `promise` 객체로 반환됩니다. 물론 직접 프로미스 정적 메서드를 이용해 상태를 다르게 지정하여 반환이 가능합니다.

`await`은 `promise` 비동기 처리가 완료될 때까지 코드 실행을 일시 중지하고, 기다린다는 뜻으로 이해하면 됩니다. 즉, 비동기 작업을 동기 작업처럼 수행할 수 있습니다.
## Javascript's Event Loop, Call Stack & Callback Queue

Javascript 싱글 스레드(Single Thread)로 동작합니다. 즉, 한 번에 한 작업만 수행할 수 있습니다. 하지만 비동기 작업을 지원하기 위해 이벤트 루프(Event Loop)와 콜백 큐(Callback Queue)를 활용합니다.
### Event Loop

![500](https://blog.kakaocdn.net/dn/bEeJN4/btsabeBnUWX/exb9jS9LXWWW7oM1Yk832K/img.png)
[출처: Javascript - ProtoType과 class#Event Loop](https://wikidocs.net/251900)

이벤트 루프는 콜스택과 콜백 큐를 감시하며 콜스택이 비어있으면 콜백 큐에 있는 함수를 콜스택으로 옮겨 실행합니다. 이 덕분에 Javascript는 싱글 스레드지만 비동기 작업을 처리할 수 있습니다.

1. Call Stack: 자바스크립트 함수 호출이 쌓이는 스택입니다.
2. Web APIs: 타이머, 네트워크 요청 등을 처리하는 브라우저의 API입니다.
3. Callback Queue: 비동기 작업이 완료되면 해당 콜백 함수가 이 큐에 추가됩니다.
4. Event Loop: 호출 스택이 비어 있을 때 콜백 큐에서 콜백 함수를 꺼내 호출 스택에 추가하여 실행합니다.
### Call stack

현재 실행 중인 함수들이 쌓이는 스택 구조입니다. 함수가 호출되면 콜스택에 쌓이고, 실행이 끝나면 콜스택에서 제거됩니다.

```js
function a() { console.log("a 실행"); }
function b() { console.log("b 실행"); a(); }
b();
```

1. `b()` 호출 → 콜스택에 `b` 추가
2. `console.log("b 실행")` → 실행
3. `a()` 호출 → 콜스택에 `a` 추가
4. `console.log("a 실행")` → 실행
5. `a` 종료 → 콜스택에서 제거
6. `b` 종료 → 콜스택에서 제거
### Callback Queue

비동기 작업이 완료되면 콜백 함수는 콜백 큐로 전달됩니다. 콜스택이 비어야 콜백 큐에 있는 함수가 콜스택으로 이동하여 실행됩니다.

```js
console.log("작업 1 시작");

setTimeout(() => {
  console.log("작업 2 완료"); // 2초 후 콜백 큐에 들어감
}, 2000);

console.log("작업 3 시작");
```

1. `console.log("작업 1 시작")` → 실행
2. `setTimeout` 등록 → 콜백 큐에 일정 시간 후 함수 예약
3. `console.log("작업 3 시작")` → 실행
4. 2초 후 `console.log("작업 2 완료")`가 콜백 큐에 들어가고, 콜스택이 비면 실행

실제로는 매크로 태스크(Macro Task)와 마이크로 태스크(Micro Task) 두 가지 큐가 존재합니다.
- 매크로 태스크 큐: `setTimeout`, `setInterval`, `setImmediate`, UI 렌더링 등
- 마이크로 태스크 큐: `Promise.then`, `MutationObserver`, `queueMicrotask` 등

```js
console.log("시작");

setTimeout(() => {
  console.log("setTimeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("끝");
// 시작
// 끝
// Promise
// setTimeout
```

실제로 이벤트 루프는 매크로 태스크를 하나 처리할 때마다, 대기 중인 마이크로 태스크 큐를 모두 비우고 다음 매크로 태스크를 실행합니다.  이 때문에 `Promise`(Micro Task)가 `setTimeout`(Macro Task)보다 먼저 실행되는 결과가 나옵니다.
## 참고자료
- [MDN Web Docs - Async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
- https://www.youtube.com/watch?v=8aGhZQkoFbQ
- [Javascript - Prototype과 class](https://wikidocs.net/251900)
