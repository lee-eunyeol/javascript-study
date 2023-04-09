# 6장 객체(2)

## 6.7 객체 확장

- 자바스크립트 프로그램에서 객체의 프로퍼티를 다른 객체에 복사하는 것은 흔한 일이다.
    
    ```tsx
    let tartget = { x : 1 }, source = { y: 2, z: 3};
    for(let key of Object.keys(source)) {
    	target[key] = source[key];	
    }
    target // => {x: 1, y: 2, z: 3}
    ```
    
    - 이는 자주이루어지는 일이므로 여러 자바스크립트 프레임워크에서 이런 동작을 수행하는 유틸리티 함수를 정의하고 그 이름은 대부분 extend()이다
        - ES6에서 이런 기능은 Object.assign() 이라는 이름으로 도입했다.
    - 프로퍼티를 객체에서 다른객체로 복사하는 이유 중 하나는 소스객체에 기본값을 정의해 두고, 대상 객체에 그런 이름이 존재하지 않는다면 복사해서 쓰려는 목적이다.
        
        ```tsx
        o = Object.assign({}, defaults, o);
        ```
        

---

## 6.8 객체 직렬화

- 객체 직렬화는 객체를 문자열로 변환하는 작업이다.
    - 이 문자열은 나중에 다시 객체로 되돌릴 수 있다.
        - JSON.stringfy()와 JSON.parse()는 자바스크립트 객체를 직력화하고 되돌리는 함수이다.
            - 이 함수는 데이터 교환 형식인 JSON을 쓰는것이다.
            - JSON은 자바스크립트 객체 표기법의 약어이며, 문법은 자바스크립트의 객체와 배열 리터럴과 아주 비슷하다.
            
- JSON 문법은 자바스크립트 문법의 부분집합이며, 자바스크립트 값 전체를 표현하지는 못한다.
    - 표현가능한 값
        - `객체, 배열, 문자열, 유한한 숫자, true, false, null`
        - `NaN, Infinity, -Infinity` → 이는 null로 직렬화된다.
    - 표현 불가능한 값
        - `함수, RegExp, Error객체, undefined` 는 직렬화 하거나 복원할 수 없다.

---

## 6.9 객체 메서드

- 프로토타입 없이 생성한 객체를 제외하면 자바스크립트 객체는 모두 Object.prototype의 프로퍼티를 상속한다.
    - 이렇게 상속되는 프로퍼티는 대부분 메서드이며, 어디서든 사용할 수 있어 자바스크립트 프로그래머들이 특히 관심을 둔다.

### 6.9.1 toString()

- toString() 메서드에는 인자가 없다.
    - 이 메서드는 호출한 객체의 값을 나타내는 문자열을 반환한다.
        - 자바스크립트는 객체를 문자열로 변환해야 할 때마다 이 메서드를 호출한다.
        - ex) + 연산자로 문자열과 객체를 병합, 문자열을 예상하는 함수에 객체를 전달하는 상황
        
- 기본 toString()메서드는 별로 도움이 되진 않는다.
    
    ```tsx
    let s = { x: 1, y: 1}.toString(); // s == "[object Object]"
    ```
    
    - 따라서 여러 클래스에서 자신만의 toString()을 정의하곤 한다.
        - 배열을 문자열로 변환하면 배열 요소 각각을 문자열로 변환한 리스트를 얻는다
        - 함수를 문자열로 변환하면 소스코드를 얻게 한다.
        
        ```tsx
        let point = {
        		x: 1,
        		y: 2,
        		toString: function() { return  `(${this.x}, ${this.y})`}; }
        String(point) // => "(1, 2)": 문자열로 변환할 때 toString()을 호출
        ```
        
    

### 6.9.2 toLocaleString() 메서드

- 모든 객체에는 기본인 toString() 메서드 외에 toLocaleString() 메서드도 있다
    - 이 메서드의 목적은 지역에 맞는 문자열 표현을 반환하는 것이다.
    
    > 하지만 Object에 정희된 기본 toLocaleString()메서드 자체에는 지역화 기능이 전혀 없고, 그저 toString()을 오출해 그 값을 반환하기만 한다.
    > 
    - Date와 숫자 클래스에는 숫자, 날짜, 시간을 지역의 관습에 맞게 표현하는 toLocaleString()이 있다.

