コードが動くか要確認



ほほｈ

{...todo}とすると、todoオブジェクトを展開して、渡せる。

```js
const todo = {
    id: 1,
    text: 'text'
}

const Todo = () => {
    return (
        <div>
            <h2>{id}</h2>
            <p>{text}</p>    
        </div>
    )
}

<li key={index}>
    <Todo {...todo} />
</li>

//<Todo todo={todo}} />と同じ
// つまりpropsとして渡す、かつ展開している
```


