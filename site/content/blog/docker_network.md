---
title: "Dockerネットワーク"
date: 2020-10-09T11:02:32Z
draft: false
author: "Masataka Okada"

# post thumb 
image: "images/post/docker.png"

# meta description
description: "docker-composeのネットワークをつなげる方法"

# taxonomies
categories:
  - "Docker"

# post type
type: "post"
---

## docker-composeのネットワークについて

​
今回はdocker-composeのネットワーク同士を繋げる方法について説明していきます。

Dockerを使って開発する際にAPI側とフロント側でリポジトリを分けたいというモチベーションが出てくる場合があるかと思います。

ですが、docker-compose.ymlを分割すると別のネットワークになってしまい、コンテナからAPIにアクセスすることができなくなってしまいます。

それを解決する方法がないかと調べてみました。

## 結論

ネットワークを別で作成し、それぞれのdocker-composeにそのネットワークを指定する。

## 具体的なコード

まず、共通で使うネットワークを作成します。

````
docker network create --driver bridge docker_network
````

あとはそれぞれのdocker-composeに作成したネットワークを使うように指定するだけです。

````
services:
  front:
    build: .
    volumes:
      - .:/usr/src/app
    stdin_open: true
    command: sh -c "cd react-sample && yarn start"
    ports:
      - "8000:3000"
    tty: true
    networks:
      - docker_network
networks:
  docker_network:
    external: true
````

## まとめ

もっと良いやり方があるのかと思いましたが、このような方法しか見つけることができませんでした。

dockerのデータが逼迫してきて

````
docker system prune
````

などを使用すると作成したネットワークが消えてしまうのであまり良くないです。

よりよい方法を見つけるために日々精進していきます！
それでは！
