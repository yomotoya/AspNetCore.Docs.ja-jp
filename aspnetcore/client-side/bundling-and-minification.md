---
title: "バンドルと ASP.NET Core の縮小"
author: spboyer
description: 
keywords: "ASP.NET Core、バンドルと縮小、CSS、JavaScript、縮小、BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 810d89798330aeb1c387ec85eb05b1f4efde167a
ms.sourcegitcommit: 275a5381b6172b4f0b5fcd1d252aff03d3dae166
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/30/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a><span data-ttu-id="ac7e5-103">バンドルと ASP.NET Core の縮小</span><span class="sxs-lookup"><span data-stu-id="ac7e5-103">Bundling and minification in ASP.NET Core</span></span>

<span data-ttu-id="ac7e5-104">バンドルと縮小 2 つの方法は、web アプリケーションのページ読み込みのパフォーマンスを向上させるために ASP.NET で使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-104">Bundling and minification are two techniques you can use in ASP.NET to improve page load performance for your web application.</span></span> <span data-ttu-id="ac7e5-105">複数のファイルを 1 つのファイルに連結するバンドルします。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-105">Bundling combines multiple files into a single file.</span></span> <span data-ttu-id="ac7e5-106">縮小では、さまざまなスクリプトと CSS を小さいペイロードで結果を別のコードの最適化を実行します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-106">Minification performs a variety of different code optimizations to scripts and CSS, which results in smaller payloads.</span></span> <span data-ttu-id="ac7e5-107">一緒に使用すると、バンドルと縮小ロード時間、パフォーマンスが向上サーバーへの要求の数を減らすと、要求されたアセット (CSS および JavaScript ファイルなど) のサイズを小さきます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-107">Used together, bundling and minification improves load time performance by reducing the number of requests to the server and reducing the size of the requested assets (such as CSS and JavaScript files).</span></span>

<span data-ttu-id="ac7e5-108">この記事では、バンドルと縮小を含む ASP.NET Core アプリケーションでこれらの機能を使用する方法を使用する利点について説明します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-108">This article explains the benefits of using bundling and minification, including how these features can be used with ASP.NET Core applications.</span></span>

## <a name="overview"></a><span data-ttu-id="ac7e5-109">概要</span><span class="sxs-lookup"><span data-stu-id="ac7e5-109">Overview</span></span>

<span data-ttu-id="ac7e5-110">ASP.NET Core アプリ バンドル化とクライアント側のリソースを縮小するための複数のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-110">In ASP.NET Core apps, there are multiple options for bundling and minifying client-side resources.</span></span> <span data-ttu-id="ac7e5-111">MVC の中核となるテンプレートでは、構成ファイルと BuildBundlerMinifier NuGet パッケージを使用して既定のソリューションを提供します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-111">The core templates for MVC provide an out-of-the-box solution using a configuration file and BuildBundlerMinifier NuGet package.</span></span> <span data-ttu-id="ac7e5-112">などのサード パーティ製ツール[Gulp](using-gulp.md)と[Grunt](using-grunt.md)がプロセスに必要なその他のワークフローまたは複雑な作業に同じタスクを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-112">Third party tools, such as [Gulp](using-gulp.md) and [Grunt](using-grunt.md) are also available to accomplish the same tasks should your processes require additional workflow or complexities.</span></span> <span data-ttu-id="ac7e5-113">デザイン時のバンドルと縮小を使用すると、縮小されたファイルは、アプリケーションの展開する前に作成されます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-113">By using design-time bundling and minification, the minified files are created prior to the application’s deployment.</span></span> <span data-ttu-id="ac7e5-114">バンドル化と縮小展開する前に、サーバー負荷の軽減の利点を提供します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-114">Bundling and minifying before deployment provides the advantage of reduced server load.</span></span> <span data-ttu-id="ac7e5-115">ただし、そのデザイン時のバンドルを認識することが重要と縮小ビルド複雑さが増加し、静的ファイルでのみ動作します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-115">However, it’s important to recognize that design-time bundling and minification increases build complexity and only works with static files.</span></span>

