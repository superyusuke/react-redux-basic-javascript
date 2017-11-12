# A Dummy's Guide to Redux and Thunk in React

If, like me, you've [read the Redux docs](https://redux.js.org/), [watched Dan's videos](https://egghead.io/courses/getting-started-with-redux), [done Wes' course](https://www.learnredux.com/) and still not quite grasped how to use Redux, hopefully this will help.

もし私のように、Redux のドキュメントを読んで、Dan のヴィデオコースを見て、Wes のコースも終わらせたのに、それでもまだ完全には Redux を理解できていないのではれば、本記事がお役に立てるかもしれません。

It took me a few attempts at using Redux before it clicked, so I thought I'd write down the process of converting an existing application that fetches JSON to use Redux and [Redux Thunk](https://github.com/gaearon/redux-thunk). If you don't know what Thunk is, don't worry too much, but we'll use it to make asynchronous calls in the "Redux way".

Redux をきっちり理解できる前に、Redux を使うのはかなり難しいと思うので、この記事では、既存の JSON を fetch するアプリケーションを書き換えて、Redux と Redux Thunk を使ったものへとする過程を記します。Thunk が何か知らなくてもあまり気にしなくても大丈夫です。 Redux thunk を使って "Redux way" で、非同期通信をしていきます。

This tutorial assumes you have a basic grasp of React and ES6/2015, but it should hopefully be easy enough to follow along regardless.

このチュートリアルは、React, ES6/2015 の基礎的な理解をしている前提で進めていきますが、しかし十分な理解がない場合でも読み進めれることができるように配慮しました。

## The non-Redux way

Let's start with creating a React component in`components/ItemList.js`to fetch and display a list of items.

