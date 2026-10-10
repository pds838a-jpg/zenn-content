---
title: "constなのに配列へ追加できる？「再代入」と「変更」を分けて考える"
emoji: "🌱"
type: "tech"
topics: ["javascript", "初心者", "学習"]
published: true
---

JavaScriptの復習で、今回は「constで作った配列に、なぜ値を追加できるのか」をテーマにした。

constを「変更できないもの」とだけ覚えると、配列のコードで迷いそうだ。そこで、**変数に別の値を入れること**と、**配列の中身を変えること**を分けて、短い例で整理しておきたい。

## まず、数値の場合

```js
const minutes = 30;
minutes = 45; // TypeError
```

minutesには、最初に30を入れている。そのあと45を入れ直そうとしているので、これは「再代入」になる。constでは再代入できない。

勉強時間をあとから更新するなら、次のようにletを使う。

```js
let minutes = 30;
minutes = 45;
console.log(minutes); // 45
```

ここまでは「constは入れ直せない、letは入れ直せる」と考えると整理しやすい。

## では、配列のpushはどうなる？

```js
const subjects = ["HTML", "CSS"];
subjects.push("JavaScript");

console.log(subjects);
// ["HTML", "CSS", "JavaScript"]
```

このコードではエラーにならず、配列にJavaScriptが追加される。

ポイントは、`subjects = 別の値` という代入をしていないこと。subjectsが指している配列は同じで、その配列の中身に要素を追加している。

一方、次のコードは新しい配列を代入し直すのでエラーになる。

```js
const subjects = ["HTML", "CSS"];
subjects = ["JavaScript"]; // TypeError
```

※ エラーになる例は、それぞれ別に実行するためのコード。同じスクリプトに続けて書くと、最初のエラーでその後の処理が止まる。

## 「どの操作をしたか」で見分ける

| コード | していること | constで可能か |
| --- | --- | --- |
| `subjects.push("JavaScript")` | 同じ配列へ追加 | 可能 |
| `subjects[0] = "HTML復習"` | 同じ配列の要素を変更 | 可能 |
| `subjects = ["JavaScript"]` | 別の配列を再代入 | 不可 |

「=があれば全部だめ」というわけでもない。`subjects[0] = ...` が変更しているのは配列の要素で、変数subjectsそのものへの再代入とは違う。

今の段階では、**constは変数の再代入を禁止するもので、配列の中身を固定するものではない**、という一文を軸に覚えておきたい。

## 配列を別の変数に入れると、コピーになる？

ここから少しだけ関連する話も整理する。

```js
const subjects = ["HTML", "CSS"];
const sameSubjects = subjects;

sameSubjects.push("JavaScript");

console.log(subjects);
// ["HTML", "CSS", "JavaScript"]
```

`const sameSubjects = subjects` だけでは、配列そのものをコピーしたことにはならない。二つの変数が同じ配列を参照しているので、一方から追加すると、もう一方から見える内容も変わる。

この例のような文字列の配列を別に用意したいなら、sliceでコピーできる。

```js
const subjects = ["HTML", "CSS"];
const copiedSubjects = subjects.slice();

copiedSubjects.push("JavaScript");

console.log(subjects);       // ["HTML", "CSS"]
console.log(copiedSubjects); // ["HTML", "CSS", "JavaScript"]
```

ただし、sliceは「浅いコピー」。配列の中にオブジェクトが入っている場合は、そのオブジェクトの参照まで別になるわけではない。この点は、オブジェクトの復習と合わせて次に確認したい。

## 次に自分で確かめたいこと

まずはpushをpopに変えて、同じ配列から値を削除できるかを試したい。そのあと、letで配列を作った場合と比べて、「中身の変更」と「再代入」を自分の言葉で説明できるようにしたい。

広い範囲を一度に覚えるより、今回はこの二つを混ぜないことを復習の目標にする。

[GitHubの復習用サンプル：配列とオブジェクト](https://github.com/pds838a-jpg/frontend-study/blob/main/javascript/05-arrays-and-objects.html)

## 参考

- [MDN：const](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/const)
- [MDN：Array.prototype.slice()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

※ 復習のテーマ選びと文章・コードの整理にAIの支援を使用しています。
