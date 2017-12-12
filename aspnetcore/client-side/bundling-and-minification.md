---
title: "バンドルと ASP.NET Core の縮小"
author: scottaddie
description: "ASP.NET Core web アプリケーションで静的なリソースをバンドルと縮小の手法を適用することによって最適化する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/01/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: c271b7ef386bacedbd45fbe9f62c9c486db55b36
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="bundling-and-minification"></a>バンドルと縮小

作成者: [Scott Addie](https://twitter.com/Scott_Addie)

この記事では、バンドルと縮小、ASP.NET Core web アプリを使用してこれらの機能を使用する方法などを適用することの利点について説明します。

## <a name="what-is-bundling-and-minification"></a>バンドルと縮小とは

バンドルと縮小は、web アプリに適用できます 2 つの個別のパフォーマンスの最適化です。 一緒に使用すると、バンドルと縮小パフォーマンス向上サーバー要求の数を減らすと、要求された静的な資産のサイズを小さきます。

バンドルと縮小、主に、最初のページ要求の読み込み時間を向上します。 Web ページが要求されたら、ブラウザーは、(JavaScript、CSS、およびイメージ) の静的なアセットをキャッシュします。 その結果、バンドルと縮小しないパフォーマンスを向上させる、同じページで、または同じアセットを要求している同じサイト上のページを要求するときにします。 設定しない場合、ヘッダー、資産を正しく有効期限が切れるし、バンドルと縮小を使用しない場合、ブラウザーの鮮度ヒューリスティック マーク資産古い数日後にします。 さらに、ブラウザーでは、各資産の検証要求が必要です。 この例では、バンドルと縮小は、最初のページ要求した後でもパフォーマンスの向上を提供します。

### <a name="bundling"></a>バンドル化

複数のファイルを 1 つのファイルに連結するバンドルします。 Web ページなどの web アセットを表示するために必要なサーバー要求の数を削減するバンドルします。 CSS、JavaScript などの具体的には、任意の数の個別のバンドルを作成できます。ファイル数を減らしてでは、サーバーにブラウザーまたはアプリケーションを提供するサービスから少数の HTTP 要求を意味します。 この結果では、最初のページ読み込みのパフォーマンスを向上します。

### <a name="minification"></a>縮小

縮小は、機能を変更することがなく、コードから不要な文字を削除します。 要求されたアセット (CSS、画像、および JavaScript ファイル) などの大きなサイズの縮小になります。 縮小の一般的な副作用には、1 文字に変数名を短くし、コメントと不要な空白文字を削除するが含まれます。

次の JavaScript 関数を考慮してください。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

縮小では、次に、関数が軽減されます。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

だけでなく、コメントと不要な空白文字を削除するには、次のパラメーターおよび変数名が次のように変更されました。

元 | 名前の変更
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>バンドルと縮小の影響

次の表は、個別に資産の読み込みとバンドルと縮小を使用して違いを示します。

操作 | B/M と | B/M なし | 変更
--- | :---: | :---: | :---:
ファイルの要求  | 7   | 18     | 157%
サポート技術情報の転送 | 156 | 264.68 | 70%
読み込み時間 (ミリ秒) | 885 | 2360   | 167%

ブラウザーは HTTP 要求ヘッダーに関して非常に詳細です。 合計バイト数は送信バンドルときに、メトリックが大幅に軽減を説明しました。 ただし、この例をローカルに実行、読み込み時間は大幅に向上を示します。 アセットのバンドルと縮小を使用して、ネットワーク経由で転送されるときに、パフォーマンス向上が実現されます。

## <a name="choose-a-bundling-and-minification-strategy"></a>バンドルと縮小戦略を選択します。

MVC および Razor ページのプロジェクト テンプレートは、バンドルと縮小 JSON 構成ファイルで構成されるため、ボックスのソリューションを提供します。 などのサード パーティ製ツール、 [Gulp](xref:client-side/using-gulp)と[Grunt](xref:client-side/using-grunt)ランナーをタスクで、もう少し複雑に同じタスクを実行します。 開発ワークフローにはバンドルと縮小以外の処理が必要な場合、サード パーティ製のツールは非常によく適合&mdash;linting と画像の最適化などです。 デザイン時のバンドルと縮小を使用すると、縮小されたファイルは、アプリの展開する前に作成されます。 バンドル化と縮小展開する前に、サーバー負荷の軽減の利点を提供します。 ただし、そのデザイン時のバンドルを認識することが重要と縮小ビルド複雑さが増加し、静的ファイルでのみ動作します。

## <a name="configure-bundling-and-minification"></a>バンドルと縮小を構成します。

MVC および Razor ページのプロジェクト テンプレートは、提供、 *bundleconfig.json*構成ファイルの各バンドルのオプションを定義します。 カスタムの JavaScript の既定では、1 つのバンドルの構成が定義されている (*wwwroot/js/site.js*) とスタイル シート (*wwwroot/css/site.css*) ファイル。

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

バンドルのオプションは次のとおりです。

* `outputFileName`: を出力するバンドル ファイルの名前。 相対パスを含めることができます、 *bundleconfig.json*ファイル。 **必須**
* `inputFiles`: をまとめるためにファイルの配列。 これらは、構成ファイルへの相対パスです。 **省略可能な**、*、空の値が空の出力ファイルの結果します。 [グロブ](http://www.tldp.org/LDP/abs/html/globbingref.html)パターンがサポートされています。
* `minify`出力の: サイズ縮小オプションを入力します。 **省略可能な**、*既定 -`minify: { enabled: true }`*
  * 出力ファイルの種類ごとの構成オプションのとおりです。
    * [CSS の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: プロジェクト ファイルに生成されたファイルを追加するかどうかを示すフラグです。 **省略可能な**、*既定 - false*
* `sourceMap`:、バンドルしたファイルのソース マップを生成するかどうかを示すフラグ。 **省略可能な**、*既定 - false*
* `sourceMapRootPath`: 生成されたソース マップ ファイルを保存するルート パス。

## <a name="build-time-execution-of-bundling-and-minification"></a>バンドルと縮小のビルド時の実行

[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet パッケージにより、バンドルの実行とビルド時に縮小します。 パッケージでは挿入[MSBuild ターゲット](/visualstudio/msbuild/msbuild-targets)ビルドおよびクリーン時に実行します。 *Bundleconfig.json*定義した構成に基づいて、出力ファイルを生成するために、ビルド プロセスによってファイルが分析されます。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

追加、 *BuildBundlerMinifier*をプロジェクトにパッケージします。

プロジェクトをビルドします。 次の出力ウィンドウに表示されます。

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

プロジェクトをクリーンアップします。 次の出力ウィンドウに表示されます。

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli) 

追加、 *BuildBundlerMinifier*をプロジェクトにパッケージ。

```console
dotnet add package BuildBundlerMinifier
```

ASP.NET を使用して場合 1.x のコア、新しく追加されたパッケージを復元します。

```console
dotnet restore
```

プロジェクトをビルドします。

```console
dotnet build
```

次に表示されます。

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

プロジェクトをクリーンアップするには。

```console
dotnet clean
```

次の出力が表示されます。

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>バンドルと縮小のアドホック実行

プロジェクトをビルドせず、アドホックごとに、バンドルと縮小のタスクを実行することができます。 追加、 [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet パッケージをプロジェクトに。

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

このパッケージに含める .NET Core CLI を拡張し、 *dotnet バンドル*ツールです。 パッケージ マネージャー コンソール (PMC) ウィンドウまたはコマンド シェルで、次のコマンドを実行できます。

```console
dotnet bundle
```

> [!IMPORTANT]
> NuGet Package Manager の依存関係ファイルに追加 *.csproj として`<PackageReference />`ノード。 `dotnet bundle`コマンドが、.NET Core の CLI に登録されている場合にのみ、`<DotNetCliToolReference />`ノードを使用します。 *.Csproj ファイルを適宜変更します。

## <a name="add-files-to-workflow"></a>ワークフローにファイルを追加します。

例を考えてみます追加*custom.css*ファイルは、次のような追加。

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

縮小する*custom.css*とバンドルおよび*site.css*に、 *site.min.css*ファイルに追加しへの相対パス*bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> また、次のグロブ パターンを使用できます。
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> このグロブ パターンでは、すべての CSS ファイルと一致して、縮小されたファイルのパターンを除外します。

アプリケーションをビルドします。 開いている*site.min.css*の内容を確認および*custom.css*は、ファイルの末尾に追加されます。

## <a name="environment-based-bundling-and-minification"></a>環境に基づくのバンドルと縮小

ベスト プラクティスとしては、実稼働環境でアプリのバンドルと縮小されたファイルを使用してください。 開発中は、元のファイルをアプリのデバッグを容易に行います。

使用して、ページに含めるファイルを指定、[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)ビューにします。 環境タグ ヘルパーは、固有の仕様で実行されている場合にのみ、コンテンツをレンダリング[環境](xref:fundamentals/environments)です。

次`environment`で実行する場合、タグが未処理の CSS ファイルを表示、`Development`環境。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

次`environment`以外の環境で実行する場合、タグは、バンドルと縮小された CSS ファイルをレンダリング`Development`です。 たとえばで実行されている`Production`または`Staging`これらのスタイル シートのレンダリングのトリガーします。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Gulp から bundleconfig.json を使用します。

アプリのバンドルと縮小のワークフローに追加の処理が必要な場合があります。 例として、画像の最適化、キャッシュが破壊 CDN 資産処理します。 これらの要件を満たすには、Gulp を使用するバンドルと縮小のワークフローに変換できます。

### <a name="use-the-bundler--minifier-extension"></a>バンドルと縮小化拡張機能を使用します。

Visual Studio[バンドルと縮小化](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)拡張機能が Gulp への変換を処理します。

右クリックし、 *bundleconfig.json*ソリューション エクスプ ローラーでファイルおよび選択した**バンドルと縮小化** > **Gulp を変換しています.**:

![コンテキスト メニュー項目の変換の Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

*Gulpfile.js*と*package.json*ファイルは、プロジェクトに追加します。 サポートしている[npm](https://www.npmjs.com/)で表示されているパッケージ、 *package.json*ファイルの`devDependencies`セクションがインストールされます。

グローバルの依存関係として Gulp CLI をインストールする [PMC] ウィンドウで、次のコマンドを実行します。

```console
npm i -g gulp-cli
```

*Gulpfile.js*ファイルの読み取り、 *bundleconfig.json*ファイルを入力、出力、および設定します。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>手動で変換します。

Visual Studio またはバンドルと縮小化拡張機能が使用できない場合は手動で変換します。

追加、 *package.json*を次のファイル`devDependencies`プロジェクトのルートに。

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

同じレベルで、次のコマンドを実行して、依存関係をインストール*package.json*:

```console
npm i
```

グローバルの依存関係として Gulp CLI をインストールします。

```console
npm i -g gulp-cli
```

コピー、 *gulpfile.js*ファイルの名前をプロジェクト ルート下。

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Gulp タスクの実行

Visual Studio でプロジェクトをビルドする前に、Gulp 縮小タスクをトリガーするには、次のコードを追加[MSBuild ターゲット](/visualstudio/msbuild/msbuild-targets)*.csproj ファイル。

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

この例では、すべてのタスクが内で定義、`MyPreCompileTarget`ターゲットの前に、定義済み実行`Build`ターゲットです。 Visual Studio の出力 ウィンドウで、次のような出力が表示されます。

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

代わりに、Visual Studio のタスク ランナー エクスプ ローラーを使用して、Visual Studio の特定のイベントに Gulp タスクをバインドする可能性があります。 参照してください[既定のタスクを実行している](xref:client-side/using-gulp#running-default-tasks)手順については、その作業します。

## <a name="additional-resources"></a>その他の技術情報

* [Gulp の使用](xref:client-side/using-gulp)
* [Grunt の使用](xref:client-side/using-grunt)
* [複数の環境の使用](xref:fundamentals/environments)
* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
