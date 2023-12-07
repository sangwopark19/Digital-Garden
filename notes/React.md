
## 초기 구조
---
- README.md: 문서
- package.json: 종속성 정의 (Maven, Gradle과 유사)
- node_moduls: 종속성 파일 다운로드 폴더
- React Initialization:
	- public/index.html: 웹페이지에서 가장 먼저 로드되는 페이지, root div를 포함한다.
	- src/index.js: root div에 들어가는 내용, 리액트 앱을 초기화하고 **앱 컴포넌트**라는 걸 로드하는 것이다.  (기본적으로 App.js를 가지고 있다.)
		- src/index.css: 전체 애플리케이션의 스타일을 담고 있다.
	- src/App.js:기본으로 있는 **앱 컴포넌트**
		- src/App.css: 앱 컴포넌트를 위한 css
		- src/App.test.js: 앱 컴포넌트를 위한 Unit Test
			- 자바와 다르게 App.test.js가 App.js와 같은 디렉토리에 있다. 즉 javascript에서는 단위 테스트 코드가 프로덕션 코드와 함께 있다.

## 리액트 컴포넌트란
---
- App Component는 **First component**로 리액트 앱의 기본이 되는 컴포넌트이다.
- 그 외의 컴포넌트
	- 일반적으로 App Component의 **자식**으로 만들며 여러가지 기능이 있다.
		- View (JSX or JavaScript)
		- Logic (JavaScript)
		- Styling (CSS)
		- State (Internal Data Store)(컴포넌트 내부 데이터 저장소: 멤버변수)
		- Props (Pass Data)(데이터 넘기기)
- 리액트 컴포넌트의 이름은 **항상 대문자로** 시작 해야한다.
- 일반 html태크는 **항상 소문자만 사용** 해야한다.

### 사용법
- 서로 다른 모듈에서 클래스를 사용하려면 **임포트**를 해야 한다.
```js
// 기본 임포트
import FirstComponent from './components/FirstComponent';

// 이름지정 임포트 (중괄호를 사용해야함)
import {SecondComponent} from './components/FirstComponent';

```
#### 예시
컴포넌트는 파일당 하나만 있는게 가장 좋다.
다음과 같은 컴포넌트를가 있을때
```js
export default function FirstComponent() {

return (

<div className='FirstComponent'>FirstComponent</div>

)

}
```
App.js에서 사용하기 위해선 다음 코드를 사용해야 한다.
```js
import './App.css';

import FirstComponent from './components/FirstComponent';

  

function App() {

return (

<div className="App">

<FirstComponent />

</div>

);

}
```

- 컴포넌트 파일당 export default는 **무조건 있어야 하고 한개만 있어야 한다**
	- 같은파일에 컴포넌트를 추가로 넣고싶으면 다음과 같이 사용한다
```js
export default function FirstComponent() {

return (

<div className='FirstComponent'>FirstComponent</div>

)

}

default function SecondComponent() {

return (

<div className='FirstComponent'>FirstComponent</div>

)

}
```
그리고 app.js 에서 불러올때 다음과 같이 중괄호가 들어가야 한다.
```js
import {SecondComponent} from './components/FirstComponent';

```

## State
---
리액트 초기에는 클래스 컴포넌트만 state를 가질 수 있었다.
하지만 16.8 버전 이후 함수 컴포넌트도 **Hooks**를 이용하면 state를 가질 수 있게 되었다.
즉 이제는 **클래스 컴포넌트를 사용할 필요가 없다. 함수 컴포넌트만 사용하자.**

### hooks
`useSate()`는 2개의 배열값을 리턴한다.
- 현재상태 (초기값)
- 상태를 업데이트하는 함수
아래 예시는 초기값을 0으로 설정하고 함수가 실행될 때마다 초기값에서 +1 하는 예시다
```js
const [count, setCount] = useState(0);

function incremmentCounterfunction() {
	setCount(count + 1)
}
```


### dom 접근방식
- 페이지 로드 시 React가 가상DOM v1을 생성
- 페이지 업데이트 후 가상 DOM v2 생성
- React가 v1, v2의 차이를 계산
- 변경사항을 동기화 (HTML 페이지 업데이트)

- **요약**: DOM을 직접 업데이트하지 않는다.
	- React는 변경 사항을 식별하고 DOM을 **효율적으로** 업데이트한다.
## JSX
---
Javascript XML
대부분의 리액트 프로젝트는 view를 JSX로 표현한다.

JSX의 특징은 다음과 같다.
- HTML보다 엄격하다
	- **닫는태그** 필수
	- 최상위 태그는 **한개만**
		- 여러개의 태그가 있을시 부모 태그로 묶어줘야함

## Babel
---
Babel은 JSX를 JS로 변환한다.
### 쓰는 이유
최신 Javascript 문법이 구버전 브라우저에서도 동작 가능하게 컴파일 해주는 기술

## JSX에서 CSS 정의
---

- style
	- 스타일 값을 객체로 넘겨줘야하기 때문에 {}한번, 그리고 스타일을 객체로 감싸기위해 {}한번 **총 2번** {}로 감싸야 한다.
```css
<button style={{borderRadius:"30px"}}>
```

- className
	스타일 jsx내부 자바스크립트 정의방법
```js
const buttonStyle = {borderRadius:"30px"}}
```
	외부 css 정의 방법
```js
import './style.css'

<button className='buttonStyle'>
```


