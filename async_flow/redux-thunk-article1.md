# A Dummy's Guide to Redux and Thunk in React

If, like me, you've [read the Redux docs](https://redux.js.org/), [watched Dan's videos](https://egghead.io/courses/getting-started-with-redux), [done Wes' course](https://www.learnredux.com/) and still not quite grasped how to use Redux, hopefully this will help.

もし私のように、Redux のドキュメントを読んで、Dan のヴィデオコースを見て、Wes のコースも終わらせたのに、それでもまだ完全には Redux を理解できているのではれば、本記事がお役に立てるかもしれません。

It took me a few attempts at using Redux before it clicked, so I thought I'd write down the process of converting an existing application that fetches JSON to use Redux and[Redux Thunk](https://github.com/gaearon/redux-thunk). If you don't know what Thunk is, don't worry too much, but we'll use it to make asynchronous calls in the "Redux way".

This tutorial assumes you have a basic grasp of React and ES6/2015, but it should hopefully be easy enough to follow along regardless.

## The non-Redux way

Let's start with creating a React component in`components/ItemList.js`to fetch and display a list of items.

