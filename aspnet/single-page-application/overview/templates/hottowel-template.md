---
uid: single-page-application/overview/templates/hottowel-template
title: ホット Towel テンプレート |Microsoft Docs
author: madskristensen
description: HotTowel テンプレート
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834409"
---
<a name="hot-towel-template"></a><span data-ttu-id="2e2bf-103">Hot Towel テンプレート</span><span class="sxs-lookup"><span data-stu-id="2e2bf-103">Hot Towel template</span></span>
====================
<span data-ttu-id="2e2bf-104">によって[Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="2e2bf-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="2e2bf-105">ホットのタオル MVC テンプレートは、John Papa によって書き込まれます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="2e2bf-106">ダウンロードするバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="2e2bf-107">Visual Studio 2012 用のホット タオル MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="2e2bf-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="2e2bf-108">Visual Studio 2013 のホット タオルの MVC テンプレート</span><span class="sxs-lookup"><span data-stu-id="2e2bf-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="2e2bf-109">ホットのタオル: ない SPA に移動するためです。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="2e2bf-110">SPA を構築したいが、開始する場所を決定することはできませんか?</span><span class="sxs-lookup"><span data-stu-id="2e2bf-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="2e2bf-111">ホット タオルを使用し、秒単位では、SPA とその上に構築する必要があるすべてのツールをしましたします。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="2e2bf-112">ホットのタオルでは、ASP.NET を使用したシングル ページ アプリケーション (SPA) を構築するための出発点を作成します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="2e2bf-113">すぐするモジュール式の構造を提供、コード、ビューのナビゲーション、データ バインディング、豊富なデータ管理および単純ですが、洗練されたスタイルをします。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="2e2bf-114">ホット タオルは、すべてのプラミングではないアプリに集中できるように、SPA を構築する必要があるものを提供します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="2e2bf-115">SPA を構築について詳しく学習[John Papa のビデオ、チュートリアル、および Pluralsight コース](http://johnpapa.net/spa?vsix)します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="2e2bf-116">アプリケーションの構造</span><span class="sxs-lookup"><span data-stu-id="2e2bf-116">Application Structure</span></span>

<span data-ttu-id="2e2bf-117">ホットのタオル SPA には、アプリケーションを定義する JavaScript と HTML ファイルを含むアプリ フォルダーが提供します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="2e2bf-118">アプリ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-118">Inside the App folder:</span></span>

- <span data-ttu-id="2e2bf-119">durandal</span><span class="sxs-lookup"><span data-stu-id="2e2bf-119">durandal</span></span>
- <span data-ttu-id="2e2bf-120">サービス</span><span class="sxs-lookup"><span data-stu-id="2e2bf-120">services</span></span>
- <span data-ttu-id="2e2bf-121">ビューモデル</span><span class="sxs-lookup"><span data-stu-id="2e2bf-121">viewmodels</span></span>
- <span data-ttu-id="2e2bf-122">ビュー</span><span class="sxs-lookup"><span data-stu-id="2e2bf-122">views</span></span>

<span data-ttu-id="2e2bf-123">アプリ フォルダーには、モジュールのコレクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="2e2bf-124">これらのモジュールは、機能をカプセル化し、他のモジュールの依存関係を宣言します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="2e2bf-125">Views フォルダーには、アプリケーションの HTML が含まれているし、ビューモデル フォルダーにはビュー (一般的な MVVM パターン) のプレゼンテーション ロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="2e2bf-126">サービス フォルダーは、アプリケーションには、HTTP データを取得またはローカル ストレージとの対話など必要なすべての一般的なサービスの格納に最適です。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="2e2bf-127">サービス モジュールからコードを再利用する複数のビューモデル一般的です。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="2e2bf-128">ASP.NET MVC のサーバー側アプリケーションの構造</span><span class="sxs-lookup"><span data-stu-id="2e2bf-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="2e2bf-129">ホットのタオルは親しみやすく、強力な ASP.NET MVC の構造に基づいています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="2e2bf-130">アプリ\_開始</span><span class="sxs-lookup"><span data-stu-id="2e2bf-130">App\_Start</span></span>
- <span data-ttu-id="2e2bf-131">Content</span><span class="sxs-lookup"><span data-stu-id="2e2bf-131">Content</span></span>
- <span data-ttu-id="2e2bf-132">Controllers</span><span class="sxs-lookup"><span data-stu-id="2e2bf-132">Controllers</span></span>
- <span data-ttu-id="2e2bf-133">モデル</span><span class="sxs-lookup"><span data-stu-id="2e2bf-133">Models</span></span>
- <span data-ttu-id="2e2bf-134">スクリプト</span><span class="sxs-lookup"><span data-stu-id="2e2bf-134">Scripts</span></span>
- <span data-ttu-id="2e2bf-135">Views</span><span class="sxs-lookup"><span data-stu-id="2e2bf-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="2e2bf-136">おすすめのライブラリ</span><span class="sxs-lookup"><span data-stu-id="2e2bf-136">Featured Libraries</span></span>

- <span data-ttu-id="2e2bf-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="2e2bf-137">ASP.NET MVC</span></span>
- <span data-ttu-id="2e2bf-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2e2bf-138">ASP.NET Web API</span></span>
- <span data-ttu-id="2e2bf-139">ASP.NET Web の最適化 - バンドルと縮小</span><span class="sxs-lookup"><span data-stu-id="2e2bf-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="2e2bf-140">[Breeze.js](http://Breezejs.com) -豊富なデータ管理</span><span class="sxs-lookup"><span data-stu-id="2e2bf-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="2e2bf-141">[Durandal.js](http://Durandaljs.com) -ナビゲーションとビューの構成</span><span class="sxs-lookup"><span data-stu-id="2e2bf-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="2e2bf-142">[Knockout.js](http://Knockoutjs.com) -データのバインド</span><span class="sxs-lookup"><span data-stu-id="2e2bf-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="2e2bf-143">[Require.js](http://requirejs.org) -AMD と最適化モジュール</span><span class="sxs-lookup"><span data-stu-id="2e2bf-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="2e2bf-144">[Toastr.js](http://jpapa.me/c7toastr) -ポップアップ メッセージ</span><span class="sxs-lookup"><span data-stu-id="2e2bf-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="2e2bf-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - 堅牢な CSS スタイル</span><span class="sxs-lookup"><span data-stu-id="2e2bf-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="2e2bf-146">Visual Studio 2012 のホット タオル SPA テンプレートを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="2e2bf-147">Visual Studio 2012 のテンプレートとしてホット タオルをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="2e2bf-148">クリックするだけ`File`  |  `New Project`選択`ASP.NET MVC 4 Web Application`します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="2e2bf-149">選択し、' ホット タオル Single Page Application"テンプレートと実行。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="2e2bf-150">NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="2e2bf-151">ホットのタオルが空の既存の ASP.NET MVC プロジェクトを補足する NuGet パッケージもあります。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="2e2bf-152">Nuget を使用してをインストールし、実行します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="2e2bf-153">ホットのタオルの構築方法</span><span class="sxs-lookup"><span data-stu-id="2e2bf-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="2e2bf-154">単に開始のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-154">Simply start adding code!</span></span>

1. <span data-ttu-id="2e2bf-155">独自のサーバー側のコード (Breeze.js で真価) を Entity Framework では可能であれば、WebAPI の追加します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="2e2bf-156">ビューを追加、`App/views`フォルダー</span><span class="sxs-lookup"><span data-stu-id="2e2bf-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="2e2bf-157">Viewmodel の追加、`App/viewmodels`フォルダー</span><span class="sxs-lookup"><span data-stu-id="2e2bf-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="2e2bf-158">新しいビューへの HTML と Knockout のデータ バインディングを追加します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="2e2bf-159">ナビゲーションのルートを更新します。 `shell.js`</span><span class="sxs-lookup"><span data-stu-id="2e2bf-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="2e2bf-160">HTML または JavaScript のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="2e2bf-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="2e2bf-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2e2bf-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="2e2bf-162">index.cshtml は、開始のルートと MVC アプリケーション用のビューです。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="2e2bf-163">すべての標準のメタ タグ、css のリンク、および期待する JavaScript の参照が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="2e2bf-164">本文には、1 つが含まれています。`<div>`これは、すべてのコンテンツ (ビュー) が配置される要求されたとき。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="2e2bf-165">`@Scripts.Render` Require.js を使用して、含まれているアプリケーションのコードの開始ポイントを実行する、`main.js`ファイル。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="2e2bf-166">スプラッシュ スクリーンは、アプリが読み込まれるときに、スプラッシュ スクリーンを作成する方法を示すために提供されます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="2e2bf-167">App/main.js</span><span class="sxs-lookup"><span data-stu-id="2e2bf-167">App/main.js</span></span>

<span data-ttu-id="2e2bf-168">`main.js`ファイルには、アプリが読み込まれるとすぐに実行されるコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="2e2bf-169">これは、ナビゲーションのルートを定義、ビュー、スタートアップ設定、および任意のセットアップ/ブートス トラップなど、アプリケーションのデータを準備します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="2e2bf-170">`main.js`起動アプリケーションの開始を支援する durandal のモジュールのいくつかのファイルを定義します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="2e2bf-171">定義するステートメントは、関数の場合、利用できるように、モジュールの依存関係を解決に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="2e2bf-172">まず、デバッグ メッセージが有効なコンソール ウィンドウに、アプリケーションのパフォーマンスがどのようなイベントに関するメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="2e2bf-173">App.start のコードでは、durandal framework アプリケーションを起動するように指示します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="2e2bf-174">Durandal に知っているすべてのビューとビューモデルがそれぞれの名前付きの同じフォルダーに含まれるように、規則が設定されます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="2e2bf-175">最後に、`app.setRoot`読み込みが起動し、`shell`事前に定義されたを使用して`entrance`アニメーション。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="2e2bf-176">Views</span><span class="sxs-lookup"><span data-stu-id="2e2bf-176">Views</span></span>

<span data-ttu-id="2e2bf-177">内のビューにある、`App/views`フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="2e2bf-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="2e2bf-178">shell.html</span></span>

<span data-ttu-id="2e2bf-179">`shell.html` HTML のマスター レイアウトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="2e2bf-180">すべて、その他のビューの合成されるどこかの側で、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="2e2bf-181">ホットのタオルを提供します、`shell`でこのような 3 つのリージョン: ヘッダー、コンテンツ領域、およびフッターです。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="2e2bf-182">これらの各リージョンが読み込まれる内容が要求されたときに、その他のビューを形成します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="2e2bf-183">`compose`ヘッダーとフッターのバインドが困難をポイントするホット タオルでコーディング、`nav`と`footer`ビューをそれぞれします。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="2e2bf-184">セクションの compose バインド`#content`にバインドされて、`router`モジュールのアクティブな項目です。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="2e2bf-185">つまり、クリックしたときに対応するビューのナビゲーション リンクがこの領域に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="2e2bf-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="2e2bf-186">nav.html</span></span>

<span data-ttu-id="2e2bf-187">`nav.html` SPA のナビゲーション リンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="2e2bf-188">これは、メニュー構造を配置できる場所、たとえばです。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="2e2bf-189">多くの場合 (Knockout を使用) にバインドされたデータには、`router`モジュールで定義されているナビゲーションを表示する、`shell.js`します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="2e2bf-190">Knockout はデータ バインド属性し、バインドに、 `shell` viewmodel ナビゲーション ルートを表示して (Twitter Bootstrap を使用して) プログレス バーを表示する場合、`router`モジュールが (参照別に 1 つのビューから移動中です`router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="2e2bf-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="2e2bf-191">home.html と details.html</span><span class="sxs-lookup"><span data-stu-id="2e2bf-191">home.html and details.html</span></span>

<span data-ttu-id="2e2bf-192">これらのビューには、カスタム ビューの HTML が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="2e2bf-193">ときに、`home`のリンクを`nav`ビューのメニューをクリックすると、`home`ビューのコンテンツ エリアに格納されます、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="2e2bf-194">これらのビューは拡張、または独自のカスタム ビューに置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="2e2bf-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="2e2bf-195">footer.html</span></span>

<span data-ttu-id="2e2bf-196">`footer.html`の下部に、フッターに表示される HTML が含まれています、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="2e2bf-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="2e2bf-197">ViewModels</span></span>

<span data-ttu-id="2e2bf-198">Viewmodel がある、`App/viewmodels`フォルダー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="2e2bf-199">shell.js</span><span class="sxs-lookup"><span data-stu-id="2e2bf-199">shell.js</span></span>

<span data-ttu-id="2e2bf-200">`shell` Viewmodel にはプロパティおよびにバインドされている関数が含まれています、`shell`ビュー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="2e2bf-201">メニュー ナビゲーションのバインドがある多くの場合は (を参照してください、`router.mapNav`ロジック)。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="2e2bf-202">home.js と details.js</span><span class="sxs-lookup"><span data-stu-id="2e2bf-202">home.js and details.js</span></span>

<span data-ttu-id="2e2bf-203">これらの viewmodel にバインドされている関数とプロパティを含む、`home`ビュー。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="2e2bf-204">ビューのプレゼンテーション ロジックが含まれていて、データとビューの間の結び付きは、します。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="2e2bf-205">Services</span><span class="sxs-lookup"><span data-stu-id="2e2bf-205">Services</span></span>

<span data-ttu-id="2e2bf-206">サービスは、アプリ/サービス フォルダーで表示されます。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="2e2bf-207">理想的には、取得、およびリモート データを送信するため、dataservice モジュールなど、将来のサービスを配置する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="2e2bf-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="2e2bf-208">logger.js</span></span>

<span data-ttu-id="2e2bf-209">ホットのタオルの提供、`logger`サービス フォルダー内のモジュール。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="2e2bf-210">`logger`モジュールは、ログ メッセージをコンソールとトーストをポップアップで、ユーザーに最適です。</span><span class="sxs-lookup"><span data-stu-id="2e2bf-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
