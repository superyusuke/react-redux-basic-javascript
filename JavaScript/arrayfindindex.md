## array.findIndex

1. .findIndex は array に対して使用することができる。
2. array の要素に対して、冒頭から順番に callback を適用していき、true を返した際に、その要素の配列内での index を返す。

次の例は、予め用意された配列 array の冒頭から、targetId である「3」を探していき、見つかった時点で findIndex のコールバックが true を返し、結果として findIndex が true になった時点の配列内の index を返し、targetIdIndex に代入される。

.findIndex 内の callback は最大で3つの引数を取ることができ、先頭から

1. その時点での捜査対象になっている配列の内容
2. その内容の、配列内での index
3. 配列全体

となっている。

```js
const array = [100, 3, 2, 5]
const targetId = 3

const targetIdIndex = array.findIndex((todo, index, array)=>{
    if(todo===targetId) {
        return true
    }
})

console.log(targetIdIndex)
```

上記の例では、引数を3つ定義しているが、実際に使用しているのは一つ目の引数である todo = 「その時点での捜査対象になっている配列の内容」 だけである。

## より簡潔な記法

### 値の評価と boolean の return を一度に行う

一つ目の例では、if文を用いて、todo と targetId が一致しているかどうかをチェックし、一致している場合に true を返していた。しかし、同じ内容は次のように短く書くことができる。何故なら「 todo===targetId 」自体が boolean 値であるからだ。

```js
const array = [100, 3, 2, 5]
const targetId = 3

const targetIdIndex = array.findIndex((todo, index, array)=>{
    return todo===targetId
})

console.log(targetIdIndex)
```

### callback 内の return 自体を省略する

さらに、ES2015 以降 のアロー関数では、callback の statement を { } で囲わず、statement  が 一行だけ書かれている場合\(正確には { } で statement を囲わない場合、statement は一行しか書けない\)、statement の値を return する。

そのため、次のように書くことでさらに短く書くことができる。

findIndex の callback 内の statement である「todo===targetId」は評価され boolean を返すので、この boolean をfindIndex に対して return する。

```js
const array = [100, 3, 2, 5]
const targetId = 5

const targetIdIndex = array.findIndex((todo, index, array)=>todo===targetId)

console.log(targetIdIndex)
```

## ragate  資料内での実際の使われ方

[redux の reducer 作成時の資料から引用・改変](https://ragate-inc.gitbooks.io/redux/content/TODOアプリ/reducer.html)

Todo リストの内容を保持する配列である state から、該当する id を持つオブジェクトが、配列内で何番目にあるのかを .findIndex を用いて捜査している。

```js
let state = [
    { id:1, text:'text1'},
    { id:2, text:'text2'},
    { id:3, text:'text3'},
]

const action = {
    todoId: 3,
}

const targetTodoIndex = state.findIndex((todo, index, array)=>todo.id===action.todoId)

console.log(targetTodoIndex)
```

冗長だがわかりやすく書くと、次のようになる。

```js
let state = [
    { id:1, text:'text1'},
    { id:2, text:'text2'},
    { id:3, text:'text3'},
]

const action = {
    todoId: 2,
}

const targetTodoIndex = state.findIndex((todo, index, array)=>{
    if(todo.id===action.todoId) {
        return true
    }
})

console.log(targetTodoIndex)
```