- 배열의 toLocalString() 메서드는 toString()과 거의 비슷하지만 **배열요소를 변환할때 toString()메서드가 아니라 toLocaleString() 메서드를 호출한다**는 점이 다르다.
    
    ```tsx
    let point = { 
    		x: 1000, 
    		y: 2000,
    		toString: function() { return `(${this.x}, ${this.y})`; }, 
    		toLocaleString: function() {
    		return `(${this.x.tolocaleString()}, ${this.y.tolocaleString()})`;
    };
    point.toString() // => "(1000, 2000)"
    point.toLocaleString() // => ”(1,000, 2,000)”: 천 단위 구분자가 있습니다.
    ```
    

### 6.9.3 valueOf() 메서드

- valueOf() 메서드는 toString() 메서드와 비슷하지만, 객체를 문자열이 아닌 다른 기본타입 (보통은 숫자)로 변환하려 할때 호출한다.
    - 자바스크립트는 기본값을 예상하는 곳에 객체를 사용하면 자동으로 이 메서드를 호출한다.
        - Date 클래스의 valueOf()는 날짜를 숫자로 변환하므로, Date 객체는  `<` 와 `>` 를 사용하여 시간 순서로 비교할 수 있다.
    
    > 아까 point 객체로 비슷하게 valueOf()를 통해 대소비교를 할 수 있도록 만들 수 있다.
    > 
    
    ```tsx
    let point = {
    		x: 3, 
    		y: 4,
    		valuOf: function() { return Math.hypot(this.x, this.y); } 
    };
    Number(point) // => 5: 숫자로 변환할 때 valueOf()가 호출됨
    point > 4 // => true
    point > 5 // => false
    point < 6 // => true
    ```
    

### 6.9.4 toJSON() 메서드

