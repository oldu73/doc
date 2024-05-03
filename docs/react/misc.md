# misc

---

## Introduction

Three essential React concepts:

- components  
- props  
- state

---

## New Project

Setup a new `React` project:

```console
npx create-react-app myapp

cd myapp

npm start
```

---

## props useState

In children component, use one liner `export default function...` syntax at declaration instead of `export` in a separate statement.

App.js:

```js
import { useState } from "react";
import AddTodo from "./components/AddTodo";
import TodoList from "./components/TodoList";

export default function App() {
  const [todoList, setTodoList] = useState([]);

  // function to pass as props
  function addTodo(content) {
    const todo = { id: crypto.randomUUID(), done: false, edit: false, content };
    console.log(todo);
    setTodoList([...todoList, todo]);
  }

  return (
    <div className="d-flex flex-row justify-content-center align-items-center p-20">
      <div className="card container p-20">
        <h1 className="mb-20">Todo list</h1>
        // children call with function as props
        <AddTodo addTodo={addTodo} />
        <TodoList />
      </div>
    </div>
  );
}
```

AddTodo.js:

```js
import { useState } from "react";

// Use this one liner export statement!
export default function AddTodo({ addTodo }) {
  const [value, setValue] = useState("");

  function handleChange(e) {
    const inputValue = e.target.value;
    setValue(inputValue);
  }

  function handleKeyDown(e) {
    if (e.key === "Enter" && value.length) {
      addTodo(value);
      setValue("");
    }
  }

  function handleClick() {
    if (value.length) {
      addTodo(value);
      setValue("");
    }
  }

  return (
    <div className="d-flex justify-content-center align-items-center mb-20">
      <input
        type="text"
        onChange={handleChange}
        onKeyDown={handleKeyDown}
        value={value}
        className="mr-15 flex-fill"
        placeholder="Ajouter une tâche"
      />
      <button onClick={handleClick} className="btn btn-primary">
        Ajouter
      </button>
    </div>
  );
}
```

---
