# 5. 문

## 5.1 표현문

- 할당문 - 변수의 값을 바꾸는 부수 효과

```jsx
greeting = `Hello ${name}`
i *= 3
counter++ 
delete o.x 
```

- 함수 호출 - 프로그램의 상태나 호스트 환경에 영향을 미치는 부수 효과

---

## 5.2 복합문과 빈 문

- 표현식 여러개를 하나로 묶는 콤마 연산자와 마찬가지로, 문 블록은 문 여러개를 묶어 복합문으로 만든다.
    
    ```html
    {
    	x = Math.PI;
    	cx = Math.cos(x);
    	console.log("cos(n) = " + cx);
    }
    ```
    
    - 자바스크립트에서 문 하나를 예상하는 곳이라면 어디든 쓸 수 있다.

- 문 블록
    - 블록은 세미콜론으로 끝나지 않는다.
    - 블록안의 행은 자신을 감싼 중괄호를 기준으로 들여쓴다.
        - 필수는 아니지만 코드를 읽고 쓰기 편해진다.
    - 자바스크립트 문법은 공식적으로 단일 하위 문을 허용한다.
    
- 복합문
    - 자바스크립트가 문 하나를 예상하는 곳에 문 여러개를 넣을때 사용
- 빈 문
    - 문이 있을것으로 예상되는 곳에 문을 쓰지 않는것
        
        ```html
        ;
        ```
        
        - 자바스크립트 인터프리터는 빈 문을 실행할때 아무일도 하지 않는다.
    - 유용한 예시
        
        ```tsx
        //배열 a를 초기화
        for(let i = 0; i <a.length; a[i++] = 0) **;**
        ```
        
        - 위 루프가 하는일은 전부 a[i++] 에 들어있어 루프바디를 쓰지 않고 바로 빈 문을 만들었다.
        - 대신 for, while, if문의 닫는괄호 다음에 실수로 세미콜론을 썼을 경우 예기치 못한 오류를 발생시킬 수 있으니 주석을 넣어두는것이 좋다
            
            ```tsx
            for(let i= 0; i<a.length; a[i++] = 0) /* 의도적으로 비움. */ ;
            ```
            

---

## 5.3 조건문

### 5.3.1 if

```jsx
if(expression) statement
```

- 괄호 안의 표현식을 평가했을때 true 같은 값이면 statement(문)을 실행하고 false 같은 값이면 실행하지 않는다

```jsx
if(expression) {
	statement1
} else {
	statement2
}
```

- else 가 있는 경우에는 표현식이 false 같은 값일 경우 statement2 를 실행한다
- 코드를 명확하게 이해하고, 관리하고, 디버그하기 쉽게 하려면 중괄호를 써야 한다

### 5.3.2 else if

- else if 문은 경우에 따라 실행되야 하는 선택지가 여러개일때 사용한다

```jsx
if(n === 1) {
	...
} else if(n === 2) {
	...
} else if(n === 3) {
	...
} else {
	...
}
```

### 5.3.3 switch

- 모든 분기점이 같은 표현식의 값으로 좌우될때 if 문을 대신해서 switch ~ case 문을 사용할 수 있다
- case 가 else if 와 같고 default 는 else 와 같다
- if 문과는 달리 케이스마다 마지막에 break 를 선언해주어야 switch 문의 끝으로 빠져나가져 이어지는 문을 실행할 수 있다
- switch 문을 실행할 때마다 case 표현식 전체가 평가되는 것은 아니므로 case 표현식에는 함수 호출이나 할당처럼 부수효과가 있는 것은 피해야 한다.

---

## 5.4 반복문

- 반복문
    - 반복문은 경로를 자기자신 쪽으로 구부려 코드 일부를 반복하는 문
        - 루프라고도 함
    - 자바스크립트에서는 `while, do/while, for, for/of, for/in` 5가지가 있다.

### 5.4.1 while

- `while` 은 자바스크립트의 기본루프
    
    ```tsx
    while(expression)
    		statement
    ```
    
