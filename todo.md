コードが動くか要確認

ほほ

[https://codesandbox.io/s/x9p4qw2j94](https://codesandbox.io/s/x9p4qw2j94)

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



