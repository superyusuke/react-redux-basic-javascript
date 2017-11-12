## Designing our state

state をデザインする。

From the work we've already done, we know that our state needs to have 3 properties: items, hasErrored and isLoading for this application to work as expected under all circumstances, which correlates to needing 3 unique actions.

すでに行ってきた作業により、私たちのアプリケーションのための state は、3つのプロパティを持てば良いことがわかっています。つまり、items, hasErrored, isLoading の3つのプロパティです。これらがあれば予想さされるアプリケーションの全ての状態に対応することができはずです。

Now, here is why Action Creators are different to Actions and do not necessarily have a 1:1 relationship: we need a fourth action creator that calls our 3 other action (creators) depending on the status of fetching the data. This fourth action creator is almost identical to our original fetchData() method, but instead of directly setting the state with this.setState({ isLoading: true }), we'll dispatch an action to do the same: dispatch(isLoading(true)).

?Now, here is why Action Creators are different to Actions and do not necessarily have a 1:1 relationship? 我々は4番目のアクションクリエイターが必要です。これは他の3つの action (もしくは  action creator) を、data の fetch の状態に応じて呼び出すものです。(訳注:fetch を開始したら、loading を true にするアクションを呼び出し、fetch が完成したら item の state を更新する action を呼び出し、失敗したら、error 用の action を呼び出す。) この4つめの action creator は、ほとんどこの記事の冒頭で作成した元の fetchData() メソッドとほとんど同じ機能を持つものです。ただい、state を直接 this.setState({isLoading: true}) と変更するのではなくて、同じように state を変更する action を dispatch します。つまり dispatch(isLoading(true))といった具合にです。



////
なるほど。redux-thunk で store をラップする等の準備をした後、action の中で、特殊な関数を作る。return (dispatch) => {} で、非同期通信や dispatch をする関数を return する。これを実行すればいい。

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
