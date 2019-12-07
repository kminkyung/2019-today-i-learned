# Get, Set 을 통한 속성 접근 관리하기 (Accessor Descriptor)

~~~javascript
let user = {};
Object.defineProperty(user, "age", {
	get : function () {
		return this._age;
	},
	set : function(age) {
		if(age < 0) console.error("0보다 작은 수가 올 수 없습니다.");
		else this._age = age;
	},
	enumerable : true
});
user.age = 10;
console.log(user.age);
user.age = -1; 
~~~
1. 속성 기술자를 통해 user 객체의 속성 정의. `Object.defineProperty(객체, 속성이름, {속성정의})`
2. 이때 값에 접근하는 방식을 정의하는 객체를 전달하는데, 이 객체를 접근 기술자 (Accessor Descriptor)라 한다.
3. `get`과 `set`을 메소드로 가진다.
4. `get` 메소드는 속성에 **접근**할 때 호출된다.
5. `set` 메소드는 속성에 **값을 대입**할 때 호출된다.

6. 그래서 user.age 에 접근하면 `get` 메소드가 호출되어 `user_.age`의 결과를 반환한다.
7. `user.age`에 값 10을 대입한다. 그러면 `age`의 속성 접근 기술자 `set` 메소드가 호출되고, `user` 객체의 `_age` 속성에 값 10이 할당됨.

위에서는 get : function(){} 으로 get 속성 안에 함수를 담았는데  
아래처럼 get functionName(){} 을 바로 써도 된다.  
근데 get과 set이 호출될 때가 다르므로 함수 이름(name)을 똑같이 써도 되나봄?

~~~javascript
let user2 = {
	get name() {
		return this._name;
	},
	set name(val) {
		if(val.length < 3) throw new Error('3자 이상이어야 합니다.');
		this._name = val;
	}
}
user2.name = 'harin';
console.log(user2.name);
user2.name = 'ha';
~~~

1. `user2` 객체를 정의할 때 `name` 속성의 접근 기술자를 정의.
2. 객체를 정의할 때 메소드를 정의하는 메소드명 앞에 `get` 과 `set`으로 각각의 `get` 메소드와 `set` 메소드를 정의할 수 있음.

> 위 예제에서 속성 이름에 `_`를 붙이는 것은 암묵적으로 비공개(Private) 속성을 나타낸다. 자바스크립트 객체는 속성 접근 제한자가 없어서 모든 속성은 공개(Public)이다. 그래서 대체로 이름 규칙을 통해 비공개임을 나타낸다.