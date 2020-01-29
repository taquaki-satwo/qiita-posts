EC2インスタンスのCPU使用率が決められた値以上になったらAWS CloudWatchからSlackへ通知を行う。
その設定をしたときの覚書。

## 手順

1. SNSの設定
2. CloudWatchの設定
3. Incoming WebHooksの設定
4. KMSの設定
5. Lambdaの設定
6. IAMロールの修正

以下特に記述がないものはデフォルト設定。

## 1. SNS設定

### トピック作成

Amazon SNS -> トピック -> トピックの作成

| 項目 | 設定 |
|---|---|
| 名前 | cloudwatch-alarm |
| 表示名 | CloudWatch アラーム |

-> トピックの作成

### サブスクリプション作成

トピック -> 作成したトピックを選択 -> サブスクリプションの作成

| 項目 | 設定 |
|---|---|
| プロトコル | Eメール |
| エンドポイント | <通知したいメールアドレス> |

-> サブスクリプションの作成

確認メールの Confirm subscription をクリック

### テスト配信

トピック -> 作成したトピックを選択 -> メッセージの発行

| 項目 | 設定 |
|---|---|
| 件名 | SNS-test-message |
| メッセージ本文 | テスト配信です |

-> メッセージの発行

## 2. CloudWatchの設定

### 事前準備

事前に以下の設定をしておく。

1. AWS CLIのインストール
2. IAMでアクセスキーの作成
3. AWS CLI configure に Profile を設定

### アラーム作成

CloudWatch -> アラーム -> アラーム作成

* メトリクス

| 項目 | 設定 |
|---|---|
| インスタンス名 | <任意のEC２インスタンス> |
| メトリクス名 | CPUUtilization |

-> 次へ

* 条件

次の項目はお好みで。

| 項目 | 設定 |
|---|---|
| よりも | 60 |
| アラームを実行するデータポイント | 3/3 |

-> 次へ

* 通知

| 項目 | 設定 |
|---|---|
| 通知の送信先 | cloudwatch-alarm |

-> 次へ

* 名前と説明

| 項目 | 設定 |
|---|---|
| 一意の名前を定義 | ec2-CPUUtilization-check |
| アラームの説明 | EC2インスタンスのCPU使用率が60%を超えました |

-> 次へ -> アラームの作成

### アラームのテスト

```bash
aws cloudwatch set-alarm-state --alarm-name "ec2-CPUUtilization-check" --state-value ALARM --state-reason "alarm-test" --profile <事前準備で設定したProfile名>
```

#### コマンドオプション

| オプション | 説明 |
|---|---|
| --alarm-name | アラーム名 |
| --state-value | ステータス |
| --state-reason | アラートの説明 |

## 3. Incoming WebHooksの設定

https://slack.com/apps/A0F7XDUAZ--incoming-webhook- -> Add to Slack

| 項目 | 設定 |
|---|---|
| Post to Channel | <#通知したいSlackチャンネル> |
| Customize Name | CloudWatch アラーム |

-> Webhook URLを コピーしておく

## 4. KMSの設定

KMS -> カスタマー管理型のキー -> キーの作成

| 項目 | 設定 |
|---|---|
| エイリアス | cloudwatch-alerm-to-slack |
| 説明 | Slack App Incoming WebHooks の Webhook URL を暗号化するために使用します |
| キーの管理者 | 任意 |
| このアカウント | 任意 |

-> 完了 -> ARN をコピーしておく

## 5. Lambdaの設定

### 関数の作成

Lambda -> 関数 -> 関数の作成

| 項目 | 設定 |
|---|---|
| 設計図 | cloudwatch-alarm-to-slack-python |
| Function name | cloudwatch-alerm-to-slack |
| Execution role | AWS ポリシーテンプレートから新しいロールを作成 |
| Role name | cloudwatch-alerm-to-slack |
| SNS トピック | cloudwatch-alarm |
| トリガーの有効化 | チェックを入れる |
| Encryption configuration | Enable helpers for encryption in transit をチェック |
| slackChannel | <#通知したいSlackチャンネル> |
| kmsEncryptedHookUrl | 3の Webhook URL を入力 -> Encrypt |
| AWS KMS key to encrypt in transit | cloudwatch-alerm-to-slack |

-> Encrypt -> 関数の作成

### コードの修正

```python
- HOOK_URL = "https://" + boto3.client('kms').decrypt(CiphertextBlob=b64decode(ENCRYPTED_HOOK_URL))['Plaintext'].decode('utf-8')
+ HOOK_URL = boto3.client('kms').decrypt(CiphertextBlob=b64decode(ENCRYPTED_HOOK_URL))['Plaintext'].decode('utf-8')
```

```python
    alarm_name = message['AlarmName']
    #old_state = message['OldStateValue']
    new_state = message['NewStateValue']
    reason = message['NewStateReason']
+    alarm_description = message['AlarmDescription']
```

```python
-        'text': "%s state is now %s: %s" % (alarm_name, new_state, reason)
+        'text': "<!here> \nアラーム名: %s\nステータス: %s\nアラーム理由: %s\n説明: %s" % (alarm_name, new_state, reason, alarm_description)
```

## 6. IAMロールの修正

IAM -> ロール -> cloudwatch-alerm-to-slack -> AWSLambdaKMSExecutionRole-\*\*\*\*\*\*\*\*-\*\*\*\*-\*\*\*\*-\*\*\*\*-\*\*\*\*\*\*\*\*\*\*\*\* -> ポリシーの編集 -> JSON

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1443036478000",
            "Effect": "Allow",
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": [
                "<4でコピーした ARN>"
            ]
        }
    ]
}
```

-> ポリシーの確認 -> 変更の保存

### 通知テスト

```bash
aws cloudwatch set-alarm-state --alarm-name "ec2-CPUUtilization-check" --state-value ALARM --state-reason "alarm-test" --profile <Profile名>
```

<img width="549" alt="img.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/45634/c7a07647-a0d2-3c8f-ed8c-5ba8190d6ea1.png">

## 参考

以下の記事を参考にさせていただきました。ありがとうございます :pray:

* [CloudWatchアラーム通知をSlackにする](https://qiita.com/hf7777hi/items/e0f43f0fb7e2effa0af8)
* [【AWS】CloudWatchアラーム通知をLambdaでSlack投稿する](https://www.geekfeed.co.jp/geekblog/aws_cloudwatch_to_slack/)

