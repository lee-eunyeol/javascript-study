# 8장 함수

- 함수는 한 번 정의하면 몇 번이고 호출할 수 있는 자바스크립트 코드 블록이다
- 함수는 보통 인자를 사용해 반환 값을 도출하며, 이 값이 함수 호출 표현식의 값이 된다
- 매개변수 외에도 각 호출에는 호출 컨텍스트가 존재하며 이것이 this 키워드의 값이다
- 객체를 통해 함수를 호출하면 그 객체가 호출 컨텍스트, 즉 함수의 this 값이다
- 함수는 객체이므로 프로퍼티를 정의할 수 있고 함수의 메서드를 호출하는 것도 가능하다
- 자바스크립트 함수는 다른 함수 안에서 정의할 수 있으며, 이렇게 정의된 함수는 자신이 정의된 스코프의 변수에 접근할 수 있다
    - 이런 의미에서 자바스크립트의 함수는 클로저이다
    

## 8.1 함수 정의

### 8.1.1 함수 선언

```jsx
function 함수이름(콤마로 연결된 매개변수) {
	// 함수바디 : 0 개 이상의 자바스크립트 문
}

function factorial(x) {
	if(x <= 1) return 1
	return x * factorial(x-1)
}
```

- 함수 선언에서 이해해야 할 중요한 점은 함수의 이름이 변수이며 그 값은 함수 자체라는 점이다
- 함수 선언문은 자신을 포함하는 스크립트, 함수, 또는 블록 맨 위에 끌어올려지므로 함수 선언문으로 정의한 함수는 정의하기도 전에 호출할 수 있다
- return 문은 함수 실행을 멈추고 바로 다음에 있는 표현식의 값을 호출자에게 반환한다
- return 문 다음에 표현식이 없으면 함수의 반환값은 undefined 이다
- 함수에 return 문이 없으면 함수는 함수 바디의 문을 끝까지 실행하고 호출자에게 undefined를 반환한다
- ES5 이전에는 자바스크립트 파일이나 다른 함수의 최상위 레벨에서만 함수를 선언할 수 있었다
- 하지만 ES6 스트릭트 모드에서는 블록 안에서 함수를 선언할 수 있다
- 이렇게 블록 안에서 정의된 함수는 해당 블록 안에서만 존재하며 블록 바깥에서는 볼 수 없다

### 8.1.2 함수 표현식

```jsx
const square = function(x) {
	return x*x
}

// 함수 표현식에도 이름을 쓸 수 있으며 재귀 호출에 유용하다
const f = function fact(x) {
	if(x <= 1) return 1
	return x * factorial(x-1)
}

// 함수 표현식을 다른 함수의 인자로 사용할 수도 있다
[3,2,1].sort(function(a,b) {
	return a-b
})

// 함수 표현식을 정의하는 즉시 호출할 때도 있다
let tensquared = (function(x){return x*x}(10))
```

- 함수 표현식은 함수 선언과 거의 비슷하지만, 더 큰 표현식이나 문의 일부로서 존재하고 이름을 붙이지 않아도 된다는 점이 다르다
- 표현식으로 정의한 함수에 이름을 붙이는 것은 선택사항이다
- 함수 선언은 실제로 변수를 선언하며 그 변수에 함수 객체를 할당하지만, 함수 표현식은 변수를 선언하지 않는다
- 새로 정의한 함수 객체를 나중에 다시 참조해야 한다면, 선택에 따라 변수 또는 상수에 할당하면 된다
- 함수 표현식을 쓸 때는 실수로 덮어쓰지 않도록 const를 사용하는 것이 좋은 습관이다
- 함수 선언으로 함수 f()를 정의하는 것과 표현식으로 함수를 생성하고 변수 f에 할당하는 것 사이에는 중요한 차이가 있다
    - 선언 형태를 사용하면 함수 객체는 자신을 포함하는 코드가 실행되기 전에 존재하며 정의하기 전에 호출할 수 있다
    - 표현식으로 정의된 함수는 이렇게 동작하지 않고 실제로 평가되기 전에는 함수가 존재하지 않는다
- 표현식으로 정의된 함수는 변수에 할당하기 전에는 참조할 수 없다
- 따라서 표현식으로 정의된 함수는 정의하기 전에 호출할 수 없다

### 8.1.3 화살표 함수

```jsx
const sum = (x,y) => { return x + y }
const sum = (x,y) => x + y
```

- ES6 이후에는 화살표 함수라는 간결한 문법으로 함수를 정의할 수 있다
- 함수바디가 return 문 하나라면 return 키워드와 중괄호를 모두 생략하고 값을 반환하는 표현식 하나만으로 함수 바디를 구성할 수 있다

```jsx
const polynomial = x => x*x + 2*x + 3
const constantFunc = () => 42
```

- 매개변수가 정확히 하나라면 매개변수를 감싼 괄호도 생략할 수 있다
- 하지만 매개변수를 받지 않을 때는 반드시 빈 괄호를 써야 한다
- 화살표 함수를 작성할 때 함수 매개변수와 ⇒ 사이에서 줄바꿈을 해선 안된다

```jsx
const f = x => { return { value: x } } // f()는 객체를 반환함
const g = x => {{ value: x }} // g()는 객체를 반환함
const h = x => { value: x } // h()는 아무것도 반환하지 않음
const i = x => { v: x, w: x } // 문법 에러
```

- 화살표 함수바디가 return 문 하나라고 해도, 반환할 표현식이 객체 리터럴({})이라면 명시적으로 함수 바디의 중괄호와 함께 써서 명시적으로 해주어야 한다

```jsx
let filtered = [1,null,2,3].filter(x => x !== null) // [1,2,3]
let squares = [1,2,3,4].map(x => x*x) // [1,4,9,16]
```

- 화살표 함수는 문법이 간결하므로 함수를 다른 함수에 전달할때 이상적이다
- 다른 방법으로 정의된 함수는 자신만의 호출 컨텍스트를 정의하지만, 화살표 함수는 자신이 정의된 환경의 this 키워드 값을 상속한다는 결정적인 차이가 있다

### 8.1.4 중첩된 함수

- 자바스크립트에서는 다른 함수 안에 다른 함수를 중첩할 수 있다.
    
    ```tsx
    function hypotenuse(a, b) {
    		function square(x) { return x*x; } 
    		return Math.sqrt(square(a) + square(b) );
    }
    ```
    
    - 이렇게 중첩된 함수 `square` 는 자신을 포함하는 함수의 매개변수와 변수( `a, b` )에 접근할 수 있다.

---

## 8.2 함수 호출

<aside>
💡 자바스크립트 함수는 함수를 정의할 때가 아니라 호출할 때 실행된다.

</aside>

