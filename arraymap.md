# array.map

```js
let todoList = [
    { id:1, text:'text1', status:'normal'},
    { id:2, text:'text2', status:'normal'},
    { id:3, text:'text3', status:'normal'},
]

const todoId = 1

return todoList.map((todo)=>{
  if(todo.id===action.todoId) todo['status'] = action.newStatus
  return todo
}
```