<span data-ttu-id="ac7e5-116">バンドルと縮小、主に、最初のページ要求の読み込み時間を向上します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-116">Bundling and minification primarily improve the first page request load time.</span></span> <span data-ttu-id="ac7e5-117">ブラウザー キャッシュ資産 (JavaScript、CSS、および画像) で、web ページが要求された後、同じページを要求するときに、バンドルと縮小すると、すべてのパフォーマンスが向上が提供するされませんやページは、同じサイトの同じアセットを要求するようにします。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-117">Once a web page has been requested, the browser caches the assets (JavaScript, CSS and images) so bundling and minification won’t provide any performance boost when requesting the same page, or pages on the same site requesting the same assets.</span></span> <span data-ttu-id="ac7e5-118">設定しない場合、ヘッダー、資産を正しく有効期限が切れるとバンドルと縮小を使用しない場合、ブラウザーの鮮度ヒューリスティックとしてマークされます資産古い数日後とブラウザーでは資産ごとに、検証要求が必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-118">If you don’t set the expires header correctly on your assets, and you don’t use bundling and minification, the browser's freshness heuristics will mark the assets stale after a few days and the browser will require a validation request for each asset.</span></span> <span data-ttu-id="ac7e5-119">この例では、バンドルと縮小は、最初のページ要求した後でも、パフォーマンスが向上を提供します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-119">In this case, bundling and minification provide a performance increase even after the first page request.</span></span>

### <a name="bundling"></a><span data-ttu-id="ac7e5-120">バンドル化</span><span class="sxs-lookup"><span data-stu-id="ac7e5-120">Bundling</span></span>

<span data-ttu-id="ac7e5-121">バンドルは、結合または 1 つのファイルを複数のファイルにバンドルしやすく機能です。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-121">Bundling is a feature that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="ac7e5-122">1 つのファイルに複数のファイルを結合するバンドル、ために、取得して、web ページなどの web アセットを表示するために必要なサーバーに要求の数を削減します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-122">Because bundling combines multiple files into a single file, it reduces the number of requests to the server that are required to retrieve and display a web asset, such as a web page.</span></span> <span data-ttu-id="ac7e5-123">CSS、JavaScript、およびその他のバンドルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-123">You can create CSS, JavaScript and other bundles.</span></span> <span data-ttu-id="ac7e5-124">ファイル数を減らしてでは、サーバーに、ブラウザーまたはアプリケーションを提供するサービスから少数の HTTP 要求を意味します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-124">Fewer files means fewer HTTP requests from your browser to the server or from the service providing your application.</span></span> <span data-ttu-id="ac7e5-125">この結果では、最初のページ読み込みのパフォーマンスを向上します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-125">This results in improved first page load performance.</span></span>

### <a name="minification"></a><span data-ttu-id="ac7e5-126">縮小</span><span class="sxs-lookup"><span data-stu-id="ac7e5-126">Minification</span></span>

<span data-ttu-id="ac7e5-127">縮小では、さまざまな (など、CSS、画像、JavaScript ファイル) の要求された資産のサイズを小さく、さまざまなコードの最適化を実行します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-127">Minification performs a variety of different code optimizations to reduce the size of requested assets (such as CSS, images, JavaScript files).</span></span> <span data-ttu-id="ac7e5-128">縮小の一般的な結果には、不要な空白文字とコメントを削除して、1 文字に変数名を短くが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-128">Common results of minification include removing unnecessary white space and comments, and shortening variable names to one character.</span></span>

<span data-ttu-id="ac7e5-129">次の JavaScript 関数を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-129">Consider the following JavaScript function:</span></span>

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

<span data-ttu-id="ac7e5-130">縮小、後に、関数は、次に縮小されます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-130">After minification, the function is reduced to the following:</span></span>

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

<span data-ttu-id="ac7e5-131">コメントと不要な空白文字を削除するだけでなく、次のパラメーターと変数の名前が変更された (切り捨て) には、次のように。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-131">In addition to removing the comments and unnecessary whitespace, the following parameters and variable names were renamed (shortened) as follows:</span></span>