- 자바스크립트 함수를 호출하는 5가지 방법
    1. 함수로 호출
    2. 메서드로 호출
    3. 생성자로 호출
    4. call(), apply() 메서드를 통해 간접적으로 호출
    5. 자바스크립트 언어 기능을 통한 묵시적 호출

### 8.2.1 함수로 호출

- 함수는 호출 표현식을 통해 함수 또는 메서드로 호출된다.
    - 함수 표현식이 프로퍼티 접근표현식이라면 이 표현식은 메서드 호출 표현식이다.
        - 즉 해당함수가 객체 프로퍼티거나 배열요소라면 과 같은 의미.
    - 일반적인 함수 표현식 예.
        
        ```tsx
        printprops({x: 1});
        let total = distance(0, 0, 2, 1) + distance(2, 1, 3, 5);
        let probability = factorial(5)/factotial(13);
        ```
        

- 일반적인 함수 호출에서는 함수의 반환값(return 문)이 호출 표현식의 값이다.
    - return 없이 인터프리터가 함수의 끝에 도달하면 반환값은 undefined이다.
        - return 문이 있더라도, return문에 값이 없다면 반환값은 undefined이다.

- `일반 모드` 에서 함수의 호출 컨텍스트(this)는 전역객체이다.
- `스트릭트 모드` 에서 함수의 호출 컨텍스트(this)는 undefined이다.
    - 따라서 함수로 호출되도록 설계된 함수는 일반적으로 this 키워드를 사용하지 않는다.
    - 단, 화살표 문법으로 정의한 함수는 항상 자신의 정의된 곳의 this 값을 상속한다.

### 8.2.2 메서드로 호출

- 메서드는 객체 프로퍼티로 저장된 자바스크립트 함수이다.
    
    ```tsx
    //메서드 m 정의
    o.m = f;
    
    //메서드 호출
    o.m()
    ```
    
    - 메서드 호출의 인자와 반환 값은 일반적인 함수 호출과 똑같다.

<aside>
💡 메서드 호출과 함수 호출은 호출 컨텍스트가 다르다는 중요한 차이가 있다.

</aside>

- 위 예제에서, 객체 o 는 호출 컨텍스트가 되고, 함수 바디는 this를 통해 o를 참조할 수 있다.
    
    ```tsx
    let calculator= { //객체리터럴  
    	operand1: 1,
    	operand2: 1,
    	add() {  // 매서드 단축 문법올 썼습니다.
    		//this 키워드는 포함하는 객체톨 참조합니다.
    		this.result = this.operand1 + this.operand2; }
    };
    calculator.add(); // 메서드 호출올 통해 1 + 1을 계산힐니다. 
    calculator.result // => 2
    ```
    
- 메서드 호출은 대부분 점 표기법을 통해 프로퍼티에 접근하지만, 대괄호 표현식으로도 메서드를 호출할 수 있다.
    
    ```tsx
    o["m"](x,y);
    a[0](z)
    
    //좀 더 복잡한 메서드 호출
    customer.surname.toUpperCase();
    f().m(); //f()의 반환 값의 메서드인 m()을 호출
    ```
    

- 메서드와 this 키워드는 객체 지향 프로그래밍 패러다임의 핵심이다.
    - 메서드로 사용되는 함수는 모두 자신을 호출하는 객체를 묵시적 인자로 받는
    - 메서드 호출 문법은 함수가 객체에서 동작한다는 의미를 명쾌하게 전달한다.
        
        ```tsx
        //함수 호출
        setRectSize( rect, width, height);
        
        //메서드 호출 --> rect가 이 동작의 대상임을 더 명확하게 드러낸다.
        rect.setSize(width, height); 
        ```
        

- this 키워드는 변수의 스코프 규칙을 따르지 않는다.
    - 화살표 함수의 예외를 제외하면 중첩된 함수는 포함하는 함수의 this 값을 상속하지 않는다.]
    - 중첩된 함수를 메서드로 호출하면 this 값은 호출한 객체이다.
        - 중첩된 함수를 함수로 호출하면 this 값은 일반모드에선 전역객체, 스트릭트 모드에선 undefined이다.
    
    ```tsx
    let o = { //객체 o
    	m: function() { // 객체의 메서드 m
    		let self = this; // this 값을 변수 self에 저장
    		this === o // true: this 는 객체 o
    		f(); // 보조함수 f 호출
    		function f() { //중첩된 함수 f
    			this === o // => false: this는 전역객체이거나 undefined
    			self === o // => true: self는 외부 this 값
    		}
    	}
    }
    o.m()
    ```
    
    - 위 예제에서 중첩된 함수 f()에서 this키워드는 객체 o와 같지 않다.
        - 자바스크립트 언어의 결함으로 지적받는 부분
        - 우회 방법으로 self에 this를 할당하여 객체 o를 참조한다.
    - ES6 이후 부터는 중첩된 함수 f를 화살표 함수로 변환하여 this 값을 자동 상속하게 한다.
        
        ```tsx
        const f = () => {
        		this === o // true : 화살표 함수는 항상 this를 상속
        }
        ```
        

### 8.2.3 생성자로 호출

- 함수나 메서드를 호출할 때 앞에 키워드 new 를 붙이면 생성자로 호출된다.
    - 생성자 호출은 인자 처리, 호출 컨텍스트, 반환 값 들에서 일반적인 함수나 메서드 호출과 다르다.

- 생성자를 호출할때 괄호 안에 인자 리스트가 있으면 이 인자 표현식을 평가하여 함수나 메서드 호출과 같은 방법으로 함수에 전달한다.
    - 생성자 호출에서는 빈 괄호는 생략가능하다.
    
    ```tsx
    o = new Object();
    o = new Object;
    ```
    

- 생성자를 호출하면 생성자의 prototype 프로퍼티에서 지정된 객체를 상속하는 빈 객체를 새로 생성한다.
    - 생성자 함수는 객체를 초기화 할 의도로 만들어졌으며, 이렇게 **새로 생성된 객체가 호출 컨텍스트로 사용**되므로 생성자 함수는 새 객체를 this키워드로 참조할 수 있다.
    
- 생성자 함수는 일반적으로 return 키워드를 사용하지 않는다.
    - 일반적으로 새 객체를 초기화 하며 함수 바디에 끝에 도달하면 종료한다.
        - 이 경우 새 객체가 생성자 호출 표현식의 값이다.
    - 생성자가 명시적으로 return 키워드를 사용해 반환한다면, 그 객체가 호출 표현식의 값이 된다.
    

### 8.2.4 간접적 호출

- 자바스크립트 함수는 객체이며, 다른 자바스크립트 객체와 마찬가지로 메서드가 있다.
    - 이 메서드 중  call() 과 apply()는 함수를 간접적으로 호출한다.
        - 두 메서드 모두 호출 시점에 this 값을 직접 명시할 수 있으므로, 함수를 어떤 객체의 메서드로도 호출 할 수 있다.

### 8.2.5 묵시적 함수 호출

