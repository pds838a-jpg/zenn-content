---
title: "JavaScript復習①：入力値の型から条件分岐・繰り返しまで"
emoji: "📘"
type: "tech"
topics: ["javascript", "初心者", "学習"]
published: true
---

HTML・CSSの復習に続いて、授業資料のJavaScriptのテーマを整理します。今回は変数、入力値の変換、条件分岐、繰り返しです。説明は日本語、コード内の文章は韓国語にしています。

:::message
この記事とサンプルは、授業で使用した `doit-hcj-new` の15〜19フォルダーのテーマをもとに、AIの支援で整理した復習用資料です。教材の原本や解答一式の転載ではなく、学習者本人がすべての実習を完了したという記録でもありません。
:::

## HTML・CSSに「動き」を加える

HTMLで入力欄とボタンを用意し、JavaScriptで入力値を読み、計算結果を表示します。外部ファイルを使う場合は次のように書けます。

```html
<script src="app.js" defer></script>
```

`defer` を指定した外部スクリプトは、HTMLの解析が終わってから実行されます。今回の実行用サンプルは一つのファイルで読めるよう、スクリプトを `body` の末尾に置いています。

## constとletは「再代入するか」で使い分ける

```js
const days = 7;
let total = 0;
total = 30 * days;
console.log(total); // 210
```

`const` は再代入できない束縛、`let` は再代入できる変数です。`const` でも配列やオブジェクトの中身は変更できます。`var` とのスコープの違いは次の記事で整理します。

## 数字に見えても、入力値は文字列

```js
const raw = "30";
console.log(raw + 7);         // "307"
console.log(Number(raw) + 7); // 37
console.log(typeof raw);      // "string"
```

フォームの `input.value` は文字列です。`+` は文字列の連結にも使われるため、数字として計算するなら型を意識します。`prompt()` も文字列を返しますが、キャンセル時は `null` になる点がフォームと異なります。

空文字列を `Number()` に渡すと0になります。空欄を0分として扱いたくない場合は、空欄かどうかを別に確認します。

```js
function weeklyMinutes(raw) {
  const text = raw.trim();
  const minutes = Number(text);
  if (text === "" || !Number.isFinite(minutes) || minutes < 0 || minutes > 1440) {
    return "0~1440 사이의 숫자를 입력하세요.";
  }
  return `일주일: ${minutes * 7}분`;
}

console.log(weeklyMinutes("30")); // 일주일: 210분
console.log(weeklyMinutes(""));   // 入力案内
console.log(weeklyMinutes("0"));  // 일주일: 0분
```

`Number.isFinite()` は `NaN` と無限大も除外します。0は有効な入力なので、単純な `if (!minutes)` では判定しません。

## 条件分岐は境界を試す

```js
function reviewMessage(score) {
  if (score >= 80) return "다음 단계";
  if (score >= 60) return "한 번 더 복습";
  return "기초부터 복습";
}
```

この関数は入力が0〜100の有効な数であることを前提にしています。フォーム側で入力確認を済ませてから呼び出します。59と60、79と80で結果が切り替わるかを確かめると、条件の境界が分かります。

`score >= 60 && score < 80` は両方を満たす条件です。`||` は少なくとも一方、`!` は真偽の反転に使います。比較の `===` は型変換をしないので、`80 === "80"` は `false` です。`=` は代入なので混同しないようにします。

特定の値ごとに分ける場合は `switch` も使えます。`break` がないと、次の節へ進むことがあります。

```js
const subject = "js";
switch (subject) {
  case "html": console.log("문서 구조"); break;
  case "css": console.log("스타일과 배치"); break;
  default: console.log("동작과 데이터");
}
```

## 繰り返しは「いつ止まるか」を先に考える

```js
let total = 0;
for (let number = 1; number <= 10; number++) {
  total += number;
}
console.log(total); // 55
```

`for` の括弧は初期化、継続条件、更新です。`while` は先に条件を確認し、`do...while` は本体を一度実行してから条件を確認します。更新を書き忘れると、条件が変わらず処理が終わらないことがあります。

```js
const even = [];
for (let number = 1; number <= 10; number++) {
  if (number > 6) break;
  if (number % 2 !== 0) continue;
  even.push(number);
}
console.log(even); // [2, 4, 6]
```

`break` はこのループを終了し、`continue` はその回の残りを飛ばします。二重ループでは、外側の1回ごとに内側の繰り返しが一巡します。

## ブラウザーで試す

[GitHubのJavaScriptサンプル](https://github.com/pds838a-jpg/frontend-study/tree/main/javascript)をダウンロードし、`index.html` から01〜03を開きます。30、0、空欄、範囲外の数を入力して、予想と結果を比較するのがおすすめです。

続き: [関数・配列・オブジェクト](https://zenn.dev/japan_pds838a/articles/javascript-functions-arrays-objects)

## 参考

- [MDN: 文法とデータ型](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Grammar_and_types)
- [MDN: 制御フローとエラー処理](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [MDN: ループと反復処理](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Loops_and_iteration)
