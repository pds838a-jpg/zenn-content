---
title: "JavaScript復習②：関数・配列・オブジェクトで処理を整理する"
emoji: "🧩"
type: "tech"
topics: ["javascript", "初心者", "学習"]
published: false
---

> この原稿は公開記事①へ統合済みです。単独記事としての公開予定はありません。

[前回](https://zenn.dev/japan_pds838a/articles/javascript-basics-types-control)は入力値の型と制御構文を整理しました。今回は関数で処理をまとめ、配列やオブジェクトで複数の値を扱います。授業資料の17〜18フォルダーのテーマに対応する復習です。

この記事とコードはAIの支援で整理した復習用サンプルです。教材の解答一式の転載や、学習者本人の実習完了報告ではありません。コード内の文章は韓国語を基本にしています。

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

## 試す順番

[GitHubのサンプル](https://github.com/pds838a-jpg/frontend-study/tree/main/javascript)の04で引数と戻り値、05で配列とオブジェクト、06で日時・乱数・タイマーを確認できます。値を変更してから実行し、予想との差を書き残すと復習になります。

続き: [DOMとイベントで学習リストを作る](https://zenn.dev/japan_pds838a/articles/javascript-dom-events-study-list)

## 参考

- [MDN: 関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Functions)
- [MDN: Array](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN: Date](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [MDN: setInterval](https://developer.mozilla.org/ja/docs/Web/API/Window/setInterval)