- 자바스크립트에는 함수 호출처럼 보이지 않지만 함수를 호출하는 기능이 여럿 존재한다.
    - 묵시적으로 호출한 함수에서 버그, 문제가 발생한다면 디버깅하기가 매우 어려움으로 일반적인 함수에 비해 해결하기가 아주 어렵다.

`묵시적인 함수 호출을 일으키는 언어기능`

- 객체에 게터와 세터가 있을 경우
    - 프로퍼티 값에 접근할때 자동으로 호출됨
- 문자열을 받는 컨텍스트에 객체를 사용했을경우
    - toString() 메서드가 자동으로 호출됨
    - 숫자는 valueOf() 메서드가 자동으로 호출됨

> 아래 호출은 이후 책의 장에서 설명한다고 한다.
> 
- 이터러블 객체의 요소를 순회할 때
- 태그된 템플릿 리터럴
- 프록시 객체

---

## 8.3 함수 매개변수

- 자바스크립트 함수는 매개변수로 어떤 타입을 받는지 정의하지 않으며, 함수 호출 시점에도 전달받은 값의 타입을 체크하지 않는다.
    - 자바스크립트는 함수를 호출할 때 전달받은 인자의 개수 조차 체크하지 않는다.
    
    > 하위 절에서는 함수가 선언된 매개변수보다 더 많거나 더 적은 인자를 받았을대 어떻게 동작하는지 설명한다.
    
    음 TS에서는 이미 트랜스파일 이전에 버그가 명시되니 자바스크립트의 함수동작원리로써 이해하는게 좋겠다.
    > 
    

### 8.3.1 선택 사항인 매개변수와 기본 값

- 선언된 매개변수보다 적은 인자로 함수를 호출하면, 대응하는 인자가 없는 매개변수는 기본 값 (undefined)으로 정해진다.
    - 함수를 만들때 일부 인자가 선택 사항이 되도록 하면 유용하다.
    
    ```tsx
    // 객체 o의 열거 가능한 프로퍼티를 배얼 a에 추가하고 a를 반환합니다. 
    // a를 생략하면 새 배열을 생성해 반환합니다.
    function getPropertyNames(o, a) {
    		if (a === undefined) a = [); // 정의되지 않았으면 새 배열을 사용합니다. 
    		for(let property in o) a.push(property);
    		return a ;
    }
    
    // getPropertyNames()는 인자 한 개나 두 개로 호출힐 수 있슐니다. 
    let o={x:1}, p={y:2,z:3}; //테스트용 객체
    let a = getPropertyNames(o); // a== [”x”] ; o의 프로퍼티를 새 배열에 담습니다. 
    getPropertyNames(p, a); // a== [”x”,”y",”z”]; p의 프로퍼티를 추가합니다.
    ```
    
    - 선택사항인 인자를 받는 함수를 만들 때는 선택사항인 인자를 인자 리스트 마지막에 써서 생략하기 쉽게 만들어야 한다.
        - 그렇지 않고 중간에 끼어 있다면 해당 선택사한 인자를 undefined라고 직접 명시해주어야 한다.

- ES6 이후에는 함수를 정의할 때 함수 매개변수의 기본값을 정의할 수 있다.
    
    ```tsx
    // 객체 o의 열거 가농한 프로퍼티를 배얼 a에 추가하고 a를 반환합니다. 
    // a를 생략하면 새 배열을 생성해 반환합니다.
    function getPropertyNames (o, a = []) {
    		for(let property in o) a.push(property);
    		return a; 
    }
    ```
    
    - 매개변수 기본 값 표현식을 함수를 정의할 때가 아니라 호출할 때 평가된다.
        - 따라서 getPropertyNames()함수를 인자 하나로 호출할 때마다 빈 배열을 새로 생성해서 전달한다.
    - 매개변수 여러개를 받는 함수에서 앞의 매개변수의 값을 사용해 그 다음 매개변수의 기본값을 정의할 수 있다.
        
        ```tsx
        // 이 함수는 사각형의 크기를 나타내는 객체를 반환합니다.
        // 너비가 제공됐을 때만 높이를 너비의 두 배로 정합니다.
        const rectangle = (width, height=width*2) => ({width, height});
        rectangle(1) // => {width: 1, height: 2 }
        ```
        

### 8.3.2 나머지 매개변수와 가변 길이 인자 리스트

- 매개 변수 기본값과는 반대로 나머지 매개변수를 사용하여 정의된 매개변수보다 더 많은 인자를 써서 함수를 호출할 수도 있다.
    
    ```tsx
    function max(first=-Infinity, ... rest) {
    		let maxValue = first; // 첫 번째 인자가 가장 크다고 가정합니다. 
    		// 나머지 인자를 술회하면서 더 큰 값을 찾습니다.
    		for(let n of rest) {
    				if (n > maxValue) {
    					 maxValue = n;
    				} 
    		}
    		// 가장 큰 값을 반한합니다. 
    		return maxValue;
    }
    max(1, 10, 100, 2, 3, 1000, 4, 5, 6) // => 1000
    ```
    
    - 나머지 매개변수를 앞에 점 세개를 붙이는데, 반드시 함수 선언에서 마지막으로 정의된 매개변수여야 한다.
        - 나머지 매개변수를 써서 함수를 호출하면 전달한 인자는 나머지가 아닌 매개변수에 먼저 할당된다.
            - 그리고 남은 나머지 인자가 배열에 저장되며, 이 배열의 매개변수의 값이 된다.

> 위 처럼 인자 개수에 제한이 없는 함수를 가변함수 라고 부른다.
> 

### 8.3.3 Argument 객체

- 나머지 매개변수는 ES6에서 자바스크립트에 도입되었고, ES6 전에는 Arguments 객체를 써서 가변함수를 만들었다.
    - 함수 바디 안에서 arguments는 해당 호출의 Arguments 객체를 참조한다.
        - Arguments 객체는 Arraylike한 객체이며, 함수에 전달된 인자 값을 숫자로 참조할 수 있게 한다.
        
        ```tsx
        function max(x) {
        		let maxValue = -Infinity;
        		//인자를 순회하며 가장 큰 값을 찾아 기억한다.
        		for(let i = 0; i < arguments.length; i++) {
        				if (arguments[i] > maxValue) maxValue = arguments[i];
        		}
        		//가장 큰 값을 반환
        		return maxValue;
        }
        max(1, 10, 100, 2, 3, 1000, 4, 5, 6) // => 1000
        ```
        
    
- Arguments 객체는 좀 이상하게 동작하고 비효율적이며, 최적화 하기 어려운 부분이 있어 웬만하면 arguments를 사용하는 함수를 …args 나머지 매개변수로 대체하여 사용하는 것이 좋다.
- stirct 모드에서는 arguments를 예약어로 사용하여 변수로 선언할 수 없다.

