<!--
# Amazon Q Developerによる自然言語を利用したコスト分析
-->

この記事は [ZOZO Advent Calendar 2024 #8 23日目](https://qiita.com/advent-calendar/2024/zozo) の記事です。

## はじめに

2024年11月、Amazon Q Developerにコスト分析機能が追加されたことが発表されました。本記事では、その機能の使い方を深堀りしてみます。

https://aws.amazon.com/jp/about-aws/whats-new/2024/11/amazon-q-developer-natural-language-cost-analysis/

https://aws.amazon.com/jp/blogs/news/analyzing-your-aws-cost-explorer-data-with-amazon-q-developer-now-generally-available/

:::note warn
本記事に記載されているAmazon Qの出力内容は、ZOZOTOWNの実際のAWSコストに関連していません。しかし、誤解を招かないよう、数字などの情報にはマスキングを施しています。
:::

## 概要

Amazon Q Developerのコスト分析機能は、Cost Explorerと連携し、ユーザーがAWSのコストに関する質問を自然言語で行い、コスト履歴に基づく回答を得ることができる機能です。回答にはCost Explorerのパラメータやコンソールのリンクも含まれ、データの透明性が確保されます。つまり提示されたデータが求めているものであるか確認することができます。

## 導入方法

導入方法は、Cost Explorerのコンソールから画面右上にある「Amazon Q」を開くだけです。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/45634/813d5e18-97d4-6e4d-3d5f-aa877eaf13f9.png)

