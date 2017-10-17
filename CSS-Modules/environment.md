```js
$ yarn run eject
```

```
$ yarn eject
```

次に、webpack.config.dev.js, webpack.config.prod.js を変更する。

webpack.config.dev.js の 165 行目あたりを変更。具体的には modules: true にして、localIdenName でどのようなクラス名が着くかを指定する。

```js
{
                loader: require.resolve('css-loader'),
                options: {
                  importLoaders: 1,
                  modules: true,
                  localIdentName: "[name]__[local]___[hash:base64:5]"
                },
              },
```

同様に webpack.config.prod.js にも追加。

```js
{
                      loader: require.resolve('css-loader'),
                      options: {
                        importLoaders: 1,
                        minimize: true,
                        sourceMap: shouldUseSourceMap,
                        modules: true,
                        localIdentName: "[name]__[local]___[hash:base64:5]"
                      },
                    },
                  
```

## 基礎



```js
// app.js

import React, { Component } from 'react';
import styles from './App.css';

class App extends Component {
  render() {
    return (
      <div className={styles.app}>
        <p className={styles.blue}>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Blanditiis, harum!</p>
      </div>
    );
  }
}

export default App;
```

```css
/* App.css */
.app {
  background-color: red;
}

.app:hover {
  background-color: yellowgreen;
}

.blue {
  color: blue;
}
```



