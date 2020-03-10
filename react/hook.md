# React Hook
Hook은 React 16.8 버전부터 추가되었다.    
Hook을 이용하면 Class를 작성 할 필요 없이 여러 React 기능을 사용할 수 있다.    
React팀이 왜 Hook을 추가했는지 알고싶다면 첫번째 챕터부터 읽으면 되고, 그냥 Hook에 대해서 배우고 싶다면 다음 챕터부터 읽으면 된다.    
# 1. Hook 소개
## 이미 Class로 작성된 React 코드가 있나요?
---
- Hook은 `선택적 사용`이 가능하다. 기존 Class로 작성된 코드를 다시 작성할 필요 없이, 일부 컴포넌트들 안에서만 Hook을 사용할 수도 있다.

## Hook이 추가 된 이유
---
### 1) 컴포넌트 사이에서 상태와 관련된 로직을 재사용하기 쉬워진다.
React는 컴포넌트에 재사용 가능한 행동을 붙이는 방법을 제공하지 않는다. 만약 이전부터 React를 사용해온 개발자라면, 아래와 같은 방법을 사용했을것이다.
- `render props`
- `고차 컴포넌트`
그러나 이런 패턴은 코드를 추적하기 어렵게 만들고, 래퍼 지옥에 빠질 가능성이 있다.

Hook을 사용하면 컴포넌트로부터 상태 관련 로직을 추상화할 수 있다. 

### 2) 컴포넌트가 단순해지고 이해하기 쉬워진다.
`componentDidMount` 나 `componentDidUpdate` 와 같은 생명주기를 메서드 기반으로 쪼개서 사용하게 되면 컴포넌트가 복잡해지고 테스트하기도 어려워진다.    
많은 사람들이 이를 이유로 React와 별도의 상태 관리 라이브러리를 결합하여 사용하는데, 상태관리 라이브러리는 종종 너무 많은 추상화를 하고 컴포넌트 재사용을 더욱 어렵게 한다.    
Hook을 사용한다면, **로직에 기반을 둔 작은 함수로 컴포넌트를 나눌 수 있다**. 이는 나중에 `Effect Hook`을 살펴보며 더 알아볼 것이다.

### 3) Class는 사람과 기계를 혼동시킨다.
- Class는 사람을 혼동시킨다.    
    Javascript에서 어떻게 this가 동작하는지 알아야하고     
    이벤트 핸들러가 등록되는 방법을 기억해야만 하고    
    코드가 장황해진다
- Class는 기계를 혼동시킨다.
    Class는 최근 사용되는 도구에도 많은 문제를 일으킨다.    
    예를 들어 Class는 잘 Minify 되지 않고    
    핫 리로딩을 깨지기 쉽고 신뢰할 수 없게 만든다.    

이러한 문제들을 해결하기 위해 Hook은 Class없이 React 기능들을 사용할 수 있게 해준다.

# 2. Hook 개요
Hook에 대해서 전체적으로 훑어보는 것으로 시작해보자.    
각각의 Hook에 대한 설명은 각 챕터별로 깊게 들여다 볼 수 있다.

## 근데 Hook이 뭔가요?
---
함수 컴포넌트 안에서, React state와 생명주기 기능을 연동할 수 있게 해주는 함수이다.    

Class 안에서는 동작하지 않는다.
## Hook 사용 규칙
---
Hook은 그냥 JavaScript 함수이지만, 두 가지 규칙을 준수해야 한다.

- **최상위**에서만 Hook을 호출해야 한다. 반복문, 조건문, 중첩된 함수 내에서 Hook을 실행하면 안 된다.
- **React 함수 컴포넌트 내에서만 Hook을 호출**해야 한다. 일반 JavaScript 함수에서는 Hook을 호출해서는 안 된다.
- (Hook을 호출할 수 있는 곳이 딱 한 군데 더 있는데, 바로 직접 작성한 custom Hook 이다. 이것에 대해서는 나중에 알아볼 예정이다.)

## 내장 Hook
---
React에서는 내장 Hook을 몇 가지 제공한다.

### 🌿 State Hook

```js
const [count, setCount] = useState(0);
       state   변경함수              초기값
```

여기서 `useState`가 바로 Hook이다.    
Hook을 호출하여 함수 컴포넌트 안에 state를 추가하였고, 이 state는 **컴포넌트가 다시 렌더링 되어도 그대로 유지**될 것이다.    

