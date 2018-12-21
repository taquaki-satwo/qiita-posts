いくつ知っている？CSSセレクタ65パターン

CSSセレクタって草案も含めると現在65パターンあるそうです。（2018年11月時点）

[Selectors Level 4](https://www.w3.org/TR/selectors-4/)

ついつい忘れたり曖昧に覚えているものも多いです。
今回は、そんなCSSセレクタを改めて確認し、さらに独断で `基本レベル` 、 `実務レベル` 、 `チョットデキルレベル` 、 `W3Cの人レベル` の4段階に分類しました。

Can I useのリンクを付けたのでお役立てください。

# 基本レベル 14パターン

普段マークアップをしないサーバーサイドエンジニアやデザイナーも知ってたりするレベル。

## 全称セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 1 | * | すべての要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## 型セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 2 | E | 任意のE（HTMLタグ）要素 | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## IDセレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 3 | E#myid | ID名がmyidであるE要素 | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## classセレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 4 | E.warning | クラス名がwarningであるE要素 | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## 疑似クラス

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 5 | E:link | リンク先が未訪問のE要素（アンカータグ） | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 6 | E:visited | リンク先が訪問済みのE要素（アンカータグ） | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 7 | E:active | ユーザーの操作が行なわれているE要素 | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 8 | E:hover | カーソルが当たっている要素、またはその要素を子孫に持つE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 9 | E:focus | フォーカスされているE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## 属性値セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 10 | E[foo] | foo属性をもつE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 11 | E[foo="bar"] | foo属性の値がbarであるE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## 結合子

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 12 | E F | E要素の子孫であるF要素 | CSS1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 13 | E > F | E要素の直下の子であるF要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 14 | E + F | 兄弟関係の中でE要素の直後にあるF要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 


# 実務レベル

ここまで把握していたら業務で困ることは少ないのでは。というレベル。

## 疑似クラス

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 15 | E:not(s1, s2, …) | s1,s2のどれにもあてはまらないE要素 | Selectors Level 3/4 [Link](https://caniuse.com/#feat=css-sel3) | 
| 16 | E:enabled E:disabled | 操作が可能または不可能になっているフォームなどのE型のUI要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 17 | E:checked | 選択されているE型のUI要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 18 | E:nth-child(n [of S]?) | 兄弟要素の中でSに合致するn個目のE要素 | Selectors Level 3/4 [Link](https://caniuse.com/#feat=css-sel3) | 
| 19 | E:nth-last-child(n [of S]?) | 兄弟要素の中でSに合致する最後からn個目のE要素 | Selectors Level 3/4 [Link](https://caniuse.com/#feat=css-sel3) | 
| 20 | E:first-child | 兄弟要素の中で最初のE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 21 | E:last-child | 兄弟要素の中で最後のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 22 | E:only-child | 自身の他に兄弟要素がないE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 23 | E:nth-of-type(n) | 兄弟要素の中でn個目のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 24 | E:nth-last-of-type(n) | 兄弟要素の中で最後からn個目のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 25 | E:first-of-type | 兄弟要素の中で最初のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 26 | E:last-of-type | 兄弟要素の中で最後のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 27 | E:only-of-type | 自身の他に兄弟要素がないE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 28 | E:root | ドキュメントルートであるE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 29 | E:empty | 空白以外に子が無いE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 30 | E:target | 同ページ内のターゲット先のE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 

## 属性値セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 31 | E[foo^="bar"] | foo属性の値がbarで始まるE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 32 | E[foo$="bar"] | foo属性の値がbarで終わるE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 33 | E[foo*="bar"] | foo属性の値がbarを含むE要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 
| 34 | E[foo~="bar"] | foo属性の値を空白で分割したときにいずれかがbarと一致するE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 
| 35 | E[foo\|="en"] | foo属性の値をハイフンで分割したときに最初がenと一致するE要素 | CSS2.1 | [Link](https://caniuse.com/#feat=css-sel2) | 

## 結合子

| No. | パターン | 選択対照 | 仕様 | Can I use | 
| :--- | :-- | :-- | :-- | :-- | 
| 36 | E ~ F | 兄弟要素の中でE要素に続いているF要素 | Selectors Level 3 | [Link](https://caniuse.com/#feat=css-sel3) | 








# チョットデキルレベル

まだ一部のブラウザ（IE）は実装されていなかったりするセレクタ達。
実務で使うことは少ないが、これを知っていたら[チョットデキルTシャツ](https://www.google.co.jp/search?hl=ja&biw=1680&bih=916&tbm=isch&sa=1&ei=sDMcXJGKCc37wAP_94LIAw&q=%E3%83%81%E3%83%A7%E3%83%83%E3%83%88%E3%83%87%E3%82%AD%E3%83%AB+T%E3%82%B7%E3%83%A3%E3%83%84&oq=%E3%83%81%E3%83%A7%E3%83%83%E3%83%88%E3%83%87%E3%82%AD%E3%83%AB+T%E3%82%B7%E3%83%A3%E3%83%84&gs_l=img.3..0.12074.14912..15246...0.0..0.306.1457.0j7j0j1......1....1..gws-wiz-img.......0i24j0i5i30j0i4i24j0i4.q79pWmPGVD4#imgrc=4q3wBXLYzVysLM:)を着るレベル。

## 疑似クラス

| No. | パターン | 選択対照 | 仕様 | Can I use |
| :--- | :-- | :-- | :-- | :-- |
| 37 | E:dir(ltr) | 文章が左から右に書かれるのE要素 | Selectors Level 4 | [Link](https://caniuse.com/#feat=css-dir-pseudo) |
| 38 | E:read-write E:read-only | ユーザーが編集可能（不可能）なE型のUI要素 | Selectors Level 3-UI/4 | [Link](https://caniuse.com/#feat=css-read-only-write) |
| 39 | E:placeholder-shown | プレイスホルダーが表示されているE型のUI要素 | Selectors Level 3-UI/4 | [Link](https://caniuse.com/#search=placeholder-shown) |


-------------------------------------------------------------






| 40 | E:default | 一連の選択肢の中でデフォルト選択肢にされているE型のUI要素 | Selectors Level 3-UI/4 | [Link](https://caniuse.com/#feat=css-default-pseudo) |
| 41 | E:indeterminate | 不定の（チェックの有無が未設定の）状態にあるE型のUI要素 | Selectors Level 4 | [Link]() |
| 42 | "E:valid
E:invalid" | 順に、入力値が［ 妥当である, 妥当でない ］ようなE型のユーザ入力要素 | Selectors Level 3-UI/4 | [Link]() |
| 43 | "E:in-range
E:out-of-range" | 順に、入力値が［ 範囲内, 範囲外 ］にあるE型のユーザ入力要素 | Selectors Level 3-UI/4 | [Link]() |
| 44 | "E:required
E:optional" | 順に、入力が必須［ である, でない ］E型のユーザ入力要素 | Selectors Level 3-UI/4 | [Link]() |
| 45 | E:lang(zh, "*-hant") | 言語タグが［ 標準中国語（そのどの方言／書記体系も含む）, あるいは繁体字で書かれたもの ］として付与された下にあるE要素 | Selectors Level 2/4 | [Link]() |
| 46 | E:focus-within | ユーザ入力の focus を得ているか, それを包含している E要素 | Selectors Level 4 | [Link]() |
| 47 | E:focus-visible | ユーザ入力の focus を得ていてるE要素であって，UA が［ その要素用に focus 環その他の指示子を描くべきである ］と決定した要素 | Selectors Level 4 | [Link]() |

## 属性値セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use |
| :--- | :-- | :-- | :-- | :-- |
| 48 | E[foo="bar" i] | foo属性の値がbarに一致するE要素（大文字小文字を無視する） | Selectors Level 4 | [Link]() |



# W3C

## 疑似クラス

| No. | パターン | 選択対照 | 仕様 | Can I use |
| :--- | :-- | :-- | :-- | :-- |
| 49 | E:is(s1, s2, …) | s1,s2のどれかに合致するE要素 | Selectors Level 4 | [Link]() |
| 50 | E:where(s1, s2, …) | 合体セレクタ s1, s2, … のどれかに合致するE要素 — 加えて、詳細度にも寄与しない | Selectors Level 4 | [Link]() |
| 51 | E:has(rs1, rs2, …) | ［E要素を :scope 要素とする下で［ 相対セレクタ rs1, rs2, … のいずれか ］を評価したとき ］に，何らかの要素に合致するようなE要素 | Selectors Level 4 | [Link]() |
| 52 | E:any-link | リンクであるE要素 | Selectors Level 4 | [Link]() |
| 53 | E:local-link | 現在の URL を target にしているリンクであるE要素 | Selectors Level 4 | [Link]() |
| 54 | E:target-within | 現在の URL において target にされているか, それを包含しているE要素 | Selectors Level 4 | [Link]() |
| 55 | E:scope | :scope 要素に指定されているE要素 | Selectors Level 4 | [Link]() |
| 56 | E:current | 時系列再生の下で現在呈示中のE要素 | Selectors Level 4 | [Link]() |
| 57 | E:current(s) | 最も階層が深い :current 要素であって, セレクタ S に合致するE要素 | Selectors Level 4 | [Link]() |
| 58 | E:past | 時系列再生の下で過去に現れたE要素 | Selectors Level 4 | [Link]() |
| 59 | E:future | 時系列再生の下で未来に現れるE要素 | Selectors Level 4 | [Link]() |
| 60 | E:blank | 値が空欄である（空／欠落している）E型のユーザ入力要素 | Selectors Level 4 | [Link]() |
| 61 | E:user-invalid | 入力値が不正な（妥当でない†, 範囲外†, 必須にもかかわらず未入力）E型のユーザ入力要素 【†かつ，ユーザから何か入力されているような】 | Selectors Level 4 | [Link]() |
| 62 | E:nth-col(n) | グリッド／テーブル内の n 個目のカラムに属するセルを表現するE要素 | Selectors Level 4 | [Link]() |
| 63 | E:nth-last-col(n) | グリッド／テーブル内の最後から n 個目のカラムに属するセルを表現するE要素 | Selectors Level 4 | [Link]() |

## 属性値セレクタ

| No. | パターン | 選択対照 | 仕様 | Can I use |
| :--- | :-- | :-- | :-- | :-- |
| 64 | E[foo="bar" s] | foo属性の値がbarに一致するE要素（大文字小文字を無視しない） | Selectors Level 4 | [Link]() |

## 結合子

| No. | パターン | 選択対照 | 仕様 | Can I use |
| :--- | :-- | :-- | :-- | :-- |
| 65 | F || E | グリッド／テーブル内の，F要素で表現されるカラムに属するセルを表現するE要素 | Selectors Level 4 | [Link]() |
