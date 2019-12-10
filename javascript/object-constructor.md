# 생성자 함수(Object Constructor) 이해하기 
> 자바스크립트 함수는 재사용 가능한 코드의 묶음으로 사용하는 것 외에 **객체를 생성하기 위한 방법**으로도 사용된다.  
객체를 생성하기 위해 직접적으로 객체를 반환해도 되지만,  
`new` 키워드를 사용하여 함수를 호출하게 되면 `return`문이 없어도 새로운 객체가 반환된다.  
그리고 함수 바디에서 `this` 키워드를 사용하여, 반환되는 객체의 초기 상태와 행위를 정의할 수 있다. 

> 이렇게 객체를 생성하는 역할을 하는 함수를 생성자 함수라고 하는데, **생성자 함수**는  
`new` 키워드를 *사용하지 않으면* 일반적인 함수와 동일하게 동작하며 *새로운 객체를 반환하지 않는다*.

> 객체에 타입이 적용되면 해당 객체는 그 타입의 **인스턴스**라고 부른다.  
생성자 함수는 새로운 타입을 정의하는데 사용된다. 그래서 `new` 키워드로 만들어진 객체는 해당 타입의 인스턴스가 된다.
  
## 예제
~~~javascript
function Teacher(name, age, subject) {
	this.name = name;
	this.age = age;
	this.subject = subject;
	this.teach = function(student) {
		console.log(student + '에게 ' + this.subject + '를 가르칩니다.');
	};
}

const jay = new Teacher('jay', 30, 'Javascript');
console.log(jay);
jay.teach('bbo');
~~~
1. `Teacher` 생성자 함수를 정의. 매개변수로 `name`, `age`, `subject`를 정의.
2. 전달받은 매개변수의 값을 `this`의 속성으로 대입.
3. `teach` 메소드를 정의.
4. `new` 키워드와 함께 생성자 함수를 호출하면 생성자 함수 블록이 실행되고 별도의 `return`문이 없어도 **새로운 객체가 반환**됨.
5. 이때 반환되는 새로운 객체를 가리키는 것이 `this`.
6. 그래서 `jay` 변수에 반환된 객체가 할당됨.

Console 결과
~~~console
Teacher {name: "jay", age: 30, subject: "Javascript", teach: ƒ}
bbo에게 Javascript를 가르칩니다.
~~~


---

~~~javascript
console.log(jay.constructor);
console.log(jay instanceof Teacher);
~~~
1. 모든 객체는 `constructor` 속성을 가짐. 이 속성은 **객체를 만든 생성자 함수**를 가리킴.
2. 그러므로 `jay` 객체의 `constructor` 속성은 `Teacher` 생성자를 가리키고 콘솔에 해당내용이 출력됨.
3. `instanceof` 연산자를 이용하여 `jay` 객체가 `Teacher ` 생성자 함수의 인스턴스인지의 여부를 확인할 수 있다.

Console 결과
~~~console
ƒ Teacher(name, age, subject) {
	this.name = name;
	this.age = age;
	this.subject = subject;
	this.teach = function(student) {
		console.log(student + '에게 ' + this.subject + '를 가르칩니다.');
	};
}
true
~~~

---

~~~javascript
const jay2 = Teacher('jay', 30, 'Javascript');
console.log(age);
console.log(jay2);
~~~
1. `new` 키워드를 빼고 `Teacher` 생성자 함수를 호출.
2. 이때 생성자 함수의 `this`는 전역 객체를 가리킨다.
3. 전역 객체에 `name`, `age`, `subject` 속성으로 전달받은 매개변수가 할당됨
4. 그래서 전역 변수의 `age`를 참조해 콘솔에 30이 출력된다.
5. 새로운 객체가 반환되지 않아 `jay2`는 `undefined`가 출력됨.

Console 결과
~~~console
30
undefined
~~~

---

## 생성자 함수의 `new` 호출을 통한 객체 생성 과정
1. 빈 객체를 만든다.
2. 만든 빈 객체에 `this` 를 할당한다.
3. 생성자 함수 바디의 코드를 실행 (`this`에 속성 및 메소드 추가).
4. 만든 빈 객체의 `__proto__`에 생성자 함수의 `prototype` 속성을 대입한다.
5. `this`를 생성자의 반환값으로 변환한다.