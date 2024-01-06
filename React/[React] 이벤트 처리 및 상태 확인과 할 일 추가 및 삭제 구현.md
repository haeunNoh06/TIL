# 이벤트 처리 및 상태 확인과 할 일 추가 및 삭제 구현해보기
## 1. `form`에 이벤트 걸어보기 
들어가기에 앞서 `react`에서는 이벤트를 어떻게 받는지 예제를 함께 타이핑해봅시다. 
```js
// submit 이벤트의 기본 동작 방지
function handleSubmit(e) {
    e.preventDefault();
    alert("Hello, world!");
}

function Form(props) {
    return (
        <form onSubmit={handleSubmit}>
// ... 
```
![](https://velog.velcdn.com/images/1109_haeun/post/8d59feaf-cb2d-4420-b9a6-944c9f8d3f16/image.png)

<br>

## 2. 콜백을 통한 form 제출 처리
```js
function App(props) {
  function addTask(name) {
	alert(name);
  }

// ...
```
다음으로, addTask()를 `<Form />`에 prop으로 전달합니다.
```js
return (
    <div className="todoapp stack-large">
      <h1>{props.name} 컴포넌트화 완성</h1>
      <Form addTask={addTask}/>
// ...
```
```js
function Form(props) {
    // submit 이벤트의 기본 동작 방지
    function handleSubmit(e) {
        e.preventDefault();
        props.addTask("Say Hello!");
    }

    return (
// ...
```
Add(submit) 버튼을 눌렀을 경우 `handleSubmit`함수가 작동하게 되는데 이 때 `App.js`에서 `<Form />`에 넘겨주었던 `addTask`함수가 `"Say Hello!"`라는 이름으로 알림창이 뜨게 되는 것입니다. 
![](https://velog.velcdn.com/images/1109_haeun/post/ed54bcce-f7cd-4e1e-90f6-221de14af6ea/image.png)

<br>

## 3. 상태와 useState hook
> 컴포넌트 자체가 소유한 데이터를 상태라고 합니다. 

상태는 컴포넌트가 상태를 소유할 뿐만 아니라, 나중에 업데이트할 수 있기 때문에 React의 또 다른 강력한 도구입니다. 컴포넌트가 받는 props를 업데이트하는 것은 불가능합니다. props를 읽을 수만 있습니다.

> React는 상태처럼, 컴포넌트에 새로운 기능을 제공할 수 있는 다양한 특수 함수를 제공합니다. 이러한 함수를 hook이라고 하며, useState hook은 이름에서 알 수 있듯이, 컴포넌트에 상태를 부여하기 위해 정확히 필요한 것입니다.

form.js에서 hook인 useState()함수를 react로부터 가져옵니다.
```js 
import React, { useState } from "react";
```
이제 `useState`는 `Form.js`의 어디에서나 사용할 수 있습니다.

`useState`는 상태와 나중에 상태를 업데이트하는데 사용할 수 있는 함수, 두 개를 반환합니다.

`Form.js`에 해당 코드를 작성합니다.
```js
function Form(props) {
    const [name, setName] = useState("Use hooks!");

    // submit 이벤트의 기본 동작 방지
    function handleSubmit(e) {
// ...
```
이 코드는
- `name`의 초깃값을 `Use hooks!`로 주고 있어요.
- `name`을 수정하는 `setName`함수를 정의하고 있어요.

<br>

### 3.1. 상태 읽기
`input`에 들어갈 텍스트를 `name`으로 세팅하여 `name`의 값을 봅시다.
```html
			<input
                type="text"
                id="new-todo-input"
                className="input input__lg"
                name="text"
                autoComplete="off"
                value={name}
            />
```
![](https://velog.velcdn.com/images/1109_haeun/post/ae5874b4-5063-484e-8e7a-27e991a33bce/image.png)
초기값을 빈 칸으로 두고 싶다면 아래와 같이 코드를 변경하면 됩니다.
```js
function Form(props) {
    const [name, setName] = useState("");
// ...
```

<br>

### 3.2. 사용자 입력 읽기
사용자가 실시간으로 타자를 치는 이벤트를 읽고 콘솔에 `Typing!`을 띄워보겠습니다.
```js
// 사용자의 실시간 입력 받기
    function handleChange(e) {
        console.log("Typing!");
    }
// ...
			<input
                type="text"
                id="new-todo-input"
                className="input input__lg"
                name="text"
                autoComplete="off"
                value={name}
                onChange={handleChange}
            />
```
이벤트를 감지하여 타자를 친 횟수대로 `Typing!`이라는 텍스트가 잘 뜨는 것을 확인할 수 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/1e22d17c-372d-41cc-b508-f69a9e9c8b8f/image.png)
아래와 같이 `e.target.value`를 콘솔에 출력하면 `e`가 키보드의 키를 읽어 입력한 타자가 그대로 콘솔에 출력되는 것을 확인할 수 있습니다.
```js
	function handleChange(e) {
        console.log(e.target.value);
    }
```
![](https://velog.velcdn.com/images/1109_haeun/post/4ce8e812-9733-4e2e-881e-b0d73bc2dc49/image.png)

하지만 `value`값을 수정하지 않았기 때문에 `input`부분은 여전히 빈 칸으로 보여지고 있는데요, 이제는 타이핑이 감지될 때마다 `setName`함수를 이용하여 `value`값을 계속 바꿔주어야 합니다.

<br>

### 3.3. 상태 업데이트
아까 콘솔에 출력한 `e.target.value`를 그대로 `name`의 값으로 변경해주면 `input`의 `value`의 값이 바뀌며 타이핑한 텍스트가 나타나게 됩니다.
```js
	function handleChange(e) {
        setName(e.target.value);
    }
```
![](https://velog.velcdn.com/images/1109_haeun/post/f8fa2983-25c5-41c5-870c-00e4e95bbd84/image.png)
```js
    function handleSubmit(e) {
        e.preventDefault();
        props.addTask(name);
        setName("");
    }
```
타이핑 후 `Add`버튼을 누르면 해당 텍스트가 알림에 뜨게 되며, 확인을 누를 시 `input`의 텍스트가 초기화됩니다.
![](https://velog.velcdn.com/images/1109_haeun/post/5da16cbe-bf14-4d27-9116-9c7e5f524744/image.png)

<br>

## 4. 할 일(작업) 추가하기
이제는 실제로 입력한 할 일이 브라우저에 추가가 되도록 구현을 해보도록 하겠습니다.

### 4.1. 상태로서의 작업
`App.js`를 다음과 같이 수정합니다.
```js
import React, { useState } from "react";
// ...
function App(props) {
  const [tasks, setTasks] = useState(props.tasks);
  const taskList = tasks.map((task) => ( 
    <Todo 
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id} 
    /> 
  ));
// ...
```
`useState`에서 `props.tasks`를 `tasks`라는 변수로 새로 설정하였으므로 `props.tasks.map`이 아닌 `tasks.map`으로 되어 있는 것을 확인할 수 있습니다.

<br>

### 4.2. 작업 추가
그러나 한 가지 문제가 있습니다. addTask()의 name 인수를 setTasks로 전달할 수 없습니다. tasks는 객체의 배열이고 name은 문자열이기 때문입니다. 이렇게 하면, 배열이 문자열로 대체됩니다.

따라서 객체의 배열을 하나 만들어 기존 `tasks` 뒤에 붙여넣어야 합니다. 아래처럼요.
```js
function addTask(name) {
  const newTask = { id: "id", name, completed: false };
  setTasks([...tasks, newTask]);
}
```
이제 브라우저에도 할 일이 추가가 됩니다.
![](https://velog.velcdn.com/images/1109_haeun/post/982c61a7-f000-4e76-9f73-3eca3a379137/image.png)
그러나 해당 `mdn`의 자료를 착실히 따라하신 분들이라면 문제점이 있다는 것을 발견할 수 있을 것입니다.
맞습니다. 바로 새로 추가되는 `id`가 전부 동일하다는 것입니다.

> 고유한 식별자를 만들기 위해서 우리는 `JavaScript`의 라이브러리인 **nanoid**를 사용하겠습니다.

```bash
npm install nanoid
```
yarn을 사용하는 경우에는 `yarn add nanoid`를 사용하시면 됩니다.

<br>

### 4.3. `nanoid`로 고유한 식별자 생성
`App.js`에 해당 코드를 추가합니다.
```js
import { nanoid } from "nanoid";
// ...
  function addTask(name) {
    const newTask = { id: `todo-${nanoid()}`, name, completed: false };
    setTasks([...tasks, newTask]);
  }
```
`id`를 콘솔로 찍어보면 이렇게 고유한 아이디가 생성되는 것을 확인할 수 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/65f4d481-8cea-42fc-a436-a5a6ed3f1fd6/image.png)

<br>

## 5. 작업 개수 세기
그런데 작업을 추가했는데도 여전히 `3 tasks remaining`인 것이 걸립니다.
이번에는 내가 현재 추가한 작업의 개수를 세어서 나타내보겠습니다.

간단합니다. 현재 추가된 `task`가 들어있는 `taskList`의 길이만 구해주면 됩니다.
저는 디테일을 추가해서 작업이 `1`개일 경우 `task`, 여러 개일 경우 `tasks`로 텍스트를 나타내주겠습니다.
```js
  const tasksNoun = taskList.length !== 1 ? "tasks" : "task";
  const headingText = `${taskList.length} ${tasksNoun} remaining`;
  return (
    <div className="todoapp stack-large">
      <h1>{props.name} 컴포넌트화 완성</h1>
      <Form addTask={addTask}/>
      <div className="filters btn-group stack-exception">
        {btnNameList}
      </div>
      <h2 id="list-heading">{headingText}</h2>
```
![](https://velog.velcdn.com/images/1109_haeun/post/be364b2b-f7ec-4652-88b8-2fe48eb423ea/image.png)

<br>

## 6. 작업 완료
이렇게 추가한 작업들은 체크박스로 작업을 완료했다는 것을 나타낼 수 있습니다.
하지만 여기에는 보이지 않는 에러가 있습니다.
바로 체크박스가 체크되어도 해당 할 일의 상태는 완료로 바뀌지 않는다는 것입니다!

보이지 않은 에러를 확인하기 위해 `App.js`에 아래의 코드를 추가합니다.
```js
  function toggleTaskCompleted(id) {
    console.log(tasks[0]);
  }

  const taskList = tasks.map((task) => ( 
    <Todo 
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id}
      toggleTaskCompleted={toggleTaskCompleted}
    /> 
  ));
// ... 
```

`Todo.js`에도 아래와 같은 코드를 추가합니다.
```js
                <input 
                    id={props.id}
                    type="checkbox"
                    defaultChecked={props.completed} 
                    onChange={() => props.toggleTaskCompleted(props.id)}
                    />
```
보시는 것처럼 `Eat` 할 일의 체크박스가 해제되었는데도 `completed` 요소가 `true`로 나타나고 있는 문제를 확인할 수 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/4d9db7a0-6a16-48bb-8974-653a703ae909/image.png)

<br>

## 7. 브라우저와 데이터 동기화
그럼 이제 체크가 해제되면 `completed`값이 `false`가 나타날 수 있도록 수정을 해주도록 하겠습니다.

`toggleTaskCompleted`함수를 아래와 같이 업데이트합니다.
```js
  function toggleTaskCompleted(id) {
    const updatedTasks = tasks.map((task) => {
      // 만약 이 할 일이 수정된 할 일로부터 같은 id를 가지고 있다면
      if ( id === task.id ) {
        return {...task, completed: !task.completed};
      }
      return task;
    });
    setTasks(updatedTasks);
// ...
```

<br>

## 8. 작업 삭제
작업을 삭제하는 것을 `completed` 상태를 변화시키는 것과 상당히 유사합니다.

### 8.1. `deleteTask` 콜백 `props`
```js
  function deleteTask(id) {
    console.log(id);
  }

  const taskList = tasks.map((task) => ( 
    <Todo 
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id}
      toggleTaskCompleted={toggleTaskCompleted}
      deleteTask={deleteTask}
    /> 
  ));
```
`Todo.js`에서 다음과 같이 `Delete` 버튼을 수정합니다.
```js
                <button
                    type="button" 
                    className="btn btn__danger"
                    onClick={() => props.deleteTask(props.id)}
                    >
                    Delete <span className="visually-hidden">
                        {props.name}
                    </span>
                </button>
```
`Eat`부터 순서대로 `Delete`버튼을 누르게 되면 해당 `id`가 정상적으로 나타나는 것을 확인할 수 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/7928e075-da98-4997-b540-c695233d8b27/image.png)

<br>

## 9. 상태 및 UI에서 작업 삭제
`deleteTask`는 정상적으로 동작하는 것을 알게 되었으니 이제 실제로 `UI`에서 지워보도록 하겠습니다.

우리는 `id` `props`가 `deleteTask`에 전달된 `id`와 동일할 경우 새 배열에서 해당 `id`를 가진 작업을 제외할 것입니다.

`App.js`에서 `deleteTask`함수를 업데이트니다.
```js
  function deleteTask(id) {
    const remainingTasks = tasks.filter((task) => id !== task.id );
    setTasks(remainingTasks);
  }
```
삭제할 할 일의 `id`가 아닌 객체만 골라 `remainingTasks`에 새 배열을 할당하고 그 것을 `setTasks`에 재할당하여 `UI`에서 제거할 수 있는 코드입니다.

`Eat`할 일을 제거하니 `UI`에서 사라진 모습입니다.
![](https://velog.velcdn.com/images/1109_haeun/post/bf302234-03cb-4cb3-a0c4-dbfba36c2fbf/image.png)

<br>

![](https://velog.velcdn.com/images/1109_haeun/post/aa987dae-542c-4373-a88b-79db069a4909/image.png)
![](https://velog.velcdn.com/images/1109_haeun/post/69ff3324-7a8f-4d46-acf8-412fc866c674/image.png)


---
이번에는 `React`가 이벤트를 처리하고 상태를 처리하고, 작업을 추가 및 삭제하는 방법에 대해 배웠습니다.
오류가 난 부분이 있다면 알려주시면 감사하겠습니다.
읽어주셔서 감사합니다.

<br>

***