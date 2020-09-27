---
title: "Flutterチュートリアル 1"
date: 2020-09-27T11:40:42Z
draft: false
author: "Masataka Okada"

# post thumb
image: "images/featured-post/flutter_eyec.jpg"

# meta description
description: "Flutterチュートリアル 1"

# taxonomies
categories:
  - "flutter"

# post type
type: "post"
---

# Flutter のサンプルコードについて

​
今回は Flutter のインストール時に書かれているサンプルコードを用いて Flutter の基礎を解説していきます。
サンプルコードには Flutter の基礎が詰まっているため、Flutter 初心者の方はまずこのコードを理解することから始めるとよいかと思います。
​

## Widget（ウィジェット）とは

​
Flutter に登場する専門用語はいくつかありますが、一番大切な Widget という言葉についてコードを見る前にその意味を知っておきましょう。
​
Widget とは Flutter の基本構成要素です。Flutter で作られる画面の１つ１つの要素はすべて Widget でできています。例えば、テキストを表示する Widget、アイコンを表示する Widget、要素を横並びに表示する Widget などといった感じです。
​
この Widget を組み合わせることで Flutter の画面が構成されています。
​

## サンプルコード解説

​
サンプルコードを添付します（見やすくするためにコメント行は省いています）
​

```Flutter
import 'package:flutter/material.Flutter';
​
void main() => runApp(MyApp());
​
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
​
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
​
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
​
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;
​
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
​
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
​
```

​
それではコードを解説します。
​

### main 関数

​

```Flutter
void main() => runApp(MyApp());
```

​
Flutter アプリを実行するとまず呼ばれるのがこの関数です。この関数によって MyApp クラスの内容が実行されます。
​

### MyApp クラス

​

```Flutter
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

​
main 関数から呼ばれたクラスです。ここで指定しているのはアプリのタイトル（title:）やアプリのテーマカラー（theme:）などアプリの基本となる情報です。ここはまだアプリの具体的な UI 表示には関わってきていません。基本設定のためのクラスだと思っていただければ大丈夫ですね。
​
具体的な画面の表示はホームプロパティ（home:）の MyHomePage クラス以降を見ていきます。
​

### MyHomePage クラスと\_MyHomePageState クラス

​

```Flutter
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
​
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
​
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;
​
  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
​
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.display1,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

​
構成としては MyHomePage クラス内の createState()関数で\_MyHomePageState クラスを呼んでいる形になっているかと思います。そして\_MyHomePageState 内のビルド関数で初期画面の UI を構成しています。
​
ここで Widget という言葉の意味を再度思い出してください。
例えばこれはテキストを表示するウィジェットです。中に表示したいテキストを書くだけで文字を表示してくれます。
​

```Flutter
Text(
  'You have pushed the button this many times:',
),
```

​
これはフローティングアクションボタンを表示するウィジェットです。Twitter で投稿を作成するときに押下する右下にあるボタンのことですね。
​
これは先ほどと異なり３つオプションがあります。onPressed にはボタンが押下されたときの挙動を書きます。tooltip にはマウスオーバーしたときに表示される言葉を書きます。child にはアイコンのデザインを書きます。今回は数字をインクリメントしたいのでプラスボタンのデザインが採用されています。
​

```Flutter
FloatingActionButton(
  onPressed: _incrementCounter,
  tooltip: 'Increment',
  child: Icon(Icons.add),
),
```

​
このように Flutter にはさまざまな Widget があり、Widget を組み合わせて UI を作成していきます。
​
構成がわかったところで実際にカウントアップのボタンを押下した時にどのような仕組みでアプリが動くのかを見ていきましょう。
​
まず、カウントアップのボタンが押下されると、FloatingActionButton ウィジェットの
onPressed オプションの\_incrementCounter メソッドが実行されます。そうすることで\_counter という変数が１ずつインクリメントされていくわけですね。
​

```Flutter
int _counter = 0;
​
floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
​
void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }
```

​
このとき setState というメソッドが気になったかと思います。
これは Flutter が用意してくれているメソッドで、画面を再読み込みさせるメソッドです。
つまり最初は０だった\_counter 変数を１にインクリメントした後に再読み込みさせるのです。
こうすることで画面上には１が表示されることになります。
​
ボタンを押下する度にこの動作を繰り返すことで 2.3.4...と画面上の数字が増加していく仕組みになっています。
​

## まとめ

今回は Widget という言葉の説明と UI の構成、サンプルコードがどのような動きをしているのかを簡単に解説しました。
どうやって数字が増えていくのかの大まかな仕組みがわかっていただけたと思います。
​
ややこしくしないためにあえて説明を省いたり、言葉を変更している箇所も多いので、もっと詳しく知りたい方は Flutter の公式サイトを見てみてください。
​
サンプルコードが理解できたら、ウィジェットを追加したり変更を加えたりしながら学んでみてください！
​
それでは！