### 8.3.4 함수 호출과 분해 연산자

- 분해 연산자 `...` 는 개별 값이 예상되는 컨텍스트에서 배열이나 문자열같은 이터러블 객체를 분해한다.
    
    ```tsx
    let numbers = [5, 2, 10, -1, 9, 100, 1]; 
    Math.min(...numbers) // => -1
    ```
    
    `...` 는 평가를 통해 값을 얻을 수 없다는 점에서 진정한 연산자로는 볼 수 없다.
    
- 함수 정의에서 `...` 문법(나머지 매개변수) 은 분해 연산자의 정 반대로 동작한다
    - 나머지 매개변수는 여러개의 인자를 배열에 모은다
    - 분해 연산자는 객체를 분해한다.

- 나머지 매개변수와 분해연산자를 함께 쓰면 유용한 경우가 많다
    
    ```tsx
    // 이 함수는 함수를 받아 래퍼 버전올 반환합니다. 
    function timed(f) {
    		return function(...args) { // 인자를 나머지 매개변수 배열에 모읍니다. 
    				console.log(`Entering function ${f.name}`);
    				let startTime = Date.now();
    				try {
    						//인자를 모두 래퍼 버전에 전달합니다.
    						return f(...args)
    						}
    				finally {
    						//반환하기 전에 소요된 시간을 출현한다.
    						console.log(`Exiting ${f.name} after ${Date.now()-startTime}ms`};
    				}
    		}
    }
    //1과 n 사이의 숫자의 합을 계산
    function benchmark(n) {
    		let sum = 0;
    		for(let i = 1; i <= n; i++) sum += i;
    		return sum		
    }
    //테스트 함수의 래퍼 버전을 호출함
    timed(benchmark)(100000) // => 500000500000;
    ```
    

### 8.3.5 함수 인자를 매개변수로 분해

- 함수를 호출할 때 전달한 인자는 함수 정의 시 선언된 매개변수에 할당된다.
    - 이 작업은 변수 할당과 비슷하여, 함수에서도 분해 할당을 사용할 수 있다.
    
    ```tsx
    function vectorAdd([x1,y1], [x2,y2]){ //인자 두개를 매개변수 네 개로 분해한다.
    		return [x1 + x2, y1 + y2]
    }
    vectorAdd([1,2], [3,4])
    ```
    

- 마찬가지로 객체 인자를 받는 함수를 정희할때도 인자로 받은 객체를 매개변수로 분해할 수 있다.
    
    ```tsx
    // 스칼라 값율 벡터에 곱합니다.
    function vectorMultiply({x, y}, scalar) {
    		return { x: x*scalar, y: y*scalar }; 
    }
    vectorMultiply({x: 1, y: 2}, 2) // => {x: 2, y: 4}
    ```
    
- 객체의 프로퍼티를 다른 이름의 매개변수로 분해할때는 매개변수 이름과 프로퍼티 이름을 잘 구분해야 한다.
    
    ```tsx
    function vectorAdd(
    {x: x1, y: y1}, // 첫 번째 객체를 x1과 y1 매개변수로 분해합니다. 
    {x: x2, y: y2} // 두 번째 객체룰 x2와 y2 매개변수로 분해합니다.
    ){
    return { x: xl + x2, y: yl + y2 };
    }
    vectorAdd({x: 1, y: 2}, {x: 3, y: 4}) // => {x: 4, y: 6}
    ```
    
    - 프로퍼티 이름은 항상 콜론 왼쪽이고, 매개변수는 항상 오른쪽임을 기억하자.
    
- 배열을 분해하고 남은 값으로도 남은 값으로도 나머지 매개변수를 정의할 수 있다.
    - 배열에서 정의한 나머지 매개변수와 함수에서 정의한 나머지 매개변수는 다름으로 잘 구분하자
    
    ```tsx
    //이 함수는 배열인자를 받습니다. 
    //배열의 첫번째와 두번째요소는 x와 y매개변수에 할당됩니다. 
    //남는 요소는 모두 coords 배열에 저장됩니다. 첫 번째 배열을 제외한
    // 인자는 모두 rest 배열에 저장됨니다.
    function f([x, y, ...coords), ...rest) {
    		return [x+y, ...rest, ...coords]; // 분해 연산자를 썼습니다. 
    }
    f((1, 2, 3, 41, 5, 6)  //=> [3, 5, 6, 3, 4]
    ```
    
    - ES2018부터는 위와 같은 방식을 객체에서도 적용할 수 있다.
    

### 8.3.6 인자 타입

- 자바스크립트 메서드는 매개변수 타입을 선언하지 않으며 값을 전달할때도 타입을 체크하지 않는다.
    - 자바스크립트는 필요할 때마다 타입을 변환하기 때문
    - 타입 변환으로 많은 에러를 방지하긴 하지만 타입으로 인해 에러가 발생하는 경우는 당연히 있다.
        - 따라서 인자 타입을 체크하는 코드를 추가하는게 낫다.
    
    ```tsx
    // 이터러블 객체 a의 요소 합계를 반환합니다. 요소는 모두 반드시 숫자여야 합니다. 
    function sum(a) {
    		let total = 0;
    		for(let element of a) { // a가 이터러블이 아니면 TypeError.가 일어납니다.
    				if (typeof element !== ”number”) {
    				throw new TypeError("sum(): elements must be number");
    				}
    				total += element;
    		return total
    }함수
    ```
    
    > 타입스크립트의 필요성이 야기되는것 같다.
    > 
    

---

## 8.4 값인 함수

- 함수의 가장 중요한 특징은 정의하고 호출할 수 있다는 것이다.
    - 하지만 자바스크립트 함수는 문법 구조일 뿐만 아니라 값으로써 변수에 할당될 수도 있고,
    객체 프로퍼티나 배열 요소에 저장될 수도 있으며, 인자로 전달될 수도 있다.
        
        ```tsx
        function square(x) { return x * x }
        
        let s = square;
        square(4) // => 16
        square(4) // => 16
        
        //객체 프로퍼티에 할당
        let o = {square: function(x) { return x*x }}
        let y = o.square(16);
        
        //배열 요소에 할당
        let a = [x => x*x, 20] // 배열 리터럴
        a[0](a[1])
        ```
        
    - 이렇듯 함수를 값으로 취급할 수 있다는 것은 많은 유연함을 지닐 수 있게된다 ( ex) Array.sort)
    

### 8.4.1 함수 프로퍼티 직접 정의

