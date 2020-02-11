# 호이스팅
호이스팅은 **끌어올린다**라는 의미로, 변수와 함수의 **선언부분**이 최상단으로 끌어올려지는 자바스크립트의 특성이다.    
    
여기서 선언부분이 무엇을 의미할 지 주목할 필요가 있다.    
선언부분은 변수의 생성 과정을 보면 이해할 수 있다.    

# 변수의 생성 과정 3단계
1. 선언 단계 : 변수를 실행 컨텍스트의 변수 객체에 등록
2. 초기화 단계 : 등록 된 변수 객체에 메모리를 할당하며, `undefined`로 초기화
3. 할당 단계 : `undefined`로 초기화 된 변수에 실제 값을 할당

[함수 선언문, 함수 표현식](./javascript/functionStatementAndExpression.md) 글에서 함수 선언문은 호이스팅이 적용된다고 언급했었다.    
함수 선언문은 **선언과 초기화 단계를 동시에 실행**했기 때문에 식 자체가 통째로 끌어올려져서, 함수를 호출하는 코드가 함수를 선언하는 코드보다 먼저 와도 문제 없이 함수가 실행된다.    
    
그렇다면 함수 외의 변수는 어떨까?    
`var, let, const` 중 `var`을 먼저 살펴보자.

# var
```js
console.log(name); // undefined
var name = 'samsara';
console.log(name); // samsara
```

첫 줄에 `console.log(name)`을 보면 **name**이 선언되지도 않았는데 오류가 나지 않고 `undefined`로 출력된다.    
이는 호이스팅 때문이다.    
호이스팅에 의해, 실행 시점에 코드는 이렇게 변경된다.    
    
```js
var name;
console.log(name); // undefined
name = 'samsara';
console.log(name); // samsara
```

# let
그럼 let의 경우는 어떨까?    
```js
console.log(name); // ReferenceError: name is not defined
let name = 'samsara';
console.log(name); // samsara
```

음? `undefined`가 출력되는게 아닌, 오류가 발생한다.    
`let`은 호이스팅이 적용되지 않는걸까?    
**아니다!**    
이는 `var`와 `let`의 차이때문이다.    
    
위에서 설명한 [변수의 생성 과정 3단계](./javascript/hoisting?id=변수의-생성-과정-3단계)를 다시 보자.    
`var`는 선언 단계와 초기화 단계가 한 번에 이루어지고    
`let`은 선언 단계와 초기화 단계가 분리되어 진행된다.    
   
즉, `let`은 **선언만 된 상태**에서 호이스팅(선언 부분을 최상단으로!)이 일어나고, **초기화 단계가 일어나지 않아** 메모리 공간이 확보되지 않은 상태에서 호출당하니 참조 에러가 발생한것이다.    
이렇게 선언단계 ~ 초기화 단계에서 발생하는 위험한 구간을 **TDZ(Temporal Dead Zone), 일시적 사각지대** 라고 부른다.    
![일시적 사각지대](/images/hoisting01.png)
    
**결국은 호이스팅이 발생하지 않는거랑 똑같은 거 아닌가?**🤔
라고 의문이 들 수 있다. 하지만 다음 코드를 보자
```js
let name = 'samsara';

{
    console.log(name); // ReferenceError: name is not defined
    let name = 'yunhye';
}
```
**samsara**가 찍혀야 할 것 같은데, `ReferenceError: name is not defined` 오류가 발생한다.    
let은 블록 레벨 스코프를 가지므로, 블록 스코프 내에서 호이스팅이 발생했고, TDZ에 빠진것이다.