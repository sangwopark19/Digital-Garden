# Objects and Object Constructors
---
- 객체 생성자를 어떻게 작성하고 객체를 인스터화 할까?
	객채 생성자는 `function Constructor(value1, value2) {
	}`로 작성하며
	인스턴스화는 `let con = new Constructor('hi', 'hi2')`로 한다

- 프로토타입이란 무엇이며 어떻게 사용될 수 있을까?
	프로토타입은 원본객체가 상속하는 또 다른 객체이며 원본 객체는 프로토타입의 모든 메소드와 속성에 액세스 할 수 있다.
	주로 객체 상속에 이용된다

- 프로토타입 상속이란 무엇인가?
	예를 들어 속성과 메서드를가진 user 객체가 있고 비슷한 admin객체를 만들 때  user의 모든 속성과 메서드 다시 구현하는 것이 아니라 상속을 받고 싶을때 프로토타입 상속을 이용한다.

- 프로토타입 상속의 권장되는 방법과 권장되지 않는 방법은 무엇인가?
	`Object.getPrototypeOf()`를 사용해 객체의 프로토타입을 가져오거나
	`Object.setPrototypeOf()`를 사용해 설정하거나 변경하는게 권장되는 방법이다.
	
	권장되지 않는 방법은 `Player.prototypen= Person.prototype;`처럼 직접 참조하도록 설정하는방법이다.
	또한 setPrototypeOf()를 사용할때 에는 **객체를 생성하기 전에 사용**하여 프로토 타입 체인을 설정해야 한다. 객체가 이미 생성된 후에 사용하면 **성능 문제**가 발생할 수 있다.

- `this` 키워드는 다양한 상황에서 어떻게 작동하나?
	기본적으로 `person.getName()`과 같이 사용을 할 때 getName 내부에서 this는 메서드가 실행된. 앞의 객체를 참조한다.
	
	글로벌 컨텍스트에선 브라우저는 window객체, Node.js는 global 객체를 참조한다. 이 동작은 엄격 모드와 비엄격 모드 모두에서 일관된다.
	
	다음 예와 같이 간단한 함수 호출에선 엄격, 비엄격 모드가 다르다
```js
function show() {
	console.log(this);
	// 비엄격 모드는 전역객체인 window, global을 호출
	// 엄격 모드는 undefined
	}
```

this에 관한 추가정보
[this](https://www.javascripttutorial.net/javascript-this/)

