---
title: "HTMLフォームのlabelとnameを小さな例で整理する"
emoji: "📝"
type: "tech"
topics: ["html", "初心者", "学習"]
published: true
---

HTMLのフォームを学んだとき、`label`、`id`、`name`の役割が少し混乱した。どれも入力欄に関係するが、同じものではない。小さなフォームを使って、今の理解を整理しておく。

## labelとidを対応させる

`label`の`for`属性に入力欄の`id`と同じ値を書くと、ラベルと入力欄を関連付けられる。ラベルをクリックしたときに対応する入力欄へフォーカスできるため、入力する場所が分かりやすくなる。

```html
<label for="user-name">名前</label>
<input id="user-name" type="text">
```

この例では、`for="user-name"`と`id="user-name"`が対応している。値が違うと関連付けられないので、同じ文字列になっているか確認する。

## nameは送信データの項目名

フォームの値を送信するときは、`name`がデータの項目名として使われる。`id`はページ内で要素を識別するための値、`name`は送信する項目の名前、と分けると覚えやすい。

```html
<form action="/search" method="get">
  <label for="keyword">検索キーワード</label>
  <input id="keyword" name="q" type="text">
  <button type="submit">検索</button>
</form>
```

このフォームで`name="q"`を付けた入力欄に「HTML」と入力してGET送信すると、送信先のURLには`q=HTML`のような形で値が含まれる。`name`がない入力欄は、フォーム送信時の項目として扱われない。

## 今回の整理

- `label`の`for`と入力欄の`id`を一致させて関連付ける。
- `id`はページ内で要素を識別する。
- `name`はフォームで送信する項目名になる。

授業のフォーム例を見直すときは、この3つの役割を一つずつ確認したい。フォームのデータをサーバーで保存する処理は、今回の学習範囲には含めていない。