- Object.prototype에 실제로 toJSON() 메서드가 정의된 것은 아니지만, JSON.stringify() 메서든는 직렬화할 객체에서 toJSON() 메서드를 검색한다.
    - 직렬화할 객체에 이런 메서드가 존재하면 해당 메서드를 호출해 반환 값을 직렬화한다.
        - Date 클래스의 toJSON() 메서드는 직렬화 가능한 문자열 표현을 반환한다.
        
        ```tsx
        let point = { 
        		x: 1,
        		y: 2,
        		toString: function() { return `(${this.x}, ${this.y})、; },
        		toJSON: function() { return this.toString(); } };
        JSON.stringify([point]) // => ’[”(1, 2)”]’
        ```
        

---

## 6.10 확장된 객체 리터럴 문법

### 6.10.1 단축 프로퍼티

- ES6 이후에는 다음과 같이 간결한 문법을 쓸 수 있다
    
    ```jsx
    let x = 1, y = 2
    let o = {x,y}
    o.x + o.y // 3
    ```
    

### 6.10.2 계산된 프로퍼티 이름

- ES6부터 계산된 프로퍼티(computed property)라는 기능을 사용할 수 있다
    - 새 문법의 대괄호 안에는 임의의 자바스크립트 표현식이 들어간다
    - 표현식을 평가한 값을 프로퍼티 이름으로 사용하며, 필요하다면 문자열로 변환된다
    
    ```jsx
    const PROPERTY_NAME = "p1"
    const computePropertyName = () => "p2"
    
    let p = {
    	[PROPERTY_NAME]: 1,
    	[computePropertyName()]: 2
    }
    ```
    

### 6.10.3 프로퍼티 이름인 심벌

- 계산된 프로퍼티 문법을 통해 가능해진, 아주 중요한 객체 리터럴 기능이 있다
- ES6 이후에는 프로퍼티 이름에 문자열이나 심벌을 쓸 수 있다
- 심벌을 변수나 상수에 할당하면 계산된 프로퍼티 문법을 통해 그 심벌을 프로퍼티 이름으로 쓸 수 있다

```jsx
const extension = Symbol("extension symbol")
let o = {
	[extension]: {/* */}
}
o[extension].x = 0 // o 의 다른 프로퍼티와 충돌하지 않는다
```

- 심벌은 불투명한 값이고 프로퍼티 이름 외에는 다른 용도로 쓸 수 없다
    
    ⇒ 심벌은 고유한 프로퍼티 이름을 만들 때 안성맞춤이다
    
- 심벌은 객체가 아닌 기본값이므로 Symbol() 은 new 와 함께 호출되는 생성자 함수가 아니다
- 심벌의 목적은 보안이 아니라 자바스크립트 객체가 사용할 수 있는 안전한 확장 메커니즘을 정의하는 것이다

### 6.10.4 분해 연산자

- ES2018 이후에는 객체 리터럴 안에서 분해 연산자 … 를 사용해 기존 객체의 프로퍼티를 새 객체에 복사할 수 있다
    
    ```jsx
    let position = {x:0, y:0}
    let dimensions = {width: 100, height: 75}
    let rect = {...position, ...dimension}
    rect.x + rect.y + rect.width + rect.height // 175
    ```
    
- 이 문법은 보통 분해 연산자라고 부르긴 하지만, 진정한 자바스크립트 연산자라고 볼수는 없다
- 이 문법은 객체 리터럴 안에서만 사용할 수 있는 특별 문법이다
- 분해되는 객체와 프로퍼티를 받는 객체 둘 다 같은 이름의 프로퍼티를 갖는다면 해당 프로퍼티의 값은 마지막에 오는 값이 된다
    
    ```jsx
    let o = {x:1}
    let p = {x:0, ...o}
    p.x // 1
    let q = {...o, x:2}
    q.x // 2
    ```
    
- 분해 연산자는 자체 프로퍼티만 분해할 뿐 상속된 프로퍼티에는 적용되지 않는다
    
    ```jsx
    let o = Object.create({x:1})
    let p = {...o}
    p.x // undefined
    ```
    
- 객체에 프로퍼티가 n개 있으면 이 프로퍼티를 다른 객체로 분해하는 작업은 O(n) 작업이다
    
    ⇒ 따라서 … 를 루프나 재귀 함수에 넣어 데이터를 큰 객체 하나에 누산한다면 n이 커질수록 비효율적인 O(p2) 알고리즘을 쓰게 되는 것이다
    

### 6.10.5 단축 메서드

- 객체 프로퍼티로 정의된 함수를 메서드라 부른다
- ES6 에서는 객체 리터럴 문법에서 function 키워드와 콜론을 생략할 수 있다
    
    ```jsx
    let square = {
    	area() {
    		return this.side * this.side
    	}
    	side: 10
    }
    square.area() // 100
    ```
    

### 6.10.6 프로퍼티 게터와 세터

- 자바스크립트는 접근자 메서드 getter 와 setter 를 갖는 접근자 프로퍼티 역시 지원한다
- 프로그램이 접근자 프로퍼티의 값을 검색하면 자바스크립트는 인자 없이 게터 메서드를 호출한다
    - 이 메서드의 반환 값이 프로퍼티 접근 표현식의 값이다
- 프로그램에서 접근자 프로퍼티의 값을 설정하려 하면 자바스크립트는 세터 메서드를 호출하고 할당 표현식의 오른쪽 값을 인자로 전달한다
    - 이 메서드가 프로퍼티 값 세팅을 담당한다. 세터 메서드의 반환 값은 무시한다
- 프로퍼티에 게터 세터 메서드가 모두 있으면 읽기 쓰기가 모두 가능한 프로퍼티, 게터 메서드만 있다면 읽기 전용 프로퍼티, 세터 메서드만 있다면 쓰기 전용 프로퍼티이며 값을 읽으려면 항상 undefined 로 평가된다
- 접근자 프로퍼티는 객체 리터럴 문법의 확장으로 정의할 수 있다
    
    ```jsx
    let o = {
    	dataProp: value,
    	get accessorProp() {return this.dataProp}
    	set accessorProp(value) {this.dataProp = value}
    }
    ```
    
- 접근자 프로퍼티는 한 개 혹은 두 개의 메서드 형태로 정의되며 그 이름은 프로퍼티 이름과 같다
- 접근자 프로퍼티는 데이터 프로퍼티와 마찬가지로 상속된다(다른 객체의 프로토타입으로 쓸 수 있다)
- 접근자 프로퍼티를 사용하는 이유 중에는 프로퍼티에 쓸 때 유효성 검사를 하고, 읽을 때마다 다른 값을 반환하게 하는 것도 있다