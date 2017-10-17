[https://codesandbox.io/s/x9p4qw2j94](https://codesandbox.io/s/x9p4qw2j94)

{...todo} とすると、todo オブジェクトを展開して、渡せる。

つまり、todo オブジェクトの中の各プロパティ \(id: 1 と text: 'text'\) \(name: value\) を、name={value} という形で渡しているのと同じことになる。

```js
// index.js
import React from "react";
import { render } from "react-dom";
import Sub1 from "./Sub1";
import Sub2 from "./Sub2";

const todo = {
  id: 1,
  text: "text"
};

const App = () => (
  <div>
    <Sub1 {...todo} />
    <Sub1 id={todo.id} text={todo.text} />
    <Sub2 todo={todo} />
  </div>
);

render(<App />, document.getElementById("root"));
```

```js
// Sub1.js
import React from "react";

const Sub1 = ({ id, text }) => {
  return (
    <span>
      <p>
        {id} {text}
      </p>
    </span>
  );
};

export default Sub1;
```

```js
// Sub2.js
import React from 'react'

const Sub2 = ({todo}) => {
  return (
    <span>
      <p>{todo.id}</p>
    </span>
  )
}

export default Sub2
```



