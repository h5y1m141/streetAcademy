## はじめに

<!-- 何故取り上げるのかその背景について説明する。 -->

タイムラインビューを持ったスマートフォン向けのアプリケーションというものが一定数存在するかと思います。そういうアプリケーションでの日付の表示形式として **YYYY/MM/DD** だけではなく、例えば現在時刻を基準にして **何時間前** という形式で表示したいことも出てくるかと思います。

例えば、閏年を考慮したり、表示形式を自由に選べるような形にしたい・・など要望をあげるとキリがないかと思いますし、そういう要件を満たしてることを確認するためにしっかりとテストを書いて自作ライブラリを実装するとなるととても時間がかかってしまうかと思います。



## この章ではとりあげない内容

<!-- 内容的に関連しそうだけどとりあげない内容を明記する。気になる人はその情報源を簡単に示す程度に留める -->


## Moment.jsとは？
日付と時刻の面倒な変換を手軽に実現できるCommonJSなJavaScriptライブラリです。

このライブラリを利用するとこんな感じにとっても簡単に変換できます。

<p><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/h/h5y1m141/20121105/20121105082939.png" alt="f:id:h5y1m141:20121105082939p:plain" title="f:id:h5y1m141:20121105082939p:plain" class="hatena-fotolife"></p>


### 事前準備
[Moment.js](http://momentjs.com/) のサイトからMinifiedされたファイルをダウンロードします。

デフォルトは英語表記なのですが、i18n（internationalization。国際化）対応しているので、日本語表記をした別ファイルを準備することで手軽に日本語化も出来ます。
日本語化の対応を自分でやるのは面倒だなぁ・・と思っていたら、[jQuery プラグイン　livestamp.js を使ってみました。](http://gti.jp/ajax/livestamp/index.php)というエントリで[株式会社ジーティーアイ](http://gti.jp)の佐藤　毅 さんという方が[作成したmoment.lang_ja.jsが紹介されていた](http://gti.jp/ajax/livestamp/moment.lang_ja.js)のでそちらを利用しました。

### 使い方
require()で２つのファイルを読み込みます。Moment.jsを先に読み込み、その後に、佐藤　毅 さんが作成したものを読み込みますが、なぜかmoment.lang_ja.jsのファイル名だとうまく読み込めなかったのでmomentjaという形でリネームしました。

### ソース


```javascript
var dd, mainWindow, moment, momentja, timeLabel, timeLabel1, timeLabel2;

moment = require('moment.min');

momentja = require('momentja');

mainWindow = Ti.UI.createWindow({
  title: 'Moment.js Sample',
  barColor: '#59BB0C'
});

dd = new Date();

timeLabel = Ti.UI.createLabel({
  font: {
    fontSize: 24
  },
  Color: '#fff',
  left: 0,
  top: 5,
  width: 300,
  height: 100,
  text: "ベースとなる日付時刻:\n" + dd
});

timeLabel1 = Ti.UI.createLabel({
  font: {
    fontSize: 18
  },
  color: '#ddd',
  left: 0,
  top: 105,
  width: 300,
  height: 100,
  text: "ベースとなる日付時刻からの経過はfromNow()を適用:\n\n→" + (moment(dd).fromNow())
});

timeLabel2 = Ti.UI.createLabel({
  font: {
    fontSize: 18
  },
  color: '#ddd',
  left: 0,
  top: 205,
  width: 300,
  height: 100,
  text: "曜日表示はformat('dddd')とすればOK\n\n→：" + (moment(dd).format('dddd'))
});

mainWindow.add(timeLabel);

mainWindow.add(timeLabel1);

mainWindow.add(timeLabel2);

mainWindow.open();

```