- 자바스크립트 함수는 기본 값이 아니라 특별한 객체이다.
    - 따라서 함수 역시 프로퍼티를 가질 수 있다
        - 호출할 때마다 서로 다른, 고유한 정수를 반환하는 함수가 필요한 경우, 함수에서 이미 반환한 값을 추적하기위해 해당 값을 함수의 프로퍼티로 지정할 수 있다.
        
        ```tsx
        //함수 객체의 counter 프로퍼티를 초기화
        //함수 선언은 끌어올려지므로 함수 선언 이전에 할당해도 됨
        uniqueInteger.counter = 0;
        
        // 이 함수는 호출할 때마다 다른 정수를 반환
        // 자신의 프로퍼티를 사용해 어떤 값을 반환한지 판단
        function uniqueInteger() {
        		return uniqureInteger.counter++; // counter 프로퍼티를 반환하고 증가 시킴
        }
        
        uniqueInteger() // => 0
        uniqueInteger() // => 1
        ```
        

---

## 8.5 네임스페이스인 함수

- 함수 안에서 선언한 변수는 함수 바깥에서 보이지 않는다.
    - 따라서 전역 네임스페이스를 어지럽히지 않도록, 임시 네임스페이스 기능을 하는 함수를 정의하는 것이 유용할 때도 있다
    
    ```tsx
    function chunkNamespace() {
    // 코드가 여기 존채합니다. 코드에서 정의한 변수는 모두 합수의 로컬 변수이므로 
    // 전역 네임스떼이스를 어지럽히는 일은 없읍니다.
    }
    chunkNamespace(); // 단, 이 함수 호훌은 잊지 맡아야 합니다.
    
    //즉시 호출 함수 표현식 (IIFE)
    (function() { // chunkNameSpace() 함수를 익명외 표현어으로 고처 씁니다. 
    // 코드가 여기 존재합니다,
    }()); // 합수 리터럴을 종료하고 즉시 호출합니다.
    ```
    
    - 변수를 네임스페이스로 사용하는 방법은 네임스페이스 안에 있는 변수를 사용해 하나 이상의 함수를 정의하고, 정의된 함수를 네임스페이스 함수의 반환 값으로 사용할때 아주 유용하다
        
        이를 `클로저` 라고 한다.
        

---

## 8.6 클로저

- 대부분의 최신 프로그래밍 언어와 마찬가지로 자바스크립트 역시 `어휘적 스코프(lexical scope)`
    
    를 사용한다.
    
    - `어휘적 스코프` 란 함수가 호출 시점의 스코프가 아니라 자신이 정의된 시점의 변수 스코프를 사용하여 실행된다는 뜻이다.
    - 어휘적 스코프를 구현하기 위해서는 자바스크립트 함수 객체의 내부 상태에 함수의 코드뿐만 아니라, 함수가 정의된 스코프에 대한 참조도 반드시 포함 되어 있어야한다.
        - 이렇게 함수 객체와 스코프를 조합한 것을 클로저라 부른다.

- 엄밀히 말해 자바스크립트 함수는 모두 클로저지만, 대부분의 함수가 자신이 정의된 곳과 같은 스코프에서 호출되므로 보통은 클로저인지 아닌지 따질 필요가 없다.
    
    <aside>
    💡 클로저가 유용할 떄는 함수가 정의된 곳과 다른 스코프에서 호출 될때 뿐이다.
    
    </aside>
    

### 클로저

- 클로저를 이해하기 위해서는 중첩된 함수의 어휘적 스코프 규칙을 잘 살펴보아야 한다.

```tsx
let scope = “global scope”; //전역 변수
function checkscope() {
		let scope = ”local scope”;//로컬 변수
	  function f() { return scope; } // 이 스코프에 있는 값을 반환
		return f();
} checkscope() // => "local scope"
```

- checkscope() 함수는 로컬 변수를 선언하고, 그 변수의 값을 반환하는 함수를 정의해 호출 한다.
    
    > checkscope()가 “local scope”를 반환하는 이유를 명확히 이해해야 한다.
    > 
    
    ```tsx
    let scope = “global scope”; 
    function checkscope() {
    		let scope = ”local scope”; 
    		function f() { return scope; } 
    		return f ;
    }
    let s = checkscope()(); // => "local scope"
    ```
    
    - 위에서는 괄호 한쌍이 checkscope()() 로 내부에서 외부로 이동하였다.
        - 중첩된 함수 f()는 변수 scope가 “local scope”였던 스코프에서 정의되었기 때문에 f를 어디서 실행하든 상관없이 스코프가 유지된다.
            - 이것이 클로저의 강력함으로, 클로저는 자신을 정의한 외부함수의 로컬변수와 매개변수를 그대로 캡처한다.
            
- 클로저는 함수 호출 시점의 로컬 변수를 캡처하므로 이 변수를 비공개 상태로 사용할 수 있다.
    
    ```tsx
    let uniqueInteger = (function() { // 다음 함수의 비공개 상태를 
    		let counter = 0; // 정의하고 호출합니다. 
    		return function() { return counter++; };
    }());
    uniqueinteger() // => 0 
    uniqueinteger() // => 1
    ```
    
    - uniqueInteger 변수에 IIFE(즉시 함수 표현식) 을 통해 중첩된 함수(내부 함수)가 할당되었다.
    - 중첩된 함수는 자신의 스코프에 있는 변수에 접근할 수 있으므로 외부함수의 변수 counter에 접근할 수 있으며 사용도 가능하다.
    - 외부 함수가 종료되면 다른 코드에서는 counter 변수를 볼 수 없고 오직 중첩된 함수만 사용할 수 있게 된다.

- 클로저를 통해 객체를 생성할때 마다 매번 새 스코프가 생성된다.
    
    ```tsx
    function counter() {
    		let n = 0;
    		return {
    				count: function() { return n++;}
    				reset: function() { n = 0 ;}
    		}
    }
    
    let c = count(), d = count(); // 카운터 두개 생성
    c.count() // => 0
    d.count() // => 0 : c와 별도로 계산된다.
    c.reset();
    c.count() // => 0
    d.count() // => 1
    ```
    
    - c, d 모두 같은 counter 객체를 반환받았다.
        - 두 메서드가 같은 비공개 변수 n 에 접근하며, counter()를 호출할 때마다 이전 호출과 독립된 새 스코프를 생성한다
            - 따라서 counter()를 두번 호출하면 카운터 객체가 두개 생기며 이들의 비공개 변수 n은 각각 다른것이다.
            

> 클로저 기법을 통해 비공개 상태를 공유하는 방법을 일반화 할수도 있다.
> 

