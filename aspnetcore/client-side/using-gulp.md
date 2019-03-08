---
title: ASP.NET Core での Gulp を使用します。
author: rick-anderson
description: ASP.NET Core で Gulp を使用する方法について説明します。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 43277dc5910971374187f49031e74769c9e29e1f
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665627"
---
# <a name="use-gulp-in-aspnet-core"></a>ASP.NET Core での Gulp を使用します。

によって[Scott Addie](https://scottaddie.com)、 [Shayne Boyer](https://twitter.com/spboyer)、および[David 松](https://twitter.com/davidpine7)

一般的な最新の web アプリでは、ビルド プロセス可能性があります。

* バンドルし、JavaScript と CSS ファイルを縮小します。
* 各ビルドの前に、バンドルや縮小タスクを呼び出すツールを実行します。
* CSS にコンパイル未満または SASS ファイル。
* JavaScript に CoffeeScript または TypeScript ファイルをコンパイルします。

A*タスク ランナー*はこれらの日常的な開発タスクを自動化するツールです。 Visual Studio では、2 つの一般的な JavaScript ベースのタスク ランナーの組み込みサポートを提供します。[Gulp](https://gulpjs.com/)と[Grunt](using-grunt.md)します。

## <a name="gulp"></a>Gulp

Gulp は、クライアント側コードのための JavaScript ベースのストリーミング ビルド ツールキットです。 通常は、ビルド環境で特定のイベントがトリガーされたときに、一連のプロセスを使用してクライアント側ファイルをストリーム配信に使用されます。 Gulp を使用して自動化する、[バンドルと縮小](bundling-and-minification.md)または新しいビルドの前に、開発環境をクリーニングします。

Gulp タスクのセットが定義されている*gulpfile.js*します。 次の JavaScript では、Gulp モジュールが含まれます、近日公開予定のタスク内で参照されるファイルのパスを指定します。

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

上記のコードでは、ノード モジュールに必要なを指定します。 `require`関数は、依存タスクがその機能を利用するために各モジュールをインポートします。 インポートしたモジュールの各は、変数に割り当てられます。 モジュールは名前またはパスのいずれかに配置できます。 この例で、モジュールの名前`gulp`、 `rimraf`、 `gulp-concat`、 `gulp-cssmin`、および`gulp-uglify`名前によって取得されます。 さらに、CSS および JavaScript ファイルの場所を再利用して、タスク内で参照できるように、一連のパスが作成されます。 次の表の説明内に含まれるモジュールの*gulpfile.js*します。

| モジュール名 | 説明 |
| ----------- | ----------- |
| Gulp        | Gulp のストリーミングはビルド システムです。 詳細については、次を参照してください。 [gulp](https://www.npmjs.com/package/gulp)します。 |
| rimraf      | ノード削除のモジュール。 詳細については、次を参照してください。 [rimraf](https://www.npmjs.com/package/rimraf)します。 |
| gulp concat | オペレーティング システムの改行文字に基づいてファイルを連結するモジュール。 詳細については、次を参照してください。 [gulp concat](https://www.npmjs.com/package/gulp-concat)します。 |
| gulp cssmin | CSS ファイルを縮小するモジュール。 詳細については、次を参照してください。 [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)します。 |
| gulp uglify します。 | 縮小するモジュール *.js*ファイル。 詳細については、次を参照してください。 [gulp uglify](https://www.npmjs.com/package/gulp-uglify)します。 |

必要なモジュールをインポートすると、タスクを指定できます。 ここで 6 つのタスクが登録されている、次のコードで表されます。

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));
    
// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

次の表は、上記のコードで指定されたタスクの説明を提供します。

|タスク名|説明|
|--- |--- |
|クリーン: js|Rimraf ノード削除のモジュールを使用して、縮小されたバージョンの site.js ファイルを削除するタスク。|
|クリーン: css|Rimraf ノード削除のモジュールを使用して、site.css ファイルの縮小されたバージョンを削除するタスク。|
|クリーンアップ|呼び出すタスク、`clean:js`後に、タスク、`clean:css`タスク。|
|min:js|タスクを縮小し、js フォルダー内のすべての .js ファイルを連結します。 。 Min.js ファイルは除外されます。|
|min:css|タスクを縮小し、css フォルダー内のすべての .css ファイルを連結します。 。 Min.css ファイルは除外されます。|
|分|呼び出すタスク、`min:js`後に、タスク、`min:css`タスク。|

## <a name="running-default-tasks"></a>既定のタスクを実行しています。

既に新しい Web アプリを作成していない場合は、Visual Studio で新しい ASP.NET Web アプリケーション プロジェクトを作成します。

1.  開く、 *package.json*ファイル (追加ない場合があります)、次を追加します。

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2.  新しい JavaScript ファイルをプロジェクトに追加し、名前*gulpfile.js*、次のコードをコピーします。

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");
    
    const paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));
    
    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3.  **ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*、選び**Task Runner Explorer**します。
    
    ![ソリューション エクスプ ローラーから Task Runner Explorer を開く](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Task Runner Explorer** Gulp タスクの一覧が表示されます。 (をクリックする必要があります、**更新**プロジェクト名の左側に表示されるボタン)。
    
    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > **Task Runner Explorer**コンテキスト メニュー項目が表示される場合のみ*gulpfile.js*プロジェクトのルート ディレクトリにします。

4.  下に**タスク**で**Task Runner Explorer**を右クリックして**クリーン**、選び**実行**ポップアップ メニューから。

    ![タスク ランナー エクスプ ローラーのクリーン タスク](using-gulp/_static/04-TaskRunner-clean.png)

    **Task Runner Explorer**という名前の新しいタブを作成する**クリーン**で定義されているように、clean タスクを実行および*gulpfile.js*します。

5.  右クリックし、**クリーン**タスク、し、選択**バインド** > **Before Build**します。

    ![バインド BeforeBuild タスク ランナー エクスプ ローラー](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    **Before Build**バインド プロジェクトの各ビルドの前に自動的に実行するクリーン タスクを構成します。

バインドを設定する**Task Runner Explorer**の上部にあるコメントの形式で格納されます、 *gulpfile.js*と Visual Studio でのみ有効です。 Visual Studio を必要としない代わりに、gulp タスクでの自動実行を構成し、 *.csproj*ファイル。 たとえば、これに含める、 *.csproj*ファイル。

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

またはを使用してコマンド プロンプトから Visual Studio でプロジェクトを実行するときに、clean タスクが実行されるので、 [「dotnet run」](/dotnet/core/tools/dotnet-run)コマンド (実行`npm install`最初).

## <a name="defining-and-running-a-new-task"></a>定義して、新しいタスクを実行します。

新しい Gulp タスクを定義する次のように変更します。 *gulpfile.js*します。

1.  末尾に次の JavaScript を追加*gulpfile.js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    このタスクの名前は`first`、単に文字列を表示します。

2.  保存*gulpfile.js*します。

3.  **ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*、選び*Task Runner Explorer*します。

4.  **Task Runner Explorer**、右クリックして**最初**を選択し、**実行**します。

    ![最初のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/06-TaskRunner-First.png)

    出力テキストが表示されます。 一般的なシナリオに基づく例については、次を参照してください。 [Gulp レシピ](#gulp-recipes)します。

## <a name="defining-and-running-tasks-in-a-series"></a>定義して、一連のタスクを実行しています。

複数のタスクを実行するときに既定では、タスクが同時に実行します。 ただし、特定の順序でタスクを実行する場合は、する必要があります指定する各タスクが完了するもタスクとして別のタスクの完了に依存します。

1.  一連の順序で実行するタスクを定義するには、置換、`first`上で追加したタスク*gulpfile.js*次。

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
      done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
    gulp.task('default', gulp.series('series'));
    ```
 
    3 つのタスクがあるようになりました: `series:first`、 `series:second`、および`series`します。 `series:second`タスクには実行する前に完了したタスクの配列を指定する 2 番目のパラメーターが含まれています、`series:second`タスクが実行されます。 上だけのコードで指定されている、`series:first`する前にタスクを完了する必要があります、`series:second`タスクが実行されます。

2.  保存*gulpfile.js*します。

3.  **ソリューション エクスプ ローラー**、右クリック*gulpfile.js*選択**Task Runner Explorer**まだ開いていない場合。

4.  **Task Runner Explorer**を右クリックして**シリーズ**選択**実行**します。

    ![一連のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense は、コード補完、パラメーターの説明、および生産性を向上させると、エラーを減らすには、その他の機能を提供します。 Gulp タスクは、JavaScript; で記述されました。そのため、IntelliSense では、開発中に、アシスタンスを提供できます。 JavaScript を使用して IntelliSense は、オブジェクト、関数、プロパティを一覧表示されます。 現在のコンテキストに基づいて使用可能なパラメーター。 コードを完成する IntelliSense によって提供されるポップアップ リストから、コーディングのオプションを選択します。

![gulp の IntelliSense](using-gulp/_static/08-IntelliSense.png)

IntelliSense の詳細については、次を参照してください。 [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense)します。

## <a name="development-staging-and-production-environments"></a>開発、ステージング、実稼働環境

Gulp を使用して、クライアント側ファイルのステージングと運用環境を最適化する、処理されたファイルは、ローカルのステージングと運用環境の場所に保存されます。 *_Layout.cshtml*ファイルの使用、**環境**タグ ヘルパーの CSS ファイルの 2 つの異なるバージョンを提供します。 CSS ファイルの 1 つのバージョンは、開発とステージング環境と運用環境の両方のもう一方のバージョンは最適化されています。 変更すると、Visual Studio 2017 で、 **ASPNETCORE_ENVIRONMENT**環境変数を`Production`、Visual Studio は、Web アプリと、最小化された CSS ファイルへのリンクは作成します。 次のマークアップに示す、**環境**タグ ヘルパーをリンク タグを含む、 `Development` CSS ファイルと、縮小された`Staging, Production`CSS ファイル。

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

## <a name="switching-between-environments"></a>環境の切り替え

異なる環境でのコンパイルの間には、変更、 **ASPNETCORE_ENVIRONMENT**環境変数の値。

1.  **Task Runner Explorer**、いることを確認、 **min**を実行するタスクが設定されている**Before Build**。

2.  **ソリューション エクスプ ローラー**でプロジェクト名を右クリックし、選択**プロパティ**します。

    Web アプリのプロパティ シートが表示されます。

3.  **[デバッグ]** タブをクリックします。

4.  値を設定、**ホスティング環境:** 環境変数を`Production`します。

5.  キーを押して**F5**ブラウザーでアプリケーションを実行します。

6.  ブラウザーのウィンドウでは、ページを右クリックして**ソースの表示**ページの HTML を表示します。

    スタイル シートのリンクが縮小された CSS ファイルを指していることを確認します。

7.  Web アプリを停止するブラウザーを閉じます。

8.  Visual Studio で Web アプリのプロパティ シートに戻るし、変更、**ホスティング環境:** 環境変数にバックアップ`Development`します。

9.  キーを押して**F5**ブラウザーでアプリケーションをもう一度実行します。

10. ブラウザーのウィンドウでは、ページを右クリックして**ソースの表示**に HTML ページを参照してください。

    スタイル シートのリンクが指す unminified バージョンの CSS ファイルに注意してください。

ASP.NET Core での環境に関連する詳細については、次を参照してください。[複数の環境を使用して、](../fundamentals/environments.md)します。

## <a name="task-and-module-details"></a>タスクとモジュールの詳細

Gulp タスクは、関数名で登録されます。 現在のタスクの前にその他のタスクを実行する必要がある場合は、依存関係を指定できます。 追加の関数を実行し、Gulp タスクを見るだけでなくソースを設定することができます (*src*) と移行先 (*dest*) ファイルが変更されるのです。 次に、プライマリの Gulp の API 関数を示します。

|Gulp 関数|構文|説明|
|---   |--- |--- |
|タスク  |`gulp.task(name[, deps], fn) { }`|`task`関数は、タスクを作成します。 `name`パラメーター タスクの名前を定義します。 `deps`パラメーターには、このタスクを実行する前に完了するタスクの配列が含まれています。 `fn`パラメーターは、タスクの操作を実行するコールバック関数を表します。|
|ウォッチ |`gulp.watch(glob [, opts], tasks) { }`|`watch`関数は、ファイルの変更が発生した場合にファイルと実行のタスクを監視します。 `glob`パラメーターは、`string`または`array`ウォッチするファイルを決定します。 `opts`パラメーター オプションを見ているその他のファイルを提供します。|
|src   |`gulp.src(globs[, options]) { }`|`src`関数 glob 値に一致するファイルを提供します。 `glob`パラメーターは、`string`または`array`ファイルを読み取るかを決定します。 `options`パラメーターが追加のファイル オプションを提供します。|
|追加先  |`gulp.dest(path[, options]) { }`|`dest`関数は、ファイルの書き込み先の場所を定義します。 `path`パラメーターは、文字列またはコピー先のフォルダーを決定する関数。 `options`パラメーターが出力フォルダーのオプションを指定するオブジェクト。|

Gulp の API リファレンスの詳細は、次を参照してください。 [Gulp Docs API](https://gulpjs.org/API.html)します。

## <a name="gulp-recipes"></a>Gulp のレシピ

Gulp コミュニティ提供の Gulp[レシピ](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)します。 これらのレシピは、一般的なシナリオに対処する Gulp タスクで構成されます。

## <a name="additional-resources"></a>その他の技術情報

* [Gulp のドキュメント](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [バンドルと ASP.NET Core での縮小](bundling-and-minification.md)
* [ASP.NET Core で Grunt を使用します。](using-grunt.md)
