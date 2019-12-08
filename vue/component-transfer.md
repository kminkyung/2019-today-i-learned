# 상위에서 하위 컴포넌트로 데이터 전달하기

## props 속성
props 는 상위 컴포넌트 -> 하위 컴포넌트로 데이터를 전달할 때 사용하는 속성.  
props 속성을 사용하려면 먼저 아래처럼 하위 컴포넌트의 속성에 정의.
~~~javascript
Vue.component('child-component', {
	props: ['props 속성 이름']
});
~~~