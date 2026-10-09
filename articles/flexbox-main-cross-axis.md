---
title: "Flexboxの中央揃えは、縦と横より「軸」で考える"
emoji: "📐"
type: "tech"
topics: ["css", "html"]
published: true
---

Flexboxの整列で使う`justify-content`と`align-items`は、横方向と縦方向として覚えるより、主軸と交差軸で考える方が分かりやすい。`flex-direction`を変えると、その軸の向きも変わる。

## まず親にdisplay: flexを書く

並べたい要素を一つの親要素で囲み、親に`display: flex`を指定する。親がフレックスコンテナー、その直接の子がフレックスアイテムになる。

```html
<div class="container">
  <div class="item">HTML</div>
  <div class="item">CSS</div>
  <div class="item">JavaScript</div>
</div>
```

```css
.container {
  display: flex;
  flex-direction: row;
  min-height: 240px;
  padding: 16px;
  gap: 12px;
  border: 2px solid #555;
}

.item {
  padding: 12px 20px;
  background-color: #eef5ff;
}
```

この例は、一般的な横書きのページで考える。`flex-direction: row`なので、主軸は横方向、交差軸は縦方向になる。

## justify-contentは主軸方向

親に次の指定を加える。

```css
.container {
  justify-content: center;
}
```

`justify-content`は主軸方向の整列を指定する。`row`の場合は、項目のまとまりが横方向の中央に配置される。

`space-between`なら、最初と最後の項目を両端に置き、項目の間に残りの空間を分配する。ただし、分配する空間が残っていることが必要になる。

## align-itemsは交差軸方向

さらに、次の指定を加える。

```css
.container {
  align-items: center;
}
```

`align-items`は交差軸方向の整列を指定する。今回の`row`では縦方向なので、項目が上下の中央に配置される。

上下の変化を見やすくするために、この例では親に`min-height: 240px`を付けている。親の高さが内容とほぼ同じなら、中央揃えにしても変化が分かりにくい。

## columnにすると役割が逆になる？

次に、並ぶ方向を変えてみる。

```css
.container {
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```

`column`では主軸が縦方向、交差軸が横方向になる。プロパティの役割が変わるのではなく、整列の基準になる軸が変わっている。

| 指定 | row | column |
| --- | --- | --- |
| justify-content | 横方向 | 縦方向 |
| align-items | 縦方向 | 横方向 |

だから「justify-contentは横」とだけ覚えると、縦に並べたときに分かりにくくなる。「justify-contentは主軸」と覚えておきたい。

## 項目が入りきらないとき

折り返して並べたい場合は`flex-wrap: wrap`を指定する。

```css
.container {
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
}
```

`gap`は項目や行の間隔になる。複数行になった場合、各行の中の交差軸方向の整列は`align-items`、行全体の交差軸方向の配置は`align-content`で考える。`align-content`は`flex-wrap: nowrap`の単一行コンテナーでは効かない。

## 覚えておきたいこと

Flexboxで配置を確認するときは、まず`flex-direction`を見る。そのあと主軸と交差軸を確認して、どちらをそろえたいのか考える。

同じHTMLで`row`と`column`を切り替えながら、`justify-content`と`align-items`の違いを復習してみたい。
