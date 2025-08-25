# Express.js
## 개요

Node.js에서 웹 애플리케이션과 API를 쉽게 개발할 수 있도록 하는 프레임워크

- 간단하고 유연한 라우팅 시스템 제공
- 미들웨어를 통한 요청/응답 처리
- REST API 개발에 최적화
- 다양한 템플릿 엔진과 연동 가능
## 설치 방법

 Express 4.x에는 Node.js 0.10 이상, Express 5.x에는 Node.js 18 이상이 필요하고, Node.js가 이미 설치되었다고 가정합니다.

```bash
npm init # 종속성 설정
npm install express # express 설치
```

혹은 다음과 같은 명령어를 통해 express 프로젝트를 바로 생성할 수 있습니다.

```bash
npm install -g express-generator
express 
```

## 기본 라우팅
각 경로에는 하나 이상의 핸들러 함수가 있을 수 있으며, 경로가 일치하면 해당 함수가 실행됩니다. 경로 정의는 다음과 같은 구조를 갖습니다.

```js
app.METHOD(PATH, HANDLER)
```
- `app`: `express` 인스턴스
- `METHOD`: 소문자로된 HTTP 요청방법
- `PATH`: 서버의 경로
- `HANDLER` 경로가 일치할 때 실행되는 함수

간단한 예시는 다음과 같습니다.

```js
app.get('/', (req, res) => {
  res.send('Hello World!')
})
```

## 정적 파일 제공

이미지, CSS 파일, Javascript 파일과 같은 정적 파일을 제공하려면, `express.static` 미들웨어 함수가 필요합니다.

```js
express.static(root, [options])
```

- `root`: 정적 파일을 제공할 루트 디렉토리를 지정

## 템플릿 엔진

Express.js에서는 `res.render()` 메서드를 사용하여 템플릿 파일을 렌더링할 수 있습니다. 템플릿 엔진을 사용하려면, `view engine`을 설정하고 해당 엔진의 패키지를 설치해야 합니다.

### 템플릿 엔진 종류

- EJS (Embedded JavaScript)
	- HTML과 Javascript를 결합하여 사용
	- 조건문, 반복문 등 Javascript 문법을 그대로 사용할 수 있어 직관적
- Pug
	- 들여쓰기를 이용한 간결한 문법을 사용
	- 템플릿 작성이 빠르고, 코드가 깔끔하게 유지
- Handlebars
	- Mustache 스타일의 템플릿 문법을 사용
	- 로직을 템플리셍서 분리하여 유지보수성이 높음
## 출처

- [express](https://expressjs.com/)
- https://pugjs.org/api/getting-started.html
- https://www.npmjs.com/package/ejs
- https://www.npmjs.com/package/handlebars