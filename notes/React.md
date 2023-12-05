
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

## State
---
리액트 초기에는 클래스 컴포넌트만 state를 가질 수 있었다.
하지만 16.8 버전 이후 함수 컴포넌트도 **Hooks**를 이용하면 state를 가질 수 있게 되었다.
즉 이제는 **클래스 컴포넌트를 사용할 필요가 없다. 함수 컴포넌트만 사용하자.**

## JSX
---
Javascript XML
대부분의 리액트 프로젝트는 HTML을 JSX로 표현한다.
JSX의 특징은 다음과 같다.
- HTML보다 엄격하다
	- **닫는태그** 필수
	- 최상위 태그는 **한개만**
		- 여러개의 태그가 있을시 부모 태그로 묶어줘야함