- 동작순서
    - 인터프리터는 먼저 `expression` 을 평가한다.
        - 표현식이 `false` 같은 값이면 루프 바디를 건너뛰고 다음 문으로 이동한다
        - 표현식이 `true` 같은 값이면, 인터프리터는 `statement` 를 실행하고, 다시 루프 맨위로 올라가 `expression` 을 평가하길 반복한다.

```tsx
let count =0; 
while(count < 10) {
	console.log(count);
	count++; 
}
```

- 루트 카운터의 변수명으로는 일반적으로 i, j, k를 사용하지만(위에선 count), 코드를 이해하기 쉽게 하기 위해서는 더 의미있는 이름을 사용하자

### 5.4.2 do/while

- `do/while` 은 while과 비슷하지만 루프 표현식이 루프 맨 위가 아니라 맨 아래에서 평가 된다.
    - 따라서 루프 바디는 **최소 한번 실행**된다
    
    ```tsx
    do
    		statement
    while (expression);
    ```
    
    - while과 차이점
        - 루프시작을 알리는 do 키워드가 while 키워드가 모두 있어야한다.
        - do/while 루프는 반드시 항상 세미콜론으로 끝나야 한다.
            
            ```tsx
            do {
            	console.log(a[i]	
            } while(++i < len);
            ```
            
            - while은 루프바디를 중괄호로 감싼 경우에는 세미콜론이 필요하지 않다.
    

### 5.4.3 for

- `for` 문은 널리 쓰이는 패턴을 갖는 루프를 단순화한다.
    - 일반적으로 루프에서는 초기화, 테스트, 업데이트는 필수불가결한 동작이다.
        - `for` 문은 이 세 가지 동작을 표현식 하나로 묶고, 이들을 루프 문법에 명시적으로 포함한다.
        
        ```tsx
        for(initialize ; test ; increment)
        		statement
        ```
        
        - `initialize, test, increment` 는 세미콜론으로 구분하며, 각각 루프 변수의 초기화, 테스트, 증가를 담당한다.
    
- while문과의 비교
    - for 문을 while과 동등한 느낌으로 설명하자면 아래와 같다.
    
    ```tsx
    initialize;
    while(test) {
    		statement
    		increment;
    }
    ```
    

- 예시
    
    ```tsx
    for(let count = 0; count < 10; count++) {
    	console.log(count)
    }
    ```
    
- 콤마 연산자를 쓰면 여러개의 초기화와 증가 표현식을 하나로 묶어 `for` 루프에서 사용할 수 있다.
    
    ```tsx
    let i, j, sum = 0;
    for(i = 0, j = 10 ; i++, j--){
    	sum += i * j;	
    }
    ```
    
    - for 루프의 세가지 표현식은 전부 생략할 수 있지만 세미콜론은 필수이다.

### 5.4.4 for/of

- `for/of` 는 ES6에서 정의한 새 반복문이다.
    - `for/of` 는 이터러블 객체에서 동작한다.
        - 이터러블 객체로는 배열, 문자열, Set, Map이 있다.
            - 이들은 `for/of` 루프로 순회할 수 있는 일종의 연속체 또는 일련의 요소이다.
    - 숫자 배열을 순회한 for/of 문
    
    ```tsx
    let data= [1, 2, 3, 4, S, 6, 7, 8, 9], sum= 0; 
    for(let element of data) {
    		sum += element;
    }
    sum // => 45
    ```
    
    - 위 코드에서 루프 바디는 data 배열의 각 요소에 대해 한번씩 실행된다.
        - 바디를 실행하기 전 element 변수에 data 배열의 요소가 할당된다.
    - 배열은 `동적으로` 순회한다.
        - 즉 반복 중간에 배열 자체에 변화가 발생하면 반복 결과가 바뀔 수 있다.
            - 예를 들어 위 코드의 루프바디에 data.push(sum) 행을 추가하면 절대 배열의 마지막 요소에 도달할 수 없으므로 무한 루프가 만들어진다.

`for/of와 객체`

