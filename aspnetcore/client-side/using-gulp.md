---
title: "ASP.NET Core で Gulp を使用します。"
author: rick-anderson
description: "ASP.NET Core で Gulp を使用する方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: 0a3443e8187d46992f55dc537d0f400c6771c50c
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="6c6b8-103">ASP.NET Core で Gulp を使用の概要</span><span class="sxs-lookup"><span data-stu-id="6c6b8-103">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="6c6b8-104">によって[Erik Reitan](https://github.com/Erikre)、 [Scott Addie](https://scottaddie.com)、 [Daniel Roth](https://github.com/danroth27)、および[Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="6c6b8-104">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="6c6b8-105">典型的な最新の web アプリで、ビルド プロセスでは次のことがあります。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="6c6b8-106">バンドルして、JavaScript および CSS ファイルを縮小します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="6c6b8-107">ツールを実行する各ビルドの前に、バンドルと縮小のタスクを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="6c6b8-108">CSS にコンパイル未満または SASS ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="6c6b8-109">JavaScript に CoffeeScript または TypeScript ファイルをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="6c6b8-110">A*タスク ランナー*これらの日常的な開発タスクを自動化するツールです。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="6c6b8-111">Visual Studio では、2 つの JavaScript ベースの一般的なタスク ランナーの組み込みサポートが用意されています: [Gulp](https://gulpjs.com/)と[Grunt](using-grunt.md)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="6c6b8-112">gulp</span><span class="sxs-lookup"><span data-stu-id="6c6b8-112">Gulp</span></span>

<span data-ttu-id="6c6b8-113">Gulp は、JavaScript ベース ストリーミング ビルド ツールキットのクライアント側のコードです。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="6c6b8-114">通常は、ビルド環境で特定のイベントがトリガーされたときに、一連のプロセスを使用してクライアント側ファイルをストリーム配信に使用されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="6c6b8-115">Gulp を使用して自動化するインスタンスの[バンドルと縮小](bundling-and-minification.md)新しいビルドの前に、開発環境のクレンジングまたはします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="6c6b8-116">Gulp タスクのセットが定義されている*gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="6c6b8-117">次の JavaScript では、Gulp モジュールが含まれます、近日公開予定のタスク内で参照されるファイルのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="6c6b8-118">上記のコードでは、どのノード モジュールに必要なを指定します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="6c6b8-119">`require`関数が依存タスクは、その機能を利用できるように、各モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="6c6b8-120">インポートしたモジュールの各は、変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="6c6b8-121">モジュールは、名前またはパスのいずれかに配置できます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="6c6b8-122">この例では、モジュールが名前付き`gulp`、 `rimraf`、 `gulp-concat`、`gulp-cssmin`と`gulp-uglify`名前によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="6c6b8-123">さらに、一連のパスは、CSS および JavaScript ファイルの場所を再利用したり、タスク内で参照できるように作成されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="6c6b8-124">次の表の説明に含まれるモジュールの*gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="6c6b8-125">モジュール名</span><span class="sxs-lookup"><span data-stu-id="6c6b8-125">Module Name</span></span> | <span data-ttu-id="6c6b8-126">説明</span><span class="sxs-lookup"><span data-stu-id="6c6b8-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="6c6b8-127">gulp</span><span class="sxs-lookup"><span data-stu-id="6c6b8-127">gulp</span></span>        | <span data-ttu-id="6c6b8-128">Gulp ストリーミング ビルド システムです。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-128">The Gulp streaming build system.</span></span> <span data-ttu-id="6c6b8-129">詳細については、次を参照してください。 [gulp](https://www.npmjs.com/package/gulp)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="6c6b8-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="6c6b8-130">rimraf</span></span>      | <span data-ttu-id="6c6b8-131">ノードの削除モジュール。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-131">A Node deletion module.</span></span> <span data-ttu-id="6c6b8-132">詳細については、次を参照してください。 [rimraf](https://www.npmjs.com/package/rimraf)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="6c6b8-133">gulp-concat</span><span class="sxs-lookup"><span data-stu-id="6c6b8-133">gulp-concat</span></span> | <span data-ttu-id="6c6b8-134">オペレーティング システムの改行文字に基づいてファイルを連結するモジュール。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="6c6b8-135">詳細については、次を参照してください。 [gulp concat](https://www.npmjs.com/package/gulp-concat)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="6c6b8-136">gulp-cssmin</span><span class="sxs-lookup"><span data-stu-id="6c6b8-136">gulp-cssmin</span></span> | <span data-ttu-id="6c6b8-137">CSS ファイルを縮小するモジュール。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-137">A module that minifies CSS files.</span></span> <span data-ttu-id="6c6b8-138">詳細については、次を参照してください。 [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="6c6b8-139">gulp uglify します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-139">gulp-uglify</span></span> | <span data-ttu-id="6c6b8-140">縮小するモジュール*.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="6c6b8-141">詳細については、次を参照してください。 [gulp uglify](https://www.npmjs.com/package/gulp-uglify)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="6c6b8-142">必要なモジュールがインポートされると、タスクを指定できます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="6c6b8-143">ここで 6 つのタスクが登録されている、次のコードで表されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="6c6b8-144">次の表では、上記のコードで指定されているタスクの説明します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="6c6b8-145">タスク名</span><span class="sxs-lookup"><span data-stu-id="6c6b8-145">Task Name</span></span>|<span data-ttu-id="6c6b8-146">説明</span><span class="sxs-lookup"><span data-stu-id="6c6b8-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="6c6b8-147">クリーン: js</span><span class="sxs-lookup"><span data-stu-id="6c6b8-147">clean:js</span></span>|<span data-ttu-id="6c6b8-148">使用するタスクを rimraf ノード削除モジュール site.js ファイルの縮小されたバージョンを削除します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="6c6b8-149">クリーン: css</span><span class="sxs-lookup"><span data-stu-id="6c6b8-149">clean:css</span></span>|<span data-ttu-id="6c6b8-150">Site.css ファイルの縮小されたバージョンを削除する rimraf ノード削除モジュールを使用するタスク。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="6c6b8-151">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="6c6b8-151">clean</span></span>|<span data-ttu-id="6c6b8-152">呼び出すタスク、`clean:js`タスク、続けて、`clean:css`タスク。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="6c6b8-153">min:js</span><span class="sxs-lookup"><span data-stu-id="6c6b8-153">min:js</span></span>|<span data-ttu-id="6c6b8-154">タスクを縮小し、js フォルダー内のすべての .js ファイルを連結します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="6c6b8-155">。 Min.js ファイルは除外されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="6c6b8-156">min:css</span><span class="sxs-lookup"><span data-stu-id="6c6b8-156">min:css</span></span>|<span data-ttu-id="6c6b8-157">タスクを縮小し、css フォルダー内のすべての .css ファイルを連結します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="6c6b8-158">。 Min.css ファイルは除外されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="6c6b8-159">分</span><span class="sxs-lookup"><span data-stu-id="6c6b8-159">min</span></span>|<span data-ttu-id="6c6b8-160">呼び出すタスク、`min:js`タスク、続けて、`min:css`タスク。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="6c6b8-161">既定のタスクを実行しています。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-161">Running default tasks</span></span>

<span data-ttu-id="6c6b8-162">新しい Web アプリをまだ作成していない場合は、Visual Studio で新しい ASP.NET Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="6c6b8-163">新しい JavaScript ファイルをプロジェクトに追加し、名前*gulpfile.js*、次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-163">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="6c6b8-164">開く、 *package.json*ファイル (追加しない場合があります)、以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-164">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="6c6b8-165">**ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*を選択して**Task Runner Explorer**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![ソリューション エクスプ ローラーからタスク ランナー エクスプ ローラーを開く](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="6c6b8-167">**タスク ランナー エクスプ ローラー** Gulp タスクの一覧を示しています。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="6c6b8-168">(をクリックする必要があります、**更新**プロジェクト名の左側に表示されるボタンをクリックします)。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![タスク ランナー エクスプ ローラー](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="6c6b8-170">下に**タスク**で**タスク ランナー エクスプ ローラー**を右クリックして**クリーン**を選択し、**実行**ポップアップ メニューからです。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-170">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![タスク ランナー エクスプ ローラーのクリーン タスク](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="6c6b8-172">**タスク ランナー エクスプ ローラー**という名前の新しいタブが作成されます**クリーン**で定義されているように clean タスクを実行および*gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-172">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="6c6b8-173">右クリックし、**クリーン**タスク、し、選択**バインド** > **する前にビルド**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-173">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![タスク ランナー エクスプ ローラー BeforeBuild のバインド](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="6c6b8-175">**する前にビルド**バインディングが、プロジェクトの各ビルドの前に自動的に実行するクリーン タスクを構成します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-175">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="6c6b8-176">バインドを設定すると**Task Runner Explorer**の上部にあるコメントの形式で保存されます、 *gulpfile.js*し、Visual Studio でのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-176">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="6c6b8-177">Gulp のタスクの自動実行を構成するのには、代わりに Visual Studio を必要としない、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-177">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="6c6b8-178">たとえば、これに配置して*.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-178">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="6c6b8-179">Visual Studio でまたはを使用して、コマンド プロンプトから、プロジェクトを実行するときに clean タスクが実行されるようになりました、[実行 dotnet](/dotnet/core/tools/dotnet-run)コマンド (実行`npm install`最初)。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-179">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="6c6b8-180">定義して、新しいタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-180">Defining and running a new task</span></span>

<span data-ttu-id="6c6b8-181">新しい Gulp タスクを定義する次のように変更します。 *gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-181">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="6c6b8-182">末尾に次の JavaScript を追加*gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="6c6b8-182">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="6c6b8-183">このタスクの名前は`first`、文字列だけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-183">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="6c6b8-184">保存*gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-184">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="6c6b8-185">**ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*を選択して*Task Runner Explorer*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-185">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="6c6b8-186">**Task Runner Explorer**を右クリックして**最初**を選択して**実行**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-186">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![最初のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="6c6b8-188">出力テキストが表示されることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-188">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="6c6b8-189">一般的なシナリオの例で必要な場合は、Gulp レシピを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-189">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="6c6b8-190">定義して、系列内のタスクを実行しています。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-190">Defining and running tasks in a series</span></span>

<span data-ttu-id="6c6b8-191">複数のタスクを実行すると、タスクは既定では同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-191">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="6c6b8-192">ただし、特定の順序でタスクを実行する必要がある場合必要がありますを指定する各タスクが終了すると、同様にどのタスクとして別のタスクの完了に依存します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-192">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="6c6b8-193">一連の順序で実行するタスクを定義するのには、置換、`first`上で追加したタスク*gulpfile.js*次。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-193">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="6c6b8-194">次の 3 つのタスクがあるようになりました: `series:first`、 `series:second`、および`series`です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-194">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="6c6b8-195">`series:second`タスクには実行して、前に完了するタスクの配列を指定する 2 番目のパラメーターが含まれています、`series:second`タスクが実行されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-195">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="6c6b8-196">上でのみコードで指定されたとおり、`series:first`前にタスクを完了する必要があります、`series:second`タスクが実行されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-196">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="6c6b8-197">保存*gulpfile.js*です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-197">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="6c6b8-198">**ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*選択**Task Runner Explorer**まだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-198">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="6c6b8-199">**タスク ランナー エクスプ ローラー**を右クリックして**シリーズ**選択**実行**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-199">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![一連のタスクの実行タスク ランナー エクスプ ローラー](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="6c6b8-201">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="6c6b8-201">IntelliSense</span></span>

<span data-ttu-id="6c6b8-202">IntelliSense には、コード補完機能、パラメーターの説明、および生産性を向上し、エラーを削減するには、その他の機能が用意されています。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-202">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="6c6b8-203">Gulp タスクは、JavaScript; で記述されました。そのため、IntelliSense は、開発中に、アシスタンスを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-203">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="6c6b8-204">JavaScript を使用して IntelliSense リスト オブジェクト、関数、プロパティ、現在のコンテキストに基づいて、使用可能なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-204">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="6c6b8-205">コードを完了するための IntelliSense によって提供されるポップアップ リストから、コーディングのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-205">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![IntelliSense を gulp します。](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="6c6b8-207">IntelliSense の詳細については、次を参照してください。 [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-207">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="6c6b8-208">開発、ステージング、実稼働環境</span><span class="sxs-lookup"><span data-stu-id="6c6b8-208">Development, staging, and production environments</span></span>

<span data-ttu-id="6c6b8-209">Gulp 使用すると、ステージングと運用環境のクライアント側ファイルを最適化するために、処理されたファイルは、ローカルのステージングと運用場所に保存されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-209">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="6c6b8-210">*_Layout.cshtml*ファイルの使用、**環境**の CSS ファイルの 2 つの異なるバージョンを提供するためのヘルパーをタグ付けします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-210">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="6c6b8-211">開発用の CSS ファイルの 1 つのバージョンですし、ステージングと運用環境の両方の他のバージョンを最適化します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-211">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="6c6b8-212">変更すると、Visual Studio 2017 で、 **ASPNETCORE_ENVIRONMENT**環境変数を`Production`、最小化された CSS ファイルへのリンクと Web アプリ、Visual Studio はビルドされます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-212">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="6c6b8-213">次のマークアップの表示、**環境**タグ ヘルパーにリンク タグを含む、 `Development` CSS ファイルと、縮小された`Staging, Production`CSS ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-213">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="6c6b8-214">環境間の切り替え</span><span class="sxs-lookup"><span data-stu-id="6c6b8-214">Switching between environments</span></span>

<span data-ttu-id="6c6b8-215">異なる環境用にコンパイルする間には、変更、 **ASPNETCORE_ENVIRONMENT**環境変数の値。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-215">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="6c6b8-216">**Task Runner Explorer**、いることを確認、 **min**を実行するタスクが設定されている**する前にビルド**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-216">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="6c6b8-217">**ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-217">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="6c6b8-218">Web アプリのプロパティ シートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-218">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="6c6b8-219">**[デバッグ]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-219">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="6c6b8-220">値を設定、**ホスティング環境:**環境変数を`Production`です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-220">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="6c6b8-221">キーを押して**f5 キーを押して**ブラウザーで、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-221">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="6c6b8-222">ブラウザーのウィンドウで、ページを右クリックし、選択**ソースの表示**ページの HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-222">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="6c6b8-223">スタイル シートのリンクが縮小された CSS ファイルを指していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-223">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="6c6b8-224">Web アプリを停止するブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-224">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="6c6b8-225">Visual Studio で Web アプリのプロパティ シートに戻るし、変更、**ホスティング環境:**環境変数にバックアップ`Development`です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-225">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="6c6b8-226">キーを押して**f5 キーを押して**ブラウザーでアプリケーションをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-226">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="6c6b8-227">ブラウザーのウィンドウで、ページを右クリックし、選択**ソースの表示**をページの HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-227">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="6c6b8-228">スタイル シートのリンクが unminified バージョンの CSS ファイルを指していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-228">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="6c6b8-229">ASP.NET Core の環境に関連する詳細については、次を参照してください。[複数の環境で作業](../fundamentals/environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-229">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="6c6b8-230">タスクとモジュールの詳細</span><span class="sxs-lookup"><span data-stu-id="6c6b8-230">Task and module details</span></span>

<span data-ttu-id="6c6b8-231">Gulp タスクは、関数名で登録されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-231">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="6c6b8-232">現在のタスクの前に他のタスクを実行する必要がある場合は、依存関係を指定できます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-232">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="6c6b8-233">追加の関数では、実行し Gulp タスクを見るだけでなく、ソースを設定することができます (*src*) と移行先 (*dest*) 変更されているファイル。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-233">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="6c6b8-234">プライマリの Gulp API 関数を次に示します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-234">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="6c6b8-235">Gulp 関数</span><span class="sxs-lookup"><span data-stu-id="6c6b8-235">Gulp Function</span></span>|<span data-ttu-id="6c6b8-236">構文</span><span class="sxs-lookup"><span data-stu-id="6c6b8-236">Syntax</span></span>|<span data-ttu-id="6c6b8-237">説明</span><span class="sxs-lookup"><span data-stu-id="6c6b8-237">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="6c6b8-238">タスク</span><span class="sxs-lookup"><span data-stu-id="6c6b8-238">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="6c6b8-239">`task`関数は、タスクを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-239">The `task` function creates a task.</span></span> <span data-ttu-id="6c6b8-240">`name`パラメーターは、タスクの名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-240">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="6c6b8-241">`deps`パラメーターには、このタスクを実行する前に完了するタスクの配列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-241">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="6c6b8-242">`fn`パラメーターは、タスクの操作を実行するコールバック関数を表します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-242">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="6c6b8-243">ウォッチ</span><span class="sxs-lookup"><span data-stu-id="6c6b8-243">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="6c6b8-244">`watch`関数は、ファイルの変更が発生したときにファイルおよび実行するタスクを監視します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-244">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="6c6b8-245">`glob`パラメーターは、`string`または`array`ウォッチするファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-245">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="6c6b8-246">`opts`パラメーター オプションを視聴する追加のファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-246">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="6c6b8-247">src</span><span class="sxs-lookup"><span data-stu-id="6c6b8-247">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="6c6b8-248">`src`関数は glob 値に一致するファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-248">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="6c6b8-249">`glob`パラメーターは、`string`または`array`を読み取るファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-249">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="6c6b8-250">`options`パラメーターは、追加のファイル オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-250">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="6c6b8-251">追加先</span><span class="sxs-lookup"><span data-stu-id="6c6b8-251">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="6c6b8-252">`dest`関数は、ファイルの書き込み先となる場所を定義します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-252">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="6c6b8-253">`path`パラメーターが文字列またはコピー先のフォルダーを判断する関数。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-253">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="6c6b8-254">`options`パラメーターは出力フォルダーのオプションを指定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-254">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="6c6b8-255">追加の Gulp API リファレンス情報について、次を参照してください。 [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-255">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="6c6b8-256">レシピを gulp します。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-256">Gulp recipes</span></span>

<span data-ttu-id="6c6b8-257">Gulp コミュニティ提供 Gulp[レシピ](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)です。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-257">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="6c6b8-258">これらのレシピは、一般的なシナリオに対処する Gulp タスクで構成されます。</span><span class="sxs-lookup"><span data-stu-id="6c6b8-258">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c6b8-259">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="6c6b8-259">Additional resources</span></span>

* [<span data-ttu-id="6c6b8-260">Gulp ドキュメント</span><span class="sxs-lookup"><span data-stu-id="6c6b8-260">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="6c6b8-261">バンドルと ASP.NET Core の縮小</span><span class="sxs-lookup"><span data-stu-id="6c6b8-261">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="6c6b8-262">ASP.NET Core での Grunt の使用</span><span class="sxs-lookup"><span data-stu-id="6c6b8-262">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
