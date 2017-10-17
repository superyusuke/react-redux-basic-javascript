[https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md\#github-pages](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#github-pages)

大前提として、GitHubPages でリポジトリを管理していること。

package.json を開いて、homepage field を追加する。myusername は GitHub のユーザーネーム, my-app の部分は、リポジトリ名。

```
  "homepage": "https://myusername.github.io/my-app",
```

必要な npm をインストールする

```
$ yarn add gh-pages
```

deploy 用の npm スクリプトを追加する。

```
package.json: に以下２つを追加

  "scripts": {
+   "predeploy": "npm run build",
+   "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

deploy する。

```
$ npm run deploy
```

GitHub → setting → GitHub Pages → source →gh-pages branch に変更。



