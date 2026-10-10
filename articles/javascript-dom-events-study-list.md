---
title: "JavaScript復習②：DOMとイベントで学習リストを作る"
emoji: "📝"
type: "tech"
topics: ["javascript", "dom", "初心者"]
published: true
---

変数や関数を覚えたあと、画面を操作するにはDOMとイベントをつなぎます。今回は「入力した学習テーマを一覧に追加する」という小さな例で整理します。授業資料の19フォルダーのテーマに対応します。

説明は日本語、コード内の文章は韓国語です。教材の原本ではなく、AIの支援で整理した復習用サンプルです。学習者本人が実習を完了したという記録ではありません。

## 要素を選ぶ、操作を受け取る、画面を変える

DOMはHTML文書をオブジェクトの木として扱う仕組みです。

```js
const input = document.querySelector("#topic");
const list = document.querySelector("#topics");
```

`querySelector` は最初に一致した要素を返します。存在しない場合は `null` です。要素が作られる前に実行しないよう、`defer` を使うか、HTMLの末尾にスクリプトを書きます。複数要素なら `querySelectorAll` が使えます。

## ボタンではなくフォームのsubmitを扱う

```html
<form id="form">
  <label for="topic">학습 주제</label>
  <input id="topic" maxlength="80" autocomplete="off">
  <button>추가</button>
</form>
<p id="status" role="status">0개</p>
<ul id="topics"></ul>
```

フォームの `submit` イベントに処理を登録すると、追加ボタンだけでなく入力欄でEnterを押す操作も扱えます。標準の送信を止め、同じ画面で一覧を更新します。

以下のスクリプトをHTMLの後ろに置けば、追加と削除を試せます。

```html
<script>
"use strict";
const form = document.querySelector("#form");
const input = document.querySelector("#topic");
const list = document.querySelector("#topics");
const status = document.querySelector("#status");

function updateCount() {
  status.textContent = `${list.children.length}개`;
}

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const value = input.value.trim();
  if (value === "") {
    status.textContent = "주제를 입력하세요.";
    input.focus();
    return;
  }

  const item = document.createElement("li");
  const text = document.createElement("span");
  text.textContent = value;

  const remove = document.createElement("button");
  remove.type = "button";
  remove.textContent = "삭제";
  remove.setAttribute("aria-label", `${value} 삭제`);
  remove.addEventListener("click", () => {
    item.remove();
    updateCount();
    input.focus();
  });

  item.append(text, " ", remove);
  list.append(item);
  input.value = "";
  updateCount();
  input.focus();
});
</script>
```

`createElement` で要素を生成し、`append` で親へ追加し、`remove` で削除します。追加直後に入力欄へフォーカスを戻すと、キーボードで続けて入力できます。

## 入力文はtextContentで表示する

`textContent` は文字として設定し、`innerHTML` はHTMLとして解釈します。例えば `<b>JS</b>` と入力した場合、この例ではタグを含む文字列をそのまま表示します。

入力された文章を表示したいだけなら、HTMLとして組み立てる必要はありません。要素の構造は `createElement`、文章は `textContent` と分けると読みやすくなります。

## イベントの関数は「渡す」

```js
function handleClick(event) {
  console.log(event.type);
  console.log(event.currentTarget);
}
// buttonは取得済みのボタン要素
// button.addEventListener("click", handleClick);
```

`handleClick()` と書くと登録時に呼び出してしまいます。イベントが起きたときに呼んでほしい関数は `handleClick` として渡します。

`event.target` はイベントの発生元、`event.currentTarget` は今実行しているリスナーを登録した要素です。ボタン内の子要素をクリックすると、両者が異なる場合があります。

## classListで完了状態を切り替える

完成版の08では、各項目に完了ボタンも付けています。文字列にCSSクラスを付け外しして、取り消し線を表示します。

```css
.done { text-decoration: line-through; }
```

```js
// textとcompleteは各項目のspan・ボタン要素
// const done = text.classList.toggle("done");
// complete.setAttribute("aria-pressed", String(done));
```

`aria-pressed` も更新し、見た目だけでなくボタンの状態を伝えます。追加・削除で件数も更新します。この例には永続保存がないため、ページを再読み込みすると一覧は消えます。

## 確認する操作

1. 空欄や空白だけで追加し、項目が増えないことを確認する。
2. 「DOM 복습」をEnterで追加し、件数が1になることを確認する。
3. `<b>JS</b>` を入力し、太字ではなくタグを含む文字列になることを確認する。
4. 完了を切り替え、もう一度押すと元に戻ることを確認する。
5. 一つ削除し、残りの項目と件数が一致することを確認する。

実行用コードは [GitHubのjavascriptフォルダー](https://github.com/pds838a-jpg/frontend-study/tree/main/javascript) にあります。07で要素の選択・テキスト・クラスの変更を試し、08で追加・完了・削除を組み合わせます。

前の記事: [変数・制御構文・関数・配列・オブジェクト](https://zenn.dev/japan_pds838a/articles/javascript-basics-types-control)

## 参考

- [MDN: DOMの紹介](https://developer.mozilla.org/ja/docs/Web/API/Document_Object_Model/Introduction)
- [MDN: addEventListener](https://developer.mozilla.org/ja/docs/Web/API/EventTarget/addEventListener)
- [MDN: textContent](https://developer.mozilla.org/ja/docs/Web/API/Node/textContent)
- [MDN: preventDefault](https://developer.mozilla.org/ja/docs/Web/API/Event/preventDefault)