また、事前に `ce:GetCostAndUsage` のアクセス許可を設定しておく必要があります。

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "EnablesCostAnalysisInAmazonQ",
			"Effect": "Allow",
			"Action": [
				"q:*",
				"ce:GetCostAndUsage"
			],
			"Resource": "*"
		}
	]
}
```

### 料金

Amazon Qでコスト分析機能を使用する際に追加料金は発生しませんが、Amazon Q自体にはクォータが設定されているので、その点は把握しておくと良さそうです。

https://aws.amazon.com/jp/q/developer/pricing/

## プロンプトについて

Amazon Qは現在、以下のAPIと統合されています。つまり、これ以外のAPIを必要とする質問には対応していないということです。

- [GetCostAndUsage](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostAndUsage.html)
- [GetCostForecast](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostForecast.html)
- [GetDimensionValues](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetDimensionValues.html)
- [GetCostCategories](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostCategories.html)
- [GetTags](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetTags.html)

[ドキュメント](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-q-prompting-guidance.html)には、サポートされている質問とサポートされていない質問が記載されていました。その部分を引用します。なお、翻訳にはGoogle Translateを使用しました。

### サポートされている質問

| 質問のカテゴリ | 訳 | 例 | 訳 |
| --- | --- | --- | --- |
| Total costs | 総費用 | What were my costs last month? | 先月の費用はいくらでしたか? |
| Costs for a specific dimension value | 特定のディメンション値のコスト | What were my costs for S3 last month? | 先月の S3 のコストはいくらでしたか? |
| Costs broken down by a dimension | ディメンション別に見たコスト | What were my costs by service last month? | 先月のサービス別の費用はいくらでしたか? |
| Top filter or bottom filter | 上部フィルターまたは下部フィルター | What were my five most expensive services last month? | 先月最も高額だった 5 つのサービスは何でしたか? |
| Costs by charge type | 料金タイプ別の費用 | Did we receive any credits last month? | 先月はクレジットを受け取りましたか? |
| Costs for a relative time period | 相対的な期間のコスト | What were my costs last week? | 先週の費用はいくらでしたか? |
| Costs for an absolute time period | 絶対期間のコスト | What were my costs from 10/1/2024 to 10/7/2024? | 2024 年 10 月 1 日から 2024 年 10 月 7 日までのコストはいくらでしたか? |
| Time period aggregation | 期間の集計 | What were my costs for Q1? | Q1 のコストはいくらでしたか? |
| Total cost forecasts | 総コスト予測 | What is our cost forecast for this month? | 今月のコスト予測はいくらですか? |
| Cost allocation tags | コスト配分タグ | What was last month’s spend for tag key = “Application”, value = “web-app-1”? | タグキー = “Application”、値 = “web-app-1” の先月の支出はいくらでしたか? |
| Cost categories | コストカテゴリ | What was last month’s spend, broken down by cost category “cost center”? | 先月の支出をコストカテゴリ「コストセンター」別に見るといくらでしたか? |
| Month-over-month changes | 前月比の変化 | What services increased the most between April and May? | 4月から5月にかけて最も増加したサービスは何ですか? |
| List items | リスト項目 | What instance types did we use last month? | 先月はどのインスタンスタイプを使用しましたか? |
| Cost metrics | コスト指標 | What were my net amortized costs last month? | 先月の純償却費用はいくらでしたか? |

### サポートされていない質問

| 質問カテゴリ | 訳 | 例 | 訳 |
| --- | --- | --- | --- |
| "Why" questions | 「なぜ」の質問 | Why did my costs increase last month? | 先月費用が増加したのはなぜですか? |
| Data transfer | データ転送 | How much are we spending on data transfer? | データ転送にどれくらいの費用がかかりますか? |
| Free Tier | 無料レベル | Why am I being charged as I thought my instance was part of Free Tier? | インスタンスは無料利用枠の一部だと思っていたのに、なぜ料金が請求されるのですか? |
| Pricing | 価格 | How much does an EC2 t3.xlarge instance cost? | EC2 t3.xlarge インスタンスの料金はいくらですか? |
| Cost optimization or savings | コストの最適化または節約 | How can I lower my bill? | 請求額を下げるにはどうすればいいですか? |
| Savings Plans or reserved instance utilization or coverage | 貯蓄プランまたは予約インスタンスの利用率またはカバレッジ | What is my Savings Plan utilization rate? | 私の貯蓄プランの利用率はどれくらいですか? |
| Savings Plans or reserved instance inventory | Savings Plans または予約インスタンス在庫 | What active Savings Plans do I have? | 有効な貯蓄プランは何ですか? |
| Invoices or payments | 請求書または支払い | Did my last payment go through? | 前回の支払いは完了しましたか? |
| Specific resources | 特定のリソース | Which EC2 instance did I spend the most on? | どの EC2 インスタンスに最も多くの費用がかかりましたか? |
| AWS Budgets | AWS 予算 | How much of my budget have I consumed? | 予算はどれくらい消費しましたか? |
| AWS Cost Anomaly Detection | AWS コスト異常検出 | Have you detected any anomalies in my spending? | 私の支出に何か異常は見つかりましたか? |
| Service categories | サービスカテゴリー | How much did we spend on databases in October? | 10 月にデータベースにいくら費やしましたか? |
| Usage types | 使用タイプ | How much did we spend on EBSVolumeUsage:io2 last month? | 先月、EBSVolumeUsage:io2 にいくら費やしましたか? |
| API operations | API操作 | What was the spend on the NatGateway operation yesterday? | 昨日の NatGateway オペレーションの支出はいくらでしたか? |
| Usage amounts | 使用量 | How many EC2 instance hours did we use last month? | 先月、EC2 インスタンス時間を何時間使用しましたか? |

## 使ってみる

まず、「サポートされている質問」をした場合、どのような回答が得られるのかを確認します。

### サポートされている質問 その１

**質問**

> What were my costs last month?  
> 先月の費用はいくらでしたか?

**回答**

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/45634/996e0602-080f-cfbc-8923-3212505c0eea.png)

> Your total costs last month (November 2024) were $xxx.
> 
> 
> *The following Cost Explorer parameters were used to answer your question. Use the Cost Explorer link below to verify your data.*
> - Date range: *2024-11-01 through 2024-11-30 (inclusive)*
> - Granularity: *"MONTHLY"*
> - Metrics: *"UnblendedCost"*
> - Filter: *Excludes Dimensions RECORD_TYPE: "Credit", "Refund"*
> 
> Sources
> [1] [Visualize this data in AWS Cost Explorer] (https://us-east-1.console.aws.amazon.com/cost-management/xxx)
> [3] [AWS Cost Explorer GetCostAndUsage API Reference] (https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostAndUsage.html)
> [6] [Prompting guidance for cost analysis in Amazon Q Developer] (https://docs.aws.amazon.com/cost-management/latest/userguide/ce-q-prompting-guidance.html)

上記のように金額が表示され、その金額を算出するために使用されたCost Explorerのパラメータも表示されます。また、「Visualize this data in AWS Cost Explorer」にはコンソールのURLリンクが貼られています。リンクを開くと、「New cost report from Amazon Q Developer」というタイトルが付けられたコンソールが表示されます。そこに表示された金額が、回答に記載されている金額と一致することが確認できました。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/45634/d9d6081b-8fb9-d63c-07c2-8db81d6c154d.png)

### サポートされている質問 その2

**質問**

> What is our cost forecast for this month?  
> 今月のコスト予測はいくらですか?

**回答**

（以下、回答に含まれるパラメータおよびリンク部分は省略します。）

> Your forecasted cost for this month is $xxx.


こちらでも、回答とコンソールに表示された金額が一致することが確認できました。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/45634/804edf12-5465-db7d-9936-860da35bdccd.png)


次に、「サポートされていない質問」についても確認します。

### サポートされていない質問 その1

**質問**

> Why did my costs increase last month?  
> 先月費用が増加したのはなぜですか?

**回答**

> I'm sorry, but I don't have enough information to determine why your costs increased last month. The data shows the total cost for the previous month, but the current month's total cost is missing. Without knowing the current month's costs, I can't calculate the exact amount or percentage of the increase. Please let me know if you have any other questions I can assist with.
> 申し訳ございませんが、先月のコストが増加した理由を判断するのに十分な情報がありません。データには前月の合計コストが表示されていますが、今月の合計コストがありません。今月のコストがわからないため、増加の正確な金額や割合を計算することはできません。他にご質問がありましたら、お知らせください。

### サポートされていない質問 その2

**質問**

> How can I lower my bill?  
> 請求額を下げるにはどうすればいいですか?

**回答**

> To help lower your AWS bill, here's a step-by-step guide to identify cost-saving opportunities and implement best practices for cost optimization:
> 
> 1. **Analyze Your Current Costs**
>    - Open the AWS Cost Explorer in the AWS Management Console.
>    - Review your cost breakdown by service, region, and account.
>    - Identify your highest cost areas and any unexpected spikes in usage.
> 
> 2. **Utilize AWS Cost Optimization Tools**
>    - Enable AWS Trusted Advisor to receive recommendations on cost optimization.
>    - Use AWS Compute Optimizer to get instance type recommendations.
>    - Implement AWS Budgets to set custom cost and usage budgets.
> 
> <以下略>

コスト最適化に関するベストプラクティスが出力されました。

以上の結果から、「サポートされていない質問」をしても、間違った金額や情報が出力されることはなく、いわゆるハルシネーションを起こさないことが確認できました。

## 実践的な使い方をしてみる

もう少し複雑で実践的な質問をしてみます。

### シナリオ1: 繁忙期と高コストサービスを調べる

**質問**

> What were the three most expensive months last year and the three most expensive services in each of those months?
> 昨年、最も高額だった3カ月と、その各月で最も高額だった3つのサービスを教えてください。

**回答**

> The three most expensive months last year were:
> 1. **2023-11**  
> SERVICE: Amazon Relational Database Service | Service Cost: $xxx
> 2. **2023-11**  
> SERVICE: EC2 - Other | Service Cost: $xxx  
> 
> <以下略>

なぜか特定の月に高額だったサービスが列挙されてしまいました。これは望んだ回答ではなかったため、質問文を変更してみることにしました。

**質問**

> Examine the top three most costly months from January 2024 through November 2024 and list the services that increased the most during those months.
> 2024年1月から2024年11月までに最もコストがかかった上位3ヶ月を調べ、その月に最も増加したサービスをリストアップする。

**回答**

> The top 3 most costly AWS services from January 2024 through November 2024 were:
> 
> 1. Amazon Relational Database Service: $xxx
> 2. EC2 - Other: $xxx
> 3. AmazonCloudWatch: $xxx

今度は、期間内で高額だったサービスの上位3つが出力されました。このように、質問の回答を基に別の質問をするような形式では、意図した回答が得られないことが分かりました。

改めて、質問を分割して確認しました。

**質問**

> What were my three most expensive months for last year?
> 昨年、最も出費の多かった3カ月は？

**回答**

> The three most expensive months for AWS last year were:
> 1. October 2023: $xxx
> 2. December 2023: $xxx
> 3. November 2023: $xxx

**質問**

> What were my three most expensive services in October 2023?
> 2023年10月に最も高額だった3つのサービスは？

**回答**

> According to the information provided, your three most expensive AWS services in October 2023 were:
> 
> 1. **Amazon Relational Database Service**: $xxx
> 2. **EC2 - Other**: $xxx
> 3. **AmazonCloudWatch**: $xxx

意図した回答であることを確認できました。

### シナリオ2: コストの増減を考慮した次期の見積もりをする

**質問**

> What is the cost projection from 4/1/2025 to 9/30/2025 assuming a 1.5x cost increase?
> 1.5倍のコスト増を想定した場合、2025年4月1日から9月30日までのコスト見通しは？

**回答**

> The projected cost from 4/1/2025 to 9/30/2025 with a 1.5x cost increase is:
> 
> 1. **2025-04**: $xxx
> 2. **2025-05**: $xxx
> 3. **2025-06**: $xxx
> 4. **2025-07**: $xxx
> 5. **2025-08**: $xxx
> 6. **2025-09**: $xxx
> 
> To calculate the projected cost with a 1.5x increase, I multiplied each of the original monthly costs by 1.5.

意図通り、月々の想定コストに1.5を掛けた金額を得ることができました。

### シナリオ3: 異常値を検出する

指定したサービスが平均コストを上回った月を調べます。ここでも質問を分割します。

**質問**

> What werw the total and average RDS costs over the past 6 months?
> 過去6ヶ月間のRDSの総コストと平均コストは？

**回答**

> Over the past 6 months, the total RDS costs were \\$xxx and the average RDS costs were \\$xxx.

**質問**

> In which months in the last 6 months has the cost of RDS exceeded $xxx?
> 過去6ヶ月のうち、RDSの費用がxxxドルを超えた月はありますか？

**回答**

> In the last 6 months, the cost of Amazon Relational Database Service (RDS) exceeded $xxx in the following months:
> - September 2024: $xxx
> - June 2024: $xxx

ここでも、質問を分割することで意図する回答を得ることができました。

## まとめ

Cost Explorerに十分精通している方には不要かもしれませんが、Amazon Qを使用することで、Cost Explorerの複雑なパラメータを操作せずに、スピーディーにコスト分析が行えます。さらに、コンソールのURLも記載されるため、正しい結果かどうかを確認できます。ただし、複雑な分析を行う場合は、質問を分割するなどの工夫が必要です。また、英語の微妙なニュアンスの違いによって出力結果が異なることもありました。これは今後のモデル強化や日本語対応に期待したい点です。

以上です。
