---
title: "JavaScript復習①：変数・制御構文から関数・配列・オブジェクトまで"
emoji: "📘"
type: "tech"
topics: ["javascript", "初心者", "学習"]
published: true
---

HTML・CSSの復習に続いて、授業資料のJavaScriptのテーマを整理します。今回は変数、入力値の変換、条件分岐、繰り返しから、関数・配列・オブジェクトまでを扱います。説明は日本語、コード内の文章は韓国語にしています。

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

`const` は再代入できない束縛、`let` は再代入できる変数です。`const` でも配列やオブジェクトの中身は変更できます。`var` とのスコープの違いは後半で整理します。

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

## 関数の戻り値と画面への表示は別

```js
function sumMinutes(first, second, third = 0) {
  return first + second + third;
}
const total = sumMinutes(20, 30, 40);
console.log(total); // 90
```

関数の定義で受け取る名前が仮引数、呼び出すときに渡す値が引数です。`third = 0` は、値を省略した場合や `undefined` を渡した場合に使う既定値です。

`return` は値を呼び出し元へ返して関数を終了します。`console.log()` はコンソールに表示する処理です。結果を後の計算に使いたいなら、表示だけでなく値を返します。戻り値を指定しない関数は `undefined` を返します。

## 関数の書き方とスコープ

```js
const double = function (value) { return value * 2; };
const average = (total, days) => total / days;
console.log(double(5));      // 10
console.log(average(90, 3));  // 30
```

前者は関数式、後者はアロー関数です。短い式を返すアロー関数では `{}` と `return` を省略できます。ただし、アロー関数は独自の `this` を持たないため、通常の関数と常に交換できるわけではありません。平均の例は日数が正の数であることを前提にしています。

```js
const label = "바깥";
if (true) {
  const label = "블록 안";
  console.log(label); // 블록 안
}
console.log(label);   // 바깥
```

`let` と `const` は `{}` によるブロックスコープを持ちます。内側の同名変数は別の束縛です。`var` は関数スコープで、`if` のブロックだけでは分離されません。

また、`let` と `const` を宣言前に読むとエラーになります。「ホイスティングされない」とだけ覚えるより、初期化前にアクセスできない期間があると理解すると整理しやすくなります。

## 配列は変更する操作とコピーする操作を分ける

```js
const subjects = ["HTML", "CSS"];
const copy = subjects.slice();
subjects.push("JavaScript", "DOM");
const removed = subjects.pop();
subjects.splice(1, 1);

console.log(removed);  // "DOM"
console.log(subjects); // ["HTML", "JavaScript"]
console.log(copy);     // ["HTML", "CSS"]
```

`push` は末尾に追加、`pop` は末尾を削除、`splice` は指定位置から変更します。`slice` は元の配列を変更せず、浅いコピーを返します。この例の要素は文字列ですが、要素がオブジェクトならコピー後も同じオブジェクトを参照する点に注意します。

`const subjects` は配列の中身を固定しません。配列そのものの再代入を禁止しています。配列は0から数えるので、最初は `subjects[0]`、最後は `subjects[subjects.length - 1]` です。

```js
subjects.forEach((subject, index) => {
  console.log(`${index}: ${subject}`);
});
console.log(subjects.join(", ")); // "HTML, JavaScript"
```

`forEach` は各要素に処理を行い、`join` は区切り文字を使って一つの文字列にします。

## オブジェクトは関連する情報を名前でまとめる

```js
const lesson = {
  topic: "JavaScript",
  minutes: 30,
  describe() {
    return `${this.topic}: ${this.minutes}분`;
  }
};
console.log(lesson.describe()); // JavaScript: 30분
```

`topic` と `minutes` はプロパティ、`describe` はメソッドです。このように `lesson.describe()` と呼ぶと、メソッド内の `this` は `lesson` を指します。関数を取り出して別の呼び方をすると同じになるとは限りません。

## Date・Math・タイマーも小さく試す

```js
const now = new Date();
console.log(now.getMonth() + 1); // 月は0〜11なので表示時に+1
const dice = Math.floor(Math.random() * 6) + 1;
console.log(dice); // 1〜6
```

`Math.random()` は0以上1未満の値を返します。6倍して切り捨てると0〜5、1を足すと1〜6になります。これは学習用の乱数例です。

タイマーはIDを保持して停止します。開始ボタンの連打で複数登録しないようにすることも必要です。

```js
let intervalId = null;
function start() {
  if (intervalId !== null) return;
  intervalId = setInterval(() => console.log("복습 시간"), 1000);
}
function stop() {
  clearInterval(intervalId);
  intervalId = null;
}
```

1000ミリ秒は実行間隔の目安で、正確な時刻を保証しません。今回の例はコールバックの実行回数を数え、厳密な経過時間としては扱いません。

## ブラウザーで試す

[GitHubのJavaScriptサンプル](https://github.com/pds838a-jpg/frontend-study/tree/main/javascript)をダウンロードし、`index.html` から01〜06を開きます。01〜03で型と制御構文、04〜06で関数・配列・組み込み機能を確認できます。30、0、空欄、範囲外の数を入力して、予想と結果を比較するのがおすすめです。

続き: [DOMとイベントで学習リストを作る](https://zenn.dev/japan_pds838a/articles/javascript-dom-events-study-list)

## 参考

- [MDN: 文法とデータ型](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Grammar_and_types)
- [MDN: 制御フローとエラー処理](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [MDN: ループと反復処理](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Loops_and_iteration)

- [MDN: 関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Functions)
- [MDN: Array](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN: Date](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [MDN: setInterval](https://developer.mozilla.org/ja/docs/Web/API/Window/setInterval)
