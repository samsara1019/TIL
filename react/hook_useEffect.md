# 
# 3. 🧬 Effect Hook

Effect Hook, 즉 `useEffect()`를 사용하면 함수 컴포넌트 내에서 side effect를 수행할 수 있다.

**side effect란?** 데이터 가져오기, 수동으로 리액트 컴포넌트 DOM 수정하기 등의 기능들을 side effect라고 부른다.

> 리액트의 class 생명주기 메서드에 친숙하다면, useEffect Hook을 `componentDidMount`와 `componentDidUpdate`, `componentWillUnmount`가 합쳐진 것으로 생각하면 좋다.

리액트 컴포넌트의 side effect는 두 종류가 있다.

- **정리가 필요없는 Effects :**  실행 이후 신경 쓸 것이 없는 경우
- **정리가 필요한 Effects :** 구독, 해지와 같이 메모리 누수가 발생하지 않도록 정리해야 하는 경우

## 정리가 필요 없는 Effects
---
마운트 될 때마다 실행하고 싶은 코드가 있다고 가정하자.

만약 클래스 컴포넌트로 작성한다면 두 군데에 같은 코드를 작성해야한다.

- componentDidMount
- componentDidUpdate

하지만 Hook을 사용하면 이런 중복을 없앨 수 있다.
```js
import React, { useState, useEffect } from 'react';

function Example() {
    const [count, setCount] = useState(0);

    useEffect(() => {
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
코드를 좀 더 자세히 들여다보자.

1. useEffect의 역할은?

    useEffect 인자로 **함수**를 넘긴다. 이 함수는 리액트 컴포넌트가 렌더링 이후에 어떤 일을 수행하는지를 말해준다. 이 함수를 Effect 라고 부른다.

2. useEffect를 컴포넌트 안에서 불러내는 이유는?

    컴포넌트 내부에 둠으로써, state와 prop에 접근할 수 있다.

3. useEffect는 매 랜더링마다 실행되는건가?

    맞다. 첫번째 렌더링과 모든 업데이트마다 실행이 된다. 물론 필요에 맞게, 예를들어 특정 state 변경에만 실행되도록 수정하는 방법도 있다. 
    ```js
    useEffect(() => {
        document.title = `You clicked ${count} times`;
    }, [count]); // count가 바뀔 때만 effect를 재실행
    ```

    혹은 마운트와 마운트 해제 시에 딱 한 번씩만 실행하고 싶다면   

    ```js
    useEffect(() => {
        // do somethin()
    }, []); // 빈 배열을 넘김
    ```
4. 모든 랜더링마다 useEffect에 다른 함수가 전달되는건가?

    맞다. 이는 의도된 것으로서, 각각의 effect가 특정한 렌더링에 속하도록 해 유용하다. 예를들어 state가 업데이트 되었는지에 대한 걱정 없이 내부에서 그 값을 읽을 수 있다.

> `componentDidMount` 혹은 `componentDidUpdate`와는 달리 `useEffect`는 **비동기**로 실행된다. 즉, 브라우저가 화면을 업데이트하는 것을 차단하지 않는다. 만일 레이아웃의 측정과 같은 동기적 실행이 필요하다면 **useLayoutEffect**라는 별도의 Hook을 사용할 수 있다.

## **정리가 필요한 Effects**
---
정리가 필요한 Effect의 예시로, **ID 기준으로** 외부 데이터에 구독하는 경우를 들어보자. 이런 경우 메모리 누수를 막기 위해 구독을 해지해줘야 한다.

만약 클래스 컴포넌트로 작성한다면 이런 코드가 작성될것이다.

- componentDidMount에서 구독
- componentWillUnmount에서 구독 해지
- componentDidUpdate에서 기존 ID 구독 해지 + 새 ID 구독

구독과 구독 해지는 같은 논리묶음 이지만, `componentDidMount`와 `componentWillUnmount라`는 다른 생명주기에 나누어 작성해야한다.

또한 만일 ID값이 변경된다면, state 변경 여부에 따른 충돌을 막기 위해 `componentDidUpdate` 안에서 처리를 해주어야한다.

하지만 Effect Hook을 사용한다면?
```js
useEffect(() => {
    function handleStatusChange(status) {
        setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // effect 이후에 어떻게 정리(clean-up)할 것인지 표시합니다.
    return function cleanup() {
        ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
});
```
구독과 해지 로직을 가까이에 묶을 수 있고, 매 렌더링마다 다른 함수가 호출되기 때문에 state의 변경 여부도 신경쓰지 않아도 된다.