---
title: "Goプログラミング実践入門 標準ライブラリでゼロからWebアプリを作る"
date: 2020-10-04T08:57:10Z
draft: false
author: "Taiga Kato"

# post thumb
image: "images/post/golang.jpeg"

# meta description
description: "Goプログラミング実践入門 標準ライブラリでゼロからWebアプリを作る"

# taxonomies
categories:
  - "golang"
  - "bookreview"

# post type
type: "post"
---

## はじめに

{{< amazon asin="B06XKPNVWV" title="Goプログラミング実践入門 標準ライブラリでゼロからWebアプリを作る" >}}

## 第一部まとめ

- POSTは冪等性でも安全でもないHTTPメソッド
- PUTとDELETEは冪等性のため、同時に２回呼び出しても状態は変わらない（ただリソースを変えるので安全ではない）
- GET、HEAD、OPTIONS、TRACEは取得するだけでリソースに変更を加えない安全なメソッド

- __ステータスコードまとめ__

| ステータスコード |                                            | 
| -------------- |:-----------------------------------------:| 
| 1xx        　　| 情報提供                                    | 
| 2xx        　　| 成功    200 OK や 201 Createdなど           | 
| 3xx        　　| リダイレクト サーバーからクライアントにリダイレクト | 
| 4xx        　　| クライアントエラー リクエスト誤り                | 
| 5xx        　　| サーバーエラー 500 Internal Server Errorが有名 | 

- __URI__

クエリの「?」やフラグメント（クライアント側で処理される）「＃」は予約語のため、URLエンコードされる
空白文字は%20のようになる

- __HTTP__

HTTP/1.xで送れるリクエストは1回につき1個

HTTP/2はHTTPメソッドとステータスは変わらないが複数リクエスト、複数レスポンスのやりとりが可能

- __Webアプリとは__

HTTPリクエストをハンドラで受けつけ、サーバーで処理をしたHTMLをレスポンスとして返すテンプレートエンジンと言える

MVCはプログラムを書きやすいようにそれを３つに分けたものでそれ自体がWebの仕組みではない

- __環境構築周り__
1. **Goのワークスペース**

$GOPATH配下に以下のディレクトリが必要
```
src = ソースファイル

pkg = パッケージ

bin = コンパイルされ実行形式になったファイル
```
2. **$GOPATH以外でライブラリを管理するためのgo mod**

ただ1.の設定ではプロジェクトごとにディレクトリを分けて管理ができなくなるため、Go 1.11でアップデートされgo modがインストールできるようになった

Dockerfileで環境を構築している場合は以下のように環境変数を設定することで使用できるようになります。

```Docker
# Go Modules を使用するために必要な環境変数 GO111MODULE を on 
ENV GO111MODULE=on
```

3. **go modの使い方**


>go mod init
>
>↓
>
>go build
>
>でdocker内部の~go/pkg/modにライブラリが保存され
>
>↓
>go install
>
>で$GOPATH/binにバイナリファイルを作成しそれを叩いてる？

Go Modules ではプロジェクトを GOPATH の外に置けるが、ライブラリのキャッシュや go install のコピー先として GOPATH (デフォルトで ~/go) が使われる。