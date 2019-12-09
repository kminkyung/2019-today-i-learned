# 자바스크립트 객체지향 프로그래밍(Object Oriented Programming) 이해하기
## 예제 1
~~~javascript
const teacherJay = {
	name : '제이',
	age : 30,
	teachJavascript : function(student) {
		student.gainExp();
	}
}

const studentBbo = {
	name : '뽀',
	age : 20,
	exp : 0,
	gainExp : function() {
		this.exp++;
	}
}

console.log(studentBbo.exp);
teacherJay.teachJavascript(studentBbo);
console.log(studentBbo.exp);
~~~
1. 제이 선생을 객체로 표현. 제이 선생은 이름과 나이를 속성으로 갖고, 자바스크립트를 가르치는 행위(메소드)를 한다.
2. `teachJavascript` 메소드는 `student`를 매개변수로 정의. 즉, `teacherJay` 객체는 `student` 객체를 사용.
3. 객체지향에서는 객체들이 서로 의사소통을 하게 되는데, 메소드를 통해 서로 메시지를 전달.
4. 객체지향에서는 협력하지 않은 객체란 존재하지 않는다. 이때, 협력은 메시지 전달을 통해 이루어짐.

5. 뽀 학생을 객체로 표현. 뽀 학생은 이름과 나이, 경험치를 속성으로 갖고, 경험치를 얻는 행위(메소드)를 한다.
6. 이 행위를 통해 내부 상태인 경험치를 변경시킬 수 있다.
  
Console 결과
~~~console
0
1
~~~

* 객체지향에서는 무수히 많은 객체들을 공통적인 특성을 기준으로 객체를 묶어서 하나의 타입으로 정의.
* 이렇게 타입을 정의하는 작업을 분류(classification)라고 하며, 이는 일종의 추상화를 하는 것.
* 예를 들어, 세상에는 자바스크립트를 가르치는 많은 선생이 존재하고, 제이도 그중 일부 객체이다.
* 이때 우리는 자바스크립트 선생이란 타입을 분류하고, 모든 자바스크립트 선생은 자바스크립트를 가르치는 공통 특징이 있다고 정의할 수 있다.

* 자바스크립트는 **프로토타입 기반으로 객체지향 프로그래밍**을 지원.
* 자바의 클래스 기반과의 큰 차이점으로 프로토타입으로 객체에 공통 사항을 적용할 수 있다.
* 즉, 모든 객체는 다른 객체의 원형(Prototype)이 될 수 있다.
* 특징을 묘사하는 원형 객체를 만들고 이 원형 객체에 기반하는 여러 객체들을 만들면 모두 같은 특징을 가질 수 있다.

---
## 예제 2
~~~javascript
const studentProto = {
	gainExp : function() {
		this.exp++;
	}
}
const harin = {
	name : '하린',
	age : 10,
	exp : 0,
	__proto__ : studentProto
}
const bbo = {
	name : '뽀',
	age : 20,
	exp : 10,
	__proto__ : studentProto
}
bbo.gainExp();
harin.gainExp();
harin.gainExp();
console.log(harin);
console.log(bbo);
~~~
1. 학생의 경험치를 얻는 행위를 `gainExp` 메소드로 작성한 원형(Prototype) 객체를 정의.
2. 이름, 나이, 경험치를 가지는 `harin` 객체를 정의. 자바스크립트에서는 `__proto__` 속성으로 원형 객체를 정의할 수 있다.
3. 모든 자바스크립트 객체는 `__proto__` 속성을 가지는데 예제 코드에서 처럼 별도로 `__proto__` 속성에 다른 객체를 할당하지 않으면 기본적으로 `Object.prototype` 객체가 연결되어 있다.
4. `harin` 객체는 `__proto__` 속성에 `studentProto` 객체를 연결했기 때문에 경험치를 얻는 `gainExp` 메소드 사용가능.
5. `harin` 객체와 `bbo` 객체 모두 경험치를 얻는 행위(`gainExp` 메소드)를 할 수 있다. 모두 같은 원형 객체(`studentProto`)에 연결되어 있기 때문.

Console 결과
~~~console
{ age: 10, exp: 2, name: "하린" }
{ age: 20, exp: 11, name: "뽀" }
~~~