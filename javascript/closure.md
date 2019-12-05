# Closure

~~~javascript
function createCounterClosure() {
	let count = 0;
	return {
		increase : function() {
			count++;
		},
		getCount : function() {
			return count;
		}
	};
}

const counter1 = createCounterClosure();
const counter2 = createCounterClosure();

counter1.increase();
counter1.increase();
console.log('counter 1의 값 : ' + counter1.getCount());
counter2.increase();
console.log('counter 2의 값 : ' + counter2.getCount());
~~~

1. `createCounterClosure()` 함수를 정의하고 `count` 변수에 `0`을 할당.
2. `createCounterClosure()` 함수는 객체를 반환하는데 객체는 `increase`와 `getCount` 메소드가 있고, 모두 `count` 변수에 접근.
3. `createCounterClosure()` 함수를 호출하고 반환된 객체를 `counter1`과 `counter2`에 할당.
4. `counter1`과 `counter2` 객체의 increase 메소드를 호출하면 `createCounterCloure` 내부의 `count` 변수에 접근.
5. 하지만 `counter1`과 `counter2`의 `getCount`를 호출한 결과 값을 보면 `counter1`의 메소드들이 가리키는 `count`와,  
`counter2`의 메소드들이 가리키는 `count`가 **다른 값**을 가지고 있는 것을 알 수 있다.

~~~console
counter 1의 값 : 2
counter 2의 값 : 1 
~~~
위 코드에서 `counter1`과 `counter2`의 메소드들이 다른 count에 접근하는 것은 다른 렉시컬 환경(Lexical Environment)의 환경 레코드에서 `count`에 접근하기 때문.<br>이러한 현상이 가능한 이유는 바로 클로저 때문이다.

> **클로저**란 함수가 정의될 때의 렉시컬 환경을 기억하는 함수이다.
---
* `createCounterClosure` 함수선언문 안의 `increase`와 `getCount` 함수가 정의될 때의 렉시컬 환경은 `createCounterClosure` 실행 컨텍스트의
렉시컬 환경이다. 
* 이 실행 컨텍스트는 각각 `conter1`와 `counter2` 표현식에서 생성된다.
* 그래서 `increase` 함수와 `getCount` 함수는 `createCounterClosure` 실행 컨텍스트의 렉시컬 환경을 기억하고 있는 **클로저**가 된다.

* 대체로 실행 컨텍스트가 컨텍스트 스택에서 제거되면 해당 환경은 사라지기 마련인데 위 예제처럼 클로저가 만들어지면 해당환경은 사라지지 않는다.
* 왜냐하면 해당참조가 존재하기 때문(예제는 `counter1`과 `counter2`가 전역변수에 할당되어 참조가 존재).