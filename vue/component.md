# 컴포넌트 등록하기
컴포넌트를 등록하는 방법은 전역과 지역의 두 가지가 있다.
* 지역 컴포넌트(Local Component)는 특정 인스턴트에서만, 특정 범위 내에서 사용할 수 있다.
* 전역 컴포넌트(Global Component)는 여러 인스턴스에서, 뷰로 접근 가능한 모든 범위에서 공통으로 사용할 수 있다.


## 전역 컴포넌트(Global Component) 등록
전역 컴포넌트는 뷰 라이브러리를 로딩하고 나면 접근 가능한 Vue 변수를 이용하여 등록.  
전역 컴포넌트를 모든 인스턴스에 등록하려면 아래처럼 Vue 생성자에서 `.component()`를 호출하여 수행한다.
~~~javascript
Vue.component('컴포넌트 이름', {
	// 컴포넌트 내용
});
~~~

* 전역 컴포넌트 등록 형식에는 컴포넌트 이름, 컴포넌트 내용이 있다.
* 컴포넌트 이름은 `template` 속성에서 사용할 HTML 사용자 정의 태그(custom tag) 이름을 의미한다.  
태그 이름 명명 규칙은 HTML Custom Tag Spec 에서 강제하는 '모두 소문자' 와 '케밥 기법(-)' 을 **따르지 않아도** 된다.

* 컴포넌트 태그가 실제 화면의 HTML 요소로 변환될 때 표시될 속성들을 컴포넌트 내용에 작성한다. (아래 코드 참조)
* 컴포넌트 내용에는 `template`, `data`, `methods` 등 인스턴스 옵션 속성을 정의할 수 있다.

~~~html
<!DOCTYPE html>
<html lang="ko">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Vue Component Registration</title>
	<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
	<div id="app">
		<button>컴포넌트 등록</button>
		<my-component></my-component>
	</div>

	<script>
		Vue.component('my-component', {
			template : '<div>전역 컴포넌트가 등록되었습니다.</div>'
		});
		new Vue({
			el : '#app'
		});
	</script>
</body>

</html>
~~~

## 지역 컴포넌트 등록
지역 컴포넌트 등록은 전역 컴포넌트 등록과 다르게 인스턴스에 components 속성을 추가하고 등록할 컴포넌트 이름과 내용을 정의.
~~~javascript
new Vue({
	components : {
		'컴포넌트 이름' : 컴포넌트 내용
	}
})
~~~
* 컴포넌트 이름은 전역 컴포넌트와 마찬가지로 HTML에 등록할 custom tag.
* 컴포넌트 내용은 컴포넌트 태그가 실제 화면 요소로 변환될 때의 내용을 의미.

~~~html
<script>
	var cmp = {
		template : '<div>지역 컴포넌트가 등록되었습니다.</div>'
	}
	new Vue({
		el : "#app",
		components: {
			'my-local-component': cmp
		}
	});
</script>
~~~
지역 컴포넌트를 등록하는 property 이름은 components(복수)라는 것을 유의하자.
