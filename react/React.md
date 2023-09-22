# React 팁

- [React 팁](#react-팁)
  - [참고 주소](#참고-주소)
  - [useEffect](#useeffect)
  - [리스트 추가시 스크롤바 가장 아래로](#리스트-추가시-스크롤바-가장-아래로)
  - [props초기값 설정](#props초기값-설정)
  - [props 한꺼번에 전달](#props-한꺼번에-전달)
  - [하위 컴포넌트에 children으로 전달](#하위-컴포넌트에-children으로-전달)

## 참고 주소

-   https://react-ko.dev/

## useEffect

-   기본 사용법

    ```JS
      // 렌더링 시마다 호출이 된다.
      useEffect(() => {
        console.log('렌더링!');
      });
    ```

-   state 지정

    ```JS
      let [stat, setStat] = useState();
      let [stat2, setStat2] = useState();
      // 지정된 값이(stat) 변경시 호출이 된다.
      useEffect(() => {
        console.log('렌더링!');
      }, [stat]);
      // 여러개를 넣을 수 있다.
      useEffect(() => {
        console.log('렌더링!');
      }, [stat2]);
    ```

-   첫번째 렌더링 시만 호출

    ```JS
      useEffect(() => {
        console.log('렌더링!');
      }, []);
    ```

-   언마운트(Unmount)시 호출

    ```JS
    // 컴포넌트가 언마운트시
      useEffect(() => {
        console.log('렌더링시마다 호출');
        return()=>{
            console.log('언마운트시 호출');
        }
      });
      // 특정 stat가 언마운트시
      useEffect(() => {
        console.log('스테이트가 변경될시 호출');
        return()=>{
            console.log('언마운트시 호출');
        }
      },[stat]);
      // 전체 언마운트시
      useEffect(() => {
        console.log('처음 한번만 호출');
        return()=>{
            console.log('언마운트시 호출');
        }
      },[]);
    ```

## 리스트 추가시 스크롤바 가장 아래로

```JS
  const scrollRef = useRef();
  const [msgs, setMsgs] = useState([]);

  useEffect(()=>{
    scrollRef.current.scrollIntoView({ behavior: "smooth" }));
  }, [msgs])
  return (
    <div>
      //내용 쭈욱
      {msgs.map((u, i)=>{
        return (<div>{u}<div>)
      })}
      // 스크롤바를 제어하기 위한 div
      <div ref={scroolRef}></div>
    </div>)
```

## props초기값 설정

```JS
  import React, { useState } from 'react';

  function Time({ defaultDate, message }) {
    //..code
  }

  Time.defaultProps = {
    message: 'not send'
  };

  export default Time;

```

## props 한꺼번에 전달

```JS
  const timeProps = {
    defaultDate: today,
    message: '지금 시간은?'
  };

  function App() {
    return (
      <div className="App">
        <AppHeader />
        <header className="App-header">
          <Time {...timeProps} />
        </header>
        <AppFooter />
      </div>
    );
  }
```

## 하위 컴포넌트에 children으로 전달

```JS
  import Outer from './Outer';
  function App() {
    // 루트 컴포넌트
    return (
      <div className="App">
        <Outer>
          <Time />
        </Outer>
      </div>
    );
  }

  const Outer = function({children}) {
	return (
		<div style={{ padding : 20px, backgroundColor : '#ffeaa7'}}>
			{children} // Time컴포넌트에 이렇게 전달 가능하다.
		</div>
	);
}

export default Outer;
```
## TypeScript React 한글 입력시 KeyDown, KeyUp Event 2번 발생
- 원인 :

  > 한글은 영문이나 숫자와는 다르게 여러개의 입력으로 문자가 완성이 된다.   
  > 그리하여 이벤트가 발생 하여도 문자가 완성이 되지 않는 경우가 있다.
  > 이때 event.nativeEvent.isComposing 상태가 true인 경우는 문자가 작성 중임으로 fale에 이벤트를 진행 하면 된다.