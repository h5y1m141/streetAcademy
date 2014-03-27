## はじめに

<!-- 何故、CommonJSについて取り上げるのかその背景について説明する。 -->

Titanium Studioで自動生成されるひな形程度のアプリなら問題ないが、AppStoreに公開するようなアプリ開発になると、それなりのコード量になってきます。

app.js単体にすべてを詰め込んでしまうと、コードの見通しが悪くなるため、ある処理単位でファイルを分割してしまったほうが、保守性/拡張性高くなります。

分割に関しては、WebアプリケーションのJavaScriptの場合にはファイル単位でコードを分割していくかと思いますが、Titanium Mobileの場合には、Node.jsでも採用されてCommonJSという仕様に準拠したモジュール化の仕組みが利用できます。

この章では、CommonJSについて説明をしていきます

## この章ではとりあげない内容

<!-- 内容的に関連しそうだけどとりあげない内容を明記する。気になる人はその情報源を簡単に示す程度に留める -->

それなりの規模のアプリケーション開発においては、MVC(Model View Controller)モデルに従った設計がされるかと思います。

この章ではMVCモデル自体の解説や、コード分割の設計方針などについては言及しません。


## CommonJSとは？

<!-- その章のタイトルとなるキーワード＋とは？という形で実際に説明を始める -->

[Appceleratorの公式ガイド](http://docs.appcelerator.com/titanium/3.0/#!/guide/CommonJS_Modules_in_Titanium)のSimple Usageの項目がわかりやすいのでそちらのサンプルコードを引用して簡単に解説します。


### CommonJS基礎

Titanium Studioで新規プロジェクトを作成するとこのようなディレクトリ構造になるかと思います

```sh
project directory
.
├── Resources
│   ├── KS_nav_ui.png
│   ├── KS_nav_views.png
│   └── app.js
├── build
├── manifest
└── tiapp.xml
```

app.jsと同じ階層に、**MyModule.js** という名前で新規にファイルを作成して以下のコードを記述します。

```javascript
exports.sayHello = function(name) {
    Ti.API.info('Hello '+name+'!');
};
```

app.jsに記述されてるソースコードをすべて削除した後に、以下を記述します

```javascript
// app.js
var myModule = require('MyModule'); //(1)
myModule.sayHello('Kevin');         //(2)
```

1. require()という関数を利用してMyModuleで定義されてる機能を読み込みます。１つ注意点として**読み込むファイルの拡張子を外した形式にする必要があります。** この場合には、MyModule.jsというファイルの拡張子の.jsを外した'MyModule'と記述します。
2. exports 命令を使って登録されたsayHello()という関数が app.js の中から呼び出すことができます。sayHello()関数では引数に **name** という変数を必要としており、利用時には引数を渡してます。（この場合にはKevinという名前がそれに該当します。


### 呼び出す側から見える関数と、見えない関数を定義することが出来る


### 何故このような機能が必要なのか？

意図しない形で外部から変数の値が書き換えられてしまう可能性があるからです。

## まとめ

<!-- ここまでの説明を振り返りながらまとめる -->
