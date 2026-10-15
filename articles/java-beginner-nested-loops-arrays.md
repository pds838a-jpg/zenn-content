---
title: "Javaの二重ループと配列を行・列の例で復習"
emoji: "🧩"
type: "tech"
topics: ["java", "初心者", "学習"]
published: true
---

授業のBeginner (2)で扱ったNested LoopとArrayを、小さな例で整理する。二重ループは行と列、配列は複数の同じ型の値をまとめて扱う場面に置き換えると、動きを追いやすい。

## 二重ループで行と列を表示する

```java
int rows = 3;
int columns = 4;

for (int row = 1; row <= rows; row++) {
    for (int column = 1; column <= columns; column++) {
        System.out.print("[" + row + "행 " + column + "열]");
    }
    System.out.println();
}
```

外側の`row`が1回進むごとに、内側の`column`は1から4まで動く。つまり内側のループは、外側の各回で最初から最後まで実行される。この例では3行×4列で12個の位置を表示する。

## 配列の値と添字

```java
int[] scores = {85, 92, 78, 100};
System.out.println(scores[0]); // 85
scores[2] = 80;
System.out.println(scores.length); // 4
```

配列の番号（添字）は0から始まる。4個の値なら添字は0〜3で、要素数は`length`で確認できる。`scores[4]`のように要素数と同じ番号を指定すると範囲外になるので注意したい。

## forで合計と平均を求める

```java
int total = 0;
for (int index = 0; index < scores.length; index++) {
    total += scores[index];
}

double average = (double) total / scores.length;
```

添字を0から始め、`index < scores.length`の間くり返すと、すべての要素を順番に読める。平均を小数で出したいので、割り算の前に`total`を`double`へ変換している。整数同士の割り算では小数部分が残らない。

## 自分で値を変えて確かめる

行数や列数を変えると表示件数がどう変わるか、点数を一つ追加すると平均がどう変わるかを先に予想してから実行すると、ループと配列の関係を確認しやすい。

実行用サンプルと復習ノートは[Java専用のGitHubリポジトリ](https://github.com/pds838a-jpg/java-study)に置いている。この記事は授業テーマをもとにAIの支援で作った復習メモで、実行結果や理解度を本人の経験として述べるものではない。