useState는 [배열 구조 분해(destructuring)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#%EB%B0%B0%EC%97%B4_%EA%B5%AC%EC%A1%B0_%EB%B6%84%ED%95%B4) 문법을 사용하여 호출된 state 변수들을 다른 변수명으로 할당할 수 있게 해준다.

여러 개의 state 변수를 선언 하려면 아래와 같이 하면 된다.
```js
    const [age, setAge] = useState(42);
    const [fruit, setFruit] = useState('banana');
    const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
```

### 🧬 Effect Hook

Effect Hook은 React class의 `componentDidMount` 나 `componentDidUpdate`, `componentWillUnmount` 와 같은 목적으로 제공되지만, 하나의 API로 통합된 것이다.

아래의 예시는 React가 DOM을 업데이트한 뒤에 문서의 title을 변경하는 예시이다.
```js
    function Example() {
      const [count, setCount] = useState(0);
    
      // componentDidMount, componentDidUpdate와 비슷합니다
      useEffect(() => {
        // 브라우저 API를 이용해 문서의 타이틀을 업데이트합니다
        document.title = `You clicked ${count} times`;
      });
    
      return (
        <div>
          <p>You clicked {count} times</p>
          <button onClick={() => setCount(count + 1)}>
            Click me
          </button>
        </div>
      );
    }
```
Effects는 컴포넌트 안에 선언되어있기 때문에 props와 state에 접근할 수 있다.    
React는 **첫번째 렌더링을 포함하여 매 렌더링 이후에 effects를 실행**한다.    

만약 Efftect를 해제할 필요가 있다면 해제하는 함수를 반환해주면 된다.    
예를 들어 아래의 컴포넌트는 접속 상태를 구독하는 effect를 사용했고,     
컴포넌트가 unmount될 때 구독을 해지한다.

또한 재 렌더링이 일어나 effect를 재실행하기 전에도 마찬가지로 구독을 해지한다.
```js
    import React, { useState, useEffect } from 'react';
    
    function FriendStatus(props) {
      const [isOnline, setIsOnline] = useState(null);
    
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
    
      useEffect(() => {
        ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    
        **return () => {
          ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
        };**
      });
    
      if (isOnline === null) {
        return 'Loading...';
      }
      return isOnline ? 'Online' : 'Offline';
```
이렇게 Hook을 사용하면 구독 및 해지 로직과 같이 서로 관련 있는 코드들을 한군데에 모아서 작성할 수 있는 반면에, class 컴포넌트에서는 생명주기 메서드(lifecycle methods) 각각에 쪼개서 넣어야만 했었다.    


보편적으로 사용되지 않는 내장 Hook들을 간단하게 알아보자.

### 🔌 useContext

컴포넌트를 중첩하지 않고도 React context를 구독할 수 있게 해준다. 
```js
    function Example() {
      const locale = useContext(LocaleContext);
      const theme = useContext(ThemeContext);
      // ...
    }
```
### 💽 useReducer
복잡한 컴포넌트들의 state를 reducer로 관리할 수 있게 해준다.
```js
    function Todos() {
      const [todos, dispatch] = useReducer(todosReducer);
      // ...
    }
```

## 나만의 Hook 만들기

---

상태 관련 로직을 컴포넌트간에 재사용 하고 싶다면, 기존 Class구조에서는 두 가지 방법을 사용했다.    

- higher-order components
- render props

Custom Hook은 이들 둘과는 달리 컴포넌트 트리에 새 컴포넌트를 추가하지 않고도 이것을 가능하게 해준다.    

위에서 살펴봤던 접속상태 구독/해지 로직 예시를 다시 보자. 만약 이 로직을 다른 컴포넌트에서도 재사용하고 싶다면 custom Hook으로 뽑아내면 된다. 아래의 예시는  `useFriendStatus`라는 이름의 Custom Hook으로 뽑아냈다.
```js
    import React, { useState, useEffect } from 'react';
    
    function useFriendStatus(friendID) {
      const [isOnline, setIsOnline] = useState(null);
    
      function handleStatusChange(status) {
        setIsOnline(status.isOnline);
      }
    
      useEffect(() => {
        ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
        return () => {
          ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
        };
      });
    
      return isOnline;
    }
```
이 custom Hook은 `friendID`를 인자로 받아 접속 상태를 반환하고 있다.

이제 이 custom Hook을 여러 컴포넌트에서 사용하면 된다.
```js
    function FriendStatus(props) {
      const isOnline = useFriendStatus(props.friend.id);
    
      if (isOnline === null) {
        return 'Loading...';
      }
      return isOnline ? 'Online' : 'Offline';
    }

    function FriendListItem(props) {
      const isOnline = useFriendStatus(props.friend.id);
    
      return (
        <li style={{ color: isOnline ? 'green' : 'black' }}>
          {props.friend.name}
        </li>
      );
    }
```
각 custom Hook의 state는 완전히 독립적이다. 심지어는 한 컴포넌트 안에서 같은 custom Hook을 두 번 쓸 수도 있다. 두 개의 custom Hook은 독립적으로 동작할 것이다.

Custom Hook은 기능이라기보다는 컨벤션(convention)에 가깝다. 이름이 `use`로 시작하고, 안에서 다른 Hook을 호출한다면 그 함수를 custom Hook이라고 부를 수 있다. `useSomething`이라는 네이밍 컨벤션은 linter 플러그인이 Hook을 인식하고 버그를 찾을 수 있게 해준다.