<span data-ttu-id="ac7e5-132">元</span><span class="sxs-lookup"><span data-stu-id="ac7e5-132">Original</span></span> | <span data-ttu-id="ac7e5-133">名前の変更</span><span class="sxs-lookup"><span data-stu-id="ac7e5-133">Renamed</span></span>
--- | :---:
<span data-ttu-id="ac7e5-134">imageTagAndImageID</span><span class="sxs-lookup"><span data-stu-id="ac7e5-134">imageTagAndImageID</span></span> | <span data-ttu-id="ac7e5-135">t</span><span class="sxs-lookup"><span data-stu-id="ac7e5-135">t</span></span>
<span data-ttu-id="ac7e5-136">イメージ コンテキスト</span><span class="sxs-lookup"><span data-stu-id="ac7e5-136">imageContext</span></span> | <span data-ttu-id="ac7e5-137">a</span><span class="sxs-lookup"><span data-stu-id="ac7e5-137">a</span></span>
<span data-ttu-id="ac7e5-138">imageElement</span><span class="sxs-lookup"><span data-stu-id="ac7e5-138">imageElement</span></span> | <span data-ttu-id="ac7e5-139">r</span><span class="sxs-lookup"><span data-stu-id="ac7e5-139">r</span></span>

## <a name="impact-of-bundling-and-minification"></a><span data-ttu-id="ac7e5-140">バンドルと縮小の影響</span><span class="sxs-lookup"><span data-stu-id="ac7e5-140">Impact of bundling and minification</span></span>

<span data-ttu-id="ac7e5-141">次の表は、すべてのアセットを個別に一覧表示して、バンドルと縮小を使用して、単純な web ページ上のいくつかの重要な違いを示しています。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-141">The following table shows several important differences between listing all the assets individually and using bundling and minification on a simple web page:</span></span>

<span data-ttu-id="ac7e5-142">操作</span><span class="sxs-lookup"><span data-stu-id="ac7e5-142">Action</span></span> | <span data-ttu-id="ac7e5-143">B/M と</span><span class="sxs-lookup"><span data-stu-id="ac7e5-143">With B/M</span></span> | <span data-ttu-id="ac7e5-144">B/M なし</span><span class="sxs-lookup"><span data-stu-id="ac7e5-144">Without B/M</span></span> | <span data-ttu-id="ac7e5-145">変更</span><span class="sxs-lookup"><span data-stu-id="ac7e5-145">Change</span></span>
--- | :---: | :---: | :---:
<span data-ttu-id="ac7e5-146">ファイルの要求</span><span class="sxs-lookup"><span data-stu-id="ac7e5-146">File Requests</span></span> |<span data-ttu-id="ac7e5-147">7</span><span class="sxs-lookup"><span data-stu-id="ac7e5-147">7</span></span> | <span data-ttu-id="ac7e5-148">18</span><span class="sxs-lookup"><span data-stu-id="ac7e5-148">18</span></span> | <span data-ttu-id="ac7e5-149">157%</span><span class="sxs-lookup"><span data-stu-id="ac7e5-149">157%</span></span>
<span data-ttu-id="ac7e5-150">サポート技術情報の転送</span><span class="sxs-lookup"><span data-stu-id="ac7e5-150">KB Transferred</span></span> | <span data-ttu-id="ac7e5-151">156</span><span class="sxs-lookup"><span data-stu-id="ac7e5-151">156</span></span> | <span data-ttu-id="ac7e5-152">264.68</span><span class="sxs-lookup"><span data-stu-id="ac7e5-152">264.68</span></span> | <span data-ttu-id="ac7e5-153">70%</span><span class="sxs-lookup"><span data-stu-id="ac7e5-153">70%</span></span>
<span data-ttu-id="ac7e5-154">読み込み時間 (ミリ秒)</span><span class="sxs-lookup"><span data-stu-id="ac7e5-154">Load Time (MS)</span></span> | <span data-ttu-id="ac7e5-155">885</span><span class="sxs-lookup"><span data-stu-id="ac7e5-155">885</span></span> | <span data-ttu-id="ac7e5-156">2360</span><span class="sxs-lookup"><span data-stu-id="ac7e5-156">2360</span></span> | <span data-ttu-id="ac7e5-157">167%</span><span class="sxs-lookup"><span data-stu-id="ac7e5-157">167%</span></span>

