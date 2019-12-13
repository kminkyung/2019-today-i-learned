# 상위에서 하위 컴포넌트로 데이터 전달하기
## props 속성
1. 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성.
2. 아래처럼 하위 컴포넌트의 속성에 props 를 정의한다.
~~~javascript
Vue.component('child-compnent', {
	props: ['속성이름']
});
~~~

3. 상위 컴포넌트의 HTML 코드에 등록된 `<child-component>`에 `v-bind` 속성을 추가한다.
~~~javascript
<child-component v-bind:props 속성이름="상위 컴포넌트의 data 속성"></child-component>
~~~

4. v-bind 속성의 왼쪽 값으로 **하위 컴포넌트에서 정의한 props 속성**을 넣고, 
5. 오른쪽 값으로 하위 컴포넌트에 전달할 상위 컴포넌트의 data 속성을 지정한다.

---

### 예제
~~~html
<body>
	<div id="app">
	<child-component v-bind:propsdata="message"></child-component>
	</div>

	<script>
		Vue.component('child-component', {
			props: ['propsdata'],
			template: '<p>{{ propsdata }}</p>'
		})
		new Vue({
			el: "#app",
			data: {
				message: "Hello Vue! passed from Parent Component"
			}
		});
	</script>
</body>
~~~
위 코드는 상위 컴포넌트의 `message` 속성을 하위 컴포넌트에 `props`로 전달하여 메세지를 출력한다.
props의 속성을 이해하기 위해 코드를 작성한 순서대로 살펴보자.

1. `new Vue()`로 인스턴스를 하나 생성한다.
2. `Vue.component()`를 이용하여 하위 컴포넌트인 `'child-component'`를 등록한다.
3. `'child-component'`의 내용에 `props` 속성으로 `propsdata`를 정의한다.
4. HTML에 컴포넌트 태그를 추가한다 `<child-component>` 태그의 `v-bind` 속성을 보면,  
`v-bind:propsdata="message"`는 상위 컴포넌트의 `message` 속성 값인  
Hello Vue! passed from Parent Component 텍스트를 하위 컴포넌트의 `propsdata`로 전달했다.
5. `'child-component'`의 `template` 속성에 정의된 `<p>{{ propsdata }}</p>`는 Hello Vue! passed from Parent Component가 된다.

> 여기서 컴포넌트 간의 관계를 짚고넘어가보자.<br>
예제 코드에서는 `child-component`를 전역으로 등록한 것 이외에 딱히 **상위 컴포넌트를 지정하지 않았다**.  
그럼에도 불구하고 뷰 인스턴스 안에 마치 상위 컴포넌트가 존재하는 것처럼 하위 컴포넌트로 `props`를 내려보냈다.  
그 이유는 **컴포넌트를 등록함과 동시에 뷰 인스턴스 자체가 상위 컴포넌트가 되기 때문**이다.

이렇게 인스턴스에 새로운 컴포넌트를 등록하면 *기존에 있는 컴포넌트는 상위 컴포넌트*가 되고,  
새로 등록된 컴포넌트는 하위 컴포넌트가 된다. 그리고 이렇게 새 컴포넌트를 등록한 인스턴스를  
**최상위 컴포넌트(Root Component)**라고도 부른다.


# 하위에서 상위 컴포넌트로 이벤트 전달하기
## 이벤트 발생과 수신
앞에서 배운 `props`는 상위 -> 하위 컴포넌트로 데이터를 전달하는 방식이다.
반대로 하위 -> 상위 컴포넌트 통신은 이벤트를 발생시켜(event emit) 상위 컴포넌트에 신호를 보내면 된다.

1. 상위 컴포넌트에서 하위 컴포넌트의 특정 이벤트가 발생하기를 기다리고 있다가,  
2. 하위 컴포넌트에서 특정 이벤트가 발생하면 상위 컴포넌트에서 해당 이벤트를 수신하여,  
3. 상위 컴포넌트의 메서드를 호출한다.

> 하위에서 상위 컴포넌트로 데이터를 전달할 수는 없을까?  
뷰 공식 사이트의 이벤트 발생 사용 방법에서는 하위 -> 상위로 데이터를 전달하는 방법을 다루지 않는다.  
왜냐하면 이는 뷰의 단방향 데이터 흐름에 어긋나는 구현 방법이기 때문. 하지만 향후에 복잡한  
뷰 애플리케이션을 구축할 때 이벤트 버스(Event Bus)를 이용하여 데이터를 전달해야 할 경우가 있기 때문에  
이벤트 인자로 데이터를 전달하는 방법은 *이벤트 버스* 부분에서 다룰 예정.

---

### 이벤트 발생과 수신 형식
이벤트 발생과 수신은 `$emit` 과 `v-on:` 속성을 사용하여 구현한다.
~~~javascript
// 이벤트 발생
this.$emit('이벤트명');
~~~
~~~javascript
// 이벤트 수신
<child-component v-on:이벤트명="상위 컴포넌트의 메서드명"></child-component>
~~~

