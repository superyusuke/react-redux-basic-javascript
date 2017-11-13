## Designing our state

state をデザインする。

From the work we've already done, we know that our state needs to have 3 properties: items, hasErrored and isLoading for this application to work as expected under all circumstances, which correlates to needing 3 unique actions.

すでに行ってきた作業により、私たちのアプリケーションのための state は、3つのプロパティを持てば良いことがわかっています。つまり、items, hasErrored, isLoading の3つのプロパティです。これらがあれば予想さされるアプリケーションの全ての状態に対応することができはずです。

Now, here is why Action Creators are different to Actions and do not necessarily have a 1:1 relationship: we need a fourth action creator that calls our 3 other action \(creators\) depending on the status of fetching the data. This fourth action creator is almost identical to our original fetchData\(\) method, but instead of directly setting the state with this.setState\({ isLoading: true }\), we'll dispatch an action to do the same: dispatch\(isLoading\(true\)\).

?Now, here is why Action Creators are different to Actions and do not necessarily have a 1:1 relationship?   
ではここからは、アクションクリエイターとアクションがなぜ違うものなのかということについて説明していきましょう。我々は4番目のアクションクリエイターが必要です。これは他の3つの action \(もしくは  action creator\) を、data の fetch の状態に応じて呼び出すものです。\(訳注:fetch を開始したら、loading を true にするアクションを呼び出し、fetch が完成したら item の state を更新する action を呼び出し、失敗したら、error 用の action を呼び出す。\) この4つめの action creator は、ほとんどこの記事の冒頭で作成した元の fetchData\(\) メソッドとほとんど同じ機能を持つものです。ただ、state を直接 this.setState\({isLoading: true}\) と変更するのではなくて、同じように state を変更する action を dispatch します。つまり dispatch\(isLoading\(true\)\)といった具合にです。

## Creating our actions

Let's create an actions/items.js file to contain our action creators. We'll start with our 3 simple actions.

では actions/items.js ファイルを作って、ここにアクションクリエイターを書いていきましょう。まずは3つのシンプルなものを書きます。

```js
export function itemsHasErrored(bool) {
    return {
        type: 'ITEMS_HAS_ERRORED',
        hasErrored: bool
    };
}

export function itemsIsLoading(bool) {
    return {
        type: 'ITEMS_IS_LOADING',
        isLoading: bool
    };
}

export function itemsFetchDataSuccess(items) {
    return {
        type: 'ITEMS_FETCH_DATA_SUCCESS',
        items
    };
}
```

As mentioned before, action creators are functions that return an action. We`export`each one so we can use them elsewhere in our codebase.

The first 2 action creators take a`bool`\(`true`/`false`\) as their argument and return an object with a meaningful`type`and the`bool`assigned to the appropriate property.

The third,`itemsFetchDataSuccess()`, will be called when the data has been successfully fetched, with the data passed to it as`items`. Through the magic of ES6 property value shorthands, we'll return an object with a property called`items`whose value will be the array of`items`;

_Note: that the value you use for`type`and the name of the other property that is returned is important, because you will re-use them in your reducers_

Now that we have the 3 actions which will represent our state, we'll convert our original component's`fetchData`method to an`itemsFetchData()`action creator.

By default, Redux action creators don't support asynchronous actions like fetching data, so here's where we utilise Redux Thunk. Thunk allows you to write action creators that return a function instead of an action. The inner function can receive the store methods`dispatch`and`getState`as parameters, but we'll just use`dispatch`.

A real simple example would be to manually trigger`itemsHasErrored()`after 5 seconds.



続く

////  
なるほど。redux-thunk で store をラップする等の準備をした後、action の中で、特殊な関数を作る。return \(dispatch\) =&gt; {} で、非同期通信や dispatch をする関数を return する。これを実行すればいい。

```js
export const fetch = (url) => {
    return (dispatch) => {
        dispatch(action)

        fetch(url)
            .then((res)=> {
            })
    }

}
```