<span data-ttu-id="ac7e5-158">送信されたバイト数には、ブラウザーは要求に適用される HTTP ヘッダーを非常に冗長のバンドルを大幅に軽減が必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-158">The bytes sent had a significant reduction with bundling as browsers are fairly verbose with the HTTP headers that they apply on requests.</span></span> <span data-ttu-id="ac7e5-159">ただし、この例は、ローカルで実行された、読み込み時は、大規模な向上を示します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-159">The load time shows a big improvement, however this example was run locally.</span></span> <span data-ttu-id="ac7e5-160">アセットのバンドルと縮小を使用して、ネットワーク経由で転送されるときにパフォーマンスの向上が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-160">You will get greater gains in performance when using bundling and minification with assets transferred over a network.</span></span>

## <a name="using-bundling-and-minification-in-a-project"></a><span data-ttu-id="ac7e5-161">プロジェクトでバンドルと縮小の使用</span><span class="sxs-lookup"><span data-stu-id="ac7e5-161">Using bundling and minification in a project</span></span>

<span data-ttu-id="ac7e5-162">MVC プロジェクト テンプレートでは、`bundleconfig.json`構成ファイルの各バンドルのオプションを定義します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-162">The MVC project template provides a `bundleconfig.json` configuration file which defines the options for each bundle.</span></span> <span data-ttu-id="ac7e5-163">カスタムの JavaScript の既定では、1 つのバンドルの構成が定義されている (`wwwroot/js/site.js`) とスタイル シート (`wwwroot/css/site.css`) ファイル。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-163">By default, a single bundle configuration is defined for the custom JavaScript (`wwwroot/js/site.js`) and Stylesheet (`wwwroot/css/site.css`) files.</span></span>

<span data-ttu-id="ac7e5-164">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span><span class="sxs-lookup"><span data-stu-id="ac7e5-164">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]</span></span>

<span data-ttu-id="ac7e5-165">バンドルのオプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-165">Bundle options include:</span></span>

