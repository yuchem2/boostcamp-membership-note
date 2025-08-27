# Scope
## 개요
Scope는 식별자에 대한 접근 규칙을 말합니다. 즉, **특정 변수가 유효한 코드 범위**를 뜻합니다. 일반적인 프로그래밍 언어에서 크게 두 가지의 Scope가 존재합니다.

- Global Scope: 코드의 가장 바깥 영역에 존재하며 어디서든 접근 가능한 영역
- Local Scope: 일반적으로 함수, `{}`로 묶여있는 특정 영역. `{}`로 묶인 영역에서만 접근할 수 있는 영역

Scope Chain은 Scope가 계층적으로 연결된 것으로, 변수를 찾을 때 현재 Scope에서 시작하여 상위 Scope로 계속 올라가면서 변수를 찾아가는 행위를 말합니다.
## In Javascript

자바스크립트의 스코프는 다른 언어와 조금 다른 독특한 특징들을 가지고 있습니다. 특히 `var`와 `let`, `const` 키워드에 따라 Scope의 범위가 달라지기 때문에 주의해야 합니다.
### Function Scope
`var`로 선언된 변수는 함수 전체에서 유효합니다. `{}` 블록 (`if`, `for`, `while`)을 무시하고, 오직 함수 레벨에서만 Scope를 생성합니다.

```js
function myFunction() { 
	if (true) { 
		var varVariable = 'hello'; 
	}
	// if 블록 밖이지만, 함수 안에서는 접근 가능
	console.log(varVariable); // 'hello'
} 

myFunction(); // 함수 밖에서는 접근 불가능 
// console.log(varVariable); // ReferenceError: varVariable is not defined
```
### Block Scope
`let`, `const`는 `{}`로 묶여있는 블록 내에서만 유효한 Scope를 가집니다. 이 규칙은 일반적인 프로그래밍 언어에서 사용하는 일반적인 규칙과 동일합니다.

```js
function anotherFunction() { 
	if (true) { 
		let letVariable = 'world';
		const constVariable = 'hi';
		console.log(letVariable); // 'world'
		console.log(constVariable); // 'hi' 
	}
	// if 블록 밖에서는 접근 불가능
	// console.log(letVariable); // ReferenceError: letVariable is not defined
	// console.log(constVariable); // ReferenceError: constVariable is not defined
} 
anotherFunction();
```
### Lexical Scope
Javascript에서 scope chain은 코드가 작성된 위치에 따라 결정됩니다. 이것을 Lexical Scope 또는 Static Scope라고 합니다. 함수가 어디에서 호출되었는지와는 관계없이 함수가 어디에 선언되었는지에 따라 상위 스코프가 정해집니다.

```js
var outer = '외부'; // 전역 스코프
function outerFunction() {
	var inner = '내부'; 
	function innerFunction() { 
		console.log(outer); // '외부' (선언된 위치의 상위 스코프를 참조)
		console.log(inner); // '내부' (선언된 위치의 상위 스코프를 참조)
	} 
	// 함수를 호출하는 위치가 중요하지 않습니다.
	// innerFunction();
	return innerFunction; 
} 

var lexicalClosure = outerFunction();
lexicalClosure();
```

위 예시에서 `innerFunction`은 `outerFunction` 내부에 정의되었기 때문에 `outer`와 `inner` 변수에 접근할 수 있습니다. 함수가 어디에서 호출되든 이 관계는 변하지 않습니다.
### Closure
Closure는 Lexical Scope의 결과물로, **함수가 자신이 선언될 때 환경(상위 스코프의 변수 등)을 기억하는 능력**을 의미합니다. 이러한 특징 때문에 Closure는 Javascript에서 데이터를 비공개로 만들거나 상태를 유지하는데 매우 유용하게 사용됩니다.

```js
function makeCounter() { 
	var count = 0; // 이 변수는 makeCounter의 지역 변수
	function increase() { // 이 함수는 count 변수에 대한 클로저를 형성
		count++; console.log(count);
	} 
	return increase; // 함수 자체를 반환
}

var counter = makeCounter(); // makeCounter가 실행을 마쳤지만...
counter(); // 1 (count 변수를 기억하고 접근)
counter(); // 2 (변수의 상태가 보존됨)
counter(); // 3
```

위 예시에서 `makeCounter` 함수는 실행이 끝난 후 사라져야 하지만, `increase` 함수가 `count` 변수를 기억하고 있기 때문에 변수의 상태가 계속 유지됩니다.
## 출처
- [MDN Docs - Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope)
- [MDN Docs - Scope Rule](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block#block_scoping_rules_with_let_const_class_or_function_declaration_in_strict_mode)
- [MDN Docs - Closure](https://developer.mozilla.org/en-US/docs/Glossary/Closure)
