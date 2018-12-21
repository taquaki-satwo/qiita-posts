君はいくつ知っている？CSSセレクタ65パターン（サンプルコード付き）

CSSセレクタって草案も含めると現在65パターンあるそうです。（2018年11月時点）
ついつい忘れがちになったり曖昧に覚えているものも多いです。
今回は、そんなCSSセレクタを改めて確認し、さらに独断で基本レベル、実務レベル、チョットデキルレベル、W3Cの人レベルの4段階に分類しました。
サンプルコードも付けたのでお役立てください。
ブラウザの実装がまちまちなものはCan I useのリンクも掲載しました。

なお本記事は下記のW3Cのページを基に作成しました。
[Selectors Level 4](https://www.w3.org/TR/selectors-4/)


# 基本レベル

マークアップをしないサーバーサイドエンジニアやデザイナーも知ってたりするレベル。


## 全称セレクタ

### *

対象: すべての要素
仕様: CSS2.1

<p data-height="236" data-theme-id="dark" data-slug-hash="ebpZxe" data-default-tab="result" data-user="taquaki" data-pen-title="ebpZxe" class="codepen">See the Pen <a href="https://codepen.io/taquaki/pen/ebpZxe/">ebpZxe</a> by Takaaki Sato (<a href="https://codepen.io/taquaki">@taquaki</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


## 型セレクタ

### E

対象: E要素
仕様: CSS1


## IDセレクタ

### E#myid

対象: ID名がmyidであるE要素
仕様: CSS1


## classセレクタ

### E.warning

対象: クラス名がwarningであるE要素
仕様: CSS1


## 疑似クラス

### E:link

対象: 未訪問のE要素
仕様: CSS1


### E:visited

対象: 訪問済みのE要素
仕様: CSS1


### E:active

対象: アクティブになっているE要素
仕様: CSS1


### E:hover

対象: カーソルが当たっている要素、またはその要素を子孫に持つE要素
仕様: CSS2.1


### E:focus

対象: フォーカスされているE要素
仕様: CSS2.1


## 属性値セレクタ

### E[foo]

対象: foo属性をもつE要素
仕様: CSS2.1


### E[foo="bar"]

対象: foo属性の値がbarであるE要素
仕様: CSS2.1


### E[foo~="bar"]

対象: foo属性の値が空白区切りで複数あったときに、いずれかの値がbarであるE要素
仕様: CSS2.1


## 結合子

### E F

対象: E要素の子孫であるF要素
仕様: CSS1


### E > F

対象: E要素の直下の子であるF要素
仕様: CSS2.1


### E + F

対象: 同胞群の中で，E要素の直後にあるF要素
仕様: CSS2.1


# 実務レベル

マークアップエンジニアが普段使っているもの。これを知っていたら実務で困らないはず。









