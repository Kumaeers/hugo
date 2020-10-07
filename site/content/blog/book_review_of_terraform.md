---
title: "「実践terraform awsにおけるシステム設計とベストプラクティス」をやってみた"
date: 2020-09-30T08:07:58Z
draft: false
author: "Taiga Katou"

# post thumb 
image: "images/post/terraform.jpg"

# meta description
description: "実践terraform awsにおけるシステム設計とベストプラクティスをやってみた"

# taxonomies
categories:
  - "Terraform"
  - "bookreview"

# post type
type: "post"
---

## はじめに
前からずっとInfrastructure as a codeに興味があり、こちらの本を写経してみました！

{{< amazon asin="4844378139" title="実践terraform awsにおけるシステム設計とベストプラクティス" >}}

ECS(Fargate)とCodepipelineを使って、
CI/CDのできるVueとGoのAWS環境を作ることができたので非常に良かったです。

また作業してて
```terraform
terraform destroy
```

一発でAWSの環境をお掃除できたので、コストをかけることなくAWSの学習を進められました！

コンソールからぽちぽちやるのではなくコードで記述することで、
再現性のある環境を作れるのがインフラのコード化の大きなメリットだと感じました。

## 成果物

実際にこの本を参考にして作った環境も載せておきます。

* <https://github.com/Kumaeers/terraform_fargate_CI-CD> 


Terraformの実行環境はdockerで構築し、ローカルの環境変数にAWSのキーをexportしそれをdockerにマウントすることでaws cliを起動しています。

Terraformのバージョンはv0.12.5です。

## 書評

今まで業務などで用語でしか知らなかった、ターゲットグループやCloudWatchLogsなどについて体系的に学習することができたのがとても良かったです。

またずっとECSのCI/CD環境構築に興味があったので、それを体系的に学べて実装までできたのは大きかったです。

コンソールとは違いコード化しているため、ここはどういう意味なんだろうとか思ったらそれをコメントとして記述できるのでAWSのリソースをしっかり理解しながら作れました。

⬇︎こんな風に

```Flutter
# ECSサービスは起動するタスクの数を定義でき、指定した数のタスクを維持　なんらかの理由でタスクが終了してしまった場合、自動的に新しいタスクを起動してくれる
# またECSサービスはALBとの橋渡し役にもなり、インターネットからのリクエストはALBで受けそのリクエストをコンテナにフォワードさせる
resource "aws_ecs_service" "tech-blog" {
  name            = "tech-blog"
  cluster         = aws_ecs_cluster.tech-blog.arn
  task_definition = aws_ecs_task_definition.tech-blog.arn
  # 2個以上コンテナ起動する
  desired_count = 2
  launch_type   = "FARGATE"
  # latestが最新ではないので明示的にする必要あり
  platform_version = "1.3.0"
  # タスク起動時のヘルスチェック猶予期間 0だとタスクの起動と終了が無限に続く可能性あり
  health_check_grace_period_seconds = 60

  # サブネットとセキュリティグループを設定
  network_configuration {
    assign_public_ip = false
    security_groups  = [module.nginx_sg.security_group_id]

    subnets = [
      aws_subnet.private_0.id,
      aws_subnet.private_1.id,
    ]
  }
```

AWSのリソースについては補足として

{{< amazon asin="4297113295" title="みんなのAWS" >}}

これや公式ドキュメントを読みながら進めていくと、かなり理解が進むのでオススメです。

この本をやればTerraformの公式ドキュメントさえあればなんでもリソースが作れる気がします！

それくらい良書でした。

あとTerraformのディレクトリ構成やモジュール化についてはまだまだ改善できそうなのでやっていきたいです。

またこの本の続編が出たらしいのでそちらもやってみたいですね！！！！！

* https://nekopunch.hatenablog.com/entry/2020/09/11/084342



