# 객체 속성 기술자 (Object Property Descriptor)
* Object.getOwnPropertyDescriptor()
* Ojbect.defineProperty(정의할 객체, 속성명, 속성기술자)

~~~javascript
let user = {
	name: "jaedo"
};
let descriptor = Object.getOwnPropertyDescriptor(user, "name");
console.log(descriptor);

let user2 = {};
Object.defineProperty(user2, "name", {
	value: "jeado",
	enumerable: true,
	configurable: true,
	writable: false //value 변경불가
});
console.log(user2.name);
user2.name = "bbo";
console.log(user2.name);

let user3 = {
	name: "jaedo",
	toString() {
		return this.name;
	}
};
Object.defineProperty(user3, "toString", {
	enumerable: false //for Loop, Object.keys 등의 속성나열불가
})
for(let key in user3) {
	console.log(key);
}

let user4 = {};
Object.defineProperty(user4, "name", {
	value: "jeado",
	configurable: false //속성변경불가
});
delete user4.name
console.log(user4);
Object.defineProperty(user4, "name", {
	writable: true
});
~~~

~~~console
{configurable: true, enumerable: true, value: "jaedo", writable: true}
jeado
jeado
name
{name: "jeado"}
Uncaught TypeError: Cannot redefine property: name
~~~
---
* 자바스크립트의 모든 객체 속성은 자기 자신에 대한 정보를 담고있는 **속성기술자**(Property Descriptor)를 가지고 있다.
* 이 속성 기술자는 객체로 표현된다.
* `Object.getOwnPropertyDescriptor`를 통해 속성 기술자 객체를 가지고 올 수 있다.
---
1. `user2` 객체를 선언하고 `Object.defineProperty`를 통해 해당 객체의 속성을 정의.
2. 첫번째 인자는 **속성을 정의할 객체**, 두 번째 인자는 **"속성명"**, 세 번째 인자는 **{속성 기술자}**.
3. 속성 기술자는 객체로써 다음과 같은 속성을 가진다.
	* value : 값
	* enumerable : for-in 루프나 Object.keys 메소드같이 속성을 나열할 때 나열 가능 여부를 정의
	* writable : 값을 변경할 수 있는 여부 정의
	* configurable : 속성 기술자 변경 여부 정의

4. `user2` 속성 기술자에게 writable 속성을 `false`로 주고 `value`를 `"jaedo"`로 주었다.  
5. 그렇기 때문에 `"bbo"`로 값을 재할당해도 콘솔에는 바뀌지 않고 기존 값이 출력된다.
  
6. `user3` 객체에 `toString()` 메소드로 정의하고 속성 기술자를 통해 이 메소드 `enumerable`을 `false`로 재정의.
7. `for-in` 루프로 모든 속성에 접근하여 속성 이름을 콘솔에 출력, 하지만 `toString` 속성은 `enumerable`이 `false`기 때문에 출력되지 않음.
  
8. `user4` 객체에 속성 기술자를 통하여 `name` 속성을 정의하면서 `configurable` 속성을 `false`로 지정.
9. `configurable`이 `false`기 때문에 `delete`를 통해서 `name` 속성을 지우려고 하면 해당 속성이 지워지지 않고 `false`가 return 된다.
10. `name` 속성이 지워지지 않은 것을 확인하기 위해 콘솔에 출력해보면, 출력 결과 이전과 동일하게 `name` 속성에 `"jeado"`가 할당된 것을 확인.
11. 그리고 새롭게 `name`을 속성 기술자로 재정의 하려면 `configurable`이 `false`이기 때문에 에러가 발생한다.