```tsx
// 이 함수는 지정된 이름의 프로퍼티에 대한 프로떠티 접근자 메서드를 객체 o에 추가합니다. 
// 메서드 이름은 get<name>과 set<name>으로 지정됩니다. 판별 함수가 제공됐다면
// 셰터 메서드는 인자톨 저장하기 전에 판별 함수틀 사용해 유효성올 테스트합니다.
// 판별 함수가 false률 반환힌다면 셰터 메서드는 예외를 일으킵니다.

// 이 함수의 독특한 점은 게터와 세터 메서드가 조작하는 프로퍼티 값이
// 객체 o에 저장되지 않는다는 점입니다. 값은 이 함수의 로컬 변수에만 저장됩니다. 
// 게터와 셰터 메서드는 합수에 로컬로 정의됐으므로 로컬 번수에 접근힐 수 있습니다.
// 따라서 값은 두 접근자 에서만 사용힐 수 있으며, 세터 메서드들 통하지 않고서는 
// 값을 수정하거나 저장할 수 없습니다.

function addPrivateProperty(o, name, predicate) { 
		let value; // 프로퍼티 값입니다.
		// 게터 메서드는 단순히 그 값을 반환합니다.
		o[`get${name}`] = function() { return value; };
		// 세터 메서드는 판별 합수의 판단에 따라 값을 저장하거나 예외를 일으킵니다. 
		o[`set${name}`] = function(v) {
		if (predicate&& !predicate(v)) {
				throw new TypeError(`set${name}: invalid value ${v}`);
		} else {
				value = v;
		};
}

// 다융 코드는 addPrivateProperty() 메서드의 사용 방법 예시입니다. 
let o={}; //빈객체입니다.

// 프로퍼티 접근자 메서드 getName()과 setName()을 추가합니다. 오직 문자열 값만 허용합니다. 
addPrivateProperty(o, ”Name”, x => typeof x === "string“);

o.setName("Frank"); // 프로퍼티 값을 저장합니다.
o.getName() // => ”Frank”
o.setName(0); // TypeError: 올바르지 않은 타입을 사용했습니다.
```

- 클로저를 생성할 때 클로저가 접근을 공유하면 안되는 변수에 대한 접근까지 부주의하게 공유하지 않도록 조심해야 한다.
    
    ```tsx
    // 이 함수는 항상v를 반환하는 함수를 반환합니다. 
    function constfunc(v) { return () => v; }
    // 정적 함수 배열을 생성합니다.
    let funcs = [];
    for(var i = 0; i < 10; i++) funcs[i] = constfunc(i);
    // 인덱스 5의 함수는 5를 반환합니다. 
    funcs [5]() // => 5
    ```
    
    - 위 처럼 루프에서 클로저를 여러개 생성하는 함수를 작성할때, 루프를 클로저를 정의하는 함수 안으로 옮기는 실수를 하곤 한다.
        
        ```tsx
        // 0부터 9까지의 값을 반환하는 함수 배열을 반환
        function constfuncs() {
        		let funcs = [];
        		for(var i = 0; i < 10; i++) {
        				funcs[i] = () => i; 
        		}
        		return funcs;
        let funcs = constfuncs();
        funcs[5]() // => 10; 왜 5가 아닐까요?
        ```
        
        - 위 코드처럼 클로저 10개를 생성하지만, 외부 변수 i를 모든 클로저가 공유하게 되어 반환된 배열 안의 클로저들이 모두 같은 10을 반환하게 되는 결과를 낳으며, 의도와 전혀 다른 결과를 낳았다.
        
        > 위 문제는 변수를 var로 선언했기 때문이다. ⇒ 변수 i는 루프 바디 안에만 존재 하는것이 아닌 함수 전체에 존재 하게 되어 클로저들이 이 변수를 공유하게 된것이다.
        
        ES6에서 블록스코프를 도입하고, let, const 가 생김에 따라 var i를 let 또는 const로만 변경하게 되면 매 반복마다 독립적인 스코프를 생성하여 각 스코프는 각자의 i 를 참조하여 해결할 수 있다.
        > 
        

---

## 8.7 함수 프로퍼티, 메서드, 생성자

- 함수는 특별한 종류의 자바스크립트 객체이다
- 함수는 객체이므로 다른 객체와 마찬가지로 프로퍼티와 메서드를 가질 수 있다
- 새 함수 객체를 생성하는 Function() 생성자도 있다

### 8.7.1 length 프로퍼티

- 읽기 전용이며 함수의 정의된 매개변수의 개수이며 보통 함수가 예상하는 인자 개수이다
- 함수에 나머지 매개변수가 있다면 그 매개변수는 이 length 프로퍼티에 포함되지 않는다

### 8.7.2 name 프로퍼티

- 읽기 전용이며 함수가 정의될 때 이름이 있었다면 그 이름, 익명의 함수 표현식이라면 처음 생성됐을 때 할당된 변수나 프로퍼티 이름이다
- 디버깅이나 에러 메세지에 유용하게 사용된다

### 8.7.3 prototype 프로퍼티

- 화살표 함수를 제외하면 모든 함수에는 프로토타입 객체를 참조하는 prototype 프로퍼티가 있다
- 함수의 프로토타입 객체는 모두 다르다
- 함수를 생성자로 사용하면 새로 생성된 객체는 프로토타입 객체에서 프로퍼티를 상속한다

### 8.7.4 call()과 apply() 메서드

- call() 과 apply()는 함수를 마치 다른 객체의 메서드인 것처럼 간접적으로 호출한다
- 첫 번째 인자는 함수를 호출할 객체이다
    - 이 인자가 호출 컨텍스트이며 함수 바디 안에서 this 키워드의 값이다
- 함수 f() 를 인자 없이 객체 o 의 메서드로 호출할 때는 call() 과 apply() 중 아무거나 써도 된다
    
    ```jsx
    f.call(o)
    f.apply(o)
    o.m = f
    o.m()
    ```
    
- 화살표 함수는 자신의 정의된 컨텍스트의 this 값을 상속하는데 이 값은 call()과 apply() 메서드로 덮어쓸 수 없다
- 화살표 함수에서 이들 메서드를 호출하면 첫 번째 인자는 무시되는 것이나 마찬가지이다
- call() 을 사용할때 두번째 인자는 호출될 함수에 전달된다
    - 이 인자는 화살표 함수에서도 무시되지 않는다
- apply() 메서드도  call() 메서드와 비슷하지만 함수에 전달할 인자가 배열로 제공된다는 점이 다르다
    
    ```jsx
    f.call(o,1,2)
    f.apply(o,[1,2])
    ```
    
- 함수가 받는 인자 개수에 제한이 없다면 apply() 메서드를 써서 임의의 길이를 가진 배열을 전달해 함수를 호출할 수 있다
    
    ```jsx
    let biggest = Math.max.apply(Math, arrayOfNumbers)
    ```
    

### 8.7.5 bind() 메서드

- bind() 의 주요 목적은 함수를 객체에 결합하는 것이다
- 함수 f에서 bind() 메서드를 호출하면서 객체 o를 인자로 전달하면 호출 컨텍스트가 o 로 결합된 새 함수를 반환한다
- 새 함수에 전달한 인자는 모두 원래 함수에 전달된다

```jsx
function f(y) {return this.x + y}
let o = {x:1}
let g = f.bind(o)
g(2) // 3
let p = {x:10, g}
p.g(2) // 3 => g는 여전히 o에 결합되어 있다
```

