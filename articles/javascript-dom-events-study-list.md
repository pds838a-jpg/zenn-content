---
title: "クリックで動かしたいのに、なぜ先に実行される？関数の渡し方を復習"
emoji: "🌱"
type: "tech"
topics: ["javascript", "dom", "初心者"]
published: true
---

JavaScriptのイベントを復習するテーマとして、次の二つの違いを取り上げる。

```js
button.addEventListener("click", handleClick);
button.addEventListener("click", handleClick());
```

見た目の違いは括弧の有無だけ。でも、「クリックされたら実行する」のか、「この行で実行する」のかが変わる。ボタンに動きを付ける前に、ここを整理しておきたい。

## 比べるための小さなボタン

学習リスト全体だと読むコードが多いので、まずは「押した回数を表示する」だけの例にする。

```html
<button id="count-button" type="button">눌러 보기</button>
<p id="result" aria-live="polite">0회</p>

<script>
  const button = document.querySelector("#count-button");
  const result = document.querySelector("#result");
  let count = 0;

  function handleClick() {
    count++;
    result.textContent = `${count}회`;
  }

  button.addEventListener("click", handleClick);
</script>
```

この例の表示は、ページを開いたときが0回、ボタンを押すと1回、もう一度押すと2回となる。

スクリプトはボタンと表示欄の後ろに置いている。要素がまだ存在しない時点で取得しようとする問題を、今回の比較に混ぜないためだ。

## 括弧なしは「関数を渡す」

```js
button.addEventListener("click", handleClick);
```

ここではhandleClickをその場で呼んでいない。「clickが起きたときに、この関数を呼んでほしい」と登録している。

一方、次のように括弧を付けると、その行で関数を呼び出す。

```js
button.addEventListener("click", handleClick());
```

先ほどの完成コードの最後の一行だけをこれに置き換えると、読み込み時にhandleClickが実行され、表示が1回になる。

さらに、addEventListenerへ渡るのは関数そのものではなく、**handleClickを実行した戻り値**になる。この関数にはreturnがないので、戻り値はundefined。クリック時に実行する関数は登録されず、その後ボタンを押しても回数は増えない。

※ 比較するときは一行を置き換えて再読み込みする。二つの登録行を両方書くと、最初に登録した関数が残り、比較したい条件が変わってしまう。

## returnと実行するタイミングは別に考える

今回は戻り値がundefinedなので、クリックの処理にならない。

ただし、`関数名()` を書くと必ず間違いになる、と覚えるのも違う。関数が別の関数を返す作りなら、その戻り値を登録できる場合もある。

まずは今回の例で、次の二つを分けて覚えたい。

- `handleClick`：あとで呼んでもらうために関数を渡す。
- `handleClick()`：今ここで関数を呼び、その結果を使う。

## 引数を付けて呼びたいときは？

例えば「1回ずつ」ではなく「2回分ずつ」増やしたい場合、関数に値を渡す形にできる。

先ほどのスクリプトを、次に置き換えて比較する。

```js
const button = document.querySelector("#count-button");
const result = document.querySelector("#result");
let count = 0;

function addCount(step) {
  count += step;
  result.textContent = `${count}회`;
}

button.addEventListener("click", () => {
  addCount(2);
});
```

addEventListenerに渡しているのはアロー関数。その中のaddCount(2)は、クリックされてアロー関数が呼ばれたときに実行される。表示は0回から2回、4回と増える。

「引数を渡したいから、その場で呼ぶ」のではなく、**あとで実行する関数の中に、呼び出しを書く**と考えると整理しやすい。

## 次に自分で確かめたいこと

まずは括弧を付けた場合と付けない場合で、ページを開いた直後の表示を比べたい。次にaddCount(2)をaddCount(3)へ変えて、押したときにだけ増えることを確認したい。

今回の復習では、長いコードを書くことより、「この関数はいつ実行されるのか」を一行ずつ説明できるようになることを目標にする。

[GitHubの復習用サンプル：DOMとイベント](https://github.com/pds838a-jpg/frontend-study/blob/main/javascript/07-dom-and-events.html)

## 参考

- [MDN：addEventListener()](https://developer.mozilla.org/ja/docs/Web/API/EventTarget/addEventListener)
- [MDN：関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Functions)

※ 復習のテーマ選びと文章・コードの整理にAIの支援を使用しています。
