# A Dummy's Guide to Redux and Thunk in React

If, like me, you've [read the Redux docs](https://redux.js.org/), [watched Dan's videos](https://egghead.io/courses/getting-started-with-redux), [done Wes' course](https://www.learnredux.com/) and still not quite grasped how to use Redux, hopefully this will help.

もし私のように、Redux のドキュメントを読んで、Dan のヴィデオコースを見て、Wes のコースも終わらせたのに、それでもまだ完全には Redux を理解できていないのではれば、本記事がお役に立てるかもしれません。

It took me a few attempts at using Redux before it clicked, so I thought I'd write down the process of converting an existing application that fetches JSON to use Redux and [Redux Thunk](https://github.com/gaearon/redux-thunk). If you don't know what Thunk is, don't worry too much, but we'll use it to make asynchronous calls in the "Redux way".

Redux をきっちり理解できる前に、Redux を使うのはかなり難しいと思うので、この記事では、既存の JSON を fetch するアプリケーションを書き換えて、Redux と Redux Thunk を使ったものへとする過程を記します。Thunk が何か知らなくてもあまり気にしなくても大丈夫です。 Redux thunk を使って "Redux way" で、非同期通信をしていきます。

This tutorial assumes you have a basic grasp of React and ES6/2015, but it should hopefully be easy enough to follow along regardless.

このチュートリアルは、React, ES6/2015 の基礎的な理解をしている前提で進めていきますが、しかし十分な理解がない場合でも読み進めれることができるように配慮しました。

## The non-Redux way

Redux を使わない方法

Let's start with creating a React component in`components/ItemList.js`to fetch and display a list of items.

ではまずは `components/ItemList.js` の中に、fetch をしてそれを元にアイテムのリストを表示するコンポーネントを作っていきましょう。

### Laying the foundations
基礎となるレイヤーを作成する

First we'll setup a static component with a state that contains various items to output, and 2 boolean states to render something different when it's loading or errored respectively.

まずは static なコンポネーントを作っていきます。このコンポーネントは、state に、出力するアイテムのための情報、それから 2 つのブーリアンを持ちます。この boolean は、それぞれローディング時とエラーが発生した時に異なる内容を表示するために必要な state です。

(訳注:GitHub に各段階のソースが公開されているので活用ください)

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

To fetch the items, we're going to use the aptly named Fetch API. Fetch makes making requests much easier than the classic XMLHttpRequest and returns a promise of the resolved response (which is important to Thunk). Fetch isn't available in all browsers, so you'll need to add it as a dependency to your project with:


