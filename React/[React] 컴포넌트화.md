# 리액트 앱 컴포넌트화 
## 1. `<Todo />` 만들기
현재는 html로 하드코딩하여 하나씩 나열한 상태입니다.
따라서 보기에는 문제가 없지만 코드 가독성이 떨어진다는 단점이 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/bf9b2595-ff5b-4c3b-886b-eee7c9db3c3b/image.png)

이러한 상태에서 `Todo.js`를 만들고
```js
export default function Todo() {
  return (
    <li className="todo stack-small">
      <div className="c-cb">
        <input id="todo-0" type="checkbox" defaultChecked={true} />
        <label className="todo-label" htmlFor="todo-0">
          Eat
        </label>
      </div>
      <div className="btn-group">
        <button type="button" className="btn">
          Edit <span className="visually-hidden">Eat</span>
        </button>
        <button type="button" className="btn btn__danger">
          Delete <span className="visually-hidden">Eat</span>
        </button>
      </div>
    </li>
  );
}
```
`App.js`에서 우리가 만든 `Todo.js`를 사용하게 하기 위해 `import`를 해준 후 `<Todo />`를 세 개 추가하면 보다 가독성 있는 코드를 작성할 수 있습니다.
```js
import Todo from "./components/Todo";
// ...
<ul
  role="list"
  className="todo-list stack-large stack-exception"
  aria-labelledby="list-heading">
  <Todo />
  <Todo />
  <Todo />
</ul>
```
![](https://velog.velcdn.com/images/1109_haeun/post/f5835643-8a82-4ad0-8b9e-7c2a0f42f1dc/image.png)

<br>

### 1.1. TodoList의 할 일 이름 구분하기 `(name)`
하지만 보다시피 `Eat`이 세 번 반복되고 있습니다.
따라서 우리는 `Todo`에 매개변수를 추가하여 각자 구분된 객체로 관리할 것입니다.

`name`에 할 일의 이름을 적어 `Todo`로 보내봅시다.
```html
	<ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading"
        > //  브라우저가 스크린 리더 사용자에게 전달해야 할 내용을 상황에 따라 적어야 할 때 사용 https://velog.io/@a_in/WAI-ARIA-role-aria-label
        <Todo name="Eat" />
        <Todo name="Sleep" />
        <Todo name="Repeat" />
      </ul>
```
그럼 `Todo.js`에서 `props`로 매개변수를 받아 `name`을 사용할 수 있게 됩니다.
```js
import React from "react";

function Todo(props) {
    return (
        <li className="todo stack-small">
            <div className="c-cb">
                <input id="todo-0" type="checkbox" defaultChecked={true} />
                <label className="todo-label" htmlFor="todo-0">
                    {props.name}
                </label>
            </div>
            <div className="btn-group">
                <button type="button" className="btn">
                    Edit <span className="visually-hidden">Eat</span>
                </button>
                <button type="button" className="btn btn__danger">
                    Delete <span className="visually-hidden">Eat</span>
                </button>
            </div>
        </li>
    );
}

export default Todo;
```
![](https://velog.velcdn.com/images/1109_haeun/post/eaddfd1c-ef60-4888-b649-621a8aaa6490/image.png)

<br>

### 1.2. 체킹 여부 관리하기 `(completed)`
이번에는 `Eat`에만 초반에 체크가 되어 있었으면 좋겠습니다.
이럴 때도 매개변수를 사용하여 핸들링합니다.

`completed`라는 이름의 매개변수를 가지고 `Eat`에만 `true`, 나머지 할 일에는 `false`를 주겠습니다.
```html 
	<ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading"
        >
        <Todo name="Eat" completed={true}/>
        <Todo name="Sleep" completed={false}/>
        <Todo name="Repeat" completed={false}/>
      </ul>
```
그럼 또 `input`의 `defaultChecked`에 `props`의 `completed`를 넣어주면 됩니다.
```js
import React from "react";

function Todo(props) {
    return (
        <li className="todo stack-small">
            <div className="c-cb">
                <input id="todo-0" type="checkbox" defaultChecked={props.completed} />
                <label className="todo-label" htmlFor="todo-0">
                    {props.name}
                </label>
            </div>
            <div className="btn-group">
                <button type="button" className="btn">
                    Edit <span className="visually-hidden">Eat</span>
                </button>
                <button type="button" className="btn btn__danger">
                    Delete <span className="visually-hidden">Eat</span>
                </button>
            </div>
        </li>
    );
}

export default Todo;
```
이렇게 `Eat`에만 체크가 된 모습을 확인할 수 있습니다.
![](https://velog.velcdn.com/images/1109_haeun/post/190a1ebb-fc27-4a47-9bd0-4f9a90b76c64/image.png)

<br>

### 1.3. 고유한 `id`값 관리하기 (id) 
`html`의 요소들은 각각 고유한 `id`값을 가져야합니다.
하지만 지금 `<Todo />`의 `id`는 모두 동일한 `todo-0`입니다.

따라서 매개변수를 이용하여 `id`값도 전달해주겠습니다.
```html
	<ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading"
        >
        <Todo name="Eat" completed={true} id="todo-0" />
        <Todo name="Sleep" completed={false} id="todo-1" />
        <Todo name="Repeat" completed={false} id="todo-2" />
      </ul>
```
`input`의 `id`값에 `props`의 `id`를 변수로 주게 된다면 각각 다른 `id`값을 가지게 되어 구분될 것입니다. 
```js
import React from "react";

function Todo(props) {
    return (
        <li className="todo stack-small">
            <div className="c-cb">
                <input id={props.id} type="checkbox" defaultChecked={props.completed} />
                <label className="todo-label" htmlFor="todo-0">
                    {props.name}
                </label>
            </div>
            <div className="btn-group">
                <button type="button" className="btn">
                    Edit <span className="visually-hidden">Eat</span>
                </button>
                <button type="button" className="btn btn__danger">
                    Delete <span className="visually-hidden">Eat</span>
                </button>
            </div>
        </li>
    );
}

export default Todo;
```
그런데 `App.js`에서 `<Todo />`는 `props`, 즉 매개변수만 달라지는 것 뿐이지 사실상 `<Todo />`가 세 번 반복되는 반복 작업입니다.
우리는 이것을 `js`로 간소화시킬 수 있습니다.

<br>

### 1.4. 할 일을 데이터로
`index.js`에서 `DATA`라는 변수를 하나 만들고 그 데이터 안에 우리가 줄 매개변수 데이터를 한 데 모아 `<App />`에 `tasks`라는 변수로 전달합니다.

이렇게 된다면 `App.js`를 거칠 필요없이 바로 `App.js`에서 데이터를 받아 렌더링할 수 있는 것입니다. 그렇게 된다면 세 번 반복하던 작업도 필요없어질 수 있겠죠.
```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const DATA = [
  { id: "todo-0", name: "Eat", completed: true },
  { id: "todo-1", name: "Sleep", completed: false },
  { id: "todo-2", name: "Repeat", completed: false }
];

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App name="노하은" tasks={DATA} />
  </React.StrictMode>
);
```

<br>

### 1.5. 반복으로 렌더링
이제는 `App.js`에서 받아온 `tasks`데이터를 통해 반복으로 렌더링하는 작업을 해보겠습니다.
```js
function App(props) {
  const taskList = props.tasks?.map((task) => task.name );
  return (
    // ...
```
위의 코드를 설명하자면
- `props`객체에서 `tasks` 속성을 가져옵니다.
- `tasks`가 존재한다면 `(tasks?)` 각각의 `tasks`에 대한 `map`함수를 실행합니다.
- `map`이 실행되면 각 `task`의 `name`이라는 속성을 추출합니다.
- 최종적으로는 `taskList`에 새롭게 추출한 `task.name` 속성값이 들어가있는 새로운 형태의 배열이 만들어집니다.

이렇게 `name`이 모여진 `taskList`를 `ul`안에 넣게 되면 텍스트만 나열될 것입니다.
```html
	<ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading">
        {taskList}
      </ul>
```
![](https://velog.velcdn.com/images/1109_haeun/post/11c6f60f-4316-4098-bf8c-73bbb5a286eb/image.png)

이렇게 하면 현재 할 일의 이름이 구조화되지 않은 텍스트로 렌더링됩니다.
`li` `checkbox` `button`이 누락되었기 때문입니다.

이를 해결하려면 `map()`에서 `<Todo />` 컴포넌트를 반환해야 합니다.
```js
function App(props) {
  const taskList = props.tasks?.map((task) => <Todo /> );
  return (
    // ... 
```
![](https://velog.velcdn.com/images/1109_haeun/post/5ddfbd59-21b5-431e-8328-56630848ec76/image.png)

이제 형태는 갖춰졌지만 할 일, 체크여부가 누락되었습니다.
`<Todo />`를 전달할 때는 각 컴포넌트의 `id` `name` `completed` 속성을 같이 전달해주어야 합니다.
```js
function App(props) {
  const taskList = props.tasks.map((task) => ( 
    <Todo id={task.id} name={task.name} completed={task.completed} /> 
  ));
  return (
    // ... 
```
![](https://velog.velcdn.com/images/1109_haeun/post/09f76197-25a7-4e6f-bdb8-57275ad33cb7/image.png)

### 1.6. 고유키`(key)`
`key`는 React의 예약어라고 보면 됩니다.
> key란? React가 관리하는 특별한 props로 다른 용도로 key라는 단어를 사용할 수 없다.

`key`는 고유해야 하므로 각 `task` 객체의 `id`를 `key`로 재사용할 것입니다.
```js
function App(props) {
  const taskList = props.tasks.map((task) => ( 
    <Todo 
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id} 
    /> 
  ));
  return (
    // ... 
```

중요!
> **반복문으로 렌더링하는 모든 것에 고유한 키를 전달해야 합니다.**

브라우저에서 보이는 것에 영향을 주지는 않지만 고유한 키를 사용하지 않는다면 React가 콘솔에는 경고를 기록하고 앱이 이상하게 작동할 수도 있게 됩니다.

## 2. `Form.js`, `FilterButton.js` 컴포넌트화 하기
이제 `Todo.js(li)`는 컴포넌트화를 마쳤습니다.
하지만 아직 `form태그`와 `button태그`의 컴포넌트화가 되지 않았습니다.
이것들도 마저 컴포넌트화를 해주도록 하겠습니다.

```js
// Form.js
import React from "react";

function Form(props) {
    return (
        <form>
            <h2 className="label-wrapper">
                <label htmlFor="new-todo-input" className="label__lg">
                    What needs to be done?
                </label>
            </h2>
            <input
                type="text"
                id="new-todo-input"
                className="input input__lg"
                name="text"
                autoComplete="off"
            />
            <button type="submit" className="btn btn__primary btn__lg">
                Add
            </button>
        </form>
    );
}

export default Form;
```
```js
// FilterButton.js
import React from "react";

function FilterButton(props) {
    return (
        <button type="button" className="btn toggle-btn" aria-pressed="true">
            <span className="visually-hidden">Show </span>
            <span>{props.name}</span>
            <span className="visually-hidden"> tasks</span>
        </button>
    );
}

export default FilterButton;
```
```js
// App.js
import Todo from "./components/Todo";
import Form from "./components/Form";
import FilterButton from "./components/FilterButton";

function App(props) {
  console.log(props.buttons);
  const taskList = props.tasks.map((task) => ( 
    <Todo 
      id={task.id}
      name={task.name}
      completed={task.completed}
      key={task.id} 
    /> 
  ));
  const btnNameList = props.buttons.map((btn) => (
    <FilterButton
      name={btn.name}
      key={btn.id}
    />
  ));
  return (
    <div className="todoapp stack-large">
      <h1>TodoMatic for {props.name}</h1>
      <Form />
      <div className="filters btn-group stack-exception">
        {btnNameList}
      </div>
      <h2 id="list-heading">3 tasks remaining</h2>
      <ul
        role="list"
        className="todo-list stack-large stack-exception"
        aria-labelledby="list-heading"//  브라우저가 스크린 리더 사용자에게 전달해야 할 내용을 상황에 따라 적어야 할 때 사용 https://velog.io/@a_in/WAI-ARIA-role-aria-label
        >
        {taskList}
      </ul>
    </div>
  );
}

export default App;
```
```js
// index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const DATA = [
  { id: "todo-0", name: "Eat", completed: true },
  { id: "todo-1", name: "Sleep", completed: false },
  { id: "todo-2", name: "Repeat", completed: false }
];

const BTN = [
  { id: "btn-0", name: "All" },
  { id: "btn-1", name: "Active" },
  { id: "btn-2", name: "Completed" }
];

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App name="노하은" tasks={DATA} buttons={BTN}/>
  </React.StrictMode>
);
```
이렇게 `All` `Active` `Completed` 버튼과 각 `tasks`의 컴포넌트화를 마쳤습니다. 
![](https://velog.velcdn.com/images/1109_haeun/post/1fdf833a-affa-4dce-b296-10f305111a82/image.png)
![](https://velog.velcdn.com/images/1109_haeun/post/c2a18ae0-a773-4ed1-8c88-43619bbe3f35/image.png)

<br>

---
이렇게 `React`의 컴포넌트화를 간략하게 공부해보았습니다.
오류가 있는 부분은 알려주시면 감사하겠습니다.
봐주셔서 감사합니다.

<br>

***