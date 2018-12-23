いくつ知っている？CSSセレクタ65パターン（サンプルコード付き）

CSSセレクタって草案も含めると現在65パターンあるそうです。（2018年11月時点）

[Selectors Level 4](https://www.w3.org/TR/selectors-4/)

ついつい忘れたり曖昧に覚えているものも多いです。
今回は、そんなCSSセレクタを改めて確認し、さらに独断で `基本レベル` 、 `実務レベル` 、 `チョットデキルレベル` 、 `W3Cの人レベル` の4段階に分類しました。

個人的に重要と思ったセレクタには要所々々でサンプルコードを付けたのでお役立てください。
ブラウザの実装がまちまちなものはCan I useのリンクも掲載しました。

# 基本レベル 14パターン

普段マークアップをしないサーバーサイドエンジニアやデザイナーも知ってたりするレベル。

## 全称セレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 1 | * | すべての要素 |

## 型セレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 2 | E | 任意のE（HTMLタグ）要素 |

## IDセレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 3 | E#myid | ID名がmyidであるE要素 |

## classセレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 4 | E.warning | クラス名がwarningであるE要素 |

## 疑似クラス

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 5 | E:link | リンク先が未訪問のE要素（アンカータグ） |
| 6 | E:visited | リンク先が訪問済みのE要素（アンカータグ） |
| 7 | E:active | ユーザーの操作が行なわれているE要素 |
| 8 | E:hover | カーソルが当たっている要素、またはその要素を子孫に持つE要素 |
| 9 | E:focus | フォーカスされているE要素 |

### :link :visited :active :hover の違い

<p data-height="265" data-theme-id="dark" data-slug-hash="ebpZxe" data-default-tab="css,result" data-user="taquaki" data-pen-title="CSS Selectors #1" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/ebpZxe/">CSS Selectors #1</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 属性値セレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 10 | E[foo] | foo属性をもつE要素 |
| 11 | E[foo="bar"] | foo属性の値がbarであるE要素 |

## 結合子

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 12 | E F | E要素の子孫であるF要素 |
| 13 | E > F | E要素の直下の子であるF要素 |
| 14 | E + F | 兄弟関係の中でE要素の直後にあるF要素 |


# 実務レベル 22パターン

主なブラウザは実装済み。ここまで把握していたら業務で困ることは少ないのでは。というレベル。

## 疑似クラス

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 15 | E:not(s1, s2, …) | s1、s2、…のどれにもあてはまらないE要素 |
| 16 | E:enabled<br>E:disabled | 操作が可能または不可能になっているE型のUI要素 |
| 17 | E:checked | 選択されているE型のUI要素 |
| 18 | E:nth-child(n [of S]?) | 兄弟要素の中でSに合致するn個目のE要素 |
| 19 | E:nth-last-child(n [of S]?) | 兄弟要素の中でSに合致する最後からn個目のE要素 |
| 20 | E:first-child | 兄弟要素の中で最初のE要素 |
| 21 | E:last-child | 兄弟要素の中で最後のE要素 |
| 22 | E:only-child | 自身の他に兄弟要素がないE要素 |
| 23 | E:nth-of-type(n) | 同じ親をもつE要素の中でn個目のもの |
| 24 | E:nth-last-of-type(n) | 同じ親をもつE要素の中で最後からn個目のもの |
| 25 | E:first-of-type | 同じ親をもつE要素の中で最初のもの |
| 26 | E:last-of-type | 同じ親をもつE要素の中で最後のもの |
| 27 | E:only-of-type | 同じ親をもつE要素が自身の他にないもの |
| 28 | E:root | ドキュメントルートであるE要素（html要素） |
| 29 | E:empty | 空白も含めた子をもたないE要素 |
| 30 | E:target | ターゲット（アンカーリンク）先のE要素 |

### :nth-child と :nth-of-type の違い

<p data-height="265" data-theme-id="dark" data-slug-hash="oJZVRB" data-default-tab="css,result" data-user="taquaki" data-pen-title="CSS Selectors #2" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/oJZVRB/">CSS Selectors #2</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### 奥が深い :empty

<p data-height="265" data-theme-id="dark" data-slug-hash="gZmyWd" data-default-tab="css,result" data-user="taquaki" data-pen-title="CSS Selectors #4" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/gZmyWd/">CSS Selectors #4</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 属性値セレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 31 | E[foo^="bar"] | foo属性の値がbarで始まるE要素 |
| 32 | E[foo$="bar"] | foo属性の値がbarで終わるE要素 |
| 33 | E[foo*="bar"] | foo属性の値がbarを含むE要素 |
| 34 | E[foo~="bar"] | foo属性の値を空白で分割したときにいずれかがbarと一致するE要素 |
| 35 | E[foo｜="en"] ※ | foo属性の値をハイフンで分割したときに最初がenと一致するE要素 |

※ なぜか|（パイプ）がバックスラッシュでエスケープされなかったので全角で書いてます。

### E[foo*="bar"] と E[foo~="bar"] と E[foo\|="en"]

<p data-height="265" data-theme-id="dark" data-slug-hash="dwvLYR" data-default-tab="css,result" data-user="taquaki" data-pen-title="CSS Selectors #3" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/dwvLYR/">CSS Selectors #3</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 結合子

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 36 | E ~ F | 兄弟要素の中でE要素に続いているF要素 |


# チョットデキルレベル 13パターン

Selectors Level 4で定義されている新しいセレクタたち。一部のブラウザ（IE）で未実装なものも多い。
これを知っていたら[チョットデキルTシャツ](https://www.google.co.jp/search?hl=ja&tbm=isch&source=hp&biw=1680&bih=916&ei=d88cXIb7Ic76-QbUlq7wBA&q=%E3%83%81%E3%83%A7%E3%83%83%E3%83%88%E3%83%87%E3%82%AD%E3%83%AB+T%E3%82%B7%E3%83%A3%E3%83%84&oq=%E3%83%81%E3%83%A7%E3%83%83%E3%83%88%E3%83%87%E3%82%AD%E3%83%AB+T%E3%82%B7%E3%83%A3%E3%83%84&gs_l=img.3..0.1359.10303..10622...1.0..0.156.2583.0j20......1....1..gws-wiz-img.....0..0i4j0i4i24j0i5i30j0i4i10i24j0i24.xnbB1LadfhQ)を着るレベル。

## 疑似クラス

| No. | パターン | 選択対照 | Can I use |
| :--- | :-- | :-- | :-- |
| 37 | E:is(s1, s2, …) | s1、s2、…のどれかにマッチするE要素 | [Link](https://caniuse.com/#feat=css-matches-pseudo) |
| 38 | E:dir(ltr) | 文章が左から右に書かれるのE要素 | [Link](https://caniuse.com/#feat=css-dir-pseudo) |
| 39 | E:read-write<br>E:read-only | ユーザーが編集可能（不可能）なE型のUI要素 | [Link](https://caniuse.com/#feat=css-read-only-write) |
| 40 | E:placeholder-shown | プレイスホルダーが表示されているE型のUI要素 | [Link](https://caniuse.com/#search=placeholder-shown) |
| 41 | E:default | デフォルト選択されているE型のUI要素 | [Link](https://caniuse.com/#feat=css-default-pseudo) |
| 42 | E:indeterminate | 選択が未定の状態にあるE型のUI要素 | [Link](https://caniuse.com/#feat=css-indeterminate-pseudo) |
| 43 | E:valid<br>E:invalid | 入力された値が適切（不適切）であるE型のUI要素 | - |
| 44 | E:in-range<br>E:out-of-range | 入力された値が範囲内（外）であるE型のUI要素 | [Link](https://caniuse.com/#feat=css-in-out-of-range) |
| 45 | E:required<br>E:optional | 入力が必須（任意）であるE型のUI要素 | [Link](https://caniuse.com/#feat=css-optional-pseudo) |
| 46 | E:lang(zh, "*-hant") | 指定した言語（例は中国語）であるE要素 | [Link](https://caniuse.com/#feat=css-sel2) |
| 47 | E:focus-within | フォーカスされている、もしくはそれを含んでいるE要素 | [Link](https://caniuse.com/#feat=css-focus-within) |
| 48 | E:focus-visible | フォーカスされておりフォーカスリング表示すべきと判断されたE要素 | [Link](https://caniuse.com/#feat=css-focus-visible) |

### :default と :indeterminate

indeterminateはJSとの合わせ技が必要

<p data-height="265" data-theme-id="dark" data-slug-hash="bOqJvy" data-default-tab="css,result" data-user="taquaki" data-pen-title="CSS Selectors #5" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/bOqJvy/">CSS Selectors #5</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

## 属性値セレクタ

| No. | パターン | 選択対照 | Can I use |
| :--- | :-- | :-- | :-- |
| 49 | E[foo="bar" i] | foo属性の値がbar（大文字小文字を無視）であるE要素 | [Link](https://caniuse.com/#feat=css-case-insensitive) |


# W3Cの人レベル 16パターン

将来ブラウザに実装予定のセレクタ。
なので仕様が変わる可能性大。これを知っているあなたはもしかしてW3Cの人なのでは。

## 疑似クラス

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 50 | E:where(s1, s2, …) | s1、s2、…のどれかにマッチするE要素かつ詳細度に影響しない |
| 51 | E:has(rs1, rs2, …) | E要素内でrs1、rs2、…のどれかにマッチする要素 |
| 52 | E:any-link | リンクである（href属性をもつ）E要素 |
| 53 | E:local-link | 現在のページのURLをターゲット（アンカーリンク）にしている（href属性をもつ）E要素 |
| 54 | E:target-within | ターゲット（アンカーリンク）先、またはそれを含むE要素 |
| 55 | E:scope | :scope要素であるE要素（現在の仕様では:rootと等価） |
| 56 | E:current | 映像などの何らかのタイムラインの中で現在表示中である、またはそれを含むE要素 |
| 57 | E:current(s) | :current要素であり、SであるE要素 |
| 58 | E:past | 映像などの何らかのタイムラインの中で過去に表示したE要素 |
| 59 | E:future | 映像などの何らかのタイムラインの中で未来に表示するE要素 |
| 60 | E:blank | 値が空欄であるE型のUI要素 |
| 61 | E:user-invalid | ユーザーが入力中に入力された値が適切ではないと判断されたE型のUI要素 |
| 62 | E:nth-col(n) | 表などにおいてn個目のカラムに属するセルであるE要素 |
| 63 | E:nth-last-col(n) | 表などにおいて最後からn個目のカラムに属するセルであるE要素 |

## 属性値セレクタ

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 64 | E[foo="bar" s] | foo属性の値がbar（大文字小文字を無視しない）であるE要素 |

## 結合子

| No. | パターン | 選択対照 |
| :--- | :-- | :-- |
| 65 | F ｜｜ E ※ | 表などにおいてF要素のカラムに属するセルであるE要素 |

※ なぜか|（パイプ）がバックスラッシュでエスケープされなかったので全角で書いてます。

以上です！