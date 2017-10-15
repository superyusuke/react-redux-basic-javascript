一旦 return の外で変数内にJSX を入れて、その変数を return する。

```js
const Component A = ({id, date, text}) => {
    const button = <button onClick={console.log(id)}>{date}:{text}</button>
    return button
}
```

```js
const Component A = ({id, date, text}) => {
    const button = () => {
        switch (boolean) {
            case true:
                return <button onClick={console.log(id)}>{date}:{text}</button>
                
            case false:
                return <button onClick={console.log(id)}>{date}:{text}false</button>
            default:
                return <button>button</button>
        }
    }    
    return button
}

```



