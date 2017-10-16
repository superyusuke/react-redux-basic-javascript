```js
$ yarn run eject
```

```
$ yarn eject
```

config/webpack.config.dev.js - 166行目あたり \([ragate 社様資料](https://ragate-inc.gitbooks.io/reactjs/content/)から引用\)

```
{
            test: /\.css$/,
            use: [
              require.resolve('style-loader'),
              'css-loader?importLoaders=1&modules&localIdentName=[path]___[name]__[local]___[hash:base64:5]',
              // {
              //   loader: require.resolve('css-loader?modules'),
              //   options: {
              //     importLoaders: 1,
              //   },
              // },
```