* `$emit()`을 호출하면 괄호 안의 이벤트명의 이벤트가 발생한다.  
* 일반적으로 `$emit()`을 호출하는 위치는 하위 컴포넌트의 특정 메서드 내부이다.  
따라서, `$emit()`을 호출할 때 사용하는 `this`는 하위 컴포넌트를 가리킨다.
* 호출한 이벤트는 하위 컴포넌트를 등록하는 태그(상위 컴포넌트의 `template` 속성에 위치)에서 `v-on:`으로 받는다.
* 하위 컴포넌트에서 발생한 이벤트 명을 `v-on:` 속성에 지정하고, 속성의 값에 이벤트가 발생했을 때 호출될 상위 컴포넌트의 메서드를 지정한다.

---

### 예제
~~~html
	<div id="app">
	<!-- <child-component v-bind:propsdata="message"></child-component> -->
	<child-component v-on:show-log="printText"></child-component>
	</div>

	<script>
		Vue.component('child-component', {
			template: '<button v-on:click="showLog">show</button>',
			methods: {
				showLog: function() {
					this.$emit('show-log');
				}
			}
		})
	var app =	new Vue({
			el: "#app",
			data: {
				message: "Hello Vue! passed from Parent Component"
			},
			methods: {
				printText: function() {
					console.log("received an event");
				}
			}
		});
	</script>
~~~

이 코드는 `child-component`의 [show] 버튼을 클릭하여 이벤트를 발생시키고,  
발생한 이벤트로 상위 컴포넌트(여기서 루트 컴포넌트)의 `printText()` 메서드를 실행시키는 예제.

[show] 버튼을 클릭했을 때 처리되는 과정

1. [show] 버튼을 클릭하면 클릭 이벤트 `v-on:click="showLog"`에 따라 `showLog()` 메서드가 실행됨.
2. `showLog()` 메서드 안에서 `this.$emit('show-log')`가 실행되면서 `show-log` 이벤트가 발생.
3. `show-log` 이벤트는 `<child-component>`에서 정의한 `v-on:show-log`에 전달되고, `v-on:show-log`의  
대상 메서드인 최상위 컴포넌트의 메서드 `printText()`가 실행된다.
4. `printText()`는 received an event라는 콘솔 로그를 출력한다.


# 같은 레벨의 컴포넌트 간 통신
## 이벤트 버스 형식
~~~javascript
// 이벤트 버스를 위한 추가 인스턴스 1개 생성
var eventBus = new Vue();
~~~
1. 이벤트 버스를 구현하려면 애플리케이션 로직을 담는 인스턴스와는 별개로 새로운 인스턴스를 1개 더 생성하여,  
새 인스턴스를 이용하여 이벤트를 보내고 받는다.

~~~javascript
// 이벤트를 보내는 컴포넌트
methods : {
	메서드명: function() {
		eventBus.$emit('이벤트명', 데이터);
	}
}
~~~
2. 이벤트를 보내는 컴포넌트에서는 `.$emit('이벤트명', 데이터)`을 보낸다.

~~~javascript
// 이벤트를 받는 컴포넌트
methods : {
	created: function() {
		eventBus.$on('이벤트명', function(데이터) {

		});
	}
}
~~~
3. 이벤트를 받는 컴포넌트에서는 `.$on('이벤트명', function(데이터){})`를 구현한다.

---

### 예제
~~~html
	<div id="app">
		<child-component></child-component>
	</div>

	<script>
		var eventBus = new Vue();
		Vue.component('child-component', {
			template: '<div>하위 컴포넌트 영역입니다. <button v-on:click="showLog">show</button></div>',
			methods: {
				showLog: function() {
					eventBus.$emit('triggerEventBus', 100);
				}
			}
		});

		var app = new Vue({
			el: "#app",
			created: function() {
				eventBus.$on('triggerEventBus', function(value) {
					console.log("이벤트를 전달받음. 전달받은 값 : ", value);
				});
			}
		});
	</script>
~~~
위 코드는 등록한 하위 컴포넌트의 [show] 버튼을 클릭했을 때 이벤트 버스를 이용하여 상위 컴포넌트로 데이터를 전달하는 코드.

1. 먼저 이벤트 버스로 활용할 새 인스턴스 1개를 생성하고, `eventBus`라는 변수에 참조한다.
2. 이제 `eventBus` 변수로 새 인스턴스의 속성과 메서드에 접근할 수 있다.
3. 하위 컴포넌트에는 `template` 속성과 `methods` 속성을 정의한다.
4. `template` 속성에는 '하위 컴포넌트 영역입니다.' 라는 텍스트와 [show] 버튼을 추가한다.
5. `methods` 속성에는 `showLog()` 메서드를 정의하고, 메서드 안에는 `eventBus.$emit()`을 선언하여  
`triggerEventBus`라는 이벤트를 발생하는 로직을 추가한다. 이 이벤트는 발생할 때 수신하는 쪽에 인자 값으로  
100이라는 숫자를 함께 전달한다.
6. 상위 컴포넌트의 `created` 라이프 사이클 훅에 `eventBus.$on()`으로 이벤트를 받는 로직을 선언한다.  
발생한 이벤트 `triggerEventBus`를 수신할 때 앞에서 전달된 인자 값 100이 콘솔에 출력된다.


> 이벤트 버스를 활용하면 `props` 속성을 이용하지 않고도 원하는 컴포넌트 간에 직접적으로 데이터를  
전달할 수 있어 편리하지만, 컴포넌트가 많아지면 어디서 어디로 보냈는지 관리가 되지 않는 문제가 발생한다.  
이 문제를 해결하려면 *뷰엑스(Vuex)*라는 상태 관리 도구가 필요하다.