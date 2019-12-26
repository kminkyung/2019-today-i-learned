# 클래스 정적 메소드와 속성 정의하기
* 일반적인 메소드는 해당 클래스의 인스턴스를 통해 호출한다.
* 반면, 정적 메소드란 **클래스를 통해 직접 호출하는 메소드**를 말한다.
* 클래스에서 정적 메소드는 `static` 키워드를 사용하여 정의한다.

~~~javascript
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
`Math.random()` 함수는 0부터 1까지의 난수를 반환한다.
3. 반환된 값에 1000을 곱하고 그 결과를 `Math.floor` 함수의 인자로 전달하면 내림하여 소수점을 버린다.  
그러므로 0부터 1000 사이의 랜덤값을 얻을 수 있다.
4. `static build()`는 이 랜덤값을 id로 하는 상품 **인스턴스**(new Product)를 반환한다.

5. 세금을 계산하여 반환하는 `getTaxPrice()` 정적 메소드를 정의.
6. 상품 클래스의 생성자 함수(`constructor()`)를 정의.


~~~javascript
class DeposableProduct extends Product {
	depose() {
		this.deposed = true;
	}
}

const gum = Product.build('껌', 1000);
console.log(gum);

const clothes = new DeposableProduct(1, '옷', 2000);
const taxPrice = DeposableProduct.getTaxtPrice(clothes);
console.log(taxPrice);
~~~

1. 폐기 가능한 상품 클래스를 정의 (`class DeposableProduct`).
2. `DeposableProduct` 클래스는 `Product` 클래스를 상속(`extends`).  
생성자 함수(Object Constructor)의 `prototype` 기반 상속과는 다르게 클래스로 상속하게 되면 **정적 메소드 또한 상속**하게 된다.
