# 화살표 함수 (Arrow Function)
> ES6에서는 기존 함수를 간결하게 표현할 수 있고 기능이 개선된 화살표 함수가 추가되었다.  
화살표 함수는 `function` 키워드를 사용하지 않고 화살표 모양의 `=>` 연산자를 이용하여 정의한다.  
화살표 함수를 정의할 때는 몇 가지 규칙이 있다.
* 매개변수가 하나일 경우에는 인자를 정의할 때 괄호생략 가능.
* 매개변수가 **없거나** 둘 이상일 경우 괄호 작성.
* 화살표 함수 코드 블록{} 을 지적하지 않고 한 문장으로 작성 시 `return` 문을 사용하지 않아도 화살표 오른쪽 표현식의 계산 결과값이 반환됨.
* 화살표 함수 코드 블록{} 을 지정했을 경우 반환하고자 하는 값에 `return` 문을 작성해야 한다. <br>`return` 문 없을 시 `undefined` 반환.

~~~javascript
const double = x => x + x;
console.log(double(2));
~~~
매개변수 `x` 를 전달 받아 `x + x` 결과를 반환하는 화살표 함수를 정의하고 `double` 변수에 할당.  
`double(2)`는 `2 + 2` 결과인 `4`가 반환되고, console.log에 전달하여 4가 출력됨.

~~~javascript
const add = (a, b) => a + b;
console.log(add(1, 2));
~~~
`a`와 `b` **두 매개변수**를 가지는 화살표 함수 정의. 그래서 매개변수에는 괄호를 사용하고, 코드 블록은 한 문장이기 때문에 두 매개변수 합의 결과값이 반환.

~~~javascript
const printArguments = () => {
	console.log(printArguments(arguments));
}
printArguments(1, 2, 3); // 화살표함수는 기본함수와 다르게 arguments 객체가 만들어지지 않아 에러가 발생
~~~
아무런 매개변수를 정의하지 않았기 때문에 괄호로 빈 매개변수를 표현.  
화살표 함수 코드 블록을 작성하고 내부에 `arguments` 객체를 콘솔에 출력. `return`문이 없기 때문에 반환값은 없다.  
인자로 `1, 2, 3`을 전달하면서 정의된 화살표 함수를 호출하지만, 콘솔에는 `Uncaught ReferenceError`가 발생.  
화살표 함수는 기본 함수와 다르게 **`arguments` 객체가 만들어지지 않아** 에러가 발생한다.

~~~javascript
const sum = (...args) => {
	let total = 0;
	for(let i = 0; i < args.length; i++) {
		total += args[i];
	}
	return total;
}
console.log(sum(1, 2, 3));
~~~
전달받은 인자들의 합을 구하는 화살표 함수를 정의.  
`arguments` 객체 대신 나머지 연산자(`...`)를 통하여 매개변수를 정의. 
`args`는 전달받은 인자 목록을 배열로 사용할 수 있다.  
그리고 화살표 함수 코드 블록에 대괄호를 사용하였기 때문에 `return`문을 작성하여 반환값을 명시한다.

~~~javascript
setTimeout(() => {
	console.log('화살표 함수');
}, 10);
~~~
화살표 함수 또 한 함수의 인자로 전달 가능.  
`setTimeout()` 함수의 인자로 화살표 함수가 전달되고 이때 매개변수가 없어 괄호를 작성해준다.

Console 결과
~~~console
4
3
6
Uncaught ReferenceError : arguments is not defined
화살표 함수
~~~
