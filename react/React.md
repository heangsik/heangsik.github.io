# React 팁

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
