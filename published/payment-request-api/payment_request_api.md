# Web決済を快適にするPayment Requesrt API

## Payment Requesrt APIとは

Googleの調査では、ユーザーはPCで購入する場合より2倍も多く、モバイルでの購入を取りやめており、そのほとんどの原因が入力フォームの使いづらさにあるそうです。

この問題を解決するために「ブラウザ自体に決済フォーム機能を付けちゃえば良いのでは？」と考えられたのがPayment Request APIです。

実際どんなものなのかはこちらの動画で確認できます。
https://www.youtube.com/watch?v=undqD82MBvA

ここで表示されるUIはサイト開発者が作ったものではなくブラウザの機能として提供しているもので、これにより従来のフォームよりも手軽に正確な情報をユーザーから取得することが可能になります。

## どう便利なのか

Payment Requesrt APIのもっとも大きなメリットは、Googleアカウント、Microsoftアカウント等からユーザーの支払い情報を取得、またはブラウザに保存された情報からフォームの自動入力が行われユーザーの負担を大きく減らすことができるということです。

また、サイトによってバラバラだった入力項目、入力方法（全角・半角、ハイフンのありなし、ふりがなとフリガナ）が統一され一貫性のあるUI/UXを提供できるようになります。

開発者側のメリットとしては、Web標準に則ったフォーマットで情報を扱えるようになることや豊富な支払いオプション、配送オプションを手軽に実装できることが挙げられます。

## 実装してみた

実際に実装してみたサンプルが次のURLになります。
PCからはChromeまたはEdgeで、モバイルからはChromeからアクセスしてみてください。
https://paymentapi-de226.firebaseapp.com/

ソースコードはこちらです。
https://github.com/taquaki-satwo/paymentAPI

メインのコードは以下です。

```JavaScript
window.onload = function () {
  var $btn = document.getElementById('btn');
  // 支払いボタンをクリックでAPIをキック
  $btn.addEventListener('click', onBuyClicked, false);
}

function onBuyClicked() {
  // Payment Request APIが使用できない環境では従来の決済ページ（/checked）に移動させる
  if (!window.PaymentRequest) {
    location.href = '/checkout';
    return;
  }

  // 支払いオプションの設定をする
  var supportedInstruments = [{
    supportedMethods: ['basic-card'],
    data: {
      supportedNetworks: [
        'visa', 'mastercard', 'amex', 'discover', 'diners', 'jcb', 'unionpay'
      ]
    }
  }];

  // 支払い金額を設定する
  var details = {
    displayItems: [
      {
        label: '通常商品',
        amount: { currency: 'YEN', value: '10000' }
      },
      {
        label: 'クーポン',
        amount: { currency: 'YEN', value: '-2000' }
      }
    ],
    total: {
      label: '合計',
      amount: { currency: 'YEN', value: '8000' }
    }
  };

  // PaymentRequestオブジェクトを生成しブラウザに表示する
  var request = new PaymentRequest(supportedInstruments, details);
  request.show();
}
```

## 実際問題使えるのか

さて、良い事だらけに見えるPayment Requesrt APIですが欠点もあります。
まずブラウザの実装状況です。

<!-- image -->

Can I Useで確認したところ、Edge、Chromeが先行してますが、日本では特にユーザーが多いiOS Safariの実装が進んでいません。polyfill（古いブラウザ向けの機能拡張のようなもの）の提供もないそうです。Firefoxに至っては…。

次にもっと重要な話をすると、Payment Requesrt APIは入力フォームを提供するだけのものであり、サーバーサイドの実装は全然別の話であるという点です。

ただ前述のSafariですが、Mac開発者用ベータ版でプレビューが進んでおり来年以降のiOS Safariにも実装されることが予想されます。そうなると事例や利用者が増え導入の動機が高まりそうです。

## 参考

Payment Request API: 統合ガイド
https://developers.google.com/web/fundamentals/payments/?hl=ja

Payment Request API で簡単・高速な決済を実現する
https://developers.google.com/web/updates/2016/07/payment-request?hl=ja
