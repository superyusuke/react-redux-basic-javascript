[https://github.com/superyusuke/react-simple-todo-list](https://github.com/superyusuke/react-simple-todo-list)

```js
import React, { Component } from 'react'
import Todo from './Todo'

export default class TodoList extends Component {
  render () {
    // アクション情報を取得
    const actions = {
      destoryTodo: this.props.destoryTodo,
      updateTodo: this.props.updateTodo,
    }

    // TODO情報を取得
    const todos = this.props.todos

    // 各TODO情報をを作成
    const notStartedTodos = todos.filter((todo) => todo.status === 1).map((todo, index) => {
        return <li key={index}><Todo {...actions} {...todo} /></li>
      })
    const inProgressTodos = todos.filter((todo) => todo.status === 2).map((todo, index) => {
        return <li key={index}><Todo {...actions} {...todo} /></li>
      })
    const doneTodos = todos.filter((todo) => todo.status === 3).map((todo, index) => {
        return <li key={index}><Todo {...actions} {...todo} /></li>
      })

    // 各TODOを表示
    return (
      <div className="TodoList">
        <h2>未着手のTODO</h2>
        <ul>
          {notStartedTodos}
        </ul>
        <h2>進行中のTODO</h2>
        <ul>
          {inProgressTodos}
        </ul>
        <h2>完了のTODO</h2>
        <ul>
          {doneTodos}
        </ul>
      </div>
    )
  }
}
```



