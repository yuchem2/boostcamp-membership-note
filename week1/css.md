# CSS(Cascading Style Sheets)
## 개요

CSS는 웹페이지의 디자인과 레이아웃을 담당하는 스타일 시트 언어. HTML이 웹페이지의 뼈대와 내용을 구성한다면, CSS는 그 뼈대에 색을 입히고, 크기를 조절하고, 위치를 배치하는 등 시각적인 부분을 담당합니다. 
## 특징과 개념

### 기본 문법
CSS 코드는 선택자와 선언부로 구성됩니다.
- Selector: 스타일을 적용할 HTML 요소를 지정합니다.
	- `tagName` : 태그를 지정하여 스타일을 적용
	- `.className`: `class` 속성 값이 `className`인 모든 요소에 스타일을 적용
	- `#idName`: `id` 속성 값이 `idName`인 단일 요소에 스타일을 적용
- Declaration: 선택된 요소에 적용할 스타일을 정의
	- 속성-값 형태로 스타일을 적용합니다.

Selector은 위에서 언급한 방법처럼 단일 속성을 선택하는 방법 외에도 다양한 활용 방법이 존재

```css
div.myclassname { ... } /* tag, class 요소 선택자 함께 사용 */
h1, span, div { ... } /* 그룹 선택 */
div > span { ... } /* 하위 요소 선택 */
div > p:nth-child(2) { ... } /* `div`의 자식 요소 중 두 번째에 위치하는 요소가 `<p>` 태그일 경우에만 선택 */
div > p:nth-of-type(3n + 1) { ... } /* 3n+1 공식에 해당하는 p 태그를 선택 (1, 4, 7...번째) */
```
### 적용 방법
CSS를 HTML에 적용하는 방법은 주로 세 가지 방법이 있습니다.

- Inline Style
	- HTML 태그의 `style` 속성에 직접 CSS 코드를 작성
	- 간단하지만 코드의 가독성이 떨어짐
- Internal Stylesheet
	- HTML 문서의 `<head>` 태그 내부에 `<style>` 태그를 사용하여 CSS 코드를 작성
	- 해당 HTML 문서에만 스타일이 적용
- External Stylesheet
	- 별도의 `.css` 파일을 만들어 HTML 문서에서 `<link>` 태그를 사용하여 연결하는 방식
	- 가장 일반적이고 권장되는 방식, 여러 HTML 페이지에서 하나의 CSS 파일을 공유할 수 있어 유지보수가 용이
	- 하나의 CSS 파일이 매우 커질 수 있다는 단점 존재
### Cascading
여러 개의 스타일 규칙이 충돌할 경우 어떤 규칙을 우선적으로 적용할 지를 결정하는 방식
- 인라인 스타일 > 내부 스타일 시트 = 외부 스타일 시트 > 브라우저 기본 설정(user agent)
- 선택자의 구체성이 높을수록 우선순위가 높음 ( `id` 선택자 > `class` 선택자 > `tag` 선택자)
- 만약 우선순위가 동일하다면 나중에 선언한 것이 적용
### 상속
CSS에서 상속은 부모 요소에 적용된 스타일이 자식 요소에 자동으로 전달되는 특성을 의미합니다.모든 CSS 속성이 상속되는 것은 아니며, 주로 텍스트와 관련된 속성들이 상속됩니다.
#### 상속되는 주요 속성 (Inherited Properties)
- `color`: 글자 색상
- `font-family`: 글꼴 종류
- `font-size` 글자 크기
- `font-weight`: 글자 굵기
- `line-height`: 줄 간격
- `text-align`: 텍스트 정렬
#### 상속되지 않는 주요 속성 (Non-inherited Properties)
반면, 너비, 높이, 여백, 테두리 등 레이아웃과 관련된 속성들은 상속되지 않습니다.
- `width`: 너비
- `height`: 높이
- `margin`: 외부 여백
- `padding`: 내부 여백
- `border`: 테두리
- `background-color`: 배경색
#### 상속 제어하기
- `inherit`: 부모 요소의 값을 강제로 상속받도록 합니다.
- `initial`: 해당 속성의 기본값으로 되돌립니다.
- `unset`: 속성을 상속되는 속성은 `inherit`으로, 상속되지 않는 속성은 `initial`로 설정합니다.
## CSS Layout

