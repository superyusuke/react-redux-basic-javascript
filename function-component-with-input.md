[https://github.com/superyusuke/react-simple-todo-list](https://github.com/superyusuke/react-simple-todo-list)

```js
import React from 'react'

const TodoStoreForm = ({storeTodo}) => {
  let input
  let input2
  const submitHandler = (e) => {
    e.preventDefault()
    const todoTitle = input.value
    if (todoTitle) {
      storeTodo(todoTitle)
      input.value = ''
    }
    if (input2.value) {
      console.log(input2.value)
    }
  }

  return (
    <div className="TodoStoreForm">
      <form onSubmit={(e) => submitHandler(e)}>
        <input type="text" ref={(i) => input = i}/>
        <input type="text" ref={(i) => input2 = i} placeholder="just for console.log"/>
        <input type="submit" value="追加"/>
      </form>
    </div>
  )
}

export default TodoStoreForm
```

```js
<input ref={(i) => input = i}/>
```





