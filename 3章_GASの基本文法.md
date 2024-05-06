## 3. GASの基本文法

GASはJavaScriptをベースにしているため、JavaScriptの知識があれば比較的簡単に学ぶことができます。この章では、GASを書くための基本的な文法について説明します。

### 3.1 変数と型

GASでは`var`、`let`、`const`を使って変数を宣言できます。`var`は関数スコープ、`let`と`const`はブロックスコープを持ちます。`const`は再代入不可の変数を宣言するために使います。

```javascript
var x = 1;
let y = "hello";
const z = true;
```

GASで使える主な型は以下の通りです。

- Number（数値）
- String（文字列）
- Boolean（真偽値）
- Array（配列）
- Object（オブジェクト）

### 3.2 演算子と式

GASでは、JavaScriptと同じ演算子が使えます。

- 算術演算子（`+`, `-`, `*`, `/`, `%`, `++`, `--`）
- 代入演算子（`=`, `+=`, `-=`, `*=`, `/=`, `%=`）
- 比較演算子（`==`, `!=`, `===`, `!==`, `>`, `<`, `>=`, `<=`）
- 論理演算子（`&&`, `||`, `!`）

これらの演算子を使って式を書くことができます。

```javascript
let x = 1 + 2 * 3;
let y = (x > 5) && (x < 10);
```

### 3.3 制御構文（if文、for文、while文）

GASの制御構文は、JavaScriptとほぼ同じです。

- if文
```javascript
let x = 5;
if (x > 0) {
  console.log("xは正の数です");
} else if (x < 0) {
  console.log("xは負の数です");
} else {
  console.log("xは0です");
}
```

- for文
```javascript
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

- while文
```javascript
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

### 3.4 関数の定義と呼び出し

GASでは、`function`キーワードを使って関数を定義します。

```javascript
function addNumbers(a, b) {
  return a + b;
}
```

関数は定義した名前で呼び出すことができます。

```javascript
let result = addNumbers(1, 2);
console.log(result); // 3
```

### 3.5 練習問題

1. 2つの数値を引数として受け取り、その和を返す関数を作成してください。
2. 1から100までの整数の中から、3の倍数と5の倍数を出力する関数を作成してください。
3. 引数として受け取った文字列が回文（前から読んでも後ろから読んでも同じ文字列）であるかどうかを判定する関数を作成してください。

(続く)