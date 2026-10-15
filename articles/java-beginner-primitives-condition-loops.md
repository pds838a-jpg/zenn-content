---
title: "Javaの基本型とif・繰り返しを小さな例で復習"
emoji: "☕"
type: "tech"
topics: ["java", "初心者", "学習"]
published: false
---

授業のBeginner (1)で扱ったPrimitive Type、`if`、`for`、`while`、`break`、`continue`を、短いコードで整理してみる。構文を個別に覚えるだけでなく、変数の値がどう変わり、どの条件で処理が分かれるかを追いたい。

## 基本型とString

Javaには値の種類に応じた基本データ型がある。たとえば、整数には`int`、小数には`double`、1文字には`char`、trueかfalseには`boolean`を使う。

```java
int studyMinutes = 90;
double targetHours = 1.5;
char level = 'B';
boolean completed = studyMinutes >= targetHours * 60;
String subject = "Java";
```

`String`も基本型だと思ってしまいそうだが、文字列を扱うクラスである。`char`はシングルクォートで1文字、`String`はダブルクォートで文字列を書く。

## ifで条件を分ける

```java
if (completed && level == 'A') {
    System.out.println("목표를 달성하고 이해도도 높아요.");
} else if (completed) {
    System.out.println("목표 공부 시간을 달성했어요.");
} else {
    System.out.println("조금 더 공부해 봐요.");
}
```

`if`から順に条件を調べ、最初にtrueになったブロックを実行する。`&&`は両方の条件がtrueのときにtrueになる。条件の順番を変えると、先に実行されるブロックも変わる。

## forとwhile

回数が決まっている繰り返しは`for`で書くと見通しやすい。

```java
for (int day = 1; day <= 7; day++) {
    System.out.println(day + "일째");
}
```

`while`は、条件がtrueの間くり返す。

```java
int remaining = 3;
while (remaining > 0) {
    System.out.println(remaining);
    remaining--;
}
```

`remaining--`を忘れると条件が変わらず、ループが終わらないことがある。条件に使う値がどこで更新されるかも確認したい。

## breakとcontinue

`break`はループ自体を終了し、`continue`はその回の残りを飛ばして次の回へ進む。休みの日だけ学習時間に加えず、合計が目標に届いたら終了する例で比べる。

```java
int totalMinutes = 0;
for (int day = 1; day <= 7; day++) {
    if (day == 3) {
        continue;
    }
    totalMinutes += 30;
    if (totalMinutes >= 120) {
        break;
    }
}
```

`continue`のあとにある処理は、その回では実行されない。一方、`break`に入ると残りの日の繰り返しも行わない。この二つを混同しないように、どこへ移るのかをコード上で追ってみる。

## 確認したいこと

- `day == 3`を`day == 2`に変えると合計はどうなるか。
- `continue`を`break`に置き換えると、何日目で終わるか。
- `while`の条件に使う変数を更新しなかったら、どうなるか。

実行用の4つのサンプルと復習ノートは[Java専用のGitHubリポジトリ](https://github.com/pds838a-jpg/java-study)にまとめている。この記事は授業テーマをもとにAIの支援で作った復習メモで、実行結果や理解度を本人の経験として述べるものではない。
