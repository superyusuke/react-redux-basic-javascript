# array.slice と array.splice

.slice と .splice はどちらも、配列から一部を取り出す、もしくは一部を取り除いた配列を作る際に必要となるが、.slice は元の配列に変更がされない＝非破壊、.splice はもとの配列に変更を加える＝破壊的変更である点が異なる。そのため redux の state 操作のように immutable な操作を行うことが必要な場合には、.slice が主に使われる。

```js
// slice(n)はn番目を含んでそれ以降の配列を返す
var newList = list.slice(2)
//console.log(newList);
 
// slice(0,n)は0から、n番目を含まない、その範囲の配列を返す
var newList = list.slice(0,2)
//console.log(newList);
 
// slice(m,n)はmから、n番目を含まない、その範囲の配列を返す
var newList = list.slice(1,2)
//console.log(newList);
```

## 配列の一部を取り除く

配列である list から、index 番目の要素を取り除いた配列を返す。

```js
const list = [1,2,3,4,5,6,7,8,9]

const removeCounter = (list, index) => {
  return [
    ...list.slice(0, index),
    ...list.slice(index + 1)
  ];
};

console.log(removeCounter(list,1))
```

```js
list.slice(0, index) //0から index 番目を除くところまでの配列を返す。
list.slice(index + 1) // index+1 番目から始めるので index 番目の要素は除かれる。それ以降のすべての配列を返す。
```