CSS 레이아웃은 웹페이지의 요소를 원하는 위치에 배치하고 구조화하는 데 사용되는 기술을 통칭합니다. 과거에는 `float`, `position` 등을 사용하여 복잡한 레이아웃을 만들었지만, 현대 CSS에서는 훨씬 강력하고 효율적인 레이아웃 기술이 등장하여 복잡한 웹페이지도 쉽게 만들 수 있게 되었습니다.
### Box Model
CSS의 모든 요소는 사각형 박스로 취급됩니다. 이 박스는 콘텐츠, 패딩, 테두리, 마진으로 구성됩니다.
- Content: 텍스트나 이미지가 들어가는 실제 내용 영역. `width`와 `height` 속성으로 크기를 조절합니다.
- Padding: 콘텐츠와 테두리 사이의 내부 여백.
- Border: 콘텐츠와 패딩을 감싸는 테두리.
- Margin: 테두리 바깥쪽의 외부 여백. 다른 요소와의 간격을 조절합니다.
### Inline, Block, Inline-Block
CSS의 모든 HTML 요소는 기본적으로 `inline` 또는 `block`이라는 `display` 속성 값 중 하나를 가집니다. 이 속성은 요소가 웹페이지에서 어떻게 배치될지를 결정합니다.

- Inline (인라인): 요소는 줄을 바꾸지 않고 텍스트 흐름에 따라 한 줄에 나란히 배치됩니다. `width`, `height`를 설정할 수 없으며, 상하 `margin`과 `padding`이 적용되지 않습니다. (`<span>`, `<a>` 등)
- Block (블록): 요소는 새로운 줄에서 시작하며, 항상 한 줄 전체를 차지합니다. `width`, `height` 및 상하좌우 `margin`, `padding`을 모두 적용할 수 있습니다. (`<div>`, `<p>` 등)
- Inline-Block (인라인-블록): `inline`처럼 한 줄에 나란히 배치되면서, `block`처럼 `width`, `height` 및 상하좌우 `margin`, `padding`을 모두 적용할 수 있습니다.
### Flexbox
1차원(One-dimensional) 레이아웃 시스템으로, 한 줄(row) 또는 한 열(column) 방향으로 요소를 배치하고 정렬하는 데 최적화되어 있습니다. 컨테이너에 `display: flex;` 속성을 부여하여 사용합니다.

- 정렬: `justify-content` (주축 정렬), `align-items` (교차축 정렬) 속성을 사용하여 요소를 간편하게 정렬할 수 있습니다.
- 공간 분배: `flex-grow`, `flex-shrink`를 이용해 남은 공간을 요소에 분배할 수 있습니다.
- 반응: 반응형 디자인에 유연하게 대응할 수 있습니다.
### Grid
2차원(Two-dimensional) 레이아웃 시스템으로, 행(row)과 열(column)을 동시에 사용하여 요소를 배치하는 데 사용됩니다. 표(table)처럼 구조적인 레이아웃을 만들 때 매우 유용합니다. 컨테이너에 `display: grid;` 속성을 부여하여 사용합니다.

- 복잡한 레이아웃: 웹사이트의 전체적인 페이지 구조(예: 헤더, 사이드바, 메인 콘텐츠, 푸터 등)를 쉽게 만들 수 있습니다.
- 정교한 배치: `grid-template-rows`, `grid-template-columns` 등으로 행과 열의 크기를 정하고, `grid-area` 등을 사용하여 요소를 원하는 위치에 배치할 수 있습니다.
### Positioning
요소를 문서 흐름에서 벗어나 원하는 위치에 배치하는 방법

- `position: static`: 기본값. 문서 흐름에 따라 배치됩니다.
- `position: relative`: 기본 위치를 기준으로 `top`, `bottom`, `left`, `right` 값을 사용하여 위치를 이동시킵니다.
- `position: absolute`: 가장 가까운 `relative`, `absolute`, `fixed`, 또는 `sticky` 속성을 가진 부모 요소를 기준으로 위치를 이동시킵니다.
- `position: fixed`: 뷰포트(브라우저 화면)를 기준으로 위치를 고정합니다. 스크롤해도 항상 같은 위치에 나타납니다.
- `position: sticky`: 스크롤 위치에 따라 `relative`와 `fixed` 속성 사이를 전환합니다. 일정 스크롤 위치에 도달하면 요소가 고정됩니다.