- 객체는 기본적으로 이터러블이 아니다
    - 그러므로 일반적인 객체에 for/of를 사용하려 한다면 TypeError를 일으킨다.
        - 대신, 객체의 프로퍼티를 순회하고 싶다면 이후 설명할 `for/in` 루프를 사용하거라, Object.keys() 메서드에 for/of를 써야한다.
    - 객체 프로퍼티의 키와 값이 모두 필요하다면 Object.entries() 를 사용하자
        
        ```tsx
        let pairs = "";
        for(let [k,v] of Object.entries(o)){
        		pairs += k + v;
        }
        pairs // => "x1y2z3"
        ```
        

`for/of와 문자열`

- ES6 이후에는 문자열을 다음과 같이 문자단위로 순회할 수 있다.
    
    ```tsx
    let frequency = {};
    for(let letter of ”mississippi”) {
    if (frequency[letter]) { 
    		frequency [letter]++;
    } else {
    		frequency [letter] = 1;
    } }
    frequency // ==> {m: 1, i: 4, s: 4, p: 2}
    ```
    

`for/of와 세트, 맵`

- ES6에 내장된 Set과 Map 클래스는 이터러블이다.
    - Set 예시
        
        ```tsx
        let text == ”Na na na na na na na na Batman!”; 
        let wordSet == new Set(text.split(” ”));
        let unique == [];
        for(let word of wordSet) {
        		unique.push(word);
        }
        unique // ==> [”Na”, l ’na’', ”Batman!“]
        ```
        
    - Map 예시
        
        ```tsx
        let m = new Map([[1, "one"]])
        for(let [key,value] of m){
        		key // => 1
        		value // => "one"		
        }
        ```
        
    

`for/await 를 사용한 비동기 순회`

- ES2018은 비동기 이터레이터라는 새로운 이터레이터 도입 및 비동동기 이터레이터와 함께 동작하도록 `for/await` 루프를 도입했다.
    
    ```tsx
    // 스트림때서 비동기적으로 덩어리를 읽고 출력함. 
    async function printStream(stream) {
    		for await (let chunk of stream) { 
    				console.log(chunk);
    		}
    }
    ```
    

### 5.4.5 for/in

- `for/in` 루프는 for/of와 다르게 이터러블 객체가 아닌 어떤 객체든 쓸 수 있다.
    - `for/in` 문은 지정된 객체의 프로퍼티 이름을 순회한다.
        
        ```tsx
        for (variable in object)
        		statement
        ```
        
    
- 예시
    
    ```tsx
    for(let p in o) {
    		console.log(o[p])
    }
    ```
    
- 동작 원리
    - 자바스크립트 인터프리터는 `for/in` 문을 실행할 때 첫 번째로 object 표현식을 평가한다.
        - 그 표현식이 null/undefined라면, 인터프리터는 루프를 건너뛴다.
        - 그렇지 않다면 객체의 열거가능한 프로퍼티 각각에 한번씩 루프바디를 실행한다.
    - 위의 열거가능한 프로퍼티를 `variable` 표현식을 평가하고 그 변수에 프로퍼티 이름을 할당한다.
    

- 자바스크립트 배열은 좀 특별한 종류의 객체이며 배열 인덱스는 `for/in` 루프에서 열거할 수 있는 객체 프로퍼티이다.
    - 가끔 for/of를 써야하는데 for/in을 사용하여 버그를 만드는일이 있는데, 대부분 배열을 대상으로 작업할때에는 for/of가 맞는 루프이다.
        
        > 배열에 for/in을 쓰면 인덱스만 가져오는것이기 때문에 사용할 일이 거의 없어 그런것 같다.
        > 
        
- `for/in` 루프는 실제로 객체의 프로퍼티 전체를 열거하지는 않는다.
    - 이름이 Symbol인 프로퍼티는 열거 X
    - 자바스크립트에서 정의하는 내장 메서드 또한 열거하지 않는다.
        - 대신, 우리가 직접 정의한 메서드는 기본적으로 열거 가능하다. (이후 이들을 열거 불가능하도록 하는 방법을 설명해준다고 함)