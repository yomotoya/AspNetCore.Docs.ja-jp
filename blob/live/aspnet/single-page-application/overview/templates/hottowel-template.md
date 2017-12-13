---
uid: single-page-application/overview/templates/hottowel-template
title: "ホット タオル テンプレート |Microsoft ドキュメント"
author: madskristensen
description: "HotTowel テンプレート"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a><span data-ttu-id="6ca29-103">ホット タオル テンプレート</span><span class="sxs-lookup"><span data-stu-id="6ca29-103">Hot Towel template</span></span>
====================
<span data-ttu-id="6ca29-104">によって[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="6ca29-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="6ca29-105">ホット タオル MVC テンプレートは、John Papa によって書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="6ca29-106">ダウンロードするには、どのバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="6ca29-107">Visual Studio 2012 用のホット タオル MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="6ca29-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="6ca29-108">Visual Studio 2013 のホット タオル MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="6ca29-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> <span data-ttu-id="6ca29-109">ホット タオル: 1 つもない SPA に移動したくないためです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="6ca29-110">SPA を構築したいが、開始する場所を決定することはできません。</span><span class="sxs-lookup"><span data-stu-id="6ca29-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="6ca29-111">ホット タオルを使用し、秒単位で、SPA および上でビルドする必要があります。 すべてのツールを必要があります。</span><span class="sxs-lookup"><span data-stu-id="6ca29-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="6ca29-112">ホット タオルでは、ASP.NET を使用した単一ページ アプリケーション (SPA) を構築するための優れた開始ポイントを作成します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="6ca29-113">すぐするモジュール式の構造を提供コード、ビューのナビゲーション、データ バインディング、豊富なデータ管理および単純ですが、洗練されたスタイルです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="6ca29-114">ホット タオルでは、組み込みではないアプリに集中できるように、SPA をビルドする必要がありますすべてを提供します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="6ca29-115">SPA を構築について学習[John Papa のビデオ、チュートリアル、Pluralsight コース](http://johnpapa.net/spa?vsix)です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="6ca29-116">アプリケーションの構造</span><span class="sxs-lookup"><span data-stu-id="6ca29-116">Application Structure</span></span>

<span data-ttu-id="6ca29-117">アプリケーションを定義する JavaScript と HTML ファイルを含むアプリ フォルダーが提供されるホット タオル SPA です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="6ca29-118">アプリのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-118">Inside the App folder:</span></span>

- <span data-ttu-id="6ca29-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="6ca29-119">durandal</span></span>
- <span data-ttu-id="6ca29-120">サービス</span><span class="sxs-lookup"><span data-stu-id="6ca29-120">services</span></span>
- <span data-ttu-id="6ca29-121">viewmodels</span><span class="sxs-lookup"><span data-stu-id="6ca29-121">viewmodels</span></span>
- <span data-ttu-id="6ca29-122">ビュー</span><span class="sxs-lookup"><span data-stu-id="6ca29-122">views</span></span>

<span data-ttu-id="6ca29-123">App フォルダーには、モジュールのコレクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="6ca29-124">これらのモジュールは、機能をカプセル化し、その他のモジュールの依存関係を宣言します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="6ca29-125">Views フォルダーには、アプリケーションの HTML が含まれているし、viewmodels フォルダーには、ビュー (共通 MVVM パターン) のプレゼンテーション ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="6ca29-126">サービスのフォルダーは、HTTP データの取得やローカル ストレージの相互作用など、アプリケーションを必要とする共通のサービスのハウジングに最適です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="6ca29-127">サービス モジュールからコードを再利用する複数の viewmodels 一般的なです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="6ca29-128">ASP.NET MVC のサーバー側アプリケーションの構造</span><span class="sxs-lookup"><span data-stu-id="6ca29-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="6ca29-129">ホット タオルは、使い慣れたと強力な ASP.NET MVC 構造に基づいています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="6ca29-130">アプリ\_開始</span><span class="sxs-lookup"><span data-stu-id="6ca29-130">App\_Start</span></span>
- <span data-ttu-id="6ca29-131">Content</span><span class="sxs-lookup"><span data-stu-id="6ca29-131">Content</span></span>
- <span data-ttu-id="6ca29-132">コント ローラー</span><span class="sxs-lookup"><span data-stu-id="6ca29-132">Controllers</span></span>
- <span data-ttu-id="6ca29-133">モデル</span><span class="sxs-lookup"><span data-stu-id="6ca29-133">Models</span></span>
- <span data-ttu-id="6ca29-134">スクリプト</span><span class="sxs-lookup"><span data-stu-id="6ca29-134">Scripts</span></span>
- <span data-ttu-id="6ca29-135">ビュー</span><span class="sxs-lookup"><span data-stu-id="6ca29-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="6ca29-136">機能を備えたライブラリ</span><span class="sxs-lookup"><span data-stu-id="6ca29-136">Featured Libraries</span></span>

- <span data-ttu-id="6ca29-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6ca29-137">ASP.NET MVC</span></span>
- <span data-ttu-id="6ca29-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="6ca29-138">ASP.NET Web API</span></span>
- <span data-ttu-id="6ca29-139">ASP.NET Web Optimization - バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="6ca29-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="6ca29-140">[Breeze.js](http://Breezejs.com)の豊富なデータ管理</span><span class="sxs-lookup"><span data-stu-id="6ca29-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="6ca29-141">[Durandal.js](http://Durandaljs.com) -ナビゲーションとビューの構成</span><span class="sxs-lookup"><span data-stu-id="6ca29-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="6ca29-142">[Knockout.js](http://Knockoutjs.com)のデータ バインディング</span><span class="sxs-lookup"><span data-stu-id="6ca29-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="6ca29-143">[Require.js](http://requirejs.org) -AMD と最適化モジュール</span><span class="sxs-lookup"><span data-stu-id="6ca29-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="6ca29-144">[Toastr.js](http://jpapa.me/c7toastr) -ポップアップ メッセージ</span><span class="sxs-lookup"><span data-stu-id="6ca29-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="6ca29-145">[Twitter のブートス トラップ](http://twitter.github.com/bootstrap/)- 堅牢な CSS スタイル</span><span class="sxs-lookup"><span data-stu-id="6ca29-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="6ca29-146">Visual Studio 2012 ホット タオル SPA テンプレートを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="6ca29-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="6ca29-147">ホット タオルは、Visual Studio 2012 のテンプレートとしてインストールできます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="6ca29-148">クリックするだけ`File`  |  `New Project`選択`ASP.NET MVC 4 Web Application`です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="6ca29-149">選択し、' ホット タオル単一ページ アプリケーション"テンプレートと実行。</span><span class="sxs-lookup"><span data-stu-id="6ca29-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="6ca29-150">NuGet パッケージを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="6ca29-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="6ca29-151">ホット タオルが空の既存の ASP.NET MVC プロジェクトを補足する NuGet パッケージもあります。</span><span class="sxs-lookup"><span data-stu-id="6ca29-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="6ca29-152">Nuget を使用してをインストールし、実行します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="6ca29-153">ホット タオルで構築する方法は?</span><span class="sxs-lookup"><span data-stu-id="6ca29-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="6ca29-154">コードを追加するだけで開始します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-154">Simply start adding code!</span></span>

1. <span data-ttu-id="6ca29-155">独自のサーバー側のコード (を本当に Breeze.js で際立つ) 可能であれば Entity Framework と WebAPI を追加します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="6ca29-156">ビューを追加、`App/views`フォルダー</span><span class="sxs-lookup"><span data-stu-id="6ca29-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="6ca29-157">Viewmodels を追加、`App/viewmodels`フォルダー</span><span class="sxs-lookup"><span data-stu-id="6ca29-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="6ca29-158">新しいビューへの HTML および Knockout のデータ バインディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="6ca29-159">内のナビゲーションのルートを更新します。`shell.js`</span><span class="sxs-lookup"><span data-stu-id="6ca29-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="6ca29-160">HTML または JavaScript のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="6ca29-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="6ca29-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="6ca29-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="6ca29-162">index.cshtml は、開始のルートと MVC アプリケーション用のビューです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="6ca29-163">すべての標準のメタ タグ、css リンク、および期待する JavaScript 参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="6ca29-164">本文には、1 つが含まれています。`<div>`これは、すべてのコンテンツ (ビュー) を配置する場所が要求したときにします。</span><span class="sxs-lookup"><span data-stu-id="6ca29-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="6ca29-165">`@Scripts.Render` Require.js を使用して、含まれているアプリケーションのコードの開始ポイントを実行する、`main.js`ファイル。</span><span class="sxs-lookup"><span data-stu-id="6ca29-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="6ca29-166">スプラッシュ スクリーンは、アプリが読み込まれるときに、スプラッシュ スクリーンを作成する方法を説明するものです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="6ca29-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="6ca29-167">App/main.js</span></span>

<span data-ttu-id="6ca29-168">`main.js`ファイルには、アプリが読み込まれるとすぐに実行されるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="6ca29-169">これは、ナビゲーション ルートを定義、ビューを設定および任意セットアップ/ブートス トラップ アプリケーションのデータの準備などを実行する場所です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="6ca29-170">`main.js`ファイルには、起動アプリケーションの開始を支援する durandal のモジュールのいくつかを定義します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="6ca29-171">定義するステートメントは、関数の利用できるように、モジュールの依存関係の解決に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="6ca29-172">まず、デバッグ メッセージが有効なコンソール ウィンドウに、アプリケーションの実行、どのようなイベントに関するメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="6ca29-173">App.start のコードでは、durandal framework アプリケーションを起動するように指示します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="6ca29-174">規則は、durandal がすべてのビューを認識し、viewmodels がそれぞれの名前付きの同じフォルダーに含まれるように設定されています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="6ca29-175">最後に、`app.setRoot`読み込みが開始されると、`shell`事前に定義を使用して`entrance`アニメーション。</span><span class="sxs-lookup"><span data-stu-id="6ca29-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="6ca29-176">ビュー</span><span class="sxs-lookup"><span data-stu-id="6ca29-176">Views</span></span>

<span data-ttu-id="6ca29-177">内のビューにある、`App/views`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="6ca29-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="6ca29-178">shell.html</span></span>

<span data-ttu-id="6ca29-179">`shell.html` HTML のマスターのレイアウトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="6ca29-180">側で任意の場所すべて、他のビューの構成は、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="6ca29-181">ホット タオルの提供、`shell`このような 3 つの領域を持つ: ヘッダー、コンテンツ領域およびフッターです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="6ca29-182">これらの地域のそれぞれが読み込まれる内容が要求されたときに、他のビューを形成します。</span><span class="sxs-lookup"><span data-stu-id="6ca29-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="6ca29-183">`compose`ヘッダーとフッターのバインドがハードコードされたホット タオルを指すように、`nav`と`footer`ビューをそれぞれします。</span><span class="sxs-lookup"><span data-stu-id="6ca29-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="6ca29-184">作成するバインディング セクションの`#content`にバインドされて、`router`モジュールのアクティブな項目です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="6ca29-185">つまりをクリックするは対応するビューのナビゲーション リンクは、この領域に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="6ca29-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="6ca29-186">nav.html</span></span>

<span data-ttu-id="6ca29-187">`nav.html` SPA のナビゲーション リンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6ca29-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="6ca29-188">これは、メニュー構造を配置できるなどです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="6ca29-189">多くの場合、これは、データ (Knockout を使用して) バインドを`router`モジュールで定義されているナビゲーションを表示する、`shell.js`です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="6ca29-190">Knockout 検索データ バインド属性およびバインドに、 `shell` viewmodel ナビゲーション ルートを表示して、(Twitter のブートス トラップを使用して) プログレス バーを表示する場合、`router`別 (を参照する 1 つのビューからの移動の途中では、モジュール`router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="6ca29-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="6ca29-191">home.html と details.html</span><span class="sxs-lookup"><span data-stu-id="6ca29-191">home.html and details.html</span></span>

<span data-ttu-id="6ca29-192">これらのビューには、カスタム ビューの HTML が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="6ca29-193">ときに、`home`内のリンク、`nav`ビューのメニューをクリックすると、`home`コンテンツの領域で、ビューが配置される、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="6ca29-194">これらのビュー補強したり、独自のカスタム ビューに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="6ca29-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="6ca29-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="6ca29-195">footer.html</span></span>

<span data-ttu-id="6ca29-196">`footer.html`の下部にある、フッターに表示される HTML が含まれています、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="6ca29-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="6ca29-197">ViewModels</span></span>

<span data-ttu-id="6ca29-198">ViewModels に含まれて、`App/viewmodels`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="6ca29-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="6ca29-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="6ca29-199">shell.js</span></span>

<span data-ttu-id="6ca29-200">`shell` Viewmodel プロパティおよびにバインドされている関数を含む、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="6ca29-201">メニュー ナビゲーションのバインディングがある多くの場合は (を参照してください、`router.mapNav`ロジック)。</span><span class="sxs-lookup"><span data-stu-id="6ca29-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="6ca29-202">home.js と details.js</span><span class="sxs-lookup"><span data-stu-id="6ca29-202">home.js and details.js</span></span>

<span data-ttu-id="6ca29-203">これら viewmodels プロパティおよびにバインドされている関数を含む、`home`ビュー。</span><span class="sxs-lookup"><span data-stu-id="6ca29-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="6ca29-204">また、ビューのプレゼンテーション ロジックを含んでいて、グルー データとビューの間は。</span><span class="sxs-lookup"><span data-stu-id="6ca29-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="6ca29-205">サービス</span><span class="sxs-lookup"><span data-stu-id="6ca29-205">Services</span></span>

<span data-ttu-id="6ca29-206">アプリケーション/サービス フォルダーには、サービスが見つかりませんでした。</span><span class="sxs-lookup"><span data-stu-id="6ca29-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="6ca29-207">理想的には、dataservice モジュールを取得して、リモート データの送信を担当するなど、将来のサービスを配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6ca29-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="6ca29-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="6ca29-208">logger.js</span></span>

<span data-ttu-id="6ca29-209">ホット タオルの提供、 `logger` services フォルダー内のモジュール。</span><span class="sxs-lookup"><span data-stu-id="6ca29-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="6ca29-210">`logger`モジュールは、のログ メッセージをコンソール、およびトースト ポップアップでユーザーに最適です。</span><span class="sxs-lookup"><span data-stu-id="6ca29-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
