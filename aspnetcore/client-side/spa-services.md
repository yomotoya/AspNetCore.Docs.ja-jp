---
title: ASP.NET Core でのシングル ページ アプリケーションを作成するのに JavaScriptServices を使用します。
author: scottaddie
description: ASP.NET Core でサポートされるシングル ページ アプリケーション (SPA) を作成する JavaScriptServices の使用の利点について説明します。
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: ee772e67ef14608bcc6e3498ade00424ff6090e5
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121377"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="584d2-103">ASP.NET Core でのシングル ページ アプリケーションを作成するのに JavaScriptServices を使用します。</span><span class="sxs-lookup"><span data-stu-id="584d2-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="584d2-104">によって[Scott Addie](https://github.com/scottaddie)と[Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="584d2-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="584d2-105">シングル ページ アプリケーション (SPA) は、その固有の機能豊富なユーザー エクスペリエンスのための web アプリケーションの人気のある型です。</span><span class="sxs-lookup"><span data-stu-id="584d2-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="584d2-106">クライアント側 SPA フレームワークやライブラリの統合など[Angular](https://angular.io/)または[React](https://facebook.github.io/react/)、ASP.NET Core は難しいようにサーバー側のフレームワークを使用します。</span><span class="sxs-lookup"><span data-stu-id="584d2-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="584d2-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices)統合プロセスの手間を削減するために開発されました。</span><span class="sxs-lookup"><span data-stu-id="584d2-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="584d2-108">これにより、別のクライアントおよびサーバー テクノロジ スタックとの間のシームレスな操作ができます。</span><span class="sxs-lookup"><span data-stu-id="584d2-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="584d2-109">JavaScriptServices とは</span><span class="sxs-lookup"><span data-stu-id="584d2-109">What is JavaScriptServices</span></span>

<span data-ttu-id="584d2-110">JavaScriptServices は、ASP.NET Core 用のクライアント側のテクノロジのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="584d2-110">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="584d2-111">その目的は、Spa を構築するための開発者の推奨されるサーバー側プラットフォームとしての ASP.NET Core の位置です。</span><span class="sxs-lookup"><span data-stu-id="584d2-111">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="584d2-112">JavaScriptServices は、次の 3 つの個別の NuGet パッケージで構成されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-112">JavaScriptServices consists of three distinct NuGet packages:</span></span>

* <span data-ttu-id="584d2-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="584d2-113">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="584d2-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="584d2-114">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="584d2-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="584d2-115">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="584d2-116">これらのパッケージは便利な場合します。</span><span class="sxs-lookup"><span data-stu-id="584d2-116">These packages are useful if you:</span></span>

* <span data-ttu-id="584d2-117">サーバーでの JavaScript を実行します。</span><span class="sxs-lookup"><span data-stu-id="584d2-117">Run JavaScript on the server</span></span>
* <span data-ttu-id="584d2-118">SPA フレームワークやライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="584d2-118">Use a SPA framework or library</span></span>
* <span data-ttu-id="584d2-119">Webpack とクライアント側アセットをビルドします。</span><span class="sxs-lookup"><span data-stu-id="584d2-119">Build client-side assets with Webpack</span></span>

<span data-ttu-id="584d2-120">SpaServices パッケージを使用してこの記事内のフォーカスの多くは配置されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-120">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="584d2-121">SpaServices とは</span><span class="sxs-lookup"><span data-stu-id="584d2-121">What is SpaServices</span></span>

<span data-ttu-id="584d2-122">SpaServices は、Spa を構築するための開発者の推奨されるサーバー側プラットフォームとしての ASP.NET Core の位置に作成されました。</span><span class="sxs-lookup"><span data-stu-id="584d2-122">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="584d2-123">SpaServices は ASP.NET core で Spa を開発する必要はありませんし、特定のクライアント フレームワークにロックしません。</span><span class="sxs-lookup"><span data-stu-id="584d2-123">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="584d2-124">SpaServices は、次のような便利なインフラストラクチャを提供します。</span><span class="sxs-lookup"><span data-stu-id="584d2-124">SpaServices provides useful infrastructure such as:</span></span>

* [<span data-ttu-id="584d2-125">サーバー側の事前</span><span class="sxs-lookup"><span data-stu-id="584d2-125">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="584d2-126">Webpack 開発ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="584d2-126">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="584d2-127">ホットなモジュールの交換</span><span class="sxs-lookup"><span data-stu-id="584d2-127">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="584d2-128">ルーティングのヘルパー</span><span class="sxs-lookup"><span data-stu-id="584d2-128">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="584d2-129">総称して、これらのインフラストラクチャ コンポーネントは、開発ワークフローと、実行時のエクスペリエンスの両方を強化します。</span><span class="sxs-lookup"><span data-stu-id="584d2-129">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="584d2-130">コンポーネントを個別に採用することができます。</span><span class="sxs-lookup"><span data-stu-id="584d2-130">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="584d2-131">SpaServices を使用するための前提条件</span><span class="sxs-lookup"><span data-stu-id="584d2-131">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="584d2-132">SpaServices を使用するには、次のようにインストールします。</span><span class="sxs-lookup"><span data-stu-id="584d2-132">To work with SpaServices, install the following:</span></span>

* <span data-ttu-id="584d2-133">[Node.js](https://nodejs.org/) (バージョン 6 以降) で npm</span><span class="sxs-lookup"><span data-stu-id="584d2-133">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="584d2-134">これらのコンポーネントがインストールされを検出できることを確認するには、コマンドラインから、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="584d2-134">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="584d2-135">注: Azure の web サイトにデプロイする場合不要ここで何もする&mdash;Node.js がインストールされ、サーバー環境で使用できます。</span><span class="sxs-lookup"><span data-stu-id="584d2-135">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="584d2-136">Visual Studio 2017 を使用して Windows の場合は、SDK がインストールを選択して、 **.NET Core クロス プラットフォーム開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="584d2-136">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="584d2-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="584d2-137">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="584d2-138">サーバー側の事前</span><span class="sxs-lookup"><span data-stu-id="584d2-138">Server-side prerendering</span></span>

<span data-ttu-id="584d2-139">ユニバーサル (isomorphic とも呼ばれます) のアプリケーションは、サーバーとクライアントの両方で実行できる JavaScript アプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="584d2-139">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="584d2-140">Angular、React、およびその他の一般的なフレームワークは、このアプリケーションの開発スタイルのユニバーサル プラットフォームを提供します。</span><span class="sxs-lookup"><span data-stu-id="584d2-140">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="584d2-141">考え方は、まず、Node.js を使用して、サーバー上のフレームワーク コンポーネントをレンダリングし、さらに、クライアントを実行しを委任します。</span><span class="sxs-lookup"><span data-stu-id="584d2-141">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="584d2-142">ASP.NET Core[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)によって提供される SpaServices、サーバー上の JavaScript 関数を呼び出すことによってサーバー側の事前の実装を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="584d2-142">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="584d2-143">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="584d2-143">Prerequisites</span></span>

<span data-ttu-id="584d2-144">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="584d2-144">Install the following:</span></span>

* <span data-ttu-id="584d2-145">[aspnet 事前](https://www.npmjs.com/package/aspnet-prerendering)npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="584d2-145">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="584d2-146">構成</span><span class="sxs-lookup"><span data-stu-id="584d2-146">Configuration</span></span>

<span data-ttu-id="584d2-147">タグ ヘルパーは、プロジェクトの名前空間の登録を使用して探索可能にされて *_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="584d2-147">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="584d2-148">これらのタグ ヘルパーは Razor ビュー内の HTML のような構文を活用することで、低レベルの Api と直接通信の複雑さで抽象化します。</span><span class="sxs-lookup"><span data-stu-id="584d2-148">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="584d2-149">`asp-prerender-module`タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="584d2-149">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="584d2-150">`asp-prerender-module`タグ ヘルパーの前のコード例で使用される実行*ClientApp/dist/main-server.js* Node.js を使用してサーバーにします。</span><span class="sxs-lookup"><span data-stu-id="584d2-150">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="584d2-151">わかりやすくするためのために、 *main server.js*ファイルは、TypeScript、JavaScript にトランス パイルもタスクの成果物、 [Webpack](http://webpack.github.io/)プロセスを構築します。</span><span class="sxs-lookup"><span data-stu-id="584d2-151">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="584d2-152">Webpack のエントリ ポイントのエイリアスを定義します`main-server`; と、このエイリアスの依存関係グラフのトラバーサルが始まり、 *ClientApp/ブート-server.ts*ファイル。</span><span class="sxs-lookup"><span data-stu-id="584d2-152">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="584d2-153">次の Angular の例では、 *ClientApp/ブート-server.ts*ファイルを利用、`createServerRenderer`関数と`RenderResult`の入力、 `aspnet-prerendering` npm パッケージを Node.js を使用してサーバー レンダリングを構成します。</span><span class="sxs-lookup"><span data-stu-id="584d2-153">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="584d2-154">サーバー側のレンダリングが厳密に型指定された JavaScript にラップされて解決関数の呼び出しに渡される宛ての HTML マークアップ`Promise`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="584d2-154">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="584d2-155">`Promise`オブジェクトの重要性は、DOM のプレース ホルダー要素の挿入のページに HTML マークアップを非同期的に提供します。</span><span class="sxs-lookup"><span data-stu-id="584d2-155">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="584d2-156">`asp-prerender-data`タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="584d2-156">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="584d2-157">組み合わせると、`asp-prerender-module`タグ ヘルパーの`asp-prerender-data`タグ ヘルパーは、Razor ビューからサーバー側 JavaScript にコンテキスト情報を渡すために使用できます。</span><span class="sxs-lookup"><span data-stu-id="584d2-157">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="584d2-158">たとえば、次のマークアップは合格ユーザー データを`main-server`モジュール。</span><span class="sxs-lookup"><span data-stu-id="584d2-158">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="584d2-159">受信した`UserName`引数の組み込みの JSON シリアライザーを使用してシリアル化し、は、`params.data`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="584d2-159">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="584d2-160">次の例で Angular 内でパーソナライズされたあいさつ文を構築するデータの使用、`h1`要素。</span><span class="sxs-lookup"><span data-stu-id="584d2-160">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="584d2-161">注: タグ ヘルパーで渡されたプロパティの名前を付けて表されます**PascalCase**表記します。</span><span class="sxs-lookup"><span data-stu-id="584d2-161">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="584d2-162">同じプロパティ名で表現は JavaScript とは異なり**camelCase**します。</span><span class="sxs-lookup"><span data-stu-id="584d2-162">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="584d2-163">既定の JSON シリアル化の構成では、この違いを担当します。</span><span class="sxs-lookup"><span data-stu-id="584d2-163">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="584d2-164">展開すると、上記のコード例に、データ渡し可能サーバーからビューに hydrating、`globals`プロパティに提供される、`resolve`関数。</span><span class="sxs-lookup"><span data-stu-id="584d2-164">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="584d2-165">`postList`配列内で定義されている、`globals`オブジェクトはグローバルで、ブラウザーにアタッチされて`window`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="584d2-165">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="584d2-166">グローバル スコープにこの変数のホイストでは、特に、サーバーで 1 回と、クライアントでは、同じデータの読み込みに関連している、作業の重複がなくなります。</span><span class="sxs-lookup"><span data-stu-id="584d2-166">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![ウィンドウ オブジェクトにアタッチされているグローバルの postList 変数](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="584d2-168">Webpack 開発ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="584d2-168">Webpack Dev Middleware</span></span>

<span data-ttu-id="584d2-169">[Webpack 開発ミドルウェア](https://webpack.github.io/docs/webpack-dev-middleware.html)Webpack がオンデマンドでリソースをビルドするための合理的な開発ワークフローが導入されています。</span><span class="sxs-lookup"><span data-stu-id="584d2-169">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="584d2-170">ミドルウェアが自動的にコンパイルし、ページがブラウザーに再読み込みする際、クライアント側のリソースの機能します。</span><span class="sxs-lookup"><span data-stu-id="584d2-170">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="584d2-171">別の方法では、サードパーティの依存関係またはカスタム コードが変更されたときに、プロジェクトの npm ビルド スクリプトを使用して Webpack を手動で起動します。</span><span class="sxs-lookup"><span data-stu-id="584d2-171">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="584d2-172">Npm スクリプトを作成する、 *package.json*ファイルは、次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="584d2-172">An npm build script in the *package.json* file is shown in the following example:</span></span>

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a><span data-ttu-id="584d2-173">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="584d2-173">Prerequisites</span></span>

<span data-ttu-id="584d2-174">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="584d2-174">Install the following:</span></span>

* <span data-ttu-id="584d2-175">[aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="584d2-175">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="584d2-176">構成</span><span class="sxs-lookup"><span data-stu-id="584d2-176">Configuration</span></span>

<span data-ttu-id="584d2-177">次のコードを使用して HTTP 要求パイプラインに Webpack 開発ミドルウェアが登録されている、 *Startup.cs*ファイルの`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="584d2-177">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

<span data-ttu-id="584d2-178">`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります[静的ファイルをホストしている登録](xref:fundamentals/static-files)を使用して、`UseStaticFiles`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="584d2-178">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="584d2-179">セキュリティ上の理由から、アプリは開発モードで実行時にのみ、ミドルウェアを登録します。</span><span class="sxs-lookup"><span data-stu-id="584d2-179">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="584d2-180">*Webpack.config.js*ファイルの`output.publicPath`プロパティに通知を監視するミドルウェア、`dist`フォルダーの変更。</span><span class="sxs-lookup"><span data-stu-id="584d2-180">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="584d2-181">ホットなモジュールの交換</span><span class="sxs-lookup"><span data-stu-id="584d2-181">Hot Module Replacement</span></span>

<span data-ttu-id="584d2-182">Webpack の考える[ホット モジュールの交換](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 機能の進化したものとして[Webpack 開発ミドルウェア](#webpack-dev-middleware)します。</span><span class="sxs-lookup"><span data-stu-id="584d2-182">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="584d2-183">すべて同じメリットが導入されて HMR が自動的に変更をコンパイルした後、ページのコンテンツを更新することでさらに、開発ワークフローを効率化します。</span><span class="sxs-lookup"><span data-stu-id="584d2-183">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="584d2-184">メモリ内の現在の状態と、SPA のデバッグ セッションに支障をきたすのブラウザーの更新でこれを混同しないでください。</span><span class="sxs-lookup"><span data-stu-id="584d2-184">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="584d2-185">Webpack 開発ミドルウェア サービスと、ブラウザーに変更がプッシュされることを意味すると、ブラウザーの間のライブ リンクがあります。</span><span class="sxs-lookup"><span data-stu-id="584d2-185">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="584d2-186">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="584d2-186">Prerequisites</span></span>

<span data-ttu-id="584d2-187">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="584d2-187">Install the following:</span></span>

* <span data-ttu-id="584d2-188">[webpack ホット ミドルウェア](https://www.npmjs.com/package/webpack-hot-middleware)npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="584d2-188">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="584d2-189">構成</span><span class="sxs-lookup"><span data-stu-id="584d2-189">Configuration</span></span>

<span data-ttu-id="584d2-190">MVC の HTTP 要求パイプラインに HMR コンポーネントを登録する必要があります、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="584d2-190">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="584d2-191">そうであったとして[Webpack 開発ミドルウェア](#webpack-dev-middleware)、`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります、`UseStaticFiles`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="584d2-191">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="584d2-192">セキュリティ上の理由から、アプリは開発モードで実行時にのみ、ミドルウェアを登録します。</span><span class="sxs-lookup"><span data-stu-id="584d2-192">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="584d2-193">*Webpack.config.js*ファイルを定義する必要があります、`plugins`場合でも、それを空の配列。</span><span class="sxs-lookup"><span data-stu-id="584d2-193">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="584d2-194">ブラウザーでアプリを読み込んだ後は、開発者ツールのコンソール タブは、HMR アクティブ化の確認を提供します。</span><span class="sxs-lookup"><span data-stu-id="584d2-194">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![ホットのモジュールの交換接続メッセージ](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="584d2-196">ルーティングのヘルパー</span><span class="sxs-lookup"><span data-stu-id="584d2-196">Routing helpers</span></span>

<span data-ttu-id="584d2-197">ほとんどの ASP.NET Core ベースの Spa では、サーバー側でルーティングだけでなくクライアント側でルーティングを必要があります。</span><span class="sxs-lookup"><span data-stu-id="584d2-197">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="584d2-198">SPA と MVC ルーティング システムは、競合することがなく個別に作業できます。</span><span class="sxs-lookup"><span data-stu-id="584d2-198">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="584d2-199">ただし、課題を 1 つエッジ ケース読んだり: 404 の HTTP 応答を識別します。</span><span class="sxs-lookup"><span data-stu-id="584d2-199">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="584d2-200">シナリオの場合を検討してください、拡張子のないルートの`/some/page`使用されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-200">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="584d2-201">要求がパターン一致、サーバー側ルートがそのパターンのクライアント側ルートが一致するものとします。</span><span class="sxs-lookup"><span data-stu-id="584d2-201">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="584d2-202">ここでの受信要求を考えてみます`/images/user-512.png`、一般に、サーバー上の画像ファイルを検索する要求。</span><span class="sxs-lookup"><span data-stu-id="584d2-202">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="584d2-203">クライアント側のアプリケーションは処理は、ほとんどありません任意のサーバー側のルートまたは静的ファイルにその要求されたリソースのパスが一致しない場合、一般に HTTP 404 状態コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="584d2-203">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="584d2-204">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="584d2-204">Prerequisites</span></span>

<span data-ttu-id="584d2-205">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="584d2-205">Install the following:</span></span>

* <span data-ttu-id="584d2-206">クライアント側ルーティング npm パッケージです。</span><span class="sxs-lookup"><span data-stu-id="584d2-206">The client-side routing npm package.</span></span> <span data-ttu-id="584d2-207">Angular を使用して、例として。</span><span class="sxs-lookup"><span data-stu-id="584d2-207">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="584d2-208">構成</span><span class="sxs-lookup"><span data-stu-id="584d2-208">Configuration</span></span>

<span data-ttu-id="584d2-209">という名前の拡張メソッド`MapSpaFallbackRoute`で使用されて、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="584d2-209">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

<span data-ttu-id="584d2-210">ヒント: ルートは、構成されている順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-210">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="584d2-211">その結果、`default`パターン一致の前のコード例ではルートが最初に使用します。</span><span class="sxs-lookup"><span data-stu-id="584d2-211">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="584d2-212">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="584d2-212">Creating a new project</span></span>

<span data-ttu-id="584d2-213">JavaScriptServices には、事前構成済みのアプリケーション テンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="584d2-213">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="584d2-214">SpaServices は、さまざまなフレームワークおよびライブラリと、Angular、React、Redux などと連携して、これらのテンプレートで使用されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-214">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="584d2-215">これらのテンプレートは、次のコマンドを実行して .NET Core CLI を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="584d2-215">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="584d2-216">使用可能な SPA テンプレートの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-216">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="584d2-217">テンプレート</span><span class="sxs-lookup"><span data-stu-id="584d2-217">Templates</span></span>                                 | <span data-ttu-id="584d2-218">短い形式の名前</span><span class="sxs-lookup"><span data-stu-id="584d2-218">Short Name</span></span> | <span data-ttu-id="584d2-219">言語</span><span class="sxs-lookup"><span data-stu-id="584d2-219">Language</span></span> | <span data-ttu-id="584d2-220">Tags</span><span class="sxs-lookup"><span data-stu-id="584d2-220">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="584d2-221">Angular と ASP.NET Core の MVC</span><span class="sxs-lookup"><span data-stu-id="584d2-221">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="584d2-222">angular</span><span class="sxs-lookup"><span data-stu-id="584d2-222">angular</span></span>    | <span data-ttu-id="584d2-223">[C#]</span><span class="sxs-lookup"><span data-stu-id="584d2-223">[C#]</span></span>     | <span data-ttu-id="584d2-224">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="584d2-224">Web/MVC/SPA</span></span> |
| <span data-ttu-id="584d2-225">React.js 付きの ASP.NET Core の MVC</span><span class="sxs-lookup"><span data-stu-id="584d2-225">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="584d2-226">react</span><span class="sxs-lookup"><span data-stu-id="584d2-226">react</span></span>      | <span data-ttu-id="584d2-227">[C#]</span><span class="sxs-lookup"><span data-stu-id="584d2-227">[C#]</span></span>     | <span data-ttu-id="584d2-228">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="584d2-228">Web/MVC/SPA</span></span> |
| <span data-ttu-id="584d2-229">MVC の ASP.NET Core react.js と Redux</span><span class="sxs-lookup"><span data-stu-id="584d2-229">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="584d2-230">reactredux</span><span class="sxs-lookup"><span data-stu-id="584d2-230">reactredux</span></span> | <span data-ttu-id="584d2-231">[C#]</span><span class="sxs-lookup"><span data-stu-id="584d2-231">[C#]</span></span>     | <span data-ttu-id="584d2-232">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="584d2-232">Web/MVC/SPA</span></span> |

<span data-ttu-id="584d2-233">SPA テンプレートのいずれかを使用して新しいプロジェクトを作成するには含める、**短い名前**内のテンプレートの[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。</span><span class="sxs-lookup"><span data-stu-id="584d2-233">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="584d2-234">次のコマンドでは、サーバー側用に構成された ASP.NET Core MVC で Angular アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="584d2-234">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="584d2-235">ランタイムの構成モードを設定します。</span><span class="sxs-lookup"><span data-stu-id="584d2-235">Set the runtime configuration mode</span></span>

<span data-ttu-id="584d2-236">2 つのプライマリのランタイム構成モードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="584d2-236">Two primary runtime configuration modes exist:</span></span>

* <span data-ttu-id="584d2-237">**開発**:</span><span class="sxs-lookup"><span data-stu-id="584d2-237">**Development**:</span></span>
  * <span data-ttu-id="584d2-238">デバッグを容易にするソース マップが含まれています。</span><span class="sxs-lookup"><span data-stu-id="584d2-238">Includes source maps to ease debugging.</span></span>
  * <span data-ttu-id="584d2-239">パフォーマンスのクライアント側のコードを最適化しません。</span><span class="sxs-lookup"><span data-stu-id="584d2-239">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="584d2-240">**実稼働**:</span><span class="sxs-lookup"><span data-stu-id="584d2-240">**Production**:</span></span>
  * <span data-ttu-id="584d2-241">ソース マップを除外します。</span><span class="sxs-lookup"><span data-stu-id="584d2-241">Excludes source maps.</span></span>
  * <span data-ttu-id="584d2-242">バンドルと縮小を使用してクライアント側のコードを最適化します。</span><span class="sxs-lookup"><span data-stu-id="584d2-242">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="584d2-243">ASP.NET Core という環境変数を使用して`ASPNETCORE_ENVIRONMENT`構成モードを格納します。</span><span class="sxs-lookup"><span data-stu-id="584d2-243">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="584d2-244">参照してください **[環境を設定](xref:fundamentals/environments#set-the-environment)** 詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="584d2-244">See **[Set the environment](xref:fundamentals/environments#set-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="584d2-245">.NET Core CLI で実行します。</span><span class="sxs-lookup"><span data-stu-id="584d2-245">Running with .NET Core CLI</span></span>

<span data-ttu-id="584d2-246">プロジェクトのルートで、次のコマンドを実行してには、必要な NuGet と npm パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="584d2-246">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="584d2-247">ビルドし、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="584d2-247">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="584d2-248">アプリケーションの起動時に従い、localhost 上で、[ランタイム構成モード](#runtime-config-mode)します。</span><span class="sxs-lookup"><span data-stu-id="584d2-248">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="584d2-249">移動する`http://localhost:5000`ブラウザーにランディング ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-249">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="584d2-250">Visual Studio 2017 での実行</span><span class="sxs-lookup"><span data-stu-id="584d2-250">Running with Visual Studio 2017</span></span>

<span data-ttu-id="584d2-251">開く、 *.csproj*によって生成されたファイル、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。</span><span class="sxs-lookup"><span data-stu-id="584d2-251">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="584d2-252">必要な NuGet と npm パッケージは、プロジェクトを開くと自動的に復元されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-252">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="584d2-253">この復元プロセスは、数分かかる場合があり、アプリケーションが完了するときに実行する準備ができます。</span><span class="sxs-lookup"><span data-stu-id="584d2-253">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="584d2-254">キーを押して、緑色の実行 ボタンをクリックします`Ctrl + F5`、し、アプリケーションのランディング ページで、ブラウザーが開かれます。</span><span class="sxs-lookup"><span data-stu-id="584d2-254">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="584d2-255">アプリケーションによる localhost で実行、[ランタイム構成モード](#runtime-config-mode)します。</span><span class="sxs-lookup"><span data-stu-id="584d2-255">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span>

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="584d2-256">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="584d2-256">Testing the app</span></span>

<span data-ttu-id="584d2-257">SpaServices テンプレートは、事前構成を使用してクライアント側のテストを実行して[Karma](https://karma-runner.github.io/1.0/index.html)と[Jasmine](https://jasmine.github.io/)します。</span><span class="sxs-lookup"><span data-stu-id="584d2-257">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="584d2-258">Jasmine は、Karma テスト ランナーはこれらのテストには、人気のある単位の JavaScript 用のテスト フレームワークです。</span><span class="sxs-lookup"><span data-stu-id="584d2-258">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="584d2-259">Karma を使用するように構成、 [Webpack 開発ミドルウェア](#webpack-dev-middleware)されるよう、開発者が停止し、変更されるたびにテストを実行するために必要はありません。</span><span class="sxs-lookup"><span data-stu-id="584d2-259">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="584d2-260">テスト_ケースまたはテスト ケース自体に対して実行されるコードであるかどうか、テストが自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-260">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="584d2-261">Angular アプリケーションを使用して、例として、2 つの Jasmine テスト_ケースが既に提供されている、`CounterComponent`で、 *counter.component.spec.ts*ファイル。</span><span class="sxs-lookup"><span data-stu-id="584d2-261">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="584d2-262">コマンド プロンプトを開き、 *ClientApp*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="584d2-262">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="584d2-263">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="584d2-263">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="584d2-264">スクリプトで定義されている設定を読み取り、Karma テスト ランナーを起動する、 *karma.conf.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="584d2-264">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="584d2-265">その他の設定の間で、 *karma.conf.js*経由で実行するテスト ファイルを識別します。 その`files`配列。</span><span class="sxs-lookup"><span data-stu-id="584d2-265">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="584d2-266">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="584d2-266">Publishing the application</span></span>

<span data-ttu-id="584d2-267">デプロイの準備完了パッケージに生成されたクライアント側アセットと発行された ASP.NET Core の成果物を組み合わせることは面倒なことができます。</span><span class="sxs-lookup"><span data-stu-id="584d2-267">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="584d2-268">さいわいにも、SpaServices がという名前のカスタム MSBuild ターゲットとそのパブリケーション全体のプロセスを調整`RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="584d2-268">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="584d2-269">MSBuild ターゲットでは、次の責任があります。</span><span class="sxs-lookup"><span data-stu-id="584d2-269">The MSBuild target has the following responsibilities:</span></span>

1. <span data-ttu-id="584d2-270">Npm パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="584d2-270">Restore the npm packages</span></span>
1. <span data-ttu-id="584d2-271">サード パーティ製のクライアント側の資産の運用グレードのビルドを作成します。</span><span class="sxs-lookup"><span data-stu-id="584d2-271">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="584d2-272">カスタムのクライアント側の資産の運用グレードのビルドを作成します。</span><span class="sxs-lookup"><span data-stu-id="584d2-272">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="584d2-273">Webpack で生成された資産を発行フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="584d2-273">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="584d2-274">MSBuild ターゲットが実行するときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="584d2-274">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="584d2-275">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="584d2-275">Additional resources</span></span>

* [<span data-ttu-id="584d2-276">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="584d2-276">Angular Docs</span></span>](https://angular.io/docs)