* <span data-ttu-id="ac7e5-166">outputFileName - 出力するバンドル ファイルの名前。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-166">outputFileName - name of the bundle file to output.</span></span> <span data-ttu-id="ac7e5-167">相対パスを含めることができます、`bundleconfig.json`ファイル。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-167">Can contain a relative path from the `bundleconfig.json` file.</span></span> <span data-ttu-id="ac7e5-168">**必須**</span><span class="sxs-lookup"><span data-stu-id="ac7e5-168">**required**</span></span>
* <span data-ttu-id="ac7e5-169">inputFiles - をまとめるためにファイルの配列。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-169">inputFiles - array of files to bundle together.</span></span> <span data-ttu-id="ac7e5-170">これらは、構成ファイルへの相対パスです。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-170">These are relative paths to the configuration file.</span></span> <span data-ttu-id="ac7e5-171">**省略可能な**、*、空の値が空の出力ファイルの結果します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-171">**optional**, *an empty value results in an empty output file.</span></span> <span data-ttu-id="ac7e5-172">[グロブ](http://www.tldp.org/LDP/abs/html/globbingref.html)パターンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-172">[globbing](http://www.tldp.org/LDP/abs/html/globbingref.html) patterns are supported.</span></span>
* <span data-ttu-id="ac7e5-173">縮小 - 出力の縮小オプションを入力します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-173">minify - minification options for the output type.</span></span> <span data-ttu-id="ac7e5-174">**省略可能な**、*既定 -`minify: { enabled: true }`*</span><span class="sxs-lookup"><span data-stu-id="ac7e5-174">**optional**, *default - `minify: { enabled: true }`*</span></span>
  * <span data-ttu-id="ac7e5-175">出力ファイルの種類ごとの構成オプションのとおりです。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-175">Configuration options are available per output file type.</span></span>
    * [<span data-ttu-id="ac7e5-176">CSS の縮小化</span><span class="sxs-lookup"><span data-stu-id="ac7e5-176">CSS Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [<span data-ttu-id="ac7e5-177">JavaScript の縮小化</span><span class="sxs-lookup"><span data-stu-id="ac7e5-177">JavaScript Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/jsminifier)
    * [<span data-ttu-id="ac7e5-178">HTML の縮小化</span><span class="sxs-lookup"><span data-stu-id="ac7e5-178">HTML Minifier</span></span>](https://github.com/madskristensen/BundlerMinifier/wiki/htmlminifier)
* <span data-ttu-id="ac7e5-179">includeInProject - 生成されたファイルをプロジェクト ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-179">includeInProject - add generated files to project file.</span></span> <span data-ttu-id="ac7e5-180">**省略可能な**、*既定 - false*</span><span class="sxs-lookup"><span data-stu-id="ac7e5-180">**optional**, *default - false*</span></span>
* <span data-ttu-id="ac7e5-181">ソースマップ - は、バンドルしたファイルのソース マップを生成します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-181">sourceMaps - generate source maps for the bundled file.</span></span> <span data-ttu-id="ac7e5-182">**省略可能な**、*既定 - false*</span><span class="sxs-lookup"><span data-stu-id="ac7e5-182">**optional**, *default - false*</span></span>

### <a name="visual-studio-2015--2017"></a><span data-ttu-id="ac7e5-183">Visual Studio 2015 2017/</span><span class="sxs-lookup"><span data-stu-id="ac7e5-183">Visual Studio 2015 / 2017</span></span>

<span data-ttu-id="ac7e5-184">開いている`bundleconfig.json`Visual Studio で、環境内にインストールされている拡張機能がない場合、プロンプトが表示されますがあるこの種類のファイルを支援できなかったことを推奨します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-184">Open `bundleconfig.json` in Visual Studio, if your environment does not have the extension installed; a prompt is presented suggesting that there is one that could assist with this file type.</span></span>

![BuildBundlerMinifier 拡張機能の提案](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

<span data-ttu-id="ac7e5-186">表示拡張機能を選択し、インストール、**バンドルと縮小化**拡張機能 (再起動が Visual Studio の必要があります)。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-186">Select View Extensions, and install the **Bundler & Minifier** extension (Requires Visual Studio restart).</span></span>

![BuildBundlerMinifier 拡張機能の提案](../client-side/bundling-and-minification/_static/view-extension.png)

<span data-ttu-id="ac7e5-188">再起動が完了したらを縮小して、クライアント側の資産のバンドルのプロセスを実行するビルドを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-188">When the restart is complete, you need to configure the build to run the processes of minifying and bundling the client-side assets.</span></span> <span data-ttu-id="ac7e5-189">右クリックし、`bundleconfig.json`ファイルおよび選択した*ビルドで有効にするバンドルしています.*.</span><span class="sxs-lookup"><span data-stu-id="ac7e5-189">Right-click the `bundleconfig.json` file and select *Enable bundle on build...*.</span></span>

<span data-ttu-id="ac7e5-190">プロジェクトをビルドし、`bundleconfig.json`構成に基づいて、出力ファイルを生成するために、ビルド プロセスに含まれます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-190">Build the project, and the `bundleconfig.json` is included in the build process to produce the output files based on the configuration.</span></span>

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a><span data-ttu-id="ac7e5-191">Visual Studio Code またはコマンド ライン</span><span class="sxs-lookup"><span data-stu-id="ac7e5-191">Visual Studio Code or Command Line</span></span>

<span data-ttu-id="ac7e5-192">Visual Studio と拡張機能は、GUI のジェスチャを使用してバンドルと縮小のプロセスをドライブします。ただし、同じ機能がで使用可能な`dotnet`CLI および BuildBundlerMinifier NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-192">Visual Studio and the extension drive the bundling and minification process using GUI gestures; however, the same capabilities are available with the `dotnet` CLI and BuildBundlerMinifier NuGet package.</span></span>

<span data-ttu-id="ac7e5-193">NuGet パッケージをプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-193">Add the NuGet package to your project:</span></span>

```console
dotnet add package BuildBundlerMinifier
```

<span data-ttu-id="ac7e5-194">依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-194">Restore the dependencies:</span></span>

```console
dotnet restore
```

<span data-ttu-id="ac7e5-195">アプリをビルドします。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-195">Build the app:</span></span>

```console
dotnet build
```

<span data-ttu-id="ac7e5-196">ビルド コマンドからの出力は、縮小や新機能が構成されているに従ってバンドル化の結果を示します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-196">The output from the build command shows the results of the minification and/or bundling according to what is configured.</span></span>

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a><span data-ttu-id="ac7e5-197">ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-197">Adding files</span></span>

<span data-ttu-id="ac7e5-198">この例では、追加の CSS ファイルが、呼び出されたが追加`custom.css`バンドルと縮小に用に構成および`site.css`、1 つの結果として得られる`site.min.css`です。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-198">In this example, an additional CSS file is added called `custom.css` and configured for bundling and minification with `site.css`, resulting in a single `site.min.css`.</span></span>

<span data-ttu-id="ac7e5-199">custom.css</span><span class="sxs-lookup"><span data-stu-id="ac7e5-199">custom.css</span></span>

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

<span data-ttu-id="ac7e5-200">相対パスを追加`bundleconfig.json`です。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-200">Add the relative path to `bundleconfig.json`.</span></span>

<span data-ttu-id="ac7e5-201">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span><span class="sxs-lookup"><span data-stu-id="ac7e5-201">[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]</span></span>

> [!NOTE]
> <span data-ttu-id="ac7e5-202">また、グロブ パターンを使用する可能性があります -`"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]`ファイルのすべての CSS を取得して、縮小されたファイルのパターンを除外します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-202">Alternatively, the globbing pattern could be used - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` which gets all CSS files and excludes the minified file pattern.</span></span>

<span data-ttu-id="ac7e5-203">アプリケーションのビルドを開く場合と`site.min.css`、ようになりましたことがわかりますの内容`custom.css`ファイルの末尾に追加されていません。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-203">Build the application and if you open `site.min.css`, you'll now notice that contents of `custom.css` has been appended to the end of the file.</span></span>

## <a name="controlling-bundling-and-minification"></a><span data-ttu-id="ac7e5-204">バンドルと縮小を制御します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-204">Controlling bundling and minification</span></span>

<span data-ttu-id="ac7e5-205">一般に、実稼働環境でのみ、アプリのバンドルと縮小されたファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-205">In general, you want to use the bundled and minified files of your app only in a production environment.</span></span> <span data-ttu-id="ac7e5-206">開発中は、するアプリがデバッグの簡略化のために、元のファイルを使用します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-206">During development, you want to use your original files so your app is easier to debug.</span></span>

<span data-ttu-id="ac7e5-207">スクリプトと、レイアウト ページで、環境タグ ヘルパーを使用して、ページに追加する CSS ファイルを指定できます (を参照してください[タグ ヘルパー](../mvc/views/tag-helpers/index.md))。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-207">You can specify which scripts and CSS files to include in your pages using the environment tag helper in your layout pages (see [Tag Helpers](../mvc/views/tag-helpers/index.md)).</span></span> <span data-ttu-id="ac7e5-208">環境タグ ヘルパーは、特定の環境で実行する場合、その内容を表示のみされます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-208">The environment tag helper will only render its contents when running in specific environments.</span></span> <span data-ttu-id="ac7e5-209">参照してください[複数の環境で作業](../fundamentals/environments.md)詳細については、現在の環境を指定する方法です。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-209">See [Working with Multiple Environments](../fundamentals/environments.md) for details on specifying the current environment.</span></span>

<span data-ttu-id="ac7e5-210">実行されているときに、次の環境タグは、未処理の CSS ファイルをレンダリングする、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-210">The following environment tag will render the unprocessed CSS files when running in the `Development` environment:</span></span>

<span data-ttu-id="ac7e5-211">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span><span class="sxs-lookup"><span data-stu-id="ac7e5-211">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]</span></span>

<span data-ttu-id="ac7e5-212">実行されているときにのみ、この環境タグは、バンドルと縮小された CSS ファイルをレンダリングする`Production`または`Staging`:</span><span class="sxs-lookup"><span data-stu-id="ac7e5-212">This environment tag will render the bundled and minified CSS files only when running in `Production` or `Staging`:</span></span>

<span data-ttu-id="ac7e5-213">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span><span class="sxs-lookup"><span data-stu-id="ac7e5-213">[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]</span></span>

## <a name="consuming-bundleconfigjson-from-gulp"></a><span data-ttu-id="ac7e5-214">Gulp から bundleconfig.json の使用</span><span class="sxs-lookup"><span data-stu-id="ac7e5-214">Consuming bundleconfig.json from Gulp</span></span>

<span data-ttu-id="ac7e5-215">アプリ バンドルと縮小ワークフローは、イメージ処理キャッシュ破壊、CDN assest 処理などの追加の処理を必要とする場合は、バンドルおよび Minify プロセスを Gulp を変換できます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-215">If your app bundling and minification workflow requires additional processes such as image processing, cache busting, CDN assest processing, etc., then you can convert the Bundle and Minify process to Gulp.</span></span>

> [!NOTE]
> <span data-ttu-id="ac7e5-216">変換オプションを Visual Studio 2015 および 2017 でのみ利用できます。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-216">Conversion option only available in Visual Studio 2015 and 2017.</span></span>

<span data-ttu-id="ac7e5-217">右クリックし、`bundleconfig.json`選択**Gulp に変換しています.**.これが生成されます、`gulpfile.js`し、必要な npm パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-217">Right-click the `bundleconfig.json` and select **Convert to Gulp...**. This will generate the `gulpfile.js` and install the necessary npm packages.</span></span>

![Gulp への変換します。](../client-side/bundling-and-minification/_static/convert-togulp.png)

<span data-ttu-id="ac7e5-219">`gulpfile.js`読み取りを生成、`bundleconfig.json`ファイルの構成、したがってそのを継続できますの入力/出力と設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-219">The `gulpfile.js` produced reads the `bundleconfig.json` file for the configuration, therefore it can continue to be used for the inputs/outputs and settings.</span></span>

<span data-ttu-id="ac7e5-220">[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span><span class="sxs-lookup"><span data-stu-id="ac7e5-220">[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]</span></span>

<span data-ttu-id="ac7e5-221">Visual Studio 2017 でプロジェクトのビルド時に Gulp を有効にするには、するには、*.csproj ファイルに、次を追加します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-221">To enable Gulp when the project builds in Visual Studio 2017, add the following to the *.csproj file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

<span data-ttu-id="ac7e5-222">Visual Studio 2015 のプロジェクトのビルド時に Gulp を有効にするには、するには、次を追加、`project.json`ファイル。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-222">To enable Gulp when the project builds in Visual Studio 2015, add the following to the `project.json` file:</span></span>

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a><span data-ttu-id="ac7e5-223">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ac7e5-223">Additional resources</span></span>

* [<span data-ttu-id="ac7e5-224">Gulp を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-224">Using Gulp</span></span>](using-gulp.md)
* [<span data-ttu-id="ac7e5-225">Grunt を使用します。</span><span class="sxs-lookup"><span data-stu-id="ac7e5-225">Using Grunt</span></span>](using-grunt.md)
* [<span data-ttu-id="ac7e5-226">複数の環境での作業</span><span class="sxs-lookup"><span data-stu-id="ac7e5-226">Working with Multiple Environments</span></span>](../fundamentals/environments.md)
* [<span data-ttu-id="ac7e5-227">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="ac7e5-227">Tag Helpers</span></span>](../mvc/views/tag-helpers/index.md)
