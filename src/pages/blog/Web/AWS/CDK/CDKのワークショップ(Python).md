---
title: "CDKのワークショップ(Python)"
templateKey: blog-post
date: 2020-07-14T23:35:00+09:00
tags: [CDK, Python]
draft: false
---

## 前提条件(PREREQUISITES)

[Prerequisites :: AWS Cloud Development Kit \(AWS CDK\) Workshop](https://cdkworkshop.com/15-prerequisites.html)

概要としては、AWS アカウントと AWS CLI、node.js(AWS CDK Toolkit の基盤)、AWS CDK Toolkit、各種言語(Node.js, Python, .NET, Java)のランタイムと IDE を用意することになる。

本ドキュメントでは Python をターゲットとする。

<!--more-->

CDK TOOLKIT について、インストール手順を引用する。詳細は[AWS CDK Toolkit :: AWS Cloud Development Kit \(AWS CDK\) Workshop](https://cdkworkshop.com/15-prerequisites/500-toolkit.html)を参照。

```bash
npm install -g aws-cdk
cdk --version
```

## 新しいプロジェクト

公式チュートリアルから、細かい手順を省いて最低限手順のみを以下に列挙する。不明点などがあれば公式サイトを参照する。

See also: [新しいプロジェクト](https://cdkworkshop.com/30-python/20-create-project.html)

### 初期設定手順 その 1

```bash
# ディレクトリ作成
mkdir cdkworkshop && cd cdkworkshop
# cdkの初期設定
cdk init sample-app --language python
# 上記手順で作成したvirutalenvの仮想環境をアクティブにする。
# (Windowsは `.env/Scripts/activate.bat` に読み替え)
source .env/bin/virtualenv
# pythonモジュールのインストール
pip install -r requirements.txt
```

#### (例) ./app.py

```python
#!/usr/bin/env python3

from aws_cdk import core

from cdkworkshop.cdkworkshop_stack import CdkWorkshopStack


app = core.App()
CdkWorkshopStack(app, "cdkworkshop", env={'region': 'us-west-2'})

app.synth()
```

リージョンなどはここで指定する

#### (例) ./cdkworshop/cdkworkshop_stack.py

```python
from aws_cdk import (
    aws_iam as iam,
    aws_sqs as sqs,
    aws_sns as sns,
    aws_sns_subscriptions as subs,
    core
)

class CdkWorkshopStack(core.Stack):

    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)

        queue = sqs.Queue(
            self, "CdkWorkshopQueue",
            visibility_timeout=core.Duration.seconds(300),
        )

        topic = sns.Topic(
            self, "CdkWorkshopTopic"
        )

        topic.add_subscription(subs.SqsSubscription(queue))
```

`__init__` 関数内で管理対象となるリソースを記載する。上記例では SQS キュー と SNS トピック。

### 初期設定手順 その 2

```bash
# スタックの合成（CloudFormationテンプレートが出力される。上記例の場合、SNSやSQSのリソースが対象となる
cdk synth
# ブートストラップスタックをインストールする。初めてデプロイするときに必要となる。
cdk bootstrap
# CDKアプリをデプロイする
cdk deploy cdkworkshop
```

### 再デプロイ手順

スタックの内容を変更した場合、以下を実行する。

```bash
# cdk差分の確認する。習慣として実行すべき。
cdk diff
# 変更をデプロイして反映する。
cdk deploy
```

### Lmabda と API Gateway の追加例

See also: [こんにちはラムダ:: AWS クラウド開発キット（AWS CDK）ワークショップ](https://cdkworkshop.com/30-python/30-hello-cdk/200-lambda.html)
See also: [API ゲートウェイ:: AWS クラウド開発キット（AWS CDK）ワークショップ](https://cdkworkshop.com/30-python/30-hello-cdk/300-apigw.html)

CDK プロジェクト内に、コードを作成する。

#### (例) ./lambda/hello.py

```python
import json


def handler(event, context):
    print('request: {}'.format(json.dumps(event)))
    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'text/plain'
        },
        'body': 'Hello, CDK! You have hit {}\n'.format(event['path'])
    }
```

#### AWS Lambda 構成ライブラリをインストールする

AWS Lambda モジュールとそのすべての依存関係をプロジェクトにインストールします。

```bash
pip install aws-cdk.aws-lambda
```

#### (例) ./cdkworkshop/cdkworkshop_stack.py

スタックに AWS Lambda 関数を追加する。

注意として、

- `lambda`は Python の組み込み識別子のため`_lambda`を使用している
- ハンドラコードのパスは CDK プロジェクトルートからの相対パス。
- ハンドラ関数の名前は `ファイル名.関数名`

```python
from aws_cdk import (
    aws_lambda as _lambda,
    aws_apigateway as apigw,
    core,
)


class CdkworkshopStack(core.Stack):

    def __init__(self, scope: core.Construct, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)

        my_lambda = _lambda.Function(
            self, 'HelloHandler',
            runtime=_lambda.Runtime.PYTHON_3_7,
            code=_lambda.Code.asset('lambda'),
            handler='hello.handler',
        )

        apigw.LambdaRestApi(
            self, 'Endpoint',
            handler=my_lambda,
        )
```

デプロイは前述と同様。

```bash
cdk diff cdkworkshop
cdk deploy cdkworkshop
```

出力の中に以下のような API Gateway エンドポイントの URL が含まれる。

```bash
CdkWorkshopStack.Endpoint8024A810 = https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/prod/
```

URL を使用し手、以下のように API のテストが実行できる。(Lambda 直接のテストは AWS コンソールなどから実施する)

```bash
curl https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/prod/
```

#### その他の公式ドキュメント

以下のページから、Lambda、APIGateway、DynamoDB を使用した実例を確認できる。内容には、デプロイ失敗時の`CloudWatchログ`の調べ方、DynamoDB テーブルへの読み書き権限や Lambda 呼び出し権限の付与方法が含まれる。

[構成の作成:: AWS クラウド開発キット（AWS CDK）ワークショップ](https://cdkworkshop.com/30-python/40-hit-counter.html)

以下のページでは DynamoDB のテーブルビューアの使用方法が含まれる。テーブルビューアを使うことで、DynamoDB の内容を HTML ページで表示できる。
![テーブルビューア](https://cdkworkshop.com/30-python/50-table-viewer/viewer1.png)

[Using construct libraries :: AWS Cloud Development Kit \(AWS CDK\) Workshop](https://cdkworkshop.com/30-python/50-table-viewer.html)

テーブルビューアのソースは以下に公開されている。

[GitHub \- eladb/cdk\-dynamo\-table\-viewer: A CDK construct which exposes an endpoint with the contents of a DynamoDB table](https://github.com/eladb/cdk-dynamo-table-viewer)

### スタックの削除

スタックの削除は、以下のコマンドを実行する。あるいは AWS コンソールからスタックを削除する。

```bash
cdk destroy
```
