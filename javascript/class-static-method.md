# 클래스 정적 메소드와 속성 정의하기

## 클래스 정적 메소드(Static Method) 정의
### 정적 메소드란?
* 일반적인 메소드는 해당 클래스의 **인스턴스**를 통해 호출한다.
* 반면, 정적 메소드란 **클래스를 통해 직접 호출하는 메소드**를 말한다.
* 클래스에서 정적 메소드는 `static` 키워드를 사용하여 정의한다.
* 클래스로 상속하게 되면 **정적 메소드 또한 상속**하게 된다.  
(생성자 함수(Object Constructor)의 `prototype` 기반 상속과 다른 점)

~~~js
class Product {
	static build(name, price) {
		const id = Math.floor(Math.random() * 1000);
		return new Product(id, name, price);
	}
	static getTaxPrice(product) {
		return (product.price * 0.1) + product.price;
	}
	constructor(id, name, price) {
		this.id = id;
		this.name = name;
		this.price = price;
	}
}
~~~
1. `static` 키워드를 사용하여 `build()` 정적 메소드를 정의함.
2. `build` 정적 메소드를 정의할 때, 상수 id를 선언하고 그 안에 `Math.random()` 호출.  
`Math.random()` 함수는 0부터 1까지의 난수를 반환한다(예: 0.29707460276260234).
3. 반환된 값에 1000을 곱하고 그 결과를 `Math.floor` 함수의 인자로 전달하면 내림하여 소수점을 버린다.  
그러므로 0부터 1000 사이의 랜덤값을 얻을 수 있다.
4. `static build()`는 이 랜덤값을 id로 하는 상품 **인스턴스**(new Product)를 반환한다.

5. 세금을 계산하여 반환하는 `getTaxPrice()` 정적 메소드를 정의.
6. 상품 클래스의 생성자 함수(`constructor()`)를 정의.


~~~js
class DeposableProduct extends Product {
	depose() {
		this.deposed = true;
	}
}

const gum = Product.build('껌', 1000);
console.log(gum);

const clothes = new DeposableProduct(1, '옷', 2000);
const taxPrice = DeposableProduct.getTaxPrice(clothes);
console.log(taxPrice);
~~~

1. 폐기 가능한 상품 클래스를 정의 (`class DeposableProduct`).
2. `DeposableProduct` 클래스는 `Product` 클래스를 상속(`extends`).  
생성자 함수(Object Constructor)의 `prototype` 기반 상속과는 다르게 클래스로 상속하게 되면 **정적 메소드 또한 상속**하게 된다.
3. `Product` 클래스의 `build` 정적 메소드를 호출한다(여기가 인스턴스를 통해 호출하는 일반 메소드와 정적 메소드의 다른 점).  
랜덤하게 아이디가 부여된, 이름이 "껌"인 상품 인스턴스가 반환되고 콘솔에 인스턴스를 출력.
~~~console
Product {id: 709, name: "껌", price: 1000}
id: 709
name: "껌"
price: 1000
__proto__: Object
~~~
4. `DeposableProduct` 인스턴스를 생성.  
`DeposableProduct` 클래스에서 `getTaxPrice` 정적 메소드를 정의하지 않았지만,  
`Product` 클래스를 상속하였기 때문에 호출이 가능.

---

## 클래스 정적 속성(Static Property) 정의
* 클래스를 정의할 때 정적 속성 또한 `static` 키워드와 `get` 키워드를 통해 정의할 수 있다.

~~~js
class ProductWithCode {
	static get CODE_PREFIX() {
		return "PRODUCT-"
	}
	constructor(id) {
		this.id
		this.code = ProductWithCode.CODE_PREFIX + id;
	}
}

const product1 = new ProductWithCode('001');
console.log(ProductWithCode.CODE_PREFIX); //"PRODUCT-"
console.log(product1.code) //"PRODUCT-001"
~~~
1. `ProductWithCode` 클래스를 정의하면서 `codePrefix` 정적 속성을 정의.  
물론 클래스 몸통 블록 밖에서 `ProductWithCode.CODE_PREFIX = "PRODUCT-"`로 정의할 수 있다.  
그렇지만 코드의 가독성을 높이려고 몸통 안에서 정의했다고 함.  `static get` 키워드를 사용.
2. `ProductWithCode` 클래스의 생성자 함수(constructor())를 정의.
3. `ProductWithCode` 클래스의 인스턴스를 생성
4. 해당 인스턴스의 code값과 `ProductWithCode` 클래스의 `CODE_PREFIX` 정적 속성을 콘솔에 출력.

> 마지막에서 두 번째 줄에서 `ProductWithCode.CODE_PREFIX()` 라고 작성해봤는데 `get`은 속성인가봄.  
console에 Uncaught TypeError 가 뜨면서 'ProductWithCode.CODE_PREFIX is not a function'라고 친절히 알려줌.  
근데 어째서 `get`을 정의할 때 메소드처럼 `CODE_PREFIX()` <- 열고 닫기를 쓰는거야?

> 또 다른 물음, 정적 속성은 원래 대문자로 쓰는 것인가? (CODE_PREFIX)

> 문제는 왜 어째서 정적 메소드를 정의하고 사용하는지 1도 모르겠다는거야.

> 정적 속성은 왜 쓰는지 어떨때 쓰는지 이것도 1도 모르겠다는 거야