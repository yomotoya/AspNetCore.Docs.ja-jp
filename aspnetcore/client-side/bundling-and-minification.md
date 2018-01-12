---
title: "バンドルと ASP.NET Core の縮小"
author: scottaddie
description: "ASP.NET Core web アプリケーションで静的なリソースをバンドルと縮小の手法を適用することによって最適化する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bundling-and-minification
ms.openlocfilehash: ac8e7fee7600dabb8f4970b5bf87ad7a57ebf17f
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2018
---
# <a name="bundling-and-minification"></a><span data-ttu-id="0ccd3-103">バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="0ccd3-103">Bundling and minification</span></span>

<span data-ttu-id="0ccd3-104">作成者: [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="0ccd3-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="0ccd3-105">この記事では、バンドルと縮小、ASP.NET Core web アプリを使用してこれらの機能を使用する方法などを適用することの利点について説明します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-105">This article explains the benefits of applying bundling and minification, including how these features can be used with ASP.NET Core web apps.</span></span>

## <a name="what-is-bundling-and-minification"></a><span data-ttu-id="0ccd3-106">バンドルと縮小とは</span><span class="sxs-lookup"><span data-stu-id="0ccd3-106">What is bundling and minification?</span></span>

<span data-ttu-id="0ccd3-107">バンドルと縮小は、web アプリに適用できます 2 つの個別のパフォーマンスの最適化です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-107">Bundling and minification are two distinct performance optimizations you can apply in a web app.</span></span> <span data-ttu-id="0ccd3-108">一緒に使用すると、バンドルと縮小パフォーマンス向上サーバー要求の数を減らすと、要求された静的な資産のサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-108">Used together, bundling and minification improve performance by reducing the number of server requests and reducing the size of the requested static assets.</span></span>

<span data-ttu-id="0ccd3-109">バンドルと縮小、主に、最初のページ要求の読み込み時間を向上します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-109">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="0ccd3-110">Web ページが要求されたら、ブラウザーは、(JavaScript、CSS、およびイメージ) の静的なアセットをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-110">Once a web page has been requested, the browser caches the static assets (JavaScript, CSS, and images).</span></span> <span data-ttu-id="0ccd3-111">その結果、バンドルと縮小しないパフォーマンスを向上させる、同じページで、または同じアセットを要求している同じサイト上のページを要求するときにします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-111">Consequently, bundling and minification don't improve performance when requesting the same page, or pages, on the same site requesting the same assets.</span></span> <span data-ttu-id="0ccd3-112">設定しない場合、ヘッダー、資産を正しく有効期限が切れるし、バンドルと縮小を使用しない場合、ブラウザーの鮮度ヒューリスティック マーク資産古い数日後にします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-112">If you don't set the expires header correctly on your assets, and if you don’t use bundling and minification, the browser's freshness heuristics mark the assets stale after a few days.</span></span> <span data-ttu-id="0ccd3-113">さらに、ブラウザーでは、各資産の検証要求が必要です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-113">Additionally, the browser requires a validation request for each asset.</span></span> <span data-ttu-id="0ccd3-114">この例では、バンドルと縮小は、最初のページ要求した後でもパフォーマンスの向上を提供します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-114">In this case, bundling and minification provide a performance improvement even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="0ccd3-115">バンドル化</span><span class="sxs-lookup"><span data-stu-id="0ccd3-115">Bundling</span></span>

<span data-ttu-id="0ccd3-116">複数のファイルを 1 つのファイルに連結するバンドルします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-116">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="0ccd3-117">Web ページなどの web アセットを表示するために必要なサーバー要求の数を削減するバンドルします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-117">Bundling reduces the number of server requests which are necessary to render a web asset, such as a web page.</span></span> <span data-ttu-id="0ccd3-118">CSS、JavaScript などの具体的には、任意の数の個別のバンドルを作成できます。ファイル数を減らしてでは、サーバーにブラウザーまたはアプリケーションを提供するサービスから少数の HTTP 要求を意味します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-118">You can create any number of individual bundles specifically for CSS, JavaScript, etc. Fewer files means fewer HTTP requests from the browser to the server or from the service providing your application.</span></span> <span data-ttu-id="0ccd3-119">この結果では、最初のページ読み込みのパフォーマンスを向上します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-119">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="0ccd3-120">縮小</span><span class="sxs-lookup"><span data-stu-id="0ccd3-120">Minification</span></span>

<span data-ttu-id="0ccd3-121">縮小は、機能を変更することがなく、コードから不要な文字を削除します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-121">Minification removes unnecessary characters from code without altering functionality.</span></span> <span data-ttu-id="0ccd3-122">要求されたアセット (CSS、画像、および JavaScript ファイル) などの大きなサイズの縮小になります。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-122">The result is a significant size reduction in requested assets (such as CSS, images, and JavaScript files).</span></span> <span data-ttu-id="0ccd3-123">縮小の一般的な副作用には、1 文字に変数名を短くし、コメントと不要な空白文字を削除するが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-123">Common side effects of minification include shortening variable names to one character and removing comments and unnecessary whitespace.</span></span>

<span data-ttu-id="0ccd3-124">次の JavaScript 関数を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-124">Consider the following JavaScript function:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

<span data-ttu-id="0ccd3-125">縮小では、次に、関数が軽減されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-125">Minification reduces the function to the following:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

<span data-ttu-id="0ccd3-126">だけでなく、コメントと不要な空白文字を削除するには、次のパラメーターおよび変数名が次のように変更されました。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-126">In addition to removing the comments and unnecessary whitespace, the following parameter and variable names were renamed as follows:</span></span>

<span data-ttu-id="0ccd3-127">元</span><span class="sxs-lookup"><span data-stu-id="0ccd3-127">Original</span></span> | <span data-ttu-id="0ccd3-128">名前の変更</span><span class="sxs-lookup"><span data-stu-id="0ccd3-128">Renamed</span></span>
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="0ccd3-129">バンドルと縮小の影響</span><span class="sxs-lookup"><span data-stu-id="0ccd3-129">Impact of bundling and minification</span></span>

<span data-ttu-id="0ccd3-130">次の表は、個別に資産の読み込みとバンドルと縮小を使用して違いを示します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-130">The following table outlines differences between individually loading assets and using bundling and minification:</span></span>

<span data-ttu-id="0ccd3-131">アクション</span><span class="sxs-lookup"><span data-stu-id="0ccd3-131">Action</span></span> | <span data-ttu-id="0ccd3-132">B/M と</span><span class="sxs-lookup"><span data-stu-id="0ccd3-132">With B/M</span></span> | <span data-ttu-id="0ccd3-133">B/M なし</span><span class="sxs-lookup"><span data-stu-id="0ccd3-133">Without B/M</span></span> | <span data-ttu-id="0ccd3-134">変更</span><span class="sxs-lookup"><span data-stu-id="0ccd3-134">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="0ccd3-135">ファイルの要求</span><span class="sxs-lookup"><span data-stu-id="0ccd3-135">File Requests</span></span>  | <span data-ttu-id="0ccd3-136">7</span><span class="sxs-lookup"><span data-stu-id="0ccd3-136">7</span></span>   | <span data-ttu-id="0ccd3-137">18</span><span class="sxs-lookup"><span data-stu-id="0ccd3-137">18</span></span>     | <span data-ttu-id="0ccd3-138">157%</span><span class="sxs-lookup"><span data-stu-id="0ccd3-138">157%</span></span>
<span data-ttu-id="0ccd3-139">サポート技術情報の転送</span><span class="sxs-lookup"><span data-stu-id="0ccd3-139">KB Transferred</span></span> | <span data-ttu-id="0ccd3-140">156</span><span class="sxs-lookup"><span data-stu-id="0ccd3-140">156</span></span> | <span data-ttu-id="0ccd3-141">264.68</span><span class="sxs-lookup"><span data-stu-id="0ccd3-141">264.68</span></span> | <span data-ttu-id="0ccd3-142">70%</span><span class="sxs-lookup"><span data-stu-id="0ccd3-142">70%</span></span>
<span data-ttu-id="0ccd3-143">読み込み時間 (ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="0ccd3-143">Load Time (ms)</span></span> | <span data-ttu-id="0ccd3-144">885</span><span class="sxs-lookup"><span data-stu-id="0ccd3-144">885</span></span> | <span data-ttu-id="0ccd3-145">2360</span><span class="sxs-lookup"><span data-stu-id="0ccd3-145">2360</span></span>   | <span data-ttu-id="0ccd3-146">167%</span><span class="sxs-lookup"><span data-stu-id="0ccd3-146">167%</span></span>

<span data-ttu-id="0ccd3-147">ブラウザーは HTTP 要求ヘッダーに関して非常に詳細です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-147">Browsers are fairly verbose with regard to HTTP request headers.</span></span> <span data-ttu-id="0ccd3-148">合計バイト数は送信バンドルときに、メトリックが大幅に軽減を説明しました。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-148">The total bytes sent metric saw a significant reduction when bundling.</span></span> <span data-ttu-id="0ccd3-149">ただし、この例をローカルに実行、読み込み時間は大幅に向上を示します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-149">The load time shows a significant improvement, however this example ran locally.</span></span> <span data-ttu-id="0ccd3-150">アセットのバンドルと縮小を使用して、ネットワーク経由で転送されるときに、パフォーマンス向上が実現されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-150">Greater performance gains are realized when using bundling and minification with assets transferred over a network.</span></span>

## <a name="choose-a-bundling-and-minification-strategy"></a><span data-ttu-id="0ccd3-151">バンドルと縮小戦略を選択します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-151">Choose a bundling and minification strategy</span></span>

<span data-ttu-id="0ccd3-152">MVC および Razor ページのプロジェクト テンプレートは、バンドルと縮小 JSON 構成ファイルで構成されるため、ボックスのソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-152">The MVC and Razor Pages project templates provide an out-of-the-box solution for bundling and minification consisting of a JSON configuration file.</span></span> <span data-ttu-id="0ccd3-153">などのサード パーティ製ツール、 [Gulp](xref:client-side/using-gulp)と[Grunt](xref:client-side/using-grunt)ランナーをタスクで、もう少し複雑に同じタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-153">Third-party tools, such as the [Gulp](xref:client-side/using-gulp) and [Grunt](xref:client-side/using-grunt) task runners, accomplish the same tasks with a bit more complexity.</span></span> <span data-ttu-id="0ccd3-154">開発ワークフローにはバンドルと縮小以外の処理が必要な場合、サード パーティ製のツールは非常によく適合&mdash;linting と画像の最適化などです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-154">A third-party tool is a great fit when your development workflow requires processing beyond bundling and minification&mdash;such as linting and image optimization.</span></span> <span data-ttu-id="0ccd3-155">デザイン時のバンドルと縮小を使用すると、縮小されたファイルは、アプリの展開する前に作成されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-155">By using design-time bundling and minification, the minified files are created prior to the app's deployment.</span></span> <span data-ttu-id="0ccd3-156">バンドル化と縮小展開する前に、サーバー負荷の軽減の利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-156">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="0ccd3-157">ただし、そのデザイン時のバンドルを認識することが重要と縮小ビルド複雑さが増加し、静的ファイルでのみ動作します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-157">However, it's important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="0ccd3-158">バンドルと縮小を構成します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-158">Configure bundling and minification</span></span>

<span data-ttu-id="0ccd3-159">MVC および Razor ページのプロジェクト テンプレートは、提供、 *bundleconfig.json*構成ファイルの各バンドルのオプションを定義します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-159">The MVC and Razor Pages project templates provide a *bundleconfig.json* configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="0ccd3-160">カスタムの JavaScript の既定では、1 つのバンドルの構成が定義されている (*wwwroot/js/site.js*) とスタイル シート (*wwwroot/css/site.css*) ファイル。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-160">By default, a single bundle configuration is defined for the custom JavaScript (*wwwroot/js/site.js*) and stylesheet (*wwwroot/css/site.css*) files:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

<span data-ttu-id="0ccd3-161">構成オプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-161">Configuration options include:</span></span>

* <span data-ttu-id="0ccd3-162">`outputFileName`: を出力するバンドル ファイルの名前。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-162">`outputFileName`: The name of the bundle file to output.</span></span> <span data-ttu-id="0ccd3-163">相対パスを含めることができます、 *bundleconfig.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-163">Can contain a relative path from the *bundleconfig.json* file.</span></span> <span data-ttu-id="0ccd3-164">**必須**</span><span class="sxs-lookup"><span data-stu-id="0ccd3-164">**required**</span></span>
* <span data-ttu-id="0ccd3-165">`inputFiles`: をまとめるためにファイルの配列。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-165">`inputFiles`: An array of files to bundle together.</span></span> <span data-ttu-id="0ccd3-166">これらは、構成ファイルへの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-166">These are relative paths to the configuration file.</span></span> <span data-ttu-id="0ccd3-167">**省略可能な**、\*、空の値が空の出力ファイルの結果します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-167">**optional**, \*an empty value results in an empty output file.</span></span> <span data-ttu-id="0ccd3-168">[グロブ](http://www.tldp.org/LDP/abs/html/globbingref.html)パターンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-168">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="0ccd3-169">`minify`出力の: サイズ縮小オプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-169">`minify`: The minification options for the output type.</span></span> <span data-ttu-id="0ccd3-170">**省略可能な**、*既定 -`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="0ccd3-170">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="0ccd3-171">出力ファイルの種類ごとの構成オプションのとおりです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-171">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="0ccd3-172">CSS の縮小化</span><span class="sxs-lookup"><span data-stu-id="0ccd3-172">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="0ccd3-173">JavaScript の縮小化</span><span class="sxs-lookup"><span data-stu-id="0ccd3-173">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [<span data-ttu-id="0ccd3-174">HTML の縮小化</span><span class="sxs-lookup"><span data-stu-id="0ccd3-174">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki)
* <span data-ttu-id="0ccd3-175">`includeInProject`: プロジェクト ファイルに生成されたファイルを追加するかどうかを示すフラグです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-175">`includeInProject`: Flag indicating whether to add generated files to project file.</span></span> <span data-ttu-id="0ccd3-176">**省略可能な**、*既定 - false*</span><span class="sxs-lookup"><span data-stu-id="0ccd3-176">**optional**, *default - false*</span></span>
* <span data-ttu-id="0ccd3-177">`sourceMap`:、バンドルしたファイルのソース マップを生成するかどうかを示すフラグ。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-177">`sourceMap`: Flag indicating whether to generate a source map for the bundled file.</span></span> <span data-ttu-id="0ccd3-178">**省略可能な**、*既定 - false*</span><span class="sxs-lookup"><span data-stu-id="0ccd3-178">**optional**, *default - false*</span></span>
* <span data-ttu-id="0ccd3-179">`sourceMapRootPath`: 生成されたソース マップ ファイルを保存するルート パス。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-179">`sourceMapRootPath`: The root path for storing the generated source map file.</span></span>

## <a name="build-time-execution-of-bundling-and-minification"></a><span data-ttu-id="0ccd3-180">バンドルと縮小のビルド時の実行</span><span class="sxs-lookup"><span data-stu-id="0ccd3-180">Build-time execution of bundling and minification</span></span>

<span data-ttu-id="0ccd3-181">[BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet パッケージにより、バンドルの実行とビルド時に縮小します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-181">The [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) NuGet package enables the execution of bundling and minification at build time.</span></span> <span data-ttu-id="0ccd3-182">パッケージでは挿入[MSBuild ターゲット](/visualstudio/msbuild/msbuild-targets)ビルドおよびクリーン時に実行します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-182">The package injects [MSBuild Targets](/visualstudio/msbuild/msbuild-targets) which run at build and clean time.</span></span> <span data-ttu-id="0ccd3-183">*Bundleconfig.json*定義した構成に基づいて、出力ファイルを生成するために、ビルド プロセスによってファイルが分析されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-183">The *bundleconfig.json* file is analyzed by the build process to produce the output files based on the defined configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="0ccd3-184">BuildBundlerMinifier は、Microsoft の提供先のサポート GitHub 上のコミュニティによって運営プロジェクトに属していません。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-184">BuildBundlerMinifier belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="0ccd3-185">問題を提出する必要があります[ここ](https://github.com/madskristensen/BundlerMinifier/issues)です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-185">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ccd3-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ccd3-186">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0ccd3-187">追加、 *BuildBundlerMinifier*をプロジェクトにパッケージします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-187">Add the *BuildBundlerMinifier* package to your project.</span></span>

<span data-ttu-id="0ccd3-188">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-188">Build the project.</span></span> <span data-ttu-id="0ccd3-189">次の出力ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-189">The following appears in the Output window:</span></span>

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

<span data-ttu-id="0ccd3-190">プロジェクトをクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-190">Clean the project.</span></span> <span data-ttu-id="0ccd3-191">次の出力ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-191">The following appears in the Output window:</span></span>

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0ccd3-192">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0ccd3-192">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0ccd3-193">追加、 *BuildBundlerMinifier*をプロジェクトにパッケージ。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-193">Add the *BuildBundlerMinifier* package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="0ccd3-194">ASP.NET を使用して場合 1.x のコア、新しく追加されたパッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-194">If using ASP.NET Core 1.x, restore the newly added package:</span></span>

```console
dotnet restore
```

<span data-ttu-id="0ccd3-195">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-195">Build the project:</span></span>

```console
dotnet build
```

<span data-ttu-id="0ccd3-196">次に表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-196">The following appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

<span data-ttu-id="0ccd3-197">プロジェクトをクリーンアップするには。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-197">Clean the project:</span></span>

```console
dotnet clean
```

<span data-ttu-id="0ccd3-198">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-198">The following output appears:</span></span>

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a><span data-ttu-id="0ccd3-199">バンドルと縮小のアドホック実行</span><span class="sxs-lookup"><span data-stu-id="0ccd3-199">Ad hoc execution of bundling and minification</span></span>

<span data-ttu-id="0ccd3-200">プロジェクトをビルドせず、アドホックごとに、バンドルと縮小のタスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-200">It's possible to run the bundling and minification tasks on an ad hoc basis, without building the project.</span></span> <span data-ttu-id="0ccd3-201">追加、 [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet パッケージをプロジェクトに。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-201">Add the [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) NuGet package to your project:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> <span data-ttu-id="0ccd3-202">BundlerMinifier.Core は、Microsoft の提供先のサポート GitHub 上のコミュニティによって運営プロジェクトに属していません。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-202">BundlerMinifier.Core belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="0ccd3-203">問題を提出する必要があります[ここ](https://github.com/madskristensen/BundlerMinifier/issues)です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-203">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="0ccd3-204">このパッケージに含める .NET Core CLI を拡張し、 *dotnet バンドル*ツールです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-204">This package extends the .NET Core CLI to include the *dotnet-bundle* tool.</span></span> <span data-ttu-id="0ccd3-205">パッケージ マネージャー コンソール (PMC) ウィンドウまたはコマンド シェルで、次のコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-205">The following command can be executed in the Package Manager Console (PMC) window or in a command shell:</span></span>

```console
dotnet bundle
```

> [!IMPORTANT]
> <span data-ttu-id="0ccd3-206">NuGet Package Manager の依存関係ファイルに追加 \*.csproj として`<PackageReference />`ノード。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-206">NuGet Package Manager adds dependencies to the \*.csproj file as `<PackageReference />` nodes.</span></span> <span data-ttu-id="0ccd3-207">`dotnet bundle`コマンドが、.NET Core の CLI に登録されている場合にのみ、`<DotNetCliToolReference />`ノードを使用します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-207">The `dotnet bundle` command is registered with the .NET Core CLI only when a `<DotNetCliToolReference />` node is used.</span></span> <span data-ttu-id="0ccd3-208">\*.Csproj ファイルを適宜変更します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-208">Modify the \*.csproj file accordingly.</span></span>

## <a name="add-files-to-workflow"></a><span data-ttu-id="0ccd3-209">ワークフローにファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-209">Add files to workflow</span></span>

<span data-ttu-id="0ccd3-210">例を考えてみます追加*custom.css*ファイルは、次のような追加。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-210">Consider an example in which an additional *custom.css* file is added resembling the following:</span></span>

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

<span data-ttu-id="0ccd3-211">縮小する*custom.css*とバンドルおよび*site.css*に、 *site.min.css*ファイルに追加しへの相対パス*bundleconfig.json*:</span><span class="sxs-lookup"><span data-stu-id="0ccd3-211">To minify *custom.css* and bundle it with *site.css* into a *site.min.css* file, add the relative path to *bundleconfig.json*:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> <span data-ttu-id="0ccd3-212">また、次のグロブ パターンを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-212">Alternatively, the following globbing pattern could be used:</span></span>
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> <span data-ttu-id="0ccd3-213">このグロブ パターンでは、すべての CSS ファイルと一致して、縮小されたファイルのパターンを除外します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-213">This globbing pattern matches all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="0ccd3-214">アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-214">Build the application.</span></span> <span data-ttu-id="0ccd3-215">開いている*site.min.css*の内容を確認および*custom.css*は、ファイルの末尾に追加されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-215">Open *site.min.css* and notice the content of *custom.css* is appended to the end of the file.</span></span>

## <a name="environment-based-bundling-and-minification"></a><span data-ttu-id="0ccd3-216">環境に基づくのバンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="0ccd3-216">Environment-based bundling and minification</span></span>

<span data-ttu-id="0ccd3-217">ベスト プラクティスとしては、実稼働環境でアプリのバンドルと縮小されたファイルを使用してください。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-217">As a best practice, the bundled and minified files of your app should be used in a production environment.</span></span> <span data-ttu-id="0ccd3-218">開発中は、元のファイルをアプリのデバッグを容易に行います。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-218">During development, the original files make for easier debugging of the app.</span></span>

<span data-ttu-id="0ccd3-219">使用して、ページに含めるファイルを指定、[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)ビューにします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-219">Specify which files to include in your pages by using the [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) in your views.</span></span> <span data-ttu-id="0ccd3-220">環境タグ ヘルパーは、固有の仕様で実行されている場合にのみ、コンテンツをレンダリング[環境](xref:fundamentals/environments)です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-220">The Environment Tag Helper only renders its contents when running in specific [environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0ccd3-221">次`environment`で実行する場合、タグが未処理の CSS ファイルを表示、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-221">The following `environment` tag renders the unprocessed CSS files when running in the `Development` environment:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0ccd3-222">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0ccd3-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0ccd3-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0ccd3-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

<span data-ttu-id="0ccd3-224">次`environment`以外の環境で実行する場合、タグは、バンドルと縮小された CSS ファイルをレンダリング`Development`です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-224">The following `environment` tag renders the bundled and minified CSS files when running in an environment other than `Development`.</span></span> <span data-ttu-id="0ccd3-225">たとえばで実行されている`Production`または`Staging`これらのスタイル シートのレンダリングのトリガーします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-225">For example, running in `Production` or `Staging` triggers the rendering of these stylesheets:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0ccd3-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0ccd3-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0ccd3-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0ccd3-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a><span data-ttu-id="0ccd3-228">Gulp から bundleconfig.json を使用します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-228">Consume bundleconfig.json from Gulp</span></span>

<span data-ttu-id="0ccd3-229">アプリのバンドルと縮小のワークフローに追加の処理が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-229">There are cases in which an app's bundling and minification workflow requires additional processing.</span></span> <span data-ttu-id="0ccd3-230">例として、画像の最適化、キャッシュが破壊 CDN 資産処理します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-230">Examples include image optimization, cache busting, and CDN asset processing.</span></span> <span data-ttu-id="0ccd3-231">これらの要件を満たすには、Gulp を使用するバンドルと縮小のワークフローに変換できます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-231">To satisfy these requirements, you can convert the bundling and minification workflow to use Gulp.</span></span>

### <a name="use-the-bundler--minifier-extension"></a><span data-ttu-id="0ccd3-232">バンドルと縮小化拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-232">Use the Bundler & Minifier extension</span></span>

<span data-ttu-id="0ccd3-233">Visual Studio[バンドルと縮小化](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier)拡張機能が Gulp への変換を処理します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-233">The Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) extension handles the conversion to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="0ccd3-234">バンドルと縮小化拡張機能は、Microsoft の提供先のないサポート GitHub 上のコミュニティによって運営プロジェクトに属しています。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-234">The Bundler & Minifier extension belongs to a community-driven project on GitHub for which Microsoft provides no support.</span></span> <span data-ttu-id="0ccd3-235">問題を提出する必要があります[ここ](https://github.com/madskristensen/BundlerMinifier/issues)です。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-235">Issues should be filed [here](https://github.com/madskristensen/BundlerMinifier/issues).</span></span>

<span data-ttu-id="0ccd3-236">右クリックし、 *bundleconfig.json*ソリューション エクスプ ローラーでファイルおよび選択した**バンドルと縮小化** > **Gulp を変換しています.**:</span><span class="sxs-lookup"><span data-stu-id="0ccd3-236">Right-click the *bundleconfig.json* file in Solution Explorer and select **Bundler & Minifier** > **Convert To Gulp...**:</span></span>

![コンテキスト メニュー項目の変換の Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

<span data-ttu-id="0ccd3-238">*Gulpfile.js*と*package.json*ファイルは、プロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-238">The *gulpfile.js* and *package.json* files are added to the project.</span></span> <span data-ttu-id="0ccd3-239">サポートしている[npm](https://www.npmjs.com/)で表示されているパッケージ、 *package.json*ファイルの`devDependencies`セクションがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-239">The supporting [npm](https://www.npmjs.com/) packages listed in the *package.json* file's `devDependencies` section are installed.</span></span>

<span data-ttu-id="0ccd3-240">グローバルの依存関係として Gulp CLI をインストールする [PMC] ウィンドウで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-240">Run the following command in the PMC window to install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="0ccd3-241">*Gulpfile.js*ファイルの読み取り、 *bundleconfig.json*ファイルを入力、出力、および設定します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-241">The *gulpfile.js* file reads the *bundleconfig.json* file for the inputs, outputs, and settings.</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a><span data-ttu-id="0ccd3-242">手動で変換します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-242">Convert manually</span></span>

<span data-ttu-id="0ccd3-243">Visual Studio またはバンドルと縮小化拡張機能が使用できない場合は手動で変換します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-243">If Visual Studio and/or the Bundler & Minifier extension aren't available, convert manually.</span></span>

<span data-ttu-id="0ccd3-244">追加、 *package.json*を次のファイル`devDependencies`プロジェクトのルートに。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-244">Add a *package.json* file, with the following `devDependencies`, to the project root:</span></span>

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

<span data-ttu-id="0ccd3-245">同じレベルで、次のコマンドを実行して、依存関係をインストール*package.json*:</span><span class="sxs-lookup"><span data-stu-id="0ccd3-245">Install the dependencies by running the following command at the same level as *package.json*:</span></span>

```console
npm i
```

<span data-ttu-id="0ccd3-246">グローバルの依存関係として Gulp CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-246">Install the Gulp CLI as a global dependency:</span></span>

```console
npm i -g gulp-cli
```

<span data-ttu-id="0ccd3-247">コピー、 *gulpfile.js*ファイルの名前をプロジェクト ルート下。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-247">Copy the *gulpfile.js* file below to the project root:</span></span>

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a><span data-ttu-id="0ccd3-248">Gulp タスクの実行</span><span class="sxs-lookup"><span data-stu-id="0ccd3-248">Run Gulp tasks</span></span>

<span data-ttu-id="0ccd3-249">Visual Studio でプロジェクトをビルドする前に、Gulp 縮小タスクをトリガーするには、次のコードを追加[MSBuild ターゲット](/visualstudio/msbuild/msbuild-targets)\*.csproj ファイル。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-249">To trigger the Gulp minification task before the project builds in Visual Studio, add the following [MSBuild Target](/visualstudio/msbuild/msbuild-targets) to the \*.csproj file:</span></span>

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

<span data-ttu-id="0ccd3-250">この例では、すべてのタスクが内で定義、`MyPreCompileTarget`ターゲットの前に、定義済み実行`Build`ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-250">In this example, any tasks defined within the `MyPreCompileTarget` target run before the predefined `Build` target.</span></span> <span data-ttu-id="0ccd3-251">Visual Studio の出力 ウィンドウで、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-251">Output similar to the following appears in Visual Studio's Output window:</span></span>

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

<span data-ttu-id="0ccd3-252">代わりに、Visual Studio のタスク ランナー エクスプ ローラーを使用して、Visual Studio の特定のイベントに Gulp タスクをバインドする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-252">Alternatively, Visual Studio's Task Runner Explorer may be used to bind Gulp tasks to specific Visual Studio events.</span></span> <span data-ttu-id="0ccd3-253">参照してください[既定のタスクを実行している](xref:client-side/using-gulp#running-default-tasks)手順については、その作業します。</span><span class="sxs-lookup"><span data-stu-id="0ccd3-253">See [Running default tasks](xref:client-side/using-gulp#running-default-tasks) for instructions on doing that.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ccd3-254">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0ccd3-254">Additional resources</span></span>

* [<span data-ttu-id="0ccd3-255">Gulp の使用</span><span class="sxs-lookup"><span data-stu-id="0ccd3-255">Using Gulp</span></span>](xref:client-side/using-gulp)
* [<span data-ttu-id="0ccd3-256">Grunt の使用</span><span class="sxs-lookup"><span data-stu-id="0ccd3-256">Using Grunt</span></span>](xref:client-side/using-grunt)
* [<span data-ttu-id="0ccd3-257">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="0ccd3-257">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="0ccd3-258">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="0ccd3-258">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
