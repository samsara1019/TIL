# 
# 3. 🌿 State Hook

State Hook, 즉 `useState()`는 앞장에서 훑어본 내용만으로 충분히 파악할 수 있을 만큼 사용하기 쉽다.

다시한번 살펴보자면, State Hook은 "state가 없는 컴포넌트"로 알려진 **함수 컴포넌트 안에서 React state를 사용할 수 있게 해주는 훅**이다.

## 전체 코드를 살펴보며 잘 이해했는지 체크해보자.
```js
//useState Hook을 React에서 가져옵니다
import React, { useState } from 'react';

function Example() {
    //state 변수와 해당 state를 갱신할 수 있는 함수가 만들어진다. 
    //또한, useState의 인자의 값으로 0을 넘겨주면 count 값을 0으로 초기화할 수 있다.
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>You clicked {count} times</p>
             <!--사용자가 버튼 클릭을 하면 setConut 함수를 호출하여 state 변수를 갱신한다. 
                    React는 새로운 count 변수를 Example 컴포넌트에 넘기며 해당 컴포넌트를 리렌더링한다.-->  
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```
## 대괄호가 의미하는것은 무엇일까?
```js
const [count, setCount] = useState(0);
```
자바스크립트 문법 중 [배열구조분해](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EB%B0%B0%EC%97%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4)이다.

`useState`는 count, setCount 두 개의 값을 반환한다. 

위 코드는 아래의 코드와 같은 동작을 한다.
```js
const countStateVariable = useState(0); // 두 개의 아이템이 있는 쌍을 반환
const count = countStateVariable[0]; // 첫 번째 아이템
const setCount = countStateVariable[1]; // 두 번째 아이템
```