- 화살표 함수는 자신의 정의된 환경의 this 값을 상속하는데 이 값은 bind() 에서 덮어쓸 수 없으므로, 함수 f() 를 화살표 함수로 정의했다면 결합은 이루어지지 않는다
- bind() 메서드는 단순히 함수를 객체에 결합하는 것으로 끝나지 않고 bind() 에 전달하는 인자 중 첫 번째를 제외한 나머지는 this 값과 함께 결합된다
- 이러한 부분 적용은 화살표 함수에도 동작한다
    - 부분 적용은 함수형 프로그래밍에서 널리 쓰이는 기법이며 커링(currying) 이라고 부르기도 한다

```jsx
let sum = (x,y) => x+y
let succ = sum.bind(null, 1) // 1이 x에 결합됨
succ(2) // 3

function f(y,z) {
	return this.x + y + z
}
let g = f.bind({x:1}, 2) // this 와 y 를 결합한다
g(3) // 6
```

- bind() 가 반환하는 함수의 name 프로퍼티는 bind() 를 호출한 함수의 name 앞에 “bound” 를 붙인 값이다

### 8.7.6 toString() 메서드

- 다른 자바스크립트 객체와 마찬가지로 함수에도 toString() 메서드가 있다
- ECMAScript 명세는 이 메서드가 함수 선언문의 문법을 지키는 문자열을 반환해야 한다고 규정하지만 대부분 환경에서 함수의 toString() 메서드는 소스 코드 전체를 반환한다
- 내장 함수는 일반적으로 “[native code]” 같은 문자열을 함수 바디로 반환한다

### 8.7.7 Function() 생성자

- 함수는 객체이므로 Function() 생성자를 사용해 새 함수를 생성할 수 있다
    
    ```jsx
    const f = new Function("x", "y", "return x*y")
    // 위 코드가 생성한 함수는 다음과 같이 익숙한 문법으로 정의한 함수와 거의 동등하다
    const f = function(x,y) {return x*y}
    ```
    
- Function() 생성자는 문자열 인자를 개수 제한 없이 받는다
- 마지막 인자는 함수 바디인 텍스트이다
    - 이 텍스트에는 자바스크립트 문을 제한 없이 넣을 수 있으며 각 문은 세미콜론으로 구분한다
- Function() 생성자에 전달하는 인자 중 함수 이름에 해당하는 것은 없다
    - 함수 리터럴과 마찬가지로 Function() 생성자 역시 익명 함수를 생성한다
- Function() 생성자는 런타임에 자바스크립트 함수를 동적으로 생성하고 컴파일 할수 있다
- Function() 생성자는 호출될때마다 함수 바디를 분석하고 새 함수 객체를 생성한다
    - 루프나 자주 호출되는 함수 안에서 생성자를 호출하는 것은 효율적이지 않다
    - 중첩된 함수와 함수 표현식은 루프 안에 있더라도 매번 재컴파일 되지 않는다
- Function() 생성자에서 아주 중요한 포인트는 생성자가 만드는 함수는 어휘적 스코프를 사용하지 않는다
    - 생성자가 만드는 함수는 항상 최상위 함수로 컴파일 된다
    
    ```jsx
    let scope = "global"
    function constructFunction() {
    	let scope = "local"
    	return new Function("return scope"); // 로컬 스코프는 캡쳐하지 않는다
    }
    //Function() 생성자가 반환하는 함수는 로컬 스코프를 사용하지 않는다
    constructFunction()() // global
    ```
    

---

## 8.8 함수형 프로그래밍

- 자바스크립트는 함수형 프로그래밍 언어는 아니지만, 함수를 객체처럼 조작할 수 있으므로 함수형 프로그래밍 기법을 사용할 수 있다
- map() 과 reduce() 같은 배열 메서드는 특히 함수형 프로그래밍 스타일에 알맞다

### 8.8.1 함수로 배열 처리

- 숫자로 이루어진 배열이 있고 이 값들의 평균과 표준 편차를 구하려 한다

```jsx
// 함수형 프로그래밍 스타일을 사용하지 않을때
let data = [1,1,3,5,5]

// 평균 구하기
let total = 0
for(let i=0; i<data.length; i++) total += data[i]
let mean = total/data.length // mean === 3, 평균은 3이다

// 표준 편차 구하기
total = 0
for(let i=0l i<data.length; i++) {
	let deviation = data[i] - mean
	total += deviation * deviation
}
let stddev = Math.sqrt(total/(data.length-1)) //stddev === 2
```

- 같은 계산을 배열 메서드 map() 과 reduce() 를 사용해 간결한 함수형 스타일로 바꿀 수 있다

```jsx
const sum = (x,y) => x+y
const square = x => x*x

// sum 과 square를 사용해 평균과 표준 편차를 계산한다
let data = [1,1,3,5,5]
let mean = data.reduce(sum)
let deviations = data.map(x => x - mean)
let stddev = Math.sqrt(deviations.map(square).reduce(sum)/(data.length-1))
stddev // 2
```

- 새로 만든 코드는 앞의 코드와 사뭇 다르지만 여전히 객체 메서드를 호출하고 있으므로 객체 지향 스타일도 남아있다
- map() 과 reduce() 메서드의 함수형 버전도 만들어 보자

```jsx
const map = function(a, ...args) {return a.map(...args)}
const reduce = function(a, ...args) {return a.reduce(...args)}

const sum = (x,y) => x+y
const square = x => x*x

let data = [1,1,3,5,5]
let mean = reduce(data, sum)/data.length
let deviations = map(data, x => x-mean)
let stddev = Math.sqrt(reduce(map(deviations,square), sum)/(data.length-1))
stddev // 2
```

### 8.8.2 고계 함수

- 고계 한수(higher-older function) 는 하나 이상의 함수를 인자로 받아 새 함수를 반환하는 함수이다

```jsx
function not(f) {
	return function(...args) { // 새 함수를 반환
		let result = f.apply(this, args) // 이 함수는 f를 호출하고
		return !result // 그 결과를 부정한다
	}
}

const even = x => x % 2 === 0
const odd = not(even)
[1,1,3,5,5].every(odd) // true
```

- not() 함수는 함수 인자를 받고 새 함수를 반환하는 고계 함수이다.
- 다음 예제로는 mapper() 함수가 있는데 이 함수는 함수 인자를 받고 그 함수를 사용해 배열을 다른 배열로 변환하는 새 함수를 반환한다
- 이 함수는 앞에서 정의한 map() 함수를 사용하며, 두 함수가 어떻게 다른지 이해하는 것이 중요하다

```jsx
const map = function(a, ...args) {return a.map(...args)}

function mapper(f) {
	return a => map(a,f)
}

const increment = x => x+1
const incrementAll = mapper(increment)
incrementAll([1,2,3])
```

- 다음은 f와 g 두 함수를 받고 f(g()) 를 계산하는 새 함수를 반환한다

