# ワークショップ「JavaScriptでスマフォアプリ開発を学ぼう」基礎編:ACSと連携させた位置情報処理

![Titanium Mobileとは？の画像](image/1stStep-001.png)

## はじめに
このワークショップでは、JavaScript でiPhone/Android向けのアプリの開発が出来るTitanium Mobileというアプリケーションを使ったスマフォアプリの作り方を教えます

ワークショップの所要時間としては60分程度を想定しております。

## 想定対象者

[「JavaScriptでスマフォアプリ開発を学ぼう」入門編](1stStep.md)を受講済の方を前提としています

## プロジェクト設定

TitaniumStudioを起動した後、File→New→Titanium Mobile Projectと進みます

![プロジェクト設定スタート画面](image/1stStep-project-configuration001.png)

Project Template画面が標示されたら、Default Projectを選択します

![プロジェクト設定スタート画面](image/1stStep-project-configuration002.png)

プロジェクト設定画面が表示されますので今回は以下入力してプロジェクトの設定を行います

- Project name: **mapWithACS**
- App Id: **com.streetacademy.mapWithACS**

![プロジェクト設定スタート画面](image/1stStep-project-configuration003.png)


しばらくして設定が完了すると、以下のような画面が表示されればOKです

![プロジェクト設定スタート画面](image/1stStep-project-configuration004.png)

## 今回作るアプリケーションのイメージ

## アプリケーション開発の前にtiapp.xmlの修正

## 参考情報

### 緯度経度と単位

- 緯度（latitude）は赤道を０度として北：＋（プラス）南：ー（マイナス）の数字で表現
- 経度（longitude）はグリニッジ天文台跡を南北へ通る子午線を０度として東：＋（プラス） 西：ー（マイナス）の数字で表現

![緯度経度のイメージ図](image/4thStep-map-knowledge-01.png)
緯度経度を10進法で示すDegree単位で東京駅周辺の丸の内のビルは+35.679113、+139.763137

※60進で表現する度分秒（DMS）というのもあるそうですがインターネット上ではDegree単位が利用されることが多いそうです

### 測地系

同じ場所でも利用する測地系（datum）によって緯度経度の値が変わる。日本で気にする必要があるのは以下２つ

- 日本測地系
- 世界測地系

日本では2002年4月1日まで日本測地系が使われてきた経緯があり、サービスによってはまだ日本測地系を利用しているものもあるそうで、日本測地系に基づいた緯度経度と世界測地系のそれとでは、約400mのズレが生じるそうです

### 位置情報取得方法


