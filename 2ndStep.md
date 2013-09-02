# ワークショップ「JavaScriptでスマフォアプリ開発を学ぼう」基礎編:HttpClientとイベントリスナー

![Titanium Mobileとは？の画像](image/1stStep-001.png)

## はじめに
このワークショップでは、JavaScript でiPhone/Android向けのアプリの開発が出来るTitanium Mobileというアプリケーションを使ったスマフォアプリの作り方を教えます

ワークショップの所要時間としては60分程度を想定しております。

## 想定対象者

[「JavaScriptでスマフォアプリ開発を学ぼう」入門編](1stStep.md)を受講済の方を前提としています

## このワークショップで学べること

インターネット上のWeb APIと連携するようなスマートフォン向けのアプリケーションを開発したいというのはよくあるケースかと思います

このワークショップを通じて、ネットワーク通信機能を活用したアプリケーション開発方法について学ぶことが出来ます

## 今回のワークショップのためのプロジェクトを設定する

TitaniumStudioを起動した後、File→New→Titanium Mobile Projectと進みます

![プロジェクト設定スタート画面](image/1stStep-project-configuration001.png)

Project Template画面が標示されたら、Default Projectを選択します

![プロジェクト設定スタート画面](image/1stStep-project-configuration002.png)

プロジェクト設定画面が表示されますので今回は以下入力してプロジェクトの設定を行います

- Project name: **httpClient**
- App Id: **com.streetacademy.httpClient**

![プロジェクト設定スタート画面](image/1stStep-project-configuration003.png)

しばらくすると設定が完了します

## WebAPIとは？

## Qiitaとは？

## Qiitaの投稿情報を取得するアプリケーションを開発する

Qiitaの開発者向けのAPIを通じて投稿情報を取得する機能を実装します

Titanium Mobileで実装をはじめる前に、Qiitaの投稿情報を取得する時のWebAPIについて簡単に説明します

### QiitaのWebAPIについて

ブラウザで以下URLにアクセスすることでパブリックな新着投稿を取得することができます。

[https://qiita.com/api/v1/items](https://qiita.com/api/v1/items)

※Google Chromeでアクセスした時の例
![ブラウザでアクセスした時のキャプチャ](image/qiitaviewer-webapi-001.jpg)

投稿情報は以下の様なJSON形式になりますが、詳しい情報を知りたい方は、[Qiitaのサイトをご覧ください](http://qiita.com/docs#13)

```javascript
[{"id": 1,
 "uuid": "1a43e55e7209c8f3c565",
 "user":
  {"name": "Hiroshige Umino",
   "url_name": "yaotti",
   "profile_image_url": "https://si0.twimg.com/profile_images/2309761038/1ijg13pfs0dg84sk2y0h_normal" },
 "title": "てすと",
 "body": "<p>foooooooooooooooo</p>\n",
 "created_at": "2012-10-03 22:12:36 +0900",
 "updated_at": "2012-10-03 22:12:36 +0900",
 "created_at_in_words": "18 hours ago",
 "updated_at_in_words": "18 hours ago",
 "tags":
  [{"name": "FOOBAR",
    "url_name": "FOOBAR",
    "icon_url": "http://qiita.com/icons/thumb/missing.png",
    "versions": ['1.2', '1.3']}],
 "stock_count": 0,
 "stock_users": [],
 "comment_count": 0,
 "url": "http://qiita.com/items/1a43e55e7209c8f3c565",
 "gist_url": null,
 "tweet": false,
 "private": false,
 "stocked": false
},
 ...
]
```

### Qiitaの投稿情報を取得する機能を実装する

QiitaのようなWebAPIと連携するアプリを開発する場合に、Titanium Mobile標準機能のhttpCLientを活用することで簡単に実現できます。
まずはhttpCLientの使い方について解説をします。


1. プロジェクト作成時に自動的に生成されたapp.jsの中身を全て削除します。

2. その後に以下を記述します

```javascript
var xhr,qiitaURL,method;
qiitaURL = "https://qiita.com/api/v1/items";
method = "GET";
xhr = Ti.Network.createHTTPClient();
xhr.open(method,qiitaURL);
xhr.onload = function(){
  var body;
  if (this.status === 200) {
    body = JSON.parse(this.responseText);
	Ti.API.info(body);
  } else {
    Ti.API.info("error:status code is " + this.status);
  }
}
xhr.onerror = function(e) {
  var error;
  error = JSON.parse(this.responseText);
  Ti.API.info(error.error);
}
xhr.timeout = 5000;
xhr.send();
```

動作確認するために、buildした結果は以下のとおりです

![buildした結果の画面イメージ](image/qiitaviewer-httpClient-iphone-001.jpg)

シミュレーターの画面には何も標示されずコンソール上に複数の文字が標示されるかと思いますので、その点が確認できたらOKなので、次で画面に表示する方法について解説します

### Qiitaの投稿情報を取得する機能の解説

ご存じの方が多いかもしれませんが、念のためTitanium MobileのhttpClientについて解説します

```javascript
var xhr,qiitaURL,method;
qiitaURL = "https://qiita.com/api/v1/items";
method = "GET";
xhr = Ti.Network.createHTTPClient(); // (1)
xhr.open(method,qiitaURL);  // (2)
xhr.onload = function(){
  var body;
  if (this.status === 200) { // (3)
    body = JSON.parse(this.responseText); // (4)
	Ti.API.info(body);
  } else {
    Ti.API.info("error:status code is " + this.status);
  }
}
xhr.onerror = function(e) { // (5)
  var error;
  error = JSON.parse(this.responseText);
  Ti.API.info(error.error);
}
xhr.timeout = 5000;
xhr.send();
```

1. httpClientを利用するためのオブジェクトを生成します

2. open()メソッドを使ってQiitaのWebAPIにアクセスします。最初の引数にHTTPメソッドを指定しますが、Qiitaの投稿の取得をする場合には、GETメソッドを指定する必要があります（詳しくは[Qiitaのドキュメント](http://qiita.com/docs#13)を参照してください)次の引数で投稿情報を取得するQiitaのエンドポイントとなるURLを指定します。

3. QiitaのWebAPIにアクセスして、接続成功したかどうかを判定して、その後の処理を実施します。具体的にはthis.statusの値を確認して、値が200の場合には接続成功しているため該当する処理を実施します

4. this.responseTextの値を確認することで、サーバから取得できた値をテキスト形式で取得できます。this.responseTextは見た目はJSON形式になっていますが、そのまま変数に代入すると文字列としてその後処理されてしまうため、JSON.parse()を使って、JSON化した状態で変数に格納します

5. 例えば、QiitaのWebAPIにアクセスして、150リクエスト/1時間というAPIの利用制限に引っかかってしまう場合などはエラーになり、その時にはonerrorイベントが呼び出されます。

イメージとしては以下のような対応関係になります

![httpClientのonloadとonerrorの対応関係](image/qiitaviewer-httpClient-overview-001.jpg)


