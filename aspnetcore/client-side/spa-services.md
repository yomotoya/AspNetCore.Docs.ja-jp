---
title: "JavaScriptServices を使用して ASP.NET Core の単一ページ アプリケーションを作成するには"
author: scottaddie
description: "ASP.NET Core 裏付け単一ページ アプリケーション (SPA) を作成する JavaScriptServices を使用する利点について説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="7919f-103">JavaScriptServices を使用して ASP.NET Core の単一ページ アプリケーションを作成するには</span><span class="sxs-lookup"><span data-stu-id="7919f-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="7919f-104">によって[Scott Addie](https://github.com/scottaddie)と[Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="7919f-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="7919f-105">単一ページ アプリケーション (SPA) は、その固有の機能豊富なユーザー エクスペリエンスのための web アプリケーションの一般的な型です。</span><span class="sxs-lookup"><span data-stu-id="7919f-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="7919f-106">クライアント側 SPA フレームワークまたはライブラリを統合するよう[角](https://angular.io/)または[反応](https://facebook.github.io/react/)、ASP.NET Core を困難になる可能性と同じようにサーバー側フレームワークでします。</span><span class="sxs-lookup"><span data-stu-id="7919f-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="7919f-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices)を統合プロセスで摩擦を減らすために開発されました。</span><span class="sxs-lookup"><span data-stu-id="7919f-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="7919f-108">異なるクライアント/サーバー テクノロジ スタックとの間のシームレスな操作を可能になります。</span><span class="sxs-lookup"><span data-stu-id="7919f-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="7919f-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7919f-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="7919f-110">JavaScriptServices とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="7919f-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="7919f-111">JavaScriptServices は、ASP.NET Core 用のクライアント側のテクノロジのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="7919f-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="7919f-112">その目的は SPAs を構築するための開発者の推奨されるサーバー側のプラットフォームとして ASP.NET Core を配置します。</span><span class="sxs-lookup"><span data-stu-id="7919f-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="7919f-113">次の 3 つの個別の NuGet パッケージの JavaScriptServices で構成されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="7919f-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="7919f-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="7919f-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="7919f-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="7919f-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="7919f-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="7919f-117">これらのパッケージは役立つ場合します。</span><span class="sxs-lookup"><span data-stu-id="7919f-117">These packages are useful if you:</span></span>
* <span data-ttu-id="7919f-118">サーバーでの JavaScript を実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="7919f-119">SPA フレームワークまたはライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="7919f-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="7919f-120">Webpack 使用したアセットのクライアント側をビルドします。</span><span class="sxs-lookup"><span data-stu-id="7919f-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="7919f-121">この記事でフォーカスの多くは、SpaServices パッケージを使用してに配置されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="7919f-122">SpaServices とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="7919f-122">What is SpaServices?</span></span>

<span data-ttu-id="7919f-123">SpaServices は SPAs を構築するための開発者の推奨されるサーバー側のプラットフォームとして ASP.NET Core の位置に作成されました。</span><span class="sxs-lookup"><span data-stu-id="7919f-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="7919f-124">SpaServices が ASP.NET Core と SPAs を開発するため必要はありませんし、特定のクライアント フレームワークに頼る必要ありません。</span><span class="sxs-lookup"><span data-stu-id="7919f-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="7919f-125">SpaServices は、次のような便利インフラストラクチャを提供します。</span><span class="sxs-lookup"><span data-stu-id="7919f-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="7919f-126">サーバー側の事前</span><span class="sxs-lookup"><span data-stu-id="7919f-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="7919f-127">Webpack デベロッパー ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7919f-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="7919f-128">ホット モジュールの交換</span><span class="sxs-lookup"><span data-stu-id="7919f-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="7919f-129">ルーティングのヘルパー</span><span class="sxs-lookup"><span data-stu-id="7919f-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="7919f-130">集合的に、これらのインフラストラクチャ コンポーネントは、開発ワークフローと、実行時のエクスペリエンスの両方を高めます。</span><span class="sxs-lookup"><span data-stu-id="7919f-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="7919f-131">コンポーネントを個別に適用することができます。</span><span class="sxs-lookup"><span data-stu-id="7919f-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="7919f-132">SpaServices を使用するための前提条件</span><span class="sxs-lookup"><span data-stu-id="7919f-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="7919f-133">SpaServices を操作するには次のようにインストールします。</span><span class="sxs-lookup"><span data-stu-id="7919f-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="7919f-134">[Node.js](https://nodejs.org/) (version 6 以降) npm で</span><span class="sxs-lookup"><span data-stu-id="7919f-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="7919f-135">これらのコンポーネントがインストールされているし、見つかることを確認するには、コマンドラインから、次を実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="7919f-136">注: を Azure の web サイトに配置する場合は、する必要はありませんここでは何も操作&mdash;Node.js がインストールされ、サーバー環境で使用できます。</span><span class="sxs-lookup"><span data-stu-id="7919f-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="7919f-137">[.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (またはそれ以降)</span><span class="sxs-lookup"><span data-stu-id="7919f-137">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="7919f-138">Visual Studio 2017 を選択してインストールする Windows の場合は、この**.NET Core クロスプラット フォーム開発**ワークロード。</span><span class="sxs-lookup"><span data-stu-id="7919f-138">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="7919f-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="7919f-139">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="7919f-140">サーバー側の事前</span><span class="sxs-lookup"><span data-stu-id="7919f-140">Server-side prerendering</span></span>

<span data-ttu-id="7919f-141">ユニバーサル (型とも呼ばれる) アプリケーションは、JavaScript アプリケーションが実行されている両方のサーバーとクライアントの対応です。</span><span class="sxs-lookup"><span data-stu-id="7919f-141">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="7919f-142">角、対応、およびその他の一般的なフレームワークは、このアプリケーションの開発スタイルのユニバーサル プラットフォームを提供します。</span><span class="sxs-lookup"><span data-stu-id="7919f-142">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="7919f-143">つまり、まず、Node.js を使用してサーバー上のフレームワーク コンポーネントを表示し、さらに、クライアントを実行しを委任します。</span><span class="sxs-lookup"><span data-stu-id="7919f-143">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="7919f-144">ASP.NET Core[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)によって提供される SpaServices サーバーに使用する JavaScript 関数を呼び出すことによってサーバー側の事前の実装を簡素化します。</span><span class="sxs-lookup"><span data-stu-id="7919f-144">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7919f-145">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7919f-145">Prerequisites</span></span>

<span data-ttu-id="7919f-146">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7919f-146">Install the following:</span></span>
* <span data-ttu-id="7919f-147">[aspnet 事前](https://www.npmjs.com/package/aspnet-prerendering)npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="7919f-147">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="7919f-148">構成</span><span class="sxs-lookup"><span data-stu-id="7919f-148">Configuration</span></span>

<span data-ttu-id="7919f-149">タグ ヘルパーは、プロジェクトの名前空間登録を使用して探索可能にされて*_ViewImports.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7919f-149">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="7919f-150">これらのタグ ヘルパーは、Razor ビュー内の HTML に似た構文を活用することで、低レベルの Api と直接通信の複雑さを抽象します。</span><span class="sxs-lookup"><span data-stu-id="7919f-150">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="7919f-151">`asp-prerender-module`ヘルパーにタグ付け</span><span class="sxs-lookup"><span data-stu-id="7919f-151">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="7919f-152">`asp-prerender-module`タグ ヘルパーに渡し、前のコード例で使用される実行*ClientApp/dist/main-server.js* Node.js を使用してサーバーにします。</span><span class="sxs-lookup"><span data-stu-id="7919f-152">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="7919f-153">わかりやすくするためのために、 *main server.js*ファイルが、TypeScript-JavaScript transpilation 内のタスクの成果物、 [Webpack](http://webpack.github.io/)プロセスをビルドします。</span><span class="sxs-lookup"><span data-stu-id="7919f-153">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="7919f-154">Webpack のエントリ ポイントのエイリアスを定義する`main-server`; でこのエイリアスの依存関係グラフの走査を開始し、 *ClientApp/ブート-server.ts*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7919f-154">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="7919f-155">次の角度の例で、 *ClientApp/ブート-server.ts*ファイルを利用、`createServerRenderer`関数と`RenderResult`の入力、 `aspnet-prerendering` Node.js を使用してサーバーのレンダリングを構成する npm パッケージです。</span><span class="sxs-lookup"><span data-stu-id="7919f-155">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="7919f-156">サーバー側のレンダリングが厳密に型指定された JavaScript にラップされて解決関数の呼び出しに渡される宛ての HTML マークアップ`Promise`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7919f-156">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="7919f-157">`Promise`オブジェクトの有意性がプレース ホルダーの DOM の要素でインジェクションのページに HTML マークアップを非同期的に提供します。</span><span class="sxs-lookup"><span data-stu-id="7919f-157">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="7919f-158">`asp-prerender-data`ヘルパーにタグ付け</span><span class="sxs-lookup"><span data-stu-id="7919f-158">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="7919f-159">組み合わせると、`asp-prerender-module`タグ ヘルパーの`asp-prerender-data`タグ ヘルパーは、Razor ビューから、サーバー側 JavaScript にコンテキスト情報を渡すために使用できます。</span><span class="sxs-lookup"><span data-stu-id="7919f-159">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="7919f-160">たとえば、次のマークアップ合格とユーザー データを`main-server`モジュール。</span><span class="sxs-lookup"><span data-stu-id="7919f-160">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="7919f-161">受信した`UserName`引数は、組み込みの JSON シリアライザーを使用してシリアル化されに格納されて、`params.data`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7919f-161">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="7919f-162">内の個人用に設定された応答メッセージを構築するために次の角度の例では、データが使用される、`h1`要素。</span><span class="sxs-lookup"><span data-stu-id="7919f-162">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="7919f-163">注: タグ ヘルパーで渡されるプロパティ名で表される**PascalCase**表記します。</span><span class="sxs-lookup"><span data-stu-id="7919f-163">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="7919f-164">JavaScript に対して、同じプロパティ名がで表されるコントラストを**キャメル ケース**です。</span><span class="sxs-lookup"><span data-stu-id="7919f-164">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="7919f-165">既定の JSON シリアル化の構成は、この違いを担当します。</span><span class="sxs-lookup"><span data-stu-id="7919f-165">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="7919f-166">前のコード例に展開するにデータをサーバーからに渡せるビュー hydrating によって、`globals`プロパティに提供される、`resolve`関数。</span><span class="sxs-lookup"><span data-stu-id="7919f-166">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="7919f-167">`postList`配列内で定義された、`globals`オブジェクトがアタッチされるブラウザーのグローバル`window`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7919f-167">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="7919f-168">このグローバル スコープの変数ホイスト サーバー上で実行し、もう一度クライアントでは、同じデータの読み込みに関連するように特に、作業の重複を除去します。</span><span class="sxs-lookup"><span data-stu-id="7919f-168">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![ウィンドウ オブジェクトにアタッチされているグローバル postList 変数](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="7919f-170">Webpack デベロッパー ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7919f-170">Webpack Dev Middleware</span></span>

<span data-ttu-id="7919f-171">[Webpack デベロッパー ミドルウェア](https://webpack.github.io/docs/webpack-dev-middleware.html)Webpack が要求時にリソースをビルドするという合理的な開発ワークフローが導入されています。</span><span class="sxs-lookup"><span data-stu-id="7919f-171">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="7919f-172">ミドルウェアは自動的にコンパイルして、ページがブラウザーで再読み込みされるときに、クライアント側のリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="7919f-172">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="7919f-173">別の方法では、サード パーティの依存関係またはカスタム コードが変更されたときに、プロジェクトの npm ビルド スクリプトを使用して Webpack を手動で起動します。</span><span class="sxs-lookup"><span data-stu-id="7919f-173">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="7919f-174">スクリプトの作成、npm、 *package.json*ファイルを次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="7919f-174">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="7919f-175">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7919f-175">Prerequisites</span></span>

<span data-ttu-id="7919f-176">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7919f-176">Install the following:</span></span>
* <span data-ttu-id="7919f-177">[aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="7919f-177">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="7919f-178">構成</span><span class="sxs-lookup"><span data-stu-id="7919f-178">Configuration</span></span>

<span data-ttu-id="7919f-179">次のコードを使用して HTTP 要求パイプラインに Webpack デベロッパー ミドルウェアが登録されている、 *Startup.cs*ファイルの`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7919f-179">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="7919f-180">`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります[静的ファイルをホストしている登録](xref:fundamentals/static-files)を介して、`UseStaticFiles`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="7919f-180">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="7919f-181">セキュリティ上の理由には、開発モードでアプリを実行する場合にのみ、ミドルウェアを登録します。</span><span class="sxs-lookup"><span data-stu-id="7919f-181">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="7919f-182">*Webpack.config.js*ファイルの`output.publicPath`プロパティ、ミドルウェアを見るには、通知、`dist`フォルダーの変更。</span><span class="sxs-lookup"><span data-stu-id="7919f-182">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="7919f-183">ホット モジュールの交換</span><span class="sxs-lookup"><span data-stu-id="7919f-183">Hot Module Replacement</span></span>

<span data-ttu-id="7919f-184">Webpack を考えてみてください[ホット モジュールの交換](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html)(HMR) 機能の発展として[Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)です。</span><span class="sxs-lookup"><span data-stu-id="7919f-184">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="7919f-185">HMR で利点ではすべて同じしますが、変更をコンパイルした後、ページのコンテンツを自動的に更新することでさらに、開発ワークフローを合理化します。</span><span class="sxs-lookup"><span data-stu-id="7919f-185">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="7919f-186">現在のインメモリ状態と SPA のデバッグ セッションに影響するブラウザーの更新でこれを混同しないでください。</span><span class="sxs-lookup"><span data-stu-id="7919f-186">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="7919f-187">Webpack デベロッパー ミドルウェア サービスと、ブラウザーに変更をプッシュすることを意味すると、ブラウザーの間のライブ リンクがあります。</span><span class="sxs-lookup"><span data-stu-id="7919f-187">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7919f-188">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7919f-188">Prerequisites</span></span>

<span data-ttu-id="7919f-189">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7919f-189">Install the following:</span></span>
* <span data-ttu-id="7919f-190">[webpack ホット ミドルウェア](https://www.npmjs.com/package/webpack-hot-middleware)npm パッケージ。</span><span class="sxs-lookup"><span data-stu-id="7919f-190">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="7919f-191">構成</span><span class="sxs-lookup"><span data-stu-id="7919f-191">Configuration</span></span>

<span data-ttu-id="7919f-192">MVC の HTTP 要求パイプライン内に HMR コンポーネントを登録する必要があります、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7919f-192">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="7919f-193">同様にも当てはまる[Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)、`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります、`UseStaticFiles`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="7919f-193">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="7919f-194">セキュリティ上の理由には、開発モードでアプリを実行する場合にのみ、ミドルウェアを登録します。</span><span class="sxs-lookup"><span data-stu-id="7919f-194">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="7919f-195">*Webpack.config.js*ファイルを定義する必要があります、`plugins`場合でも、そのまま空の配列。</span><span class="sxs-lookup"><span data-stu-id="7919f-195">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="7919f-196">ブラウザーでアプリを読み込んだ後に、開発者ツールのコンソール タブは、HMR アクティブ化の確認を提供します。</span><span class="sxs-lookup"><span data-stu-id="7919f-196">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![ホット モジュールの交換接続メッセージ](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="7919f-198">ルーティングのヘルパー</span><span class="sxs-lookup"><span data-stu-id="7919f-198">Routing helpers</span></span>

<span data-ttu-id="7919f-199">ほとんどの ASP.NET Core ベース SPAs では、サーバー側のルーティングだけでなくクライアント側のルーティングを必要があります。</span><span class="sxs-lookup"><span data-stu-id="7919f-199">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="7919f-200">SPA と MVC ルーティングのシステムは、干渉なし個別に操作できます。</span><span class="sxs-lookup"><span data-stu-id="7919f-200">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="7919f-201">課題ただし、1 つのエッジ ケース装って: 404 HTTP 応答を識別します。</span><span class="sxs-lookup"><span data-stu-id="7919f-201">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="7919f-202">シナリオを検討してください、拡張子のないルートの`/some/page`を使用します。</span><span class="sxs-lookup"><span data-stu-id="7919f-202">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="7919f-203">要求しないパターン一致がサーバー側のルートにはそのパターンがクライアント側のルートを一致と仮定します。</span><span class="sxs-lookup"><span data-stu-id="7919f-203">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="7919f-204">受信した要求を考えてみましょう`/images/user-512.png`が一般に、サーバー上の画像ファイルを検索するが必要です。</span><span class="sxs-lookup"><span data-stu-id="7919f-204">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="7919f-205">要求されたリソース、そのパスと一致していない任意のサーバー側のルートまたは静的ファイル場合可能性は高くありませんそれをクライアント側のアプリケーションで処理と — 通常 404 HTTP ステータス コードを取得します。</span><span class="sxs-lookup"><span data-stu-id="7919f-205">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="7919f-206">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="7919f-206">Prerequisites</span></span>

<span data-ttu-id="7919f-207">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="7919f-207">Install the following:</span></span>
* <span data-ttu-id="7919f-208">クライアント側ルーティング npm パッケージです。</span><span class="sxs-lookup"><span data-stu-id="7919f-208">The client-side routing npm package.</span></span> <span data-ttu-id="7919f-209">例として角の使用。</span><span class="sxs-lookup"><span data-stu-id="7919f-209">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="7919f-210">構成</span><span class="sxs-lookup"><span data-stu-id="7919f-210">Configuration</span></span>

<span data-ttu-id="7919f-211">名前付き拡張メソッド`MapSpaFallbackRoute`で使用される、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7919f-211">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="7919f-212">ヒント: ルートは、構成している順序で評価されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-212">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="7919f-213">その結果、`default`パターンに一致する最初の前のコード例にルートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-213">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="7919f-214">新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="7919f-214">Creating a new project</span></span>

<span data-ttu-id="7919f-215">JavaScriptServices には、構成済みのアプリケーション テンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="7919f-215">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="7919f-216">SpaServices は、さまざまなフレームワークおよび角、Aurelia、Knockout、対応、および Vue などのライブラリと連携して、これらのテンプレートで使用されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-216">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="7919f-217">これらのテンプレートは、次のコマンドを実行して、.NET Core CLI を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="7919f-217">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="7919f-218">使用可能な SPA テンプレートの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-218">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="7919f-219">テンプレート</span><span class="sxs-lookup"><span data-stu-id="7919f-219">Templates</span></span>                                 | <span data-ttu-id="7919f-220">短い形式の名前</span><span class="sxs-lookup"><span data-stu-id="7919f-220">Short Name</span></span> | <span data-ttu-id="7919f-221">言語</span><span class="sxs-lookup"><span data-stu-id="7919f-221">Language</span></span> | <span data-ttu-id="7919f-222">Tags</span><span class="sxs-lookup"><span data-stu-id="7919f-222">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="7919f-223">角度の MVC の ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919f-223">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="7919f-224">angular</span><span class="sxs-lookup"><span data-stu-id="7919f-224">angular</span></span>    | <span data-ttu-id="7919f-225">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-225">[C#]</span></span>     | <span data-ttu-id="7919f-226">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-226">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7919f-227">Aurelia で MVC の ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919f-227">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="7919f-228">aurelia</span><span class="sxs-lookup"><span data-stu-id="7919f-228">aurelia</span></span>    | <span data-ttu-id="7919f-229">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-229">[C#]</span></span>     | <span data-ttu-id="7919f-230">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-230">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7919f-231">Knockout.js で MVC の ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919f-231">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="7919f-232">knockout</span><span class="sxs-lookup"><span data-stu-id="7919f-232">knockout</span></span>   | <span data-ttu-id="7919f-233">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-233">[C#]</span></span>     | <span data-ttu-id="7919f-234">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-234">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7919f-235">React.js で MVC の ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919f-235">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="7919f-236">react</span><span class="sxs-lookup"><span data-stu-id="7919f-236">react</span></span>      | <span data-ttu-id="7919f-237">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-237">[C#]</span></span>     | <span data-ttu-id="7919f-238">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-238">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7919f-239">MVC の ASP.NET Core React.js と (続編)</span><span class="sxs-lookup"><span data-stu-id="7919f-239">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="7919f-240">reactredux</span><span class="sxs-lookup"><span data-stu-id="7919f-240">reactredux</span></span> | <span data-ttu-id="7919f-241">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-241">[C#]</span></span>     | <span data-ttu-id="7919f-242">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-242">Web/MVC/SPA</span></span> |
| <span data-ttu-id="7919f-243">Vue.js で MVC の ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7919f-243">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="7919f-244">vue</span><span class="sxs-lookup"><span data-stu-id="7919f-244">vue</span></span>        | <span data-ttu-id="7919f-245">[C#]</span><span class="sxs-lookup"><span data-stu-id="7919f-245">[C#]</span></span>     | <span data-ttu-id="7919f-246">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="7919f-246">Web/MVC/SPA</span></span> | 

<span data-ttu-id="7919f-247">SPA テンプレートのいずれかを使用して新しいプロジェクトを作成するには、**短い名前**でテンプレートの[dotnet 新しい](/dotnet/core/tools/dotnet-new)コマンド。</span><span class="sxs-lookup"><span data-stu-id="7919f-247">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="7919f-248">次のコマンドは、サーバー側用に構成された ASP.NET Core MVC と角運動のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7919f-248">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="7919f-249">ランタイム構成モードを設定します。</span><span class="sxs-lookup"><span data-stu-id="7919f-249">Set the runtime configuration mode</span></span>

<span data-ttu-id="7919f-250">2 つのプライマリのランタイム構成モードがあります。</span><span class="sxs-lookup"><span data-stu-id="7919f-250">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="7919f-251">**開発**:</span><span class="sxs-lookup"><span data-stu-id="7919f-251">**Development**:</span></span>
    * <span data-ttu-id="7919f-252">デバッグを容易にするソース マップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7919f-252">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="7919f-253">パフォーマンスをクライアント側のコードを最適化しません。</span><span class="sxs-lookup"><span data-stu-id="7919f-253">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="7919f-254">**実稼働**:</span><span class="sxs-lookup"><span data-stu-id="7919f-254">**Production**:</span></span>
    * <span data-ttu-id="7919f-255">ソース マップを除外します。</span><span class="sxs-lookup"><span data-stu-id="7919f-255">Excludes source maps.</span></span>
    * <span data-ttu-id="7919f-256">バンドル化と縮小を使用してクライアント側のコードを最適化します。</span><span class="sxs-lookup"><span data-stu-id="7919f-256">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="7919f-257">ASP.NET Core という環境変数を使用して`ASPNETCORE_ENVIRONMENT`構成モードを格納します。</span><span class="sxs-lookup"><span data-stu-id="7919f-257">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="7919f-258">参照してください**[環境の設定](xref:fundamentals/environments#setting-the-environment)**詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="7919f-258">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="7919f-259">.NET Core CLI で実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-259">Running with .NET Core CLI</span></span>

<span data-ttu-id="7919f-260">プロジェクトのルートに、次のコマンドを実行して、必要な NuGet と npm パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="7919f-260">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="7919f-261">構築し、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-261">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="7919f-262">アプリケーションの起動時によると localhost 上、[ランタイム構成モード](#runtime-config-mode)です。</span><span class="sxs-lookup"><span data-stu-id="7919f-262">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="7919f-263">移動`http://localhost:5000`ブラウザーのランディング ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-263">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="7919f-264">Visual Studio 2017 で実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-264">Running with Visual Studio 2017</span></span>

<span data-ttu-id="7919f-265">開く、 *.csproj*によって生成されたファイル、 [dotnet 新しい](/dotnet/core/tools/dotnet-new)コマンド。</span><span class="sxs-lookup"><span data-stu-id="7919f-265">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="7919f-266">必要な NuGet と npm パッケージは、プロジェクトを開く時に自動的に復元されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-266">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="7919f-267">この復元処理は、数分かかる場合があります、アプリケーションが、完了時に実行できる状態にします。</span><span class="sxs-lookup"><span data-stu-id="7919f-267">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="7919f-268">実行緑のボタンまたはキーを押します`Ctrl + F5`、し、アプリケーションのランディング ページで、ブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="7919f-268">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="7919f-269">Localhost をに従ってでアプリケーションを実行、[ランタイム構成モード](#runtime-config-mode)です。</span><span class="sxs-lookup"><span data-stu-id="7919f-269">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="7919f-270">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="7919f-270">Testing the app</span></span>

<span data-ttu-id="7919f-271">SpaServices テンプレートは、事前構成を使用してクライアント側のテストを実行して[Karma](https://karma-runner.github.io/1.0/index.html)と[Jasmine](https://jasmine.github.io/)です。</span><span class="sxs-lookup"><span data-stu-id="7919f-271">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="7919f-272">Jasmine に対し、Karma テスト ランナーはこれらのテストには、人気のある単体テスト フレームワークを JavaScript 用です。</span><span class="sxs-lookup"><span data-stu-id="7919f-272">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="7919f-273">Karma が使用するよう構成、 [Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)を開発者が停止し、変更されるたびに、テストを実行するため必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7919f-273">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="7919f-274">テスト_ケースまたはテスト ケース自体に対して実行されるコードであるかどうか、テストが自動的に実行されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-274">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="7919f-275">Angular アプリケーションを使用して、例として、2 つのジャスミン テスト_ケースが既に提供されている、`CounterComponent`で、 *counter.component.spec.ts*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7919f-275">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="7919f-276">プロジェクトのルートにコマンド プロンプトを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="7919f-276">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="7919f-277">スクリプトで定義された設定が読み取られます Karma テスト ランナーを起動する、 *karma.conf.js*ファイル。</span><span class="sxs-lookup"><span data-stu-id="7919f-277">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="7919f-278">その他の設定の間で、 *karma.conf.js*経由で実行するテスト ファイルを識別、`files`配列。</span><span class="sxs-lookup"><span data-stu-id="7919f-278">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="7919f-279">アプリケーションの発行</span><span class="sxs-lookup"><span data-stu-id="7919f-279">Publishing the application</span></span>

<span data-ttu-id="7919f-280">展開の準備完了のパッケージに生成されたクライアント側資産と公開済みの ASP.NET Core 成果物を組み合わせることは面倒なことができます。</span><span class="sxs-lookup"><span data-stu-id="7919f-280">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="7919f-281">さいわい、SpaServices がという名前のカスタム MSBuild ターゲットとそのパブリケーション全体のプロセスを統制`RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="7919f-281">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="7919f-282">MSBuild ターゲットには、次の責任があります。</span><span class="sxs-lookup"><span data-stu-id="7919f-282">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="7919f-283">Npm パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="7919f-283">Restore the npm packages</span></span>
1. <span data-ttu-id="7919f-284">サード パーティではクライアント側の資産の実稼働と同等のビルドを作成します。</span><span class="sxs-lookup"><span data-stu-id="7919f-284">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="7919f-285">カスタムのクライアント側の資産の実稼働と同等のビルドを作成します。</span><span class="sxs-lookup"><span data-stu-id="7919f-285">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="7919f-286">Webpack で生成された資産を発行フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="7919f-286">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="7919f-287">MSBuild ターゲットが実行するときに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7919f-287">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="7919f-288">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7919f-288">Additional resources</span></span>

* [<span data-ttu-id="7919f-289">Angular Docs</span><span class="sxs-lookup"><span data-stu-id="7919f-289">Angular Docs</span></span>](https://angular.io/docs)
