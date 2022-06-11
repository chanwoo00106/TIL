# useMemo

우선 `useMemo`를 간단하게 설명을 하자면 특정한 값을 메모리에 저장을 해서 컴포넌트가 재렌더링이 되었을 때 메모리에 들어 있는 값을 다시 재사용 하게 해주는 hook이다 

`useMemo`를 배우기에 앞서 알아야 하는 점이 있는데 함수형 컴포넌트는 함수이고 함수형 컴포넌트가 렌더링이 된다는 것은 그 함수가 호출이 된다는 것이다. 그리고 함수는 호출될 때마다 내부에 있는 모든 변수가 초기화가 된다. 

```jsx
function a() {
	const value = calc() 
	return (
		<div>{value}</div>
	)
}
```

위 예제를 보면 `a`라는 컴포넌트는 재렌더링이 될 때 `value`값은 초기화가 돼서  `calc()`라는 함수를 호출해 `value`에 값을 다시 할당해 주게 된다.

하지만 여기서 `useMemo`를 사용하게 되면 위와 같은 비효율적인 작업을 효율적이게 바꿔줄 수 있다.

```jsx
function a() {
	const value = useMemo(calc, [])
	return (
		<div>{value}</div>
	)
}
```

이제 `useMemo`에 인자에 `value` 값에 들어갈 callback 함수와 의존성 배열을 넣어준 후 `a`컴포넌트를 재렌더링 해보면 `value`는 메모리에  저장된 callback 함수의 결괏값을 가져다 사용하게 된다.

```jsx
const calc = () => {
	for (let i = 0; i <= 999999999; i++) {} // 아무튼 오래 걸리는 연산
	return 1
}

export default function App() {
	const [text, setText] = useState("")
	const value = calc();

	return (
		<>
			<input value={text} onChange={e => setText(e.target.value)} />
			<p>{text}</p>
			<div>{value}</div>
		</>
	)
}
```

좀 더 확실한 예제를 보자면 현재 input에 입력을 할 때마다 `text`값이 변경돼서 `App` 컴포넌트가 재렌더링이 되는 상황이다. 그런데 `App` 컴포넌트가 재렌더링이 됨으로써 `value`값도 초기화가 돼서 `calc`함수를 다시 호출하게 되는데 `calc`함수는 오래 걸리는 연산을 해서 `input`을 입력하는 속도도 늦춰버리고 말았다.

`text`의 값이 변경될 때 굳이 `calc`까지 다시 연산할 필요는 없으므로 `useMemo`로 감싸준다. 그다음 의존성 배열에는 `calc`함수가 첫 렌더링에만 실행되므로 그냥 빈 배열을 넘겨주면 된다.

```jsx
import {useState, useMemo} from "react";

const calc = () => {
	for (let i = 0; i <= 999999999; i++) {}
	return 1
}

export default function App() {
	const [text, setText] = useState("")
	const value = useMemo(calc, []);

	return (
		<>
			<input value={text} onChange={e => setText(e.target.value)} />
			<p>{text}</p>
			<div>{value}</div>
		</>
	)
}
```

위와 같이 바꿔서 실행해 보면 속도가 많이 개선된 것을 볼 수 있다.