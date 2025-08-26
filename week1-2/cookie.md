# Cookie

## 개요

HTTP 쿠키(웹 쿠키, 브라우저 쿠키)는 서버가 사용자의 웹 브라우저에 전송하는 작은 데이터 조각으로, 브라우저는 쿠키를 저장해 놓았다가 동일한 서버에 재 요청 시 저장된 데이터를 함께 전송합니다. 쿠키는 일반적으로 다음과 같은 목적을 위해 사용됩니다.

- 세션 관리: 서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리
- 개인화: 사용자 선호, 테마 등의 세팅
- 트래킹: 사용자 행동을 기록하고 분석하는 용도

과거에는 클라이언트 측에 정보를 저장할 때 쿠키를 주로 사용하였으나 지금은 `modern storage APIs`를 사용해 정보를 저장하는 걸 권장하고 있습니다.

## 쿠키 생성하기

HTTP 요청을 수신할 때 서버는 응답과 함께 `Set-Cookie` 헤더를 전송할 수 있습니다. 쿠키는 보통 브라우저에 의해 저장되며, 그 후 쿠키는 같은 서버에 의해 만들어진 요청들의 `Cookie` HTTP 헤더 안에 포함되어 전송됩니다.

만료일, 지속시간도 명시될 수 있고, 만료된 쿠키는 더 이상 보내지지 않습니다. 추가적으로 특정 도메인 혹은 경로 제한을 설정할 수 있습니다.
### 쿠키의 라이프타임

- 세션 쿠기 (`Expires`, `Max-Age` 속성이 없는 쿠키): 현재 세션이 끝날 때 삭제됩니다. 브라우저는 현재 세션이 끝나는 시점을 정의하며, 어떤 브라우저들은 재시작할 때 세션을 복원해 세션 쿠키가 무기한 존재할 수 있게 합니다.
- 영속적인 쿠키는 `Expires` 속성에 명시된 날짜에 삭제되거나, `Max-Age` 속성에 명시된 기간 이후에 삭제됩니다.
### 쿠키 속성

- `Secure`: HTTPS 프로토콜 상에서 암호화된 요청일 경우에만 전송됩니다. (하지만 본질적으로 안전하지 않아 민감한 정보를 담는 것은 권장되지 않습니다.)
- `HttpOnly`: XSS 공격을 방지하기 위한 속성으로, JS의 `Document.cookie` API에서 접근할 수 없습니다. 이 쿠키는 서버에게 전송되기만 합니다.
- `Domain`: 쿠키가 전송되게 될 호스트를 명시(명시되지 않는 경우 현재 문서 위치의 호스트 일부를 기본값)
- `Path`: `Cookie` 헤더를 전송하기 위하여 요청되는 URL 내에 반드시 존재해야 하는 URL 경로
- `SameSite`: 쿠키가 cross-site 요청과 함께 전송되지 않았음을 요구하게 만들어 CSRF에 대한 보호 방법을 제공 (실험적)
### 쿠키 종류
- 퍼스트파티 쿠키: `Domain` 값이 현재 보고 있는 페이지의 도메인과 같은 경우
	- 설정한 서버에만 전송
- 서드파티 쿠키: `Domain` 값이 현재 보고 있는 페이지의 도메인과 다른 경우
	- 다른 도메인의 서버 상에 저장된 이미지 혹은 컴포넌트를 포함할 수 있음
	- 주로 웹을 통한 광고와 트래킹에 사용
## Cookie-parser
`cookie-parser`는 Express.js의 미들웨어로, HTTP 요청에 포함된 쿠키를 파싱하여 `req.cookies` 객체로 접근하기 쉽게 만들어 주는 역할을 합니다.
### 핵심 기능
- **파싱**: 브라우저가 보낸 `Cookie` 헤더를 분석하여 JSON 객체 형태로 변환합니다.
- **접근성**: 파싱된 쿠키 데이터를 `req.cookies` 속성을 통해 쉽게 접근할 수 있게 합니다.

```js
var express = require('express')
var cookieParser = require('cookie-parser')

var app = express()
app.use(cookieParser())

app.get('/', function (req, res) {
  // Cookies that have not been signed
  console.log('Cookies: ', req.cookies)

  // Cookies that have been signed
  console.log('Signed Cookies: ', req.signedCookies)
})

app.listen(8080)

// curl command that sends an HTTP request with two cookies
// curl http://127.0.0.1:8080 --cookie "Cho=Kim;Greet=Hello"
```
## 출처
- https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/Cookies
- https://expressjs.com/en/resources/middleware/cookie-parser.html