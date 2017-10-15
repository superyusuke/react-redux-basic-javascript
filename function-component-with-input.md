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

以下の形で input という変数に対して DOM が紐付けられる。紐付ける変数は予め let で宣言しておく。

input.value でテキストの値が取れ、input.value = '文字列' で文字列を与えられる。 

```js
<input ref={(i) => input = i}/>
```



