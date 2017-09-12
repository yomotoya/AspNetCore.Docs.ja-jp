---
title: "ASP.NET Core Knockout.js MVVM フレームワーク"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>ASP.NET Core Knockout.js MVVM フレームワーク

によって[Steve Smith](https://ardalis.com/)

Knockout は、複雑なデータに基づくユーザー インターフェイスの作成を簡略化する一般的な JavaScript ライブラリです。 これは、単独で、または jQuery などその他のライブラリに使用できます。 その主な目的は、UI に変更が加えられた、モデルが更新されるとするように、JavaScript オブジェクトとして定義されている基になるデータ モデルに UI 要素をバインドして、その逆です。 Knockout には、web アプリケーションのクライアント側の動作でモデル View-viewmodel (MVVM) パターンの使用が容易になります。 Knockout の MVVM 実装を使用するときに 1 つについて説明する必要があります、2 つの主要な概念は、観測可能なオブジェクトとバインドされます。

## <a name="getting-started"></a>作業の開始

インストールするため、1 つの JavaScript ファイルとして knockout が展開され、それを使用して、非常に簡単を使用して[bower](bower.md)です。 既にあると仮定して[bower](bower.md)と[gulp](using-gulp.md)開くように構成、 *bower.json* ASP.NET Core プロジェクトし、次に示すように、knockout 依存関係を追加。

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

こうすると、使用することで、手動で (ビュー ‣ その他のウィンドウ ‣ タスク ランナー エクスプ ローラー) の下のタスク ランナー エクスプ ローラーを開くことで bower を実行し、bower を右クリックして タスク と実行 を選択します。 次のような結果が表示されます。

![タスク ランナー エクスプ ローラーで実行中の knockout を bower します。](knockout/_static/bower-knockout.png)

ここでは、プロジェクトの検索`wwwroot`フォルダー、knockout lib フォルダーの下にインストールを確認する必要があります。

![knockout lib フォルダーにインストールされています。](knockout/_static/wwwroot-knockout.png)

ユーザーがファイルのキャッシュされたコピーが既に与えられてし、そのため、ダウンロードする必要はありません、その確率を増加させる、実稼働環境で knockout コンテンツ配信ネットワーク、または、CDN を使用してを参照することをお勧めします。 Knockout は、ここで Microsoft Ajax CDN を含め、いくつかの Cdn で入手できます。

[http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

ページに含める Knockout、それを使用する、単に追加、`<script>`任意の場所をホストすること (アプリケーションを使用して、または、CDN によって) からファイルを参照する要素。

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>観測可能なオブジェクト、ViewModels、および単純バインディング

JavaScript を使用して、DOM に直接アクセスするか、web ページの要素を操作するや jQuery などのライブラリを使用してに習熟してあります。 通常このような動作は、特定のユーザー アクションへの応答内の要素の値を直接設定するコードの記述によって実現されます。 使用すると Knockout、宣言型の方法は取得代わりに、使用するページ上の要素はプロパティにバインドされたオブジェクトの。 DOM 要素を操作するコードを記述する代わりにユーザーの操作は、単に ViewModel オブジェクトと対話し、Knockout はページの要素を同期させるに対処します。

単純な例としては、次のページの一覧を検討してください。 含まれている、`<span>`を持つ要素を`data-bind`"作成者"テキストの内容をバインドすることを示す属性です。 次に、1 つのプロパティを持つ変数 viewModel が定義されている JavaScript ブロック`authorName`、いくつかの値に設定します。 呼び出しでは最後に、`ko.applyBindings`行われると、この viewModel 変数を渡します。

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

コンテンツはブラウザーに表示されるときに、 <span> viewModel 変数に値を持つ要素が置き換えられます。

![knockout 単純バインディング](knockout/_static/simple-binding-screenshot.png)

単純な一方向のバインディングの作業があるようになりました。 ほど、コードででしたお書き込み値を割り当てようスパンのコンテンツに JavaScript に注意してください。 ViewModel を操作する場合、ステップを実行するこの、さらに、HTML 入力用テキスト ボックスを追加したりできますと同様に、その値にバインドようにします。

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

ページを再度読み込んでを参照して、入力ボックスにこの値が実際にバインドされています。

![knockout 入力バインディング](knockout/_static/input-binding-screenshot.png)

ただし、textbox 内の値を変更すると場合の対応する値で、`<span>`要素は変更されません。 なぜですか。

問題が何も通知を受け取ること、`<span>`を更新するために必要です。 ViewModel のプロパティは、特殊な種類でラップされていない限り、不十分な場合にそれ自体では、ViewModel を更新するだけです。 使用する必要があります**オブサーバー**で行った変更を発生するが自動的に更新する必要があるプロパティに対して ViewModel です。 使用する ViewModel を変更することによって`ko.observable("value")`ViewModel だけ「値」ではなく、変更が発生するたびに、その値にバインドされている HTML 要素が更新されます。 入力すると、バインド要素への変更は表示されませんのでにフォーカスが失われますまでに、入力ボックスがその値を更新しないことに注意してください。

> [!NOTE]
> キーを押すたびに追加するだけの後に、ライブ更新のサポートを追加する`valueUpdate: "afterkeydown"`を`data-bind`属性の内容。 使用して、この動作を取得することもできます。`data-bind="textInput: authorName"`値のインスタント更新プログラムを取得します。 

当社 viewModel、ko.observable を使用するように更新した後:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout には、さまざまな種類のバインドの数がサポートされています。 バインドする方法を見たところ`text`にされ、`value`です。 指定された属性にバインドすることもできます。 たとえば、アンカー タグを含むハイパーリンクを作成する、 `src` viewModel に属性をバインドできます。 Knockout には、関数へのバインドもサポートしています。 これを示すためには、作成者の twitter のハンドルを含める viewModel を更新して、著者の twitter ページへのリンクとして twitter のハンドルを表示してみましょう。 この 3 つの段階で実行します。

まず、著者の名の後にかっこで紹介するハイパーリンクを表示する HTML を追加します。

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

TwitterUrl と twitterAlias プロパティを含める viewModel を次に、更新するには。

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

まだしていないこの twitter エイリアスの正しい URL に移動する twitterUrl 更新されましたおこの時点で: twitter.com でが指してだけのことに注意してください。また、新しい Knockout 関数を使用することに注意してください`computed`twitterUrl 用です。 これは、監視可能な関数が変更された場合、UI 要素を通知します。 ただし、viewModel であるその他のプロパティにアクセスするため、独自のステートメントの各プロパティはできるように、viewModel は作成する方法を変更する必要があります。

改訂 viewModel 宣言を次に示します。 これで、関数として宣言されています。 各プロパティが独自ステートメントであるようになりましたが、セミコロンで終わることを確認します。 TwitterAlias プロパティの値にアクセスする必要があるため、その参照には、() が含まれています。 を実行することを確認します。

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

結果は、ブラウザーで想定どおりに動作します。

![knockout ハイパーリンク](knockout/_static/hyperlink-screenshot.png)

Knockout には、クリック イベントなど、特定の UI 要素イベントへのバインドもサポートしています。 これにより、簡単かつ宣言によって UI 要素をアプリケーションの viewModel 内の関数にバインドできます。 単純な例としては、ボタンを追加できますをクリックすると、すべて大文字である、著者の twitterAlias を変更します。

最初に、ボタンを追加、ボタンへのバインドは、イベント、およびは viewModel に追加することに関数名を参照する をクリックします。

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

関数、viewModel を追加し、viewModel の状態を変更するに接続します。 TwitterAlias プロパティに新しい値を設定するにはメソッドとして呼び出すし、新しい値で渡すことがわかります。

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

コードを実行して、ボタンをクリックするとは、期待どおりに表示されたリンクを変更します。

![ハイパーリンクを大文字にします。](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>制御フロー

Knockout には、条件とループ操作を実行できるバインディングが含まれています。 ループ操作は、データのリストを UI 一覧、メニューのおよびグリッドやテーブルにバインドするため便利です。 Foreach バインディングは、配列を反復処理されます。 監視可能な配列を併用すると、項目の追加または UI ツリー内のすべての要素を再作成せずに、配列から削除する場合が UI 要素に自動的に更新されます。 次の例では、ゲームの結果の監視可能な配列を含む新しい viewModel を使用します。 使用して 2 つの列を含む、単純なテーブルにバインドされている、`foreach`上のバインド、`<tbody>`要素。 各`<tr>`内の要素`<tbody>`gameResults コレクションの要素にバインドされます。

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

この時間を使用すること ViewModel 大文字の"V"(applyBindings 呼び出し) で「新規」を使用して構築するためになる予定のために注意してください。 実行すると、ページは、次の出力が得られます。

![knockout レコード ビュー モデル](knockout/_static/record-screenshot.png)

監視可能なコレクションが動作していることを示すためには、少し多くの機能を追加してみましょう。 ViewModel するもう 1 つのゲームの結果を記録する機能を含めるし、ボタンと新しい関数を使用する一部の UI を追加おことができます。  まず、addResult メソッドを作成してみましょう。

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

このメソッドにバインドを使用して、ボタン、`click`バインド。

```html
<button data-bind="click: addResult">Add New Result</button>
```

ブラウザーで、ページを開き をクリックした回数、いくつかクリックするたびに新しいテーブルの行の結果として得られます。

![結果を追加します。](knockout/_static/record-addresult-screenshot.png)

UI では、通常、インライン、または別のフォームで新しいレコードの追加をサポートするために、いくつかの方法はあります。 全体が編集可能になるように、テキスト ボックスおよび dropdownlists を使用するテーブルに簡単に変更できます。 変更するだけ、`<tr>`に示すようにします。

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

なお`$root`可能な選択肢が公開される場所はルート ViewModel を参照します。 `$data`この場合は、単純な文字列をそれぞれが、resultChoices 配列の個々 の要素を参照して、特定のコンテキスト内では、どのような現在のモデルを参照します。

この変更により、グリッド全体は編集可能になります。

![編集可能なグリッド](knockout/_static/editable-grid-screenshot.png)

Knockout を使用していないこと、おを得ることがこれはすべて、jQuery の使用が、ほとんどの場合とないほぼ同程度に効率的です。 Knockout は、どの UI 要素に、ViewModel 内の項目の対応するバインドされたデータを追跡し、追加、削除、または更新する必要があるこれらの要素のみが更新します。 これを実現する jQuery または直接の DOM 操作を使用して社内で大幅な残存作業時間がかかるし、場合でも、テーブルのデータに基づく (win 損失レコードなど) の集計結果を表示する必要は必要がありますをもう一度ループ処理、および解析する、HTML 要素です。  Knockout で win が失われるレコードの表示は簡単です。 ViewModel 自体内で計算を実行し、単純なテキストのバインディングでそれを表示し、`<span>`です。

Win が失われるレコードの文字列を構築するには、計算観測可能なオブジェクトを使用できます。 関数呼び出しをする必要があります、ViewModel 内で監視可能なプロパティへの参照をそれ以外の場合は取得しません、観測可能なオブジェクトの値 (つまり`gameResults()`いない`gameResults`に示すコードで)。

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

この関数をバインド内の範囲を`<h1>`ページの上部にある要素。

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

結果:

![Win 損失](knockout/_static/record-winloss-screenshot.png)

行を追加または任意の行の結果の列で選択した要素を変更するには、ウィンドウの上部に表示レコードが更新されます。

値へのバインド、に加えて、バインディング内の法的なほとんどの JavaScript 式を使用することもできます。 たとえば場合は、UI 要素に値が特定のしきい値を超えた場合など、特定の条件下でのみ表示されますできますこの論理的に内で指定したバインディング式。

```html
<div data-bind="visible: customerValue > 100"></div>
```

これは、`<div>`のみが表示されます、customerValue が 100 以上である場合。

## <a name="templates"></a>テンプレート

Knockout ではの動作と UI を簡単に分離したり、オンデマンドでの大規模なアプリケーションへの UI 要素の増分読み込みできるように、テンプレートのサポートがあります。 前の例に各行独自のテンプレートをテンプレートに HTML をアウト プルでデータ バインドの呼び出しで名前によって、テンプレートを指定するだけで更新することが`<tbody>`です。

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout には、jQuery.tmpl ライブラリなど、他のテンプレート エンジンと Underscore.js のテンプレート エンジンもサポートしています。

## <a name="components"></a>コンポーネント

コンポーネントを使用すると、整理し、UI コードが依存している ViewModel データと共に通常の UI コードを再利用できます。 コンポーネントを作成するには、単にそのテンプレートとその viewModel 指定し、名前を付けます必要があります。 これは、`ko.components.register()` を呼び出すことによって行われます。 テンプレートおよび viewmodel インラインを定義するだけでなく読み込むことができるようにライブラリを使用して外部のファイルから*require.js*、その結果に非常にクリーンで効率的なコードです。

## <a name="communicating-with-apis"></a>Api との通信

Knockout は JSON 形式でデータを操作できます。 取得し、Knockout を使用してデータを保存するための一般的な方法は、jQuery をサポートするには、`$.getJSON()`データを取得する関数、および`$.post()`API エンドポイントにブラウザーからデータを送信する方法です。 もちろん、別の方法を JSON データを送受信する場合は、Knockout しても同様に機能します。

## <a name="summary"></a>概要

Knockout、ViewModel で定義されている、クライアント アプリケーションの現在の状態に UI 要素をバインドする単純で簡潔な方法を提供します。 Knockout のバインディングの構文は、処理するのには、HTML 要素に適用される、データ バインド属性を使用します。 Knockout が効率的にレンダリングして、UI 要素を追跡することによって大規模なデータ セットを更新して要素の影響を受けるのみへの変更を処理します。 大規模なアプリケーションは、テンプレートとコンポーネントは、外部ファイルからの要求時に読み込むことができますが使用する UI ロジックを分割できます。 現在のバージョン 3 Knockout は、安定した JavaScript ライブラリをリッチ クライアントの対話機能を必要とする web アプリケーションを向上させることができます。