```jsx
// f(g(...)) 를 계산하는 새 함수를 반환한다.
// 반환되는 함수 h는 인자 전체를 g에 전달하고, g의 반환 값을 f에 전달한 다음 f의 반환 값을 반환한다
function compose(f,g) {
	return function(..args) {
		return f.call(this, g.apply(this, args))
	}
}
const sum = (x,y) => x+y
const square = x => x*x
compose(square, sum)(2,3) // 25
```

### 8.8.3 함수의 부분 적용

- bind() 메서드는 왼쪽에 있는 인자를 부분적으로 적용한다
    - bind() 에 전달하는 인자는 원래 함수에 전달되는 인자 리스트의 시작 부분에 위치한다는 뜻이다
    - 반대로 오른쪽에 있는 인자를 부분적으로 적용하는 것도 가능하다

```jsx
// 이 함수의 인자는 왼쪽에 전달된다
function partialLeft(f, ...outerArgs) {
	return function(...innerArgs) {
		let args = [...outerArgs, ...innerArgs]
		return f.apply(this, args)
	}
}
// 이 함수의 인자는 오른쪽에 전달된다
function partialRight(f, ...outerArgs) {
	return function(...innerArgs) {
		let args = [...innerArgs, ...outerArgs]
		return f.apply(this, args)
	}
}

function partial(f, ...outerArgs){
	return function(...innerArgs) {
		let args = [...outerArgs]
		let innerIndex = 0
		// 인자를 순회하며 정의되지 않은 값을 내부 인자로 채운다
		for(let i=0; i<args.length;i++) {
			if(args[i] === undefined) args[i] = innerArgs[innerIndex++]
		}
		// 남은 내부 인자를 이어 붙인다
		args.push(...innerArgs.slice(innerIndex))
		return f.apply(this,args)
	}	
}
// 인자 세 개를 받는 함수
const f = function(x,y,z) { return x * (y-z) }}
// 세 가지 부분 적용이 어떻게 다르게 동작하는지 보자
partialLeft(f,2)(3,4) // -2, 첫번째 인자에 결합 2 * (3-4)
partialRight(f,2)(3,4) // 6, 마지막 인자에 결합 3 * (4-2)
partial(f,undefined,2)(3,4) // -6, 중간 인자에 결합 3 * (2-4)
```

- 부분 적용 함수를 사용하면 이미 정의한 함수를 사용해서 더 흥미로운 함수를 쉽게 정의할 수 있다

```jsx
const sum = (x,y) => x+y
const increment = partialLeft(sum,1)
const cuberoot = partialRight(Math.pow, 1/3)
cuberoot(increment(26)) // 3
```

- 부분 적용과 고계 함수를 조합하면 더 흥미롭다

```jsx
function compose(f,g) {
  return function(...args) {
    return f.call(this, g.apply(this, args))
  }
}

const not = partialLeft(compose, x => !x)
const even = x => x%2 === 0
const odd = not(even)
const isNumber = not(isNaN)
odd(3) && isNumber(2) // true
```

- 합성과 부분 적용을 사용해 평균과 표준 편차를 완전한 함수형 스타일로 개선할 수도 있다

```jsx
const sum = (x,y) => x + y
const square = x => x*x 
const map = function(a, ...args) {
  return a.map(...args)
}
const reduce = function(a, ...args) {
  return a.reduce(...args)
}
const product = (x,y) => x*y
const neg = partial(product, -1)
const sqrt = partial(Math.pow, undefined, .5)
const reciprocal = partial(Math.pow, undefined, neg(1))

//이제 평균과 표준 편차를 계산하다
let data = [1,1,3,5,5]
let mean = product(reduce(data, sum), reciprocal(data.length))
let stddev = sqrt(product(reduce(map(data,
  compose( square,
  partial(sum, neg(mean)))),
  sum),
  reciprocal(sum(data.length,neg(l)))));
[mean, stddev] // [3,2]
```

- 이 코드는 평균과 표준 편차를 계산하면서 오로지 함수 호출에만 의존했다
- 자바스크립트 코드가 얼마나 깊이 함수형 스타일을 따를 수 있는지 확인하는 연습 문제 정도로만 보면 좋다

### 8.8.4 메모제이션

- 8.4.1 절에서 이전에 계산한 결과를 캐시하는 팩토리얼 함수를 만들었다
- 함수형 프로그래밍에서는 이런 캐싱을 메모제이션이라고 부른다

```jsx
function memoize(f) {
	const cache = new Map()

	return function(...args) {
		let key = args.length + args.join("+")
		if(cache.has(key)) return cache.get(key)
		else {
			let result = f.apply(this, args)
			cache.set(key, result)
			return result
		}
	}
}
```

- memoize() 함수는 캐시로 사용할 새 객채를 생성하고 이 객체를 로컬 변수에 할당하므로, 반환된 함수 외에는 이 객체를 볼수 없다
- 반환된 함수는 인자 배열을 문자열로 변환하고 그 문자열을 캐시 객체의 프로퍼티 이름으로 사용한다
- 값이 캐시에 존재하면 바로 반환하고 그렇지 않다면 인자를 넘기면서 지정된 함수를 호출해 값을 계산하고 캐시에 저장한 다음 반환한다

```jsx
// 유클리드 알고리즘을 사용해 두 정수의 최대 공약수를 계산해 반환한다
function gcd(a,b) {
	if(a<b) [a,b] = [b,a]
	while(b !== 0) [a,b] = [b, a%b]
	return a
}

const gcdmemo = memoize(gcd)
gcdmemo(85, 187) // 17

//재귀 함수로 고쳐 쓴다면 원래 함수가 아니라 캐시를 활용하도록 고쳐 쓴 함수를 재귀적으로 호출한다
const factorial = memoize(function(n) {
	return (n <= 1) ? 1 : n * factorial(n-1)
})

factorial(5) // 120
```

---

## 8.9 요약

- function 키워드나 ES6 화살표 문법으로 함수를 정의할 수 있다
- 함수는 메서드나 생성자로도 사용할 수 있다
- ES6 기능 중에는 선택사항인 함수 매개변수에 기본값을 할당하는 기능, 나머지 매개 변수를 사용해 인자 여럿을 배열에 모으는 기능, 객체와 배열을 분해해 함수 매개변수로 사용하는 기능 등이 있다
- 분해 연산자 … 를 사용해 배열이나 이터러블 객체의 요소를 함수 인자로 전달해 호출할 수 있다
- 외부 함수 안에서 정의되고 반환된 함수는 외부 함수의 어휘적 스코프에 대한 접근을 유지하고 있으므로, 외부 함수에서 정의한 변수에 접근할 수 있다
- 함수는 자바스크립트에서 조작할 수 있는 객체이며, 이를 통해 함수형 프로그래밍 스타일을 사용할 수 있다
