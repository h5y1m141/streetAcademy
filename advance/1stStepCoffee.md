# コード分割とCoffeeScript


## 初めてのCoffeeScript

### まずは簡単な所から理解する

Titanium Mobileで新規プロジェクト作成後、自動的に生成されるapp.jsをCoffeeScriptを使って書き換えると以下のようになります。

#### カッコは不要になる

JavaScriptで記述する場合

```javascript
Titanium.UI.setBackgroundColor('#000');
```
というように、setBackgroundColor('#000') とカッコの中に値を記述するかと思います。

CoffeeScriptのソースコードの場合には、（ ）という丸括弧なしでOKで具体的には

```coffee
Titanium.UI.setBackgroundColor "#000"
```

という形でも記述できます。またこの書き方に慣れない場合には

```coffee
Titanium.UI.setBackgroundColor("#000")
```

という記述でもOKです


#### Ti.UI.createXXXの中のプロパティの指定がだいぶスッキリする

app.jsで、Ti.UI.Windowの生成をする際にJavaScriptで記述する場合だと

```javascript
var win1 = Titanium.UI.createWindow({  
    title:'Tab 1',
    backgroundColor:'#fff'
});
```
という感じで、UI.createWindowの後に、{ } のような波括弧（もしくはブレイスという言い方をします）を記述します。

CoffeeScriptのソースコードの場合には、波括弧を記述する代わりに空白文字のインデントで同じこととが実現できるため

```coffee
win1 = Titanium.UI.createWindow(
  title: "Tab 1"
  backgroundColor: "#fff"
)  
```
という形になります。JavaScriptの場合には、titleやbackgroundColorの前の空白はソースコードを見やすくするためのものでしかないですが、CoffeeScriptの場合には、この空白文字によるインデントが意味をなしており、このインデントが揃ってないと、構文として正しくないとみなされてしまいます。

なお、最初に、記述しましたが、CoffeeScriptの場合には、（ ）といった丸括弧は省略できるため

```coffee
win1 = Titanium.UI.createWindow
  title: "Tab 1"
  backgroundColor: "#fff"
```

という形にすることも出来ます。個人的にはこの書き方のほうがスッキリして好みなので以下この書き方にしていきます。
