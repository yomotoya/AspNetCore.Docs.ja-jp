---
title: ASP.NET Core での Gulp を使用します。
author: rick-anderson
description: ASP.NET Core で Gulp を使用する方法について説明します。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209873"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="03148-103">ASP.NET Core での Gulp を使用します。</span><span class="sxs-lookup"><span data-stu-id="03148-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="03148-104">によって[Scott Addie](https://scottaddie.com)、 [Shayne Boyer](https://twitter.com/spboyer)、および[David 松](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="03148-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="03148-105">一般的な最新の web アプリでは、ビルド プロセス可能性があります。</span><span class="sxs-lookup"><span data-stu-id="03148-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="03148-106">バンドルし、JavaScript と CSS ファイルを縮小します。</span><span class="sxs-lookup"><span data-stu-id="03148-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="03148-107">各ビルドの前に、バンドルや縮小タスクを呼び出すツールを実行します。</span><span class="sxs-lookup"><span data-stu-id="03148-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="03148-108">CSS にコンパイル未満または SASS ファイル。</span><span class="sxs-lookup"><span data-stu-id="03148-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="03148-109">JavaScript に CoffeeScript または TypeScript ファイルをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="03148-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="03148-110">A*タスク ランナー*はこれらの日常的な開発タスクを自動化するツールです。</span><span class="sxs-lookup"><span data-stu-id="03148-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="03148-111">Visual Studio では、2 つの一般的な JavaScript ベースのタスク ランナーの組み込みサポートを提供します。[Gulp](https://gulpjs.com/)と[Grunt](using-grunt.md)します。</span><span class="sxs-lookup"><span data-stu-id="03148-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="03148-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="03148-112">Gulp</span></span>

<span data-ttu-id="03148-113">Gulp は、クライアント側コードのための JavaScript ベースのストリーミング ビルド ツールキットです。</span><span class="sxs-lookup"><span data-stu-id="03148-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="03148-114">通常は、ビルド環境で特定のイベントがトリガーされたときに、一連のプロセスを使用してクライアント側ファイルをストリーム配信に使用されます。</span><span class="sxs-lookup"><span data-stu-id="03148-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="03148-115">Gulp を使用して自動化する、[バンドルと縮小](bundling-and-minification.md)または新しいビルドの前に、開発環境をクリーニングします。</span><span class="sxs-lookup"><span data-stu-id="03148-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="03148-116">Gulp タスクのセットが定義されている*gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="03148-117">次の JavaScript では、Gulp モジュールが含まれます、近日公開予定のタスク内で参照されるファイルのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="03148-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="03148-118">上記のコードでは、ノード モジュールに必要なを指定します。</span><span class="sxs-lookup"><span data-stu-id="03148-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="03148-119">`require`関数は、依存タスクがその機能を利用するために各モジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="03148-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="03148-120">インポートしたモジュールの各は、変数に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="03148-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="03148-121">モジュールは名前またはパスのいずれかに配置できます。</span><span class="sxs-lookup"><span data-stu-id="03148-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="03148-122">この例で、モジュールの名前`gulp`、 `rimraf`、 `gulp-concat`、 `gulp-cssmin`、および`gulp-uglify`名前によって取得されます。</span><span class="sxs-lookup"><span data-stu-id="03148-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="03148-123">さらに、CSS および JavaScript ファイルの場所を再利用して、タスク内で参照できるように、一連のパスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="03148-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="03148-124">次の表の説明内に含まれるモジュールの*gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="03148-125">モジュール名</span><span class="sxs-lookup"><span data-stu-id="03148-125">Module Name</span></span> | <span data-ttu-id="03148-126">説明</span><span class="sxs-lookup"><span data-stu-id="03148-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="03148-127">Gulp</span><span class="sxs-lookup"><span data-stu-id="03148-127">gulp</span></span>        | <span data-ttu-id="03148-128">Gulp のストリーミングはビルド システムです。</span><span class="sxs-lookup"><span data-stu-id="03148-128">The Gulp streaming build system.</span></span> <span data-ttu-id="03148-129">詳細については、[gulp](https://www.npmjs.com/package/gulp)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="03148-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="03148-130">rimraf</span></span>      | <span data-ttu-id="03148-131">ノード削除のモジュール。</span><span class="sxs-lookup"><span data-stu-id="03148-131">A Node deletion module.</span></span> <span data-ttu-id="03148-132">詳細については、[rimraf](https://www.npmjs.com/package/rimraf)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="03148-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="03148-133">gulp-concat</span></span> | <span data-ttu-id="03148-134">オペレーティング システムの改行文字に基づいてファイルを連結するモジュール。</span><span class="sxs-lookup"><span data-stu-id="03148-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="03148-135">詳細については、[gulp concat](https://www.npmjs.com/package/gulp-concat)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="03148-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="03148-136">gulp-cssmin</span></span> | <span data-ttu-id="03148-137">CSS ファイルを縮小するモジュール。</span><span class="sxs-lookup"><span data-stu-id="03148-137">A module that minifies CSS files.</span></span> <span data-ttu-id="03148-138">詳細については、[gulp cssmin](https://www.npmjs.com/package/gulp-cssmin)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="03148-139">gulp uglify します。</span><span class="sxs-lookup"><span data-stu-id="03148-139">gulp-uglify</span></span> | <span data-ttu-id="03148-140">縮小するモジュール *.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03148-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="03148-141">詳細については、[gulp uglify](https://www.npmjs.com/package/gulp-uglify)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="03148-142">必要なモジュールをインポートすると、タスクを指定できます。</span><span class="sxs-lookup"><span data-stu-id="03148-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="03148-143">ここで 6 つのタスクが登録されている、次のコードで表されます。</span><span class="sxs-lookup"><span data-stu-id="03148-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="03148-144">次の表は、上記のコードで指定されたタスクの説明を提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="03148-145">タスク名</span><span class="sxs-lookup"><span data-stu-id="03148-145">Task Name</span></span>|<span data-ttu-id="03148-146">説明</span><span class="sxs-lookup"><span data-stu-id="03148-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="03148-147">クリーン: js</span><span class="sxs-lookup"><span data-stu-id="03148-147">clean:js</span></span>|<span data-ttu-id="03148-148">Rimraf ノード削除のモジュールを使用して、縮小されたバージョンの site.js ファイルを削除するタスク。</span><span class="sxs-lookup"><span data-stu-id="03148-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="03148-149">クリーン: css</span><span class="sxs-lookup"><span data-stu-id="03148-149">clean:css</span></span>|<span data-ttu-id="03148-150">Rimraf ノード削除のモジュールを使用して、site.css ファイルの縮小されたバージョンを削除するタスク。</span><span class="sxs-lookup"><span data-stu-id="03148-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="03148-151">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="03148-151">clean</span></span>|<span data-ttu-id="03148-152">呼び出すタスク、`clean:js`後に、タスク、`clean:css`タスク。</span><span class="sxs-lookup"><span data-stu-id="03148-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="03148-153">min:js</span><span class="sxs-lookup"><span data-stu-id="03148-153">min:js</span></span>|<span data-ttu-id="03148-154">タスクを縮小し、js フォルダー内のすべての .js ファイルを連結します。</span><span class="sxs-lookup"><span data-stu-id="03148-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="03148-155">。 Min.js ファイルは除外されます。</span><span class="sxs-lookup"><span data-stu-id="03148-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="03148-156">min:css</span><span class="sxs-lookup"><span data-stu-id="03148-156">min:css</span></span>|<span data-ttu-id="03148-157">タスクを縮小し、css フォルダー内のすべての .css ファイルを連結します。</span><span class="sxs-lookup"><span data-stu-id="03148-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="03148-158">。 Min.css ファイルは除外されます。</span><span class="sxs-lookup"><span data-stu-id="03148-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="03148-159">分</span><span class="sxs-lookup"><span data-stu-id="03148-159">min</span></span>|<span data-ttu-id="03148-160">呼び出すタスク、`min:js`後に、タスク、`min:css`タスク。</span><span class="sxs-lookup"><span data-stu-id="03148-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="03148-161">既定のタスクを実行しています。</span><span class="sxs-lookup"><span data-stu-id="03148-161">Running default tasks</span></span>

<span data-ttu-id="03148-162">既に新しい Web アプリを作成していない場合は、Visual Studio で新しい ASP.NET Web アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="03148-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1. <span data-ttu-id="03148-163">開く、 *package.json*ファイル (追加ない場合があります)、次を追加します。</span><span class="sxs-lookup"><span data-stu-id="03148-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2. <span data-ttu-id="03148-164">新しい JavaScript ファイルをプロジェクトに追加し、名前*gulpfile.js*、次のコードをコピーします。</span><span class="sxs-lookup"><span data-stu-id="03148-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3. <span data-ttu-id="03148-165">**ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*、選び**Task Runner Explorer**します。</span><span class="sxs-lookup"><span data-stu-id="03148-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>

    ![ソリューション エクスプ ローラーから Task Runner Explorer を開く](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    <span data-ttu-id="03148-167">**Task Runner Explorer** Gulp タスクの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="03148-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="03148-168">(をクリックする必要があります、**更新**プロジェクト名の左側に表示されるボタン)。</span><span class="sxs-lookup"><span data-stu-id="03148-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>

    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > <span data-ttu-id="03148-170">**Task Runner Explorer**コンテキスト メニュー項目が表示される場合のみ*gulpfile.js*プロジェクトのルート ディレクトリにします。</span><span class="sxs-lookup"><span data-stu-id="03148-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4. <span data-ttu-id="03148-171">下に**タスク**で**Task Runner Explorer**を右クリックして**クリーン**、選び**実行**ポップアップ メニューから。</span><span class="sxs-lookup"><span data-stu-id="03148-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![タスク ランナー エクスプ ローラーのクリーン タスク](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="03148-173">**Task Runner Explorer**という名前の新しいタブを作成する**クリーン**で定義されているように、clean タスクを実行および*gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5. <span data-ttu-id="03148-174">右クリックし、**クリーン**タスク、し、選択**バインド** > **Before Build**します。</span><span class="sxs-lookup"><span data-stu-id="03148-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![バインド BeforeBuild タスク ランナー エクスプ ローラー](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="03148-176">**Before Build**バインド プロジェクトの各ビルドの前に自動的に実行するクリーン タスクを構成します。</span><span class="sxs-lookup"><span data-stu-id="03148-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="03148-177">バインドを設定する**Task Runner Explorer**の上部にあるコメントの形式で格納されます、 *gulpfile.js*と Visual Studio でのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="03148-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="03148-178">Visual Studio を必要としない代わりに、gulp タスクでの自動実行を構成し、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03148-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="03148-179">たとえば、これに含める、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="03148-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="03148-180">またはを使用してコマンド プロンプトから Visual Studio でプロジェクトを実行するときに、clean タスクが実行されるので、 [「dotnet run」](/dotnet/core/tools/dotnet-run)コマンド (実行`npm install`最初).</span><span class="sxs-lookup"><span data-stu-id="03148-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="03148-181">定義して、新しいタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="03148-181">Defining and running a new task</span></span>

<span data-ttu-id="03148-182">新しい Gulp タスクを定義する次のように変更します。 *gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1. <span data-ttu-id="03148-183">末尾に次の JavaScript を追加*gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="03148-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="03148-184">このタスクの名前は`first`、単に文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="03148-184">This task is named `first`, and it simply displays a string.</span></span>

2. <span data-ttu-id="03148-185">保存*gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-185">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="03148-186">**ソリューション エクスプ ローラー**を右クリックして*gulpfile.js*、選び*Task Runner Explorer*します。</span><span class="sxs-lookup"><span data-stu-id="03148-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4. <span data-ttu-id="03148-187">**Task Runner Explorer**、右クリックして**最初**を選択し、**実行**します。</span><span class="sxs-lookup"><span data-stu-id="03148-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![最初のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="03148-189">出力テキストが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03148-189">The output text is displayed.</span></span> <span data-ttu-id="03148-190">一般的なシナリオに基づく例については、[Gulp レシピ](#gulp-recipes)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="03148-191">定義して、一連のタスクを実行しています。</span><span class="sxs-lookup"><span data-stu-id="03148-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="03148-192">複数のタスクを実行するときに既定では、タスクが同時に実行します。</span><span class="sxs-lookup"><span data-stu-id="03148-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="03148-193">ただし、特定の順序でタスクを実行する場合は、する必要があります指定する各タスクが完了するもタスクとして別のタスクの完了に依存します。</span><span class="sxs-lookup"><span data-stu-id="03148-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1. <span data-ttu-id="03148-194">一連の順序で実行するタスクを定義するには、置換、`first`上で追加したタスク*gulpfile.js*次。</span><span class="sxs-lookup"><span data-stu-id="03148-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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

    <span data-ttu-id="03148-195">3 つのタスクがあるようになりました: `series:first`、 `series:second`、および`series`します。</span><span class="sxs-lookup"><span data-stu-id="03148-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="03148-196">`series:second`タスクには実行する前に完了したタスクの配列を指定する 2 番目のパラメーターが含まれています、`series:second`タスクが実行されます。</span><span class="sxs-lookup"><span data-stu-id="03148-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="03148-197">上だけのコードで指定されている、`series:first`する前にタスクを完了する必要があります、`series:second`タスクが実行されます。</span><span class="sxs-lookup"><span data-stu-id="03148-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2. <span data-ttu-id="03148-198">保存*gulpfile.js*します。</span><span class="sxs-lookup"><span data-stu-id="03148-198">Save *gulpfile.js*.</span></span>

3. <span data-ttu-id="03148-199">**ソリューション エクスプ ローラー**、右クリック*gulpfile.js*選択**Task Runner Explorer**まだ開いていない場合。</span><span class="sxs-lookup"><span data-stu-id="03148-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4. <span data-ttu-id="03148-200">**Task Runner Explorer**を右クリックして**シリーズ**選択**実行**します。</span><span class="sxs-lookup"><span data-stu-id="03148-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![一連のタスクを実行するタスク ランナー エクスプ ローラー](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="03148-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="03148-202">IntelliSense</span></span>

<span data-ttu-id="03148-203">IntelliSense は、コード補完、パラメーターの説明、および生産性を向上させると、エラーを減らすには、その他の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="03148-204">Gulp タスクは、JavaScript; で記述されました。そのため、IntelliSense では、開発中に、アシスタンスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="03148-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="03148-205">JavaScript を使用して IntelliSense は、オブジェクト、関数、プロパティを一覧表示されます。 現在のコンテキストに基づいて使用可能なパラメーター。</span><span class="sxs-lookup"><span data-stu-id="03148-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="03148-206">コードを完成する IntelliSense によって提供されるポップアップ リストから、コーディングのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="03148-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp の IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="03148-208">IntelliSense の詳細については、[JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="03148-209">開発、ステージング、実稼働環境</span><span class="sxs-lookup"><span data-stu-id="03148-209">Development, staging, and production environments</span></span>

<span data-ttu-id="03148-210">Gulp を使用して、クライアント側ファイルのステージングと運用環境を最適化する、処理されたファイルは、ローカルのステージングと運用環境の場所に保存されます。</span><span class="sxs-lookup"><span data-stu-id="03148-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="03148-211">*_Layout.cshtml*ファイルの使用、**環境**タグ ヘルパーの CSS ファイルの 2 つの異なるバージョンを提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="03148-212">CSS ファイルの 1 つのバージョンは、開発とステージング環境と運用環境の両方のもう一方のバージョンは最適化されています。</span><span class="sxs-lookup"><span data-stu-id="03148-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="03148-213">変更すると、Visual Studio 2017 で、 **ASPNETCORE_ENVIRONMENT**環境変数を`Production`、Visual Studio は、Web アプリと、最小化された CSS ファイルへのリンクは作成します。</span><span class="sxs-lookup"><span data-stu-id="03148-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="03148-214">次のマークアップに示す、**環境**タグ ヘルパーをリンク タグを含む、 `Development` CSS ファイルと、縮小された`Staging, Production`CSS ファイル。</span><span class="sxs-lookup"><span data-stu-id="03148-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="03148-215">環境の切り替え</span><span class="sxs-lookup"><span data-stu-id="03148-215">Switching between environments</span></span>

<span data-ttu-id="03148-216">異なる環境でのコンパイルの間には、変更、 **ASPNETCORE_ENVIRONMENT**環境変数の値。</span><span class="sxs-lookup"><span data-stu-id="03148-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1. <span data-ttu-id="03148-217">**Task Runner Explorer**、いることを確認、 **min**を実行するタスクが設定されている**Before Build**。</span><span class="sxs-lookup"><span data-stu-id="03148-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2. <span data-ttu-id="03148-218">**ソリューション エクスプ ローラー**でプロジェクト名を右クリックし、選択**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="03148-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="03148-219">Web アプリのプロパティ シートが表示されます。</span><span class="sxs-lookup"><span data-stu-id="03148-219">The property sheet for the Web app is displayed.</span></span>

3. <span data-ttu-id="03148-220">**[デバッグ]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="03148-220">Click the **Debug** tab.</span></span>

4. <span data-ttu-id="03148-221">値を設定、**ホスティング環境:** 環境変数を`Production`します。</span><span class="sxs-lookup"><span data-stu-id="03148-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5. <span data-ttu-id="03148-222">キーを押して**F5**ブラウザーでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="03148-222">Press **F5** to run the application in a browser.</span></span>

6. <span data-ttu-id="03148-223">ブラウザーのウィンドウでは、ページを右クリックして**ソースの表示**ページの HTML を表示します。</span><span class="sxs-lookup"><span data-stu-id="03148-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="03148-224">スタイル シートのリンクが縮小された CSS ファイルを指していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="03148-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7. <span data-ttu-id="03148-225">Web アプリを停止するブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="03148-225">Close the browser to stop the Web app.</span></span>

8. <span data-ttu-id="03148-226">Visual Studio で Web アプリのプロパティ シートに戻るし、変更、**ホスティング環境:** 環境変数にバックアップ`Development`します。</span><span class="sxs-lookup"><span data-stu-id="03148-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9. <span data-ttu-id="03148-227">キーを押して**F5**ブラウザーでアプリケーションをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="03148-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="03148-228">ブラウザーのウィンドウでは、ページを右クリックして**ソースの表示**に HTML ページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="03148-229">スタイル シートのリンクが指す unminified バージョンの CSS ファイルに注意してください。</span><span class="sxs-lookup"><span data-stu-id="03148-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="03148-230">ASP.NET Core での環境に関連する詳細については、[複数の環境を使用して、](../fundamentals/environments.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="03148-231">タスクとモジュールの詳細</span><span class="sxs-lookup"><span data-stu-id="03148-231">Task and module details</span></span>

<span data-ttu-id="03148-232">Gulp タスクは、関数名で登録されます。</span><span class="sxs-lookup"><span data-stu-id="03148-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="03148-233">現在のタスクの前にその他のタスクを実行する必要がある場合は、依存関係を指定できます。</span><span class="sxs-lookup"><span data-stu-id="03148-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="03148-234">追加の関数を実行し、Gulp タスクを見るだけでなくソースを設定することができます (*src*) と移行先 (*dest*) ファイルが変更されるのです。</span><span class="sxs-lookup"><span data-stu-id="03148-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="03148-235">次に、プライマリの Gulp の API 関数を示します。</span><span class="sxs-lookup"><span data-stu-id="03148-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="03148-236">Gulp 関数</span><span class="sxs-lookup"><span data-stu-id="03148-236">Gulp Function</span></span>|<span data-ttu-id="03148-237">構文</span><span class="sxs-lookup"><span data-stu-id="03148-237">Syntax</span></span>|<span data-ttu-id="03148-238">説明</span><span class="sxs-lookup"><span data-stu-id="03148-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="03148-239">タスク</span><span class="sxs-lookup"><span data-stu-id="03148-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="03148-240">`task`関数は、タスクを作成します。</span><span class="sxs-lookup"><span data-stu-id="03148-240">The `task` function creates a task.</span></span> <span data-ttu-id="03148-241">`name`パラメーター タスクの名前を定義します。</span><span class="sxs-lookup"><span data-stu-id="03148-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="03148-242">`deps`パラメーターには、このタスクを実行する前に完了するタスクの配列が含まれています。</span><span class="sxs-lookup"><span data-stu-id="03148-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="03148-243">`fn`パラメーターは、タスクの操作を実行するコールバック関数を表します。</span><span class="sxs-lookup"><span data-stu-id="03148-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="03148-244">ウォッチ</span><span class="sxs-lookup"><span data-stu-id="03148-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="03148-245">`watch`関数は、ファイルの変更が発生した場合にファイルと実行のタスクを監視します。</span><span class="sxs-lookup"><span data-stu-id="03148-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="03148-246">`glob`パラメーターは、`string`または`array`ウォッチするファイルを決定します。</span><span class="sxs-lookup"><span data-stu-id="03148-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="03148-247">`opts`パラメーター オプションを見ているその他のファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="03148-248">src</span><span class="sxs-lookup"><span data-stu-id="03148-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="03148-249">`src`関数 glob 値に一致するファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="03148-250">`glob`パラメーターは、`string`または`array`ファイルを読み取るかを決定します。</span><span class="sxs-lookup"><span data-stu-id="03148-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="03148-251">`options`パラメーターが追加のファイル オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="03148-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="03148-252">追加先</span><span class="sxs-lookup"><span data-stu-id="03148-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="03148-253">`dest`関数は、ファイルの書き込み先の場所を定義します。</span><span class="sxs-lookup"><span data-stu-id="03148-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="03148-254">`path`パラメーターは、文字列またはコピー先のフォルダーを決定する関数。</span><span class="sxs-lookup"><span data-stu-id="03148-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="03148-255">`options`パラメーターが出力フォルダーのオプションを指定するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="03148-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="03148-256">Gulp の API リファレンスの詳細は、[Gulp Docs API](https://gulpjs.org/API.html)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="03148-256">For additional Gulp API reference information, see [Gulp Docs API](https://gulpjs.org/API.html).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="03148-257">Gulp のレシピ</span><span class="sxs-lookup"><span data-stu-id="03148-257">Gulp recipes</span></span>

<span data-ttu-id="03148-258">Gulp コミュニティ提供の Gulp[レシピ](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md)します。</span><span class="sxs-lookup"><span data-stu-id="03148-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="03148-259">これらのレシピは、一般的なシナリオに対処する Gulp タスクで構成されます。</span><span class="sxs-lookup"><span data-stu-id="03148-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="03148-260">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="03148-260">Additional resources</span></span>

* [<span data-ttu-id="03148-261">Gulp のドキュメント</span><span class="sxs-lookup"><span data-stu-id="03148-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="03148-262">バンドルと ASP.NET Core での縮小</span><span class="sxs-lookup"><span data-stu-id="03148-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="03148-263">ASP.NET Core で Grunt を使用します。</span><span class="sxs-lookup"><span data-stu-id="03148-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
