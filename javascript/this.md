# this 이해하기
* `this`는 함수가 어떻게 호출되는지에 따라 **동적으로 결정**됨.
* `this`의  주요 목적은 작성된 코드를 여러 목적으로 재사용하기 위함.
* 그러나 호출되는 방식에 따라 동적으로 결정되어 간혹 잘못된 코드를 작성할 수 있다.

## 왜 `this`가 달라질까?
* `this`는 전역에서 사용할 수 있고, 함수 안에서도 사용할 수 있다.
* 하지만 함수는 **객체에 메소드**로 정의될 수도 있고 **생성자 함수**로 사용될 수도 있고,  
**특정 로직을 계산하여 값을 반환**하는 목적으로도 사용할 수 있다.
* 이렇게 함수가 다양하게 사용되다 보니 `this`도 각 함수별로 다르게 해석된다.
* 물론 **화살표 함수**에서의 `this`도 다르게 해석된다.
* class 안에서 사용되는 `this`는 생성자 함수(Object Constructor)와 동일하다.


### 예제1 - 전역 변수의 this는 전역 객체 window
~~~js
this.valueA = 'a';
console.log(valueA);
valueB = 'b';
console.log(this.valueB);
~~~
1. 브라우저 환경에서 `this`를 전역에서 사용하면(`this.valueA`), 전역 객체인 **Window**객체를 가리킨다.
2. 그래서 `valueA`는 `window.valueA`로 해석되고, `console.log(valueA)`는 `console.log(window.valueA)`로 해석된다.


### 예제2 - 함수 안의 this는 전역 객체 window
~~~js
function checkThis() {
	console.log(this);
}
function checkThis2() {
	'use strict';
	console.log(this);
}
checkThis(); // window
checkThis2(); // undefined
~~~
1. 함수 내에서 `this`를 사용하고, 함수를 호출하면 `this`는 전역 객체인 Window를 가리킨다. -> Why?
2. 하지만 함수 내의 코드를 엄격한 모드(`use strict`)로 실행하게 되면 `this`는 `undefined`가 된다. -> Why?

> this 예제를 여러 번 봤기 때문에 전역으로 선언된 함수나 변수들이 모두 window를 바라보는 것,  
생성자 함수나 클래스 안에서의 this가 생성자 함수와 클래스 자체를 바라보는 것을 알고 있지만,  
왜 어째서 그렇게 작동하는 지는 모르겠어.

> stric mode는 자바스크립트 코드를 좀 더 안전하고 엄격하게 작성할 수 있도록 도와준다.  
전역으로 stric mode를 지정하거나 함수 단위로도 지정할 수 있다.

### 예제3 - 생성자 함수 안의 this는 연결된 객체를 가리키지만, new 키워드 없이 호출되면 this = window
~~~js
function Product(name, price) {
	this.name = name;
	this.price = price;
}
const product1 = Product('가방', 2000);
console.log(window.name); // '가방'
console.log(window.price); // 2000
~~~
1. `Product` 함수는 생성자 함수로 작성됨.
2. 하지만 `new` 키워드 없이 호출되면, 이때 `this`는 위 예제2와 동일하게 전역 객체인 `Window`를 가리킴.
3. `new` 키워드와 함께 호출해야지만 `this`는 프로토타입 객체와 연결된 객체가 반환된다.  
(이때 `console.log(product1)`을 찍으면 `undefined` 가 나옴)  
(여기서 window객체를 가리킬 것인지 생성자 함수를 가리킬 것인지가 결정되는 것을 알 수 있다)


### 예제4 - 객체 내 메소드의 this는 객체 자신
~~~js
const product2 = {
	name : '가방2',
	price: 3000,
	getVAT() {
		return this.price / 10;
	}
}
const ValueOfProduct2 = product2.getVAT();
console.log(ValueOfProduct2); // 300
~~~
1. 객체 `product2` 내에 정의된 함수인 메소드 `getVAT()` 안에서 `this`를 사용하고,  
객체를 통해 메소드를 호출하면(`product2.getVAT`), `this`는 그 객체를 가리킨다.


### 예제5 - 객체 내 메소드에 this를 정의했어도 다른 변수를 통해 호출하면 this는 window (주어중심)
~~~js
const calVAT = product2.getVAT;
const VAT2 = calVAT();
console.log(VAT2);
~~~
1. 예제4에서 메소드 안에서 `this`를 정의했지만, 메소드를 다른 변수에 저장(`const calVAT = product2.getVAT`)하고,  
그 변수를 통해 호출(`const VAT2 = calVAT()`)하면, 일반적인 함수 호출이 되어 `this`는 전역 객체를 가리킨다.  
(그러므로 여기서 `this.price`는 `window.price = 2000`을 참조할 것이고 콘솔에는 200이 찍히는 것)
2. 즉, 호출하는 시점에 점(.) 연산자와 함께 객체가 주어져야 메소드 안의 `this`가 호출의 주체인 객체가 된다.  
(누가 소환하느냐 -> 주어가 중요하다는 뜻)


### 예제6 - this bind()
~~~js
const newCalVAT = calVAT.bind(product2);
const VAT3 = newCalVAT();
console.log(VAT3);
~~~
1. `this`는 `bind()` 메소드를 통해 전달한 인자값으로 변경할 수 있다. 