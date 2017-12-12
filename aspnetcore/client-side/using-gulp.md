---
title: "ASP.NET Core で Gulp を使用します。"
author: rick-anderson
description: "ASP.NET Core で Gulp を使用する方法を説明します。"
keywords: "ASP.NET Core、Gulp"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68f6838889cfb830f2c5a1976b3140ae5d94ac25
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>ASP.NET Core で Gulp を使用の概要 

によって[Erik Reitan](https://github.com/Erikre)、 [Scott Addie](https://scottaddie.com)、 [Daniel Roth](https://github.com/danroth27)、および[Shayne Boyer](https://twitter.com/spboyer)

典型的な最新の web アプリで、ビルド プロセスでは次のことがあります。

* バンドルして、JavaScript および CSS ファイルを縮小します。
* ツールを実行する各ビルドの前に、バンドルと縮小のタスクを呼び出します。
* CSS にコンパイル未満または SASS ファイル。
* JavaScript に CoffeeScript または TypeScript ファイルをコンパイルします。

A*タスク ランナー*これらの日常的な開発タスクを自動化するツールです。 Visual Studio では、2 つの JavaScript ベースの一般的なタスク ランナーの組み込みサポートが用意されています: [Gulp](https://gulpjs.com/)と[Grunt](using-grunt.md)です。

## <a name="gulp"></a>gulp

Gulp は、JavaScript ベース ストリーミング ビルド ツールキットのクライアント側のコードです。 通常は、ビルド環境で特定のイベントがトリガーされたときに、一連のプロセスを使用してクライアント側ファイルをストリーム配信に使用されます。 Gulp を使用して自動化するインスタンスの[バンドルと縮小](bundling-and-minification.md)新しいビルドの前に、開発環境のクレンジングまたはします。

Gulp タスクのセットが定義されている*gulpfile.js*です。 次の JavaScript では、Gulp モジュールが含まれます、近日公開予定のタスク内で参照されるファイルのパスを指定します。

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

上記のコードでは、どのノード モジュールに必要なを指定します。 `require`関数が依存タスクは、その機能を利用できるように、各モジュールをインポートします。 インポートしたモジュールの各は、変数に割り当てられます。 モジュールは、名前またはパスのいずれかに配置できます。 この例では、モジュールが名前付き`gulp`、 `rimraf`、 `gulp-concat`、`gulp-cssmin`と`gulp-uglify`名前によって取得されます。 さらに、一連のパスは、CSS および JavaScript ファイルの場所を再利用したり、タスク内で参照できるように作成されます。 次の表の説明に含まれるモジュールの*gulpfile.js*です。

|モジュール名|説明|
|---|---|
|gulp|Gulp ストリーミング ビルド システムです。 詳細については、次を参照してください。 [gulp](https://www.npmjs.com/package/gulp)です。|
|rimraf|ノードの削除モジュール。 詳細については、次を参照してください。 [rimraf](https://www.npmjs.com/package/rimraf)です。|
|gulp concat|オペレーティング システムの改行文字に基づいてファイルを連結するモジュール。 詳細については、次を参照してください。 [gulp concat](https://www.npmjs.com/package/gulp-concat)です。|
|gulp cssmin|CSS ファイルを縮小するモジュール。 詳細については、次を参照してください。 [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)です。|
|gulp uglify します。|縮小するモジュール*.js*ファイル。 詳細については、次を参照してください。 [gulp uglify](https://www.npmjs.com/package/gulp-uglify)です。|

必要なモジュールがインポートされると、タスクを指定できます。 ここで 6 つのタスクが登録されている、次のコードで表されます。

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

次の表では、上記のコードで指定されているタスクの説明します。

|タスク名|説明|
|--- |--- |
|クリーン: js|使用するタスクを rimraf ノード削除モジュール site.js ファイルの縮小されたバージョンを削除します。|
|クリーン: css|Site.css ファイルの縮小されたバージョンを削除する rimraf ノード削除モジュールを使用するタスク。|
|クリーンアップ|呼び出すタスク、`clean:js`タスク、続けて、`clean:css`タスク。|
|min:js|タスクを縮小し、js フォルダー内のすべての .js ファイルを連結します。 。 Min.js ファイルは除外されます。|
|min:css|タスクを縮小し、css フォルダー内のすべての .css ファイルを連結します。 。 Min.css ファイルは除外されます。|
|分|呼び出すタスク、`min:js`タスク、続けて、`min:css`タスク。|

## <a name="running-default-tasks"></a>既定のタスクを実行しています。

新しい Web アプリをまだ作成していない場合は、Visual Studio で新しい ASP.NET Web アプリケーション プロジェクトを作成します。

1.  新しい JavaScript ファイルをプロジェクトに追加し、名前*gulpfile.js*、次のコードをコピーします。

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  開く、 *package.json*ファイル (追加しない場合があります)、以下を追加します。

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  **ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*を選択して**Task Runner Explorer**です。
    
    ![ソリューション エクスプ ローラーからタスク ランナー エクスプ ローラーを開く](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **タスク ランナー エクスプ ローラー** Gulp タスクの一覧を示しています。 (をクリックする必要があります、**更新**プロジェクト名の左側に表示されるボタンをクリックします)。
    
    ![タスク ランナー エクスプ ローラー](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  下に**タスク**で**タスク ランナー エクスプ ローラー**を右クリックして**クリーン**を選択し、**実行**ポップアップ メニューからです。

    ![タスク ランナー エクスプ ローラーのクリーン タスク](using-gulp/_static/04-TaskRunner-clean.png)

    **タスク ランナー エクスプ ローラー**という名前の新しいタブが作成されます**クリーン**で定義されているように clean タスクを実行および*gulpfile.js*です。

5.  右クリックし、**クリーン**タスク、し、選択**バインド** > **する前にビルド**です。

    ![タスク ランナー エクスプ ローラー BeforeBuild のバインド](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **する前にビルド**バインディングが、プロジェクトの各ビルドの前に自動的に実行するクリーン タスクを構成します。

バインドを設定すると**Task Runner Explorer**の上部にあるコメントの形式で保存されます、 *gulpfile.js*し、Visual Studio でのみ有効です。 Gulp のタスクの自動実行を構成するのには、代わりに Visual Studio を必要としない、 *.csproj*ファイル。 たとえば、これに配置して*.csproj*ファイル。

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Visual Studio でまたはを使用して、コマンド プロンプトから、プロジェクトを実行するときに clean タスクが実行されるようになりました、`dotnet run`コマンド (実行`npm install`最初)。

## <a name="defining-and-running-a-new-task"></a>定義して、新しいタスクを実行します。

新しい Gulp タスクを定義する次のように変更します。 *gulpfile.js*です。

1.  末尾に次の JavaScript を追加*gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    このタスクの名前は`first`、文字列だけが表示されます。

2.  保存*gulpfile.js*です。

3.  **ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*を選択して*Task Runner Explorer*です。

4.  **Task Runner Explorer**を右クリックして**最初**を選択して**実行**です。

    ![最初のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/06-TaskRunner-First.png)

    出力テキストが表示されることが表示されます。 一般的なシナリオの例で必要な場合は、Gulp レシピを参照してください。

## <a name="defining-and-running-tasks-in-a-series"></a>定義して、系列内のタスクを実行しています。

複数のタスクを実行すると、タスクは既定では同時に実行します。 ただし、特定の順序でタスクを実行する必要がある場合必要がありますを指定する各タスクが終了すると、同様にどのタスクとして別のタスクの完了に依存します。

1.  一連の順序で実行するタスクを定義するのには、置換、`first`上で追加したタスク*gulpfile.js*次。

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    次の 3 つのタスクがあるようになりました: `series:first`、 `series:second`、および`series`です。 `series:second`タスクには実行して、前に完了するタスクの配列を指定する 2 番目のパラメーターが含まれています、`series:second`タスクが実行されます。  上でのみコードで指定されたとおり、`series:first`前にタスクを完了する必要があります、`series:second`タスクが実行されます。

2.  保存*gulpfile.js*です。

3.  **ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*選択**Task Runner Explorer**まだ開いていない場合。

4.  **タスク ランナー エクスプ ローラー**を右クリックして**シリーズ**選択**実行**です。

    ![一連のタスクの実行タスク ランナー エクスプ ローラー](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense には、コード補完機能、パラメーターの説明、および生産性を向上し、エラーを削減するには、その他の機能が用意されています。 Gulp タスクは、JavaScript; で記述されました。そのため、IntelliSense は、開発中に、アシスタンスを提供することができます。 JavaScript を使用して IntelliSense リスト オブジェクト、関数、プロパティ、現在のコンテキストに基づいて、使用可能なパラメーターです。 コードを完了するための IntelliSense によって提供されるポップアップ リストから、コーディングのオプションを選択します。

![IntelliSense を gulp します。](using-gulp/_static/08-IntelliSense.png)

IntelliSense の詳細については、次を参照してください。 [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense)です。

## <a name="development-staging-and-production-environments"></a>開発、ステージング、実稼働環境

Gulp 使用すると、ステージングと運用環境のクライアント側ファイルを最適化するために、処理されたファイルは、ローカルのステージングと運用場所に保存されます。 *_Layout.cshtml*ファイルの使用、**環境**の CSS ファイルの 2 つの異なるバージョンを提供するためのヘルパーをタグ付けします。 開発用の CSS ファイルの 1 つのバージョンですし、ステージングと運用環境の両方の他のバージョンを最適化します。 変更すると、Visual Studio 2017 で、 **ASPNETCORE_ENVIRONMENT**環境変数を`Production`、最小化された CSS ファイルへのリンクと Web アプリ、Visual Studio はビルドされます。 次のマークアップの表示、**環境**タグ ヘルパーにリンク タグを含む、 `Development` CSS ファイルと、縮小された`Staging, Production`CSS ファイル。

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>環境間の切り替え

異なる環境用にコンパイルする間には、変更、 **ASPNETCORE_ENVIRONMENT**環境変数の値。

1.  **Task Runner Explorer**、いることを確認、 **min**を実行するタスクが設定されている**する前にビルド**です。

2.  **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **プロパティ**です。

    Web アプリのプロパティ シートが表示されます。

3.  **[デバッグ]** タブをクリックします。

4.  値を設定、**ホスティング環境:**環境変数を`Production`です。

5.  キーを押して**f5 キーを押して**ブラウザーで、アプリケーションを実行します。

6.  ブラウザーのウィンドウで、ページを右クリックし、選択**ソースの表示**ページの HTML を表示します。

    スタイル シートのリンクが縮小された CSS ファイルを指していることを確認します。

7.  Web アプリを停止するブラウザーを閉じます。

8.  Visual Studio で Web アプリのプロパティ シートに戻るし、変更、**ホスティング環境:**環境変数にバックアップ`Development`です。

9.  キーを押して**f5 キーを押して**ブラウザーでアプリケーションをもう一度実行します。

10. ブラウザーのウィンドウで、ページを右クリックし、選択**ソースの表示**をページの HTML を表示します。

    スタイル シートのリンクが unminified バージョンの CSS ファイルを指していることを確認します。

ASP.NET Core の環境に関連する詳細については、次を参照してください。[複数の環境で作業](../fundamentals/environments.md)です。

## <a name="task-and-module-details"></a>タスクとモジュールの詳細

Gulp タスクは、関数名で登録されます。  現在のタスクの前に他のタスクを実行する必要がある場合は、依存関係を指定できます。 追加の関数では、実行し Gulp タスクを見るだけでなく、ソースを設定することができます (*src*) と移行先 (*dest*) 変更されているファイル。 プライマリの Gulp API 関数を次に示します。

|Gulp 関数|構文|説明|
|---   |--- |--- |
|タスク  |`gulp.task(name[, deps], fn) { }`|`task`関数は、タスクを作成します。 `name`パラメーターは、タスクの名前を定義します。 `deps`パラメーターには、このタスクを実行する前に完了するタスクの配列が含まれています。 `fn`パラメーターは、タスクの操作を実行するコールバック関数を表します。|
|ウォッチ |`gulp.watch(glob [, opts], tasks) { }`|`watch`関数は、ファイルの変更が発生したときにファイルおよび実行するタスクを監視します。 `glob`パラメーターは、`string`または`array`ウォッチするファイルを指定します。 `opts`パラメーター オプションを視聴する追加のファイルを提供します。|
|src   |`gulp.src(globs[, options]) { }`|`src`関数は glob 値に一致するファイルを提供します。 `glob`パラメーターは、`string`または`array`を読み取るファイルを指定します。 `options`パラメーターは、追加のファイル オプションを提供します。|
|追加先  |`gulp.dest(path[, options]) { }`|`dest`関数は、ファイルの書き込み先となる場所を定義します。 `path`パラメーターが文字列またはコピー先のフォルダーを判断する関数。 `options`パラメーターは出力フォルダーのオプションを指定するオブジェクト。|

追加の Gulp API リファレンス情報について、次を参照してください。 [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)です。

## <a name="gulp-recipes"></a>レシピを gulp します。

Gulp コミュニティ提供 Gulp[レシピ](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)です。 これらのレシピは、一般的なシナリオに対処する Gulp タスクで構成されます。

## <a name="additional-resources"></a>その他の技術情報

* [Gulp ドキュメント](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [バンドルと ASP.NET Core の縮小](bundling-and-minification.md)
* [ASP.NET Core での Grunt の使用](using-grunt.md)
