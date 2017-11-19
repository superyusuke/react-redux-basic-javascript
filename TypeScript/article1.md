# React and TypeScript – The Basics
React is great, and with TypeScript, it can be even better.

React は偉大です。そして TypeScript を使えば、もっと良くなります。

If you haven’t used TypeScript with React, you might be wondering how much work is required to get started, and how React development with TypeScript is different than JavaScript. I’m going to address these questions, covering everything I would have liked to find in one place when I was getting started with TypeScript—specifically, what is required to set up a React/TypeScript project, and how some of the basic React/Redux type definitions work.

React に TypeScript を組み合わせて使ったことがなかった人は、始めるためにまず何をすればいいのか疑問でしょうし、また TypeScript を使った React 開発がどのように違うのか、気になることでしょう。今回はそれらの質問に答え、TypeScript を始めるにあたって必要となることを全て紹介し、さらに React と TypeScript を組み合わせたプロジェクトに必要な環境設定と、React/Redux における型定義の基礎を紹介します。

## Project Setup

Whether you are starting from scratch or interested in migrating to TypeScript, the following will help you get your project configured correctly.

新しく TypeScript を使ったプロジェクトを開始するにせよ、既存のプロジェクトに TypeScript を追加するにせよ、以下の説明がプロジェクトの設定を正しく行うために役に立つはずです。

### webpack

Most React projects use webpack to manage their build process (transcompilation, module loading, etc.). Webpack defers to other libraries (e.g. Babel) for transpiling source JavaScript to a flavor that can run in most (or hopefully all) browsers. For example, many projects develop in ES6 or above, and then transpile down to ES5 (this page details which browsers currently support ES5).
A webpack config file that uses Babel for this type of transcompilation might look like this:


