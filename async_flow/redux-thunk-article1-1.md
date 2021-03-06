# A Dummy's Guide to Redux and Thunk in React

[Original article](https://codepen.io/stowball/post/a-dummy-s-guide-to-redux-and-thunk-in-react)

If, like me, you've [read the Redux docs](https://redux.js.org/), [watched Dan's videos](https://egghead.io/courses/getting-started-with-redux), [done Wes' course](https://www.learnredux.com/) and still not quite grasped how to use Redux, hopefully this will help.

もし私のように、Redux のドキュメントを読んで、Dan のヴィデオコースを見て、Wes のコースも終わらせたのに、それでもまだ完全には Redux を理解できていないのではれば、本記事がお役に立てるかもしれません。

It took me a few attempts at using Redux before it clicked, so I thought I'd write down the process of converting an existing application that fetches JSON to use Redux and [Redux Thunk](https://github.com/gaearon/redux-thunk). If you don't know what Thunk is, don't worry too much, but we'll use it to make asynchronous calls in the "Redux way".

Redux をきっちり理解できる前に、Redux を使うのはかなり難しいと思うので、この記事では、既存の JSON を fetch するアプリケーションを書き換えて、Redux と Redux Thunk を使ったものへと変更していく過程を記します。Thunk が何かを知らなくてもあまり気にしなくて大丈夫です。 Redux thunk は "Redux way" で非同期通信をするために用います。

This tutorial assumes you have a basic grasp of React and ES6/2015, but it should hopefully be easy enough to follow along regardless.

このチュートリアルは、React, ES6/2015 の基礎的な理解をしている前提で進めていきますが、しかし十分な理解がない場合でも読み進めれることができるように配慮しました。

## The non-Redux way

Redux を使わない方法

Let's start with creating a React component in`components/ItemList.js`to fetch and display a list of items.

ではまずは `components/ItemList.js` の中に、fetch をしてそれを元にアイテムのリストを表示するコンポーネントを作っていきましょう。

### Laying the foundations

基礎となるレイヤーを作成する

First we'll setup a static component with a state that contains various items to output, and 2 boolean states to render something different when it's loading or errored respectively.

まずは static なコンポネーントを作っていきます。このコンポーネントは、state に、出力するアイテムのための情報、それから2つのブーリアンを持ちます。この boolean は、それぞれローディング時とエラーが発生した時に異なる内容を表示するために必要な state です。

(訳注:GitHub に各段階のソースが公開されているので活用ください\)

```js
import React, { Component } from 'react';

class ItemList extends Component {
    constructor() {
        super();

        this.state = {
            items: [
                {
                    id: 1,
                    label: 'List item 1'
                },
                {
                    id: 2,
                    label: 'List item 2'
                },
                {
                    id: 3,
                    label: 'List item 3'
                },
                {
                    id: 4,
                    label: 'List item 4'
                }
            ],
            hasErrored: false,
            isLoading: false
        };
    }

    render() {
        if (this.state.hasErrored) {
            return <p>Sorry! There was an error loading the items</p>;
        }

        if (this.state.isLoading) {
            return <p>Loading…</p>;
        }

        return (
            <ul>
                {this.state.items.map((item) => (
                    <li key={item.id}>
                        {item.label}
                    </li>
                ))}
            </ul>
        );
    }
}

export default ItemList;
```

It may not seem like much, but this is a good start.

まだ十分に色々なものが揃ってはいませんが、始めるにあたって最低限のものはあります。

When rendered, the component should output 4 list items, but if you were to set isLoading or hasErrored to true, a relevant `<p></p>` would be output instead.

レンダーが実行されると、component は4つのリストアイテムを出力します。しかし isLoading もしくは hasErrored を true にすると、4つのリストアイテムを出力する代わりに、関連する `<p></p>` が出力されます。

### Making it dynamic

Hard-coding the items doesn't make for a very useful component, so let's fetch the items from a JSON API, which will also allow us to set isLoading and hasErrored as appropriate.

ハードコンディングされたアイテム(訳注:state の中に直接アイテムの内容を記録しているので、変化しない=ハードコーディング) は、実際的ではないので、次に JSON API から JSON を fetch して、さらに isLoading と hasErrored を適切に変更するアプリケーションにしていきましょう。

The response will be identical to our hard-coded list of items, but in the real world, you could pull in a list of best-selling books, latest blog posts, or whatever suits your application.

これによって返される情報は、今回は固定されたリストですが、実際のアプリケーションでは「売れ筋書籍、最新のポスト」とうとう、自分のアプリケーションに必要なものを引っ張ってくることになります。

To fetch the items, we're going to use the aptly named Fetch API. Fetch makes making requests much easier than the classic XMLHttpRequest and returns a promise of the resolved response \(which is important to Thunk\). Fetch isn't available in all browsers, so you'll need to add it as a dependency to your project with:

item を fetch するために、Fetch API を使うことしましょう。fetch は伝統的な XMLHttpRequest よりも簡単に request を発行することができ、受け取った response の promise オブジェクトを return します。(これが thunk にとっては重要です) fetch は全てのブラウザで使うことはできないので、次のものをプロジェクトに追加しましょう。

```
npm install whatwg-fetch --save
```

The conversion is actually quite simple.

加える変更は非常にシンプルです。

1. First we'll set our initial items to an empty array []
2. Now we'll add a method to fetch the data and set the loading and error states:

1. まず item の初期値を空の array [] に変更します。(訳注:constructor の中の state)
2. そして 「Date を fetch し、loading もしくは　error の状態をセットする」メソッドを加えます。

このメソッドは次のようになります。

```js
fetchData(url) {
    this.setState({ isLoading: true });

    fetch(url)
        .then((response) => {
            if (!response.ok) {
                throw Error(response.statusText);
            }

            this.setState({ isLoading: false });

            return response;
        })
        .then((response) => response.json())
        .then((items) => this.setState({ items })) // ES6 property value shorthand for { items: items }
        .catch(() => this.setState({ hasErrored: true }));
}
```

5. Then we'll call it when the component mounts:
5. 次に上記のメソッドを、component がマウントされた際に実行します。

```js
componentDidMount() {
    this.fetchData('http://5826ed963900d612000138bd.mockapi.io/items');
}
```

そうすると全体としては次のようになります。変更がない行に関しては省きました。

```js
class ItemList extends Component {
    constructor() {
        this.state = {
            items: [],
        };
    }

    fetchData(url) {
        this.setState({ isLoading: true });

        fetch(url)
            .then((response) => {
                if (!response.ok) {
                    throw Error(response.statusText);
                }

                this.setState({ isLoading: false });

                return response;
            })
            .then((response) => response.json())
            .then((items) => this.setState({ items }))
            .catch(() => this.setState({ hasErrored: true }));
    }

    componentDidMount() {
        this.fetchData('http://5826ed963900d612000138bd.mockapi.io/items');
    }

    render() {
    }
}
```

And that's it. Your component now fetches the items from a REST endpoint! You should hopefully see "Loading…" appear briefly before the 4 list items. If you pass in a broken URL to fetchData you should see our error message.

さて、これで終わりです。これによってコンポーネントは、REST endpoint からアイテムを fetch するようになりました。4つのアイテムを表示する少し前に、短い時間ではありますが「Loading...」の文字が出ていることでしょう。もし fetechData に対して正常ではない URL を与えると、error message が表示されるはずです。

However, in reality, a component shouldn't include logic to fetch data, and data shouldn't be stored in a component's state, so this is where Redux comes in.

さて上手くはいきましたが、実際のアプリケーションにおいては、コンポーネントは data を fetch するロジックを持つべきではありませんし、また取得したデータもコンポーネントの state に持つべきではありません。ここで Redux の出番です。

## fetch 補足

[https://codepen.io/nakanishi/pen/MOmJgd?editors=1111](https://codepen.io/nakanishi/pen/MOmJgd?editors=1111)

```js
const url = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json'
fetch(url)
    .then(response => response.json())
    .then(json => console.log(json[0]))
    .then(() => {throw Error('ddd')})
```

1. まず fetch\(url\) で リクエストをする。
2. .then\(\) でつなぐ。.then\(\(response\) =&gt; {console.log\(response\)}\)
3. こうすることで、レスポンスが帰ってきたら、response に引数として渡されて、callback を実行する。
4. 何かを return するまで次の .then\(\) には移行しない。return した内容が、次の .then\(\) の引数として渡される。

## Converting to Redux

To start, we need to add Redux, React Redux and Redux Thunk as dependencies of our project so we can use them. We can do that with:

まずは redux, react-redux, redux-thunk を環境に追加します。

```
npm install redux react-redux redux-thunk --save
```

### Understanding Redux

There are a few core principles to Redux which we need to understand:

いくつか Redux の核となる原則を理解しましょう。(訳注:redux の基本なので訳さない)

1. There is 1 global state object that manages the state for your entire application. In this example, it will behave identically to our initial component's state. It is the _single source of truth_
   
2. The only way to modify the state is through emitting an action, which is an object that describes what should change. Action Creators are the functions that are `dispatch`ed to emit a change – all they do is `return` an action.
   state を変える方法唯一の方法は、action を emit することで、
   
3. When an action is
   `dispatch`ed, a Reducer is the function that actually changes the state appropriate to that action – or returns the existing state if the action is not applicable to that reducer.
4. Reducers are "pure functions". They should not have any side-effects nor mutate the state – they must return a modified copy.
5. Individual reducers are combined into a single
   `rootReducer`
   to create the discrete properties of the state.
6. The Store is the thing that brings it all together: it represents the state by using the
   `rootReducer`
   , any middleware \(Thunk in our case\), and allows you to actually
   `dispatch`
   actions.
7. For using Redux in React, the
   `<`
   `Provider /`
   `>`
   component wraps the entire application and passes the
   `store`
   down to all children.

This should all become clearer as we start to convert our application to use Redux.

これらのことを明確に理解しておきましょう。
