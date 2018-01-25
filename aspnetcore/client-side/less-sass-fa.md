---
title: "以下であるため、Sass、および ASP.NET Core の優れたフォント"
author: ardalis
description: "Sass、およびフォント優れた小さい ASP.NET Core アプリケーションで使用する方法を説明します。"
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 764b11bbd301c0116488265d32f7d46dfc5bce27
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a>スタイルのアプリケーションより少ないリソースで、Sass、およびフォント優れた ASP.NET Core での概要

によって[Steve Smith](https://ardalis.com/)

Web アプリケーションのユーザーには、スタイルを設定し、全体的なエクスペリエンスがますます高期待があります。 最新の web アプリケーションは、豊富なツール、フレームワークの定義および一貫した方法で、ルック アンド フィールを管理するために頻繁に活用します。 ようなフレームワーク[ブートス トラップ](http://getbootstrap.com/)共通スタイルと web サイトのレイアウト オプションのセットを定義するために効果的に進むことができます。 ただし、ほとんどの重要なサイトも恩恵を効果的に定義およびスタイルおよびカスケード スタイル シート (CSS) ファイルでは、管理を行うことだけでなく、サイトのインターフェイスのより直観的なを支援するアイコンの画像以外に簡単にアクセスできること。 ような場合は言語とツールをサポートする[小さい](http://lesscss.org/)と[Sass](http://sass-lang.com/)などのライブラリおよび[フォント優れた](http://fontawesome.io/)付属します。

## <a name="css-preprocessor-languages"></a>CSS プリプロセッサ言語

基になる言語の操作のエクスペリエンスを改善するために、他の言語にコンパイルされる言語は、"プリプロセッサ"と呼ばれます。 CSS の 2 つの一般的なプリプロセッサがある: 小さいと Sass です。  これらのプリプロセッサでは、変数と入れ子になったルールは、大規模で複雑なスタイル シートの保守容易性を向上させるためのサポートなど、CSS に機能を追加します。 言語としての CSS は非常に基本的なサポートも、変数と、単純なものが欠落していると、傾向が肥大化し、反復的な CSS ファイルを作成します。 プリプロセッサを使用して実際のプログラミング言語の機能を追加するのに役立つの重複を減らすのスタイル ルールより効率的な整理を提供できます。 Visual Studio では、両方の以下の組み込みサポートと Sass、だけでなくこれらの言語を使用する場合、開発エクスペリエンスがさらに向上する拡張機能を提供します。

プリプロセッサが読みやすさとスタイル情報の保守容易性を向上する方法の簡単な例としては、この CSS を考慮してください。

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

以下を使用して、この書き直すことができます、重複をすべて取り除くを使用して、 *mixin* (という名前は"mix"することができます、1 つのクラスまたは別のルール セットからのプロパティ)。

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>小さい

小さい CSS プリプロセッサでは、Node.js を使用して実行されます。 以下をインストールするには、コマンド プロンプトから Node Package Manager (npm) を使用 (-g 意味「グローバル」)。

```console
npm install -g less
```

Visual Studio を使用している場合より少ないリソースをプロジェクトに以下の 1 つまたは複数のファイルを追加し、コンパイル時にそれを処理する Gulp (または Grunt) を構成で始めることができます。 追加、*スタイル*フォルダーをプロジェクトにして新しい未満という名前のファイルを追加して*main.less*このフォルダーにします。

![Less ファイルを追加します。](less-sass-fa/_static/add-less-file.png)

追加されると、フォルダー構造は次のようになります。

![フォルダー構造](less-sass-fa/_static/folder-structure.png)

今すぐ CSS にコンパイルされ、Gulp で wwwroot フォルダーに展開されているファイルをいくつかの基本的なスタイルを追加できます。

変更*main.less*に含める次の内容は、1 つの基本色から単純なカラー パレットを作成します。

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base`その他の@-prefixed項目は、変数です。 それぞれは、色を表します。 除く`@base`色の関数の使用を設定すると、: 明るく、暗くする、および回転します。 明るくして、画面の非常に期待されるものです操作を行います。スピンは、(周囲の色は、ホイール) 度の数によって、色の色合いを調整します。 小さいプロセッサには使用されていない変数を無視するためにどこかに使用する必要はこれらの変数の動作を示すためには、します。 クラスは、`.baseColor`などが生成された CSS ファイルで変数のそれぞれの計算値をデモンストレーションします。

### <a name="getting-started"></a>作業の開始

作成、 **npm 構成ファイル**(*package.json*) のプロジェクト フォルダーを参照するように編集および`gulp`と`gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

または Visual Studio で、プロジェクトのフォルダーで、コマンド プロンプトでいずれかの依存関係をインストール**ソリューション エクスプ ローラー** (**の依存関係 > npm > パッケージを復元**)。

```console
npm install
```

![VS パッケージを復元します。](less-sass-fa/_static/restore-packages.png)

プロジェクト フォルダーに作成、**構成ファイルの Gulp** (*gulpfile.js*) 自動プロセスを定義します。  表す未満、ファイル サービスおよび以下を実行するタスクの上部にある変数を追加します。

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

開く、**タスク ランナー エクスプ ローラー** (**ビュー > その他の Windows > タスク ランナー エクスプ ローラー**)。 、タスク間では、新しいタスクがという名前を表示する必要があります`less`です。 ウィンドウを更新する必要があります。

実行、`less`タスク、およびするがここに表示される内容のような出力を参照してください。

![タスク ランナーが少なく](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css*フォルダーにはここで、新しいファイルが含まれています*main.css*:

![メインの css を作成](less-sass-fa/_static/main-css-created.png)

開いている*main.css*を参照してください、次のようなものとします。

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

単純な HTML ページを追加、 *wwwroot*フォルダー、および参照*main.css*アクションのカラー パレットを表示します。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

ご覧 180 度ですぐにご利用いただけます`@base`生成に使用される`@background`の色の両カラー ホイールが発生しました`@base`:

![小さいテストの例](less-sass-fa/_static/less-test-screenshot.png)

小さいも入れ子になったメディア クエリと入れ子になったルールは、サポートを提供します。 たとえば、CSS 規則の詳細なメニューが生じるような入れ子になった階層を定義するには、このような。

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

理想的にはすべての関連するスタイル ルールは、一緒に CSS ファイル内では実際に何も規則とおそらくブロック コメントを除く、この規則を適用します。

以下を使用してこれらの同じ規則を定義する次のような。

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

この場合は、すべての下位要素のことに注意してください`nav`そのスコープ内に含まれています。 親要素の任意の繰り返しが不要になった (`nav`、 `li`、 `a`)、合計行の数が同様に下がっています (いくつかのことでは 2 番目の例では、同じ行に値を作成する場合の結果)。 非常に役に立ちます、組織的、ここではすべて、明示的に範囲指定されたスコープ内で指定した UI 要素の規則の表示を off に設定、ファイルの残りの部分から中かっこでします。

`&`構文では、以下のセレクターの機能 (&)、現在のセレクターの親を表すです。 内で、{...} ブロック、`&`を表す、`a`タグ、およびこのインターフェイス`&:link`は等価`a:link`です。

繰り返しと CSS の複雑さには、メディア クエリ、応答性の高いデザインを作成するのには、非常に便利ですが大きく貢献もできます。 小さいメディアによりクエリが許可クラス内で入れ子にできるように、すべてのクラス定義が別内で繰り返される必要があるトップレベル`@media`要素。 たとえば、応答性の高いメニューの CSS を次に示します。

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

これは、として小さいでより定義することができます。

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

すでに未満の別の機能は、事前に定義された変数から構築されているスタイル属性を許可する数学的演算のサポートです。 これにより、基本変数を修正して、すべての従属変数の値が自動的に変更するために、関連するスタイルはるかに簡単を更新します。

CSS ファイルで、特に大規模なサイトの (およびメディア クエリが使用されている場合は特に)、非常に大きくなる時間の経過と共にそれらの操作を扱いにくくする傾向があります。 Less ファイルは、個別に一緒に使用し、プル`@import`ディレクティブです。 小さいこともできますファイルをインポートする個別 CSS 同様に、必要な場合です。

*Mixins*パラメーターを受け入れることができ、mixin ガードは、特定 mixins が有効になる場合を定義する手段に宣言の形式で条件ロジックをサポート小さくします。 Mixin guard の一般的な用途は光に基づく色を調整するか、元の色は濃色。 色のパラメーターを受け入れる mixin を指定するには、mixin ガードは、その色に基づいた mixin の変更を使用できます。

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

指定された、現在`@base`の値`#663333`、次の CSS をこの少ないスクリプトが生成されます。

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

いくつかの追加の機能は、小さいが、ことができるかについてのこの機能のアイデア言語を前処理します。

## <a name="sass"></a>Sass

Sass は、構文は若干異なりますが、同じ機能の多くのサポートを提供するより少ないリソースに似ています。 これは、JavaScript ではなく、Ruby を使用して構築しており、別のセットアップ要件です。 Sass の元の言語では、中かっこまたはセミコロンを使用しませんでしたが、代わりに空白とインデントを使用してスコープを定義します。 Sass のバージョン 3 では、新しい構文が導入された、 **SCSS** ("Sassy CSS") です。 レベルのインデントおよび空白は無視されます、代わりにセミコロンと中かっこを使用してという点で、SCSS は CSS に似ています。

通常 Sass をインストールするには、まず (事前にインストールされた Mac)、Ruby をインストールして実行するとします。

```console
gem install sass
```

ただし、Visual Studio を実行している場合開始できます Sass とほぼ同じ方法未満の場合と同様です。 開いている*package.json* "gulp sass"パッケージを追加および`devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

次に、変更*gulpfile.js* sass 変数と Sass ファイルをコンパイルし、結果 wwwroot フォルダーに配置するタスクを追加します。

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Sass ファイルを追加するようになりました*main2.scss*を*スタイル*プロジェクトのルートにフォルダー。

![scss ファイルに追加します。](less-sass-fa/_static/add-scss-file.png)

開いている*main2.scss*以下を追加します。

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

すべてのファイルを保存します。 今すぐ更新**Task Runner Explorer**、表示、`sass`タスク。 実行、およびファイルの場所、 *wwwroot/css*フォルダーです。 ここでは、 *main2.css*これらの内容を持つファイル。

```css
body {
    background-color: #CC0000;
}
```

Sass に似ている小さいが、同様のメリットを提供することが、同じ入れ子がサポートしています。 ファイルは、関数を分割してを使用して含まれている、`@import`ディレクティブ。

```sass
@import 'anotherfile';
```

Sass サポート mixins 同様を使用して、`@mixin`に定義するキーワードおよび`@include`からこの例のように、それらを含める[sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Mixins、に加えて Sass もサポートしています、継承の概念を拡張する別の 1 つのクラスを許可します。 概念的には、混合が少ない CSS コードに似ています。 使用される、`@extend`キーワード。 Mixins を試すには、次を追加、 *main2.scss*ファイル。

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

出力を確認*main2.css*実行した後、`sass`内のタスク**Task Runner Explorer**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

すべてのアラート mixin の一般的なプロパティが各クラスで繰り返されることに注意してください。 Mixin でした、適切なジョブが、開発時に重複を排除することができ、多数の重複していること、必要な CSS ファイルで、潜在的なパフォーマンスの問題よりも大きくなります結果として得られるで CSS をまだ作成しています。

アラート mixin に置き換える、`.alert`クラス、および変更`@include`に`@extend`(拡張を記述する`.alert`ではなく、 `alert`)。

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Sass をもう一度実行し、結果として得られる CSS を確認します。

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

プロパティが、必要に応じて何度もとしてのみ定義されているようになりましたし、CSS が生成される向上します。

Sass には、関数と、以下のような条件付きロジック操作も含まれています。 実際には、2 つの言語の機能は、非常に似ています。

## <a name="less-or-sass"></a>小さいまたは Sass しますか?

まだや Sass の使用が少なく、通常方が優れているかどうかについての合意がない (または元の Sass または Sass 内で新しい SCSS 構文を使用するかどうかも)。 最も重要な意思決定が、おそらく**これらのツールのいずれかを使用して**だけ手動コーディングの CSS ファイルではなく、します。 作成した後も、両方の以下の意思決定し、Sass は適切な選択肢です。

## <a name="font-awesome"></a>優れたフォント

CSS プリプロセッサに加えて最新の web アプリケーションのスタイル処理の重要なリソースは、フォントすばらしいです。 フォント Awesome は、web アプリケーションで自由に使用できる 500 を超えるスケーラブル ベクター アイコンを提供するツールキットです。 ブートス トラップを使用すること、もともとられていますが依存関係またはそのフレームワークに JavaScript ライブラリにします。

フォント サービスを開始する最も簡単な方法では、パブリックのコンテンツ配信ネットワーク (CDN) の場所を使用してへの参照を追加します。

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

追加することも、Visual Studio プロジェクトに「依存関係」に追加することによってで*bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

フォントの優れたへの参照をページにある場合は後に、追加できますアイコン アプリケーション フォント優れたクラス、通常の付いた「fa-」で、インライン HTML 要素に適用することで (など`<span>`または`<i>`)。  たとえば、単純なリストおよび次のようにコードを使用してメニューにアイコンを追加できます。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

これにより、ブラウザーでは、次が生成されます。-各項目の横にあるアイコンに注意してください。

![リストのアイコン](less-sass-fa/_static/list-icons-screenshot.png)

ここで可能なアイコンの完全な一覧を表示することができます。

http://fontawesome.io/icons/

## <a name="summary"></a>まとめ

最新の web アプリケーションは、ますますは、クリーン、直感的で、およびさまざまなデバイスから簡単に使用される応答性の高い、滑らかな設計を要求します。 これらの目標を達成するために必要な CSS スタイル シートの複雑さを管理すると、小さいプリプロセッサ like を使用してまたは Sass 最適な方法はします。 さらに、ツールキット フォント素晴らしいと同様には、テキストのナビゲーション メニューによく知られているアイコンを迅速に提供し、ボタン、全体的なユーザーの向上が、アプリケーションのエクスペリエンスします。
