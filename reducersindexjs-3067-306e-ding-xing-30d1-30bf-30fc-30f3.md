# reducers/index.js での定型パターン

```js
import { combineReducers } from 'redux'
import todos from './todos'
import todos2 from './todos2'



// Storeの作成処理
const reducer = combineReducers({
  todos,
  todos2
})

export default reducer
```



