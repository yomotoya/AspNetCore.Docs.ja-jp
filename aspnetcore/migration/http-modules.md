---
title: "HTTP ハンドラーと ASP.NET Core ミドルウェアにモジュールを移行します。"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.assetid: 9c826a76-fbd2-46b5-978d-6ca6df53531a
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: eb5049d4d63c224ca74fc39072ae2c0d98ba330d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="c44e6-103">HTTP ハンドラーと ASP.NET Core ミドルウェアにモジュールを移行します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-103">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="c44e6-104">によって[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="c44e6-104">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="c44e6-105">この記事は、既存の ASP.NET を移行する方法を示しています。 [HTTP モジュールとハンドラー system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/)を ASP.NET Core[ミドルウェア](../fundamentals/middleware.md)です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-105">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="c44e6-106">モジュールとハンドラーが見直され</span><span class="sxs-lookup"><span data-stu-id="c44e6-106">Modules and handlers revisited</span></span>

<span data-ttu-id="c44e6-107">ASP.NET Core ミドルウェアを前に、最初に要約 HTTP モジュールとハンドラーの動作方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-107">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![モジュール ハンドラー](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="c44e6-109">**ハンドラーは次のとおりです。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-109">**Handlers are:**</span></span>

   * <span data-ttu-id="c44e6-110">実装するクラス[IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="c44e6-110">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="c44e6-111">など、指定されたファイル名または拡張機能で要求を処理するために使用*レポート*</span><span class="sxs-lookup"><span data-stu-id="c44e6-111">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="c44e6-112">[構成されている](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/)で*Web.config*</span><span class="sxs-lookup"><span data-stu-id="c44e6-112">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="c44e6-113">**モジュールは次のとおりです。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-113">**Modules are:**</span></span>

   * <span data-ttu-id="c44e6-114">実装するクラス[IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="c44e6-114">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="c44e6-115">要求ごとに呼び出されます</span><span class="sxs-lookup"><span data-stu-id="c44e6-115">Invoked for every request</span></span>

   * <span data-ttu-id="c44e6-116">ショート サーキット (さらに処理の停止の要求の) すること</span><span class="sxs-lookup"><span data-stu-id="c44e6-116">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="c44e6-117">HTTP 応答に追加したり、独自に作成できません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-117">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="c44e6-118">[構成されている](https://docs.microsoft.com//iis/configuration/system.webserver/modules/)で*Web.config*</span><span class="sxs-lookup"><span data-stu-id="c44e6-118">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="c44e6-119">**モジュールが受信要求を処理する順序は、によって決定されます。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-119">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="c44e6-120">[アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)、ASP.NET によって発生した一連のイベントである: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), などです。各モジュールには、1 つまたは複数のイベントのハンドラーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-120">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="c44e6-121">同一のイベントで構成されている順序*Web.config*です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-121">For the same event, the order in which they are configured in *Web.config*.</span></span>

<span data-ttu-id="c44e6-122">ライフ サイクル イベントのハンドラーを追加するだけでなく、モジュール、 *Global.asax.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="c44e6-122">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="c44e6-123">これらのハンドラーは、構成済みのモジュール内のハンドラーの後に実行します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-123">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="c44e6-124">ハンドラーとミドルウェアをモジュールから</span><span class="sxs-lookup"><span data-stu-id="c44e6-124">From handlers and modules to middleware</span></span>

<span data-ttu-id="c44e6-125">**ミドルウェアは、HTTP モジュールとハンドラーよりも簡単です。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-125">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="c44e6-126">モジュール、ハンドラー、 *Global.asax.cs*、 *Web.config* (IIS の構成) を除くアプリケーションのライフ サイクルはなくなり、</span><span class="sxs-lookup"><span data-stu-id="c44e6-126">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="c44e6-127">ミドルウェアによって作成されたモジュールとハンドラーの両方のロール</span><span class="sxs-lookup"><span data-stu-id="c44e6-127">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="c44e6-128">ミドルウェアは、コードを使用して構成されてなく*Web.config*</span><span class="sxs-lookup"><span data-stu-id="c44e6-128">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="c44e6-129">[パイプラインを分岐](../fundamentals/middleware.md#middleware-run-map-use)要求ヘッダー、クエリ文字列なども上にある URL だけでなくに基づいて、特定のミドルウェアに要求を送信することができます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-129">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="c44e6-130">**ミドルウェアは、モジュールとよく似ています。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-130">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="c44e6-131">原則として要求ごとに呼び出されます</span><span class="sxs-lookup"><span data-stu-id="c44e6-131">Invoked in principle for every request</span></span>

   * <span data-ttu-id="c44e6-132">要求をショート サーキットできなければ[要求を次のミドルウェアに渡されていません。](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="c44e6-132">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="c44e6-133">独自の HTTP 応答を作成できません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-133">Able to create their own HTTP response</span></span>

<span data-ttu-id="c44e6-134">**ミドルウェアとモジュールは、別の順序で処理されます。**</span><span class="sxs-lookup"><span data-stu-id="c44e6-134">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="c44e6-135">ミドルウェアの順序は順番に挿入された、要求パイプライン モジュールの順序の基準は、主に、順序に基づきます[アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/ms227673.aspx)イベント</span><span class="sxs-lookup"><span data-stu-id="c44e6-135">Order of middleware is based on the order in which they are inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="c44e6-136">モジュールの順序は、同じ要求と応答の中に応答のミドルウェアの順序が要求の場合の逆順になって</span><span class="sxs-lookup"><span data-stu-id="c44e6-136">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="c44e6-137">参照してください[IApplicationBuilder のミドルウェア パイプラインを作成します。](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="c44e6-137">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![ミドルウェア](http-modules/_static/middleware.png)

<span data-ttu-id="c44e6-139">認証ミドルウェア short-circuited 要求、上記の図で、どのように注意してください。</span><span class="sxs-lookup"><span data-stu-id="c44e6-139">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="c44e6-140">ミドルウェアにモジュールのコードを移行します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-140">Migrating module code to middleware</span></span>

<span data-ttu-id="c44e6-141">既存の HTTP モジュールは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-141">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="c44e6-142">ように、[ミドルウェア](../fundamentals/middleware.md) ページで、ASP.NET Core ミドルウェアが公開するクラス、`Invoke`メソッドを`HttpContext`を返すと、`Task`です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-142">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="c44e6-143">新しいミドルウェアは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-143">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="c44e6-144">上記のミドルウェア テンプレートがセクションから取得されました[ミドルウェアを記述](../fundamentals/middleware.md#middleware-writing-middleware)です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-144">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="c44e6-145">*MyMiddlewareExtensions*ヘルパー クラスを簡単にミドルウェアの構成、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="c44e6-145">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="c44e6-146">`UseMyMiddleware`メソッドは、要求パイプラインにミドルウェア クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-146">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="c44e6-147">ミドルウェアによって必要なサービスは、ミドルウェアのコンス トラクターで挿入されたを取得します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-147">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="c44e6-148">モジュールは、たとえば、ユーザーの権限がない場合、要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-148">Your module might terminate a request, for example if the user is not authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="c44e6-149">ミドルウェアの処理を呼び出していない`Invoke`パイプラインの次のミドルウェアにします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-149">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="c44e6-150">以前 middlewares は応答により、パイプラインから戻るときにも呼び出されるために、いるこの完全に終了しない、要求に留意してください。</span><span class="sxs-lookup"><span data-stu-id="c44e6-150">Keep in mind that this does not fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="c44e6-151">新しいミドルウェアにモジュールの機能を移行する場合がありますので、コードをコンパイルできないこと、`HttpContext`クラスが ASP.NET Core で大幅に変更します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-151">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c44e6-152">[後で](#migrating-to-the-new-httpcontext)、新規の ASP.NET Core HttpContext に移行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-152">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="c44e6-153">要求パイプラインに移行するモジュールの挿入</span><span class="sxs-lookup"><span data-stu-id="c44e6-153">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="c44e6-154">HTTP モジュールが、要求を使用してパイプラインに追加されます通常*Web.config*:</span><span class="sxs-lookup"><span data-stu-id="c44e6-154">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="c44e6-155">これによって変換[新しいミドルウェアを追加する](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)要求パイプラインへ、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="c44e6-155">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="c44e6-156">新しいミドルウェアを挿入するパイプラインで正確なスポットは、モジュールとして処理するイベントによって異なります (`BeginRequest`、`EndRequest`など) とその順序内のモジュールの一覧で、 *Web.config*です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-156">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="c44e6-157">既に説明したようにがありません。 アプリケーションのライフ サイクル ASP.NET Core とミドルウェアによって応答が処理される順序は、モジュールによって使用される順序によって異なります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-157">As previously stated, there is no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="c44e6-158">これ行うことができる順序の決定より困難です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-158">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="c44e6-159">順序付けには、問題になると、個別に注文する複数のミドルウェア コンポーネントに、モジュールを分割する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-159">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="c44e6-160">ミドルウェアをハンドラー コードの移行</span><span class="sxs-lookup"><span data-stu-id="c44e6-160">Migrating handler code to middleware</span></span>

<span data-ttu-id="c44e6-161">HTTP ハンドラーでは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-161">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="c44e6-162">ASP.NET Core のプロジェクトで次のようなミドルウェアをこの変換は。</span><span class="sxs-lookup"><span data-stu-id="c44e6-162">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="c44e6-163">このミドルウェアは、モジュールに対応するミドルウェアに非常に似ています。</span><span class="sxs-lookup"><span data-stu-id="c44e6-163">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="c44e6-164">唯一の違いはここでは呼び出されずに`_next.Invoke(context)`です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-164">The only real difference is that here there is no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="c44e6-165">ハンドラーは、要求パイプラインの最後があるためありません次のミドルウェアを呼び出すため理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="c44e6-165">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="c44e6-166">要求パイプラインへの移行のハンドラー挿入</span><span class="sxs-lookup"><span data-stu-id="c44e6-166">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="c44e6-167">行われる HTTP ハンドラーを構成する*Web.config*し、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-167">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="c44e6-168">要求パイプラインへの新しいハンドラー ミドルウェアを追加することで、これを変換することも、`Startup`クラス、モジュールから変換されたミドルウェアに似ています。</span><span class="sxs-lookup"><span data-stu-id="c44e6-168">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="c44e6-169">その方法を使った問題は、新しいハンドラー ミドルウェアにすべての要求が送信します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-169">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="c44e6-170">ただし、ミドルウェアに指定された拡張子を持つ要求を送信するだけです。</span><span class="sxs-lookup"><span data-stu-id="c44e6-170">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="c44e6-171">同じの機能を提供する、HTTP ハンドラーを持つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-171">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="c44e6-172">特定の拡張機能と要求パイプラインを分岐するのには、1 つのソリューションを使用して、`MapWhen`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="c44e6-172">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="c44e6-173">こうと、同じ`Configure`他のミドルウェアを追加する方法。</span><span class="sxs-lookup"><span data-stu-id="c44e6-173">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="c44e6-174">`MapWhen`これらのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-174">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="c44e6-175">受け取るラムダ、`HttpContext`し、返します`true`場合は、要求が、分岐を移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-175">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="c44e6-176">これは、要求だけでなく、その拡張機能が、要求ヘッダー、クエリ文字列パラメーターなどを基に分岐することを意味します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-176">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="c44e6-177">受け取るラムダ、`IApplicationBuilder`し、分岐のすべてのミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-177">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="c44e6-178">つまり、その他のミドルウェアを分岐に追加するには、ハンドラーのミドルウェアの前にします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-178">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="c44e6-179">すべての要求で、分岐が呼び出される前に、パイプラインにミドルウェアが追加されました。分岐による影響はありませんします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-179">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="c44e6-180">読み込みオプションのパターンを使用してミドルウェアのオプション</span><span class="sxs-lookup"><span data-stu-id="c44e6-180">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="c44e6-181">一部のモジュールとハンドラーに格納されている構成オプションがあります*Web.config*です。ただし、ASP.NET Core で新しい構成モデルが使用の代わりに*Web.config*です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-181">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="c44e6-182">新しい[構成システム](../fundamentals/configuration.md)これを解決するオプションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-182">The new [configuration system](../fundamentals/configuration.md) gives you these options to solve this:</span></span>

* <span data-ttu-id="c44e6-183">ように、ミドルウェアにオプションを直接挿入、[次のセクション](#loading-middleware-options-through-direct-injection)です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-183">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="c44e6-184">使用して、[オプション パターン](../fundamentals/configuration.md#options-config-objects):</span><span class="sxs-lookup"><span data-stu-id="c44e6-184">Use the [options pattern](../fundamentals/configuration.md#options-config-objects):</span></span>

1.  <span data-ttu-id="c44e6-185">たとえば、ミドルウェアのオプションを保持するクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-185">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="c44e6-186">オプションの値を格納します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-186">Store the option values</span></span>

    <span data-ttu-id="c44e6-187">構成システムでは、オプション値任意の場所を格納することができます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-187">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="c44e6-188">ただし、使用を最もサイト*される appsettings.json*ので、その方法を詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-188">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="c44e6-189">*MyMiddlewareOptionsSection*セクション名を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-189">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="c44e6-190">オプション クラスの名前と同じである必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-190">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="c44e6-191">オプションの値を関連付けるオプション クラス</span><span class="sxs-lookup"><span data-stu-id="c44e6-191">Associate the option values with the options class</span></span>

    <span data-ttu-id="c44e6-192">オプション パターンでは、ASP.NET Core の依存関係の挿入のフレームワークを使用して、オプションの種類を関連付けます (など`MyMiddlewareOptions`) で、`MyMiddlewareOptions`実際のオプションを持つオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="c44e6-192">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="c44e6-193">更新プログラム、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="c44e6-193">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="c44e6-194">使用する場合*される appsettings.json*、ビルダーでは、構成を追加、`Startup`コンス トラクター。</span><span class="sxs-lookup"><span data-stu-id="c44e6-194">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="c44e6-195">オプションのサービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-195">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="c44e6-196">オプション クラスと、オプションを関連付けます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-196">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="c44e6-197">ミドルウェア コンス トラクターにオプションを挿入します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-197">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="c44e6-198">これは、コント ローラーにオプションを挿入することに似ています。</span><span class="sxs-lookup"><span data-stu-id="c44e6-198">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="c44e6-199">[UseMiddleware](#http-modules-usemiddleware) 、ミドルウェアに追加する拡張メソッド、`IApplicationBuilder`依存関係の挿入を行います。</span><span class="sxs-lookup"><span data-stu-id="c44e6-199">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="c44e6-200">これに限定されません`IOptions`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c44e6-200">This is not limited to `IOptions` objects.</span></span> <span data-ttu-id="c44e6-201">ミドルウェアを必要とするその他のオブジェクトには、この方法を挿入できます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-201">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="c44e6-202">直接インジェクションを通じてミドルウェアのオプションの読み込み</span><span class="sxs-lookup"><span data-stu-id="c44e6-202">Loading middleware options through direct injection</span></span>

<span data-ttu-id="c44e6-203">オプションのパターンでは、疎結合のオプションの値とコンシューマーとの間で作成する利点があります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-203">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="c44e6-204">オプション クラスは、実際のオプション値に関連した、他のクラスは、依存関係の挿入フレームワークを通じてオプションへのアクセスを取得できます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-204">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="c44e6-205">オプションの値を渡す必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-205">There is no need to pass around options values.</span></span>

<span data-ttu-id="c44e6-206">これは、分割もオプションが異なる同じミドルウェアを 2 回、使用する場合。</span><span class="sxs-lookup"><span data-stu-id="c44e6-206">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="c44e6-207">たとえば、承認ミドルウェアの異なるロールを許可するさまざまな分岐で使用します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-207">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="c44e6-208">1 つのオプション クラスを使用して 2 つの異なるオプション オブジェクトを関連付けることはできません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-208">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="c44e6-209">解決する実際のオプションの値とオプション オブジェクトを取得するには、`Startup`クラスし、直接ミドルウェアの各インスタンスに渡します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-209">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="c44e6-210">2 番目のキーを追加*される appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="c44e6-210">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="c44e6-211">2 番目のオプションのセットを追加する、*される appsettings.json*ファイルを新しいキーを使用して一意に識別できるを。</span><span class="sxs-lookup"><span data-stu-id="c44e6-211">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="c44e6-212">オプションの値を取得し、ミドルウェアに渡したりします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-212">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="c44e6-213">`Use...` (パイプラインにミドルウェアを追加) する拡張メソッドは、オプションの値で渡す論理的な場所。</span><span class="sxs-lookup"><span data-stu-id="c44e6-213">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="c44e6-214">ミドルウェアのオプション パラメーターを受け取ることを有効にします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-214">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="c44e6-215">オーバー ロードを提供、`Use...`拡張メソッド (オプション パラメーターを使用してに渡します`UseMiddleware`)。</span><span class="sxs-lookup"><span data-stu-id="c44e6-215">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="c44e6-216">ときに`UseMiddleware`が呼び出されたパラメーターを持つパラメーターに渡して、ミドルウェア コンス トラクター ミドルウェア オブジェクトをインスタンス化時にします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-216">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="c44e6-217">オプション オブジェクトをラップするこの方法に注意してください、`OptionsWrapper`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c44e6-217">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="c44e6-218">これは、手順で実装`IOptions`ミドルウェア コンス トラクターが予期するとおり、します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-218">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="c44e6-219">新しい HttpContext への移行</span><span class="sxs-lookup"><span data-stu-id="c44e6-219">Migrating to the new HttpContext</span></span>

<span data-ttu-id="c44e6-220">先ほど見たを`Invoke`、ミドルウェア内でメソッドが型のパラメーターを受け取る`HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="c44e6-220">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="c44e6-221">`HttpContext`ASP.NET Core で大幅に変更されました。</span><span class="sxs-lookup"><span data-stu-id="c44e6-221">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="c44e6-222">このセクションでは、の最も一般的に使用されるプロパティに変換する方法を示しています。 [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext)を新しい`Microsoft.AspNetCore.Http.HttpContext`です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-222">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="c44e6-223">HttpContext</span><span class="sxs-lookup"><span data-stu-id="c44e6-223">HttpContext</span></span>

<span data-ttu-id="c44e6-224">**HttpContext.Items**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-224">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="c44e6-225">**一意の要求 ID (System.Web.HttpContext 対応はありません)**</span><span class="sxs-lookup"><span data-stu-id="c44e6-225">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="c44e6-226">使用する一意の id 要求ごとにできます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-226">Gives you a unique id for each request.</span></span> <span data-ttu-id="c44e6-227">ログに記録に非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-227">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="c44e6-228">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="c44e6-228">HttpContext.Request</span></span>

<span data-ttu-id="c44e6-229">**HttpContext.Request.HttpMethod**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-229">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="c44e6-230">**HttpContext.Request.QueryString**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-230">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="c44e6-231">**HttpContext.Request.Url**と**HttpContext.Request.RawUrl**に変換します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-231">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="c44e6-232">**HttpContext.Request.IsSecureConnection**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-232">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="c44e6-233">**HttpContext.Request.UserHostAddress**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-233">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="c44e6-234">**HttpContext.Request.Cookies**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-234">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="c44e6-235">**HttpContext.Request.RequestContext.RouteData**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-235">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="c44e6-236">**HttpContext.Request.Headers**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-236">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="c44e6-237">**HttpContext.Request.UserAgent**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-237">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="c44e6-238">**HttpContext.Request.UrlReferrer**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-238">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="c44e6-239">**HttpContext.Request.ContentType**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-239">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="c44e6-240">**HttpContext.Request.Form**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-240">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="c44e6-241">コンテンツのサブタイプが場合にのみ、フォームの値を読み取る*x-www-form-urlencoded*または*フォーム データ*です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-241">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="c44e6-242">**HttpContext.Request.InputStream**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-242">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="c44e6-243">パイプラインの最後のハンドラー型ミドルウェアでのみ、このコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-243">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="c44e6-244">要求あたり 1 回だけ上記のように、生の本体を読み取ることができます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-244">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="c44e6-245">ミドルウェアの最初の読み込み後に、本文を読み取るしようとしています。 空の本文が読み取られます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-245">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="c44e6-246">これは、バッファーからが完了するため、前に示したようにフォームを読み取り中には適用されません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-246">This does not apply to reading a form as shown earlier, because that is done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="c44e6-247">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="c44e6-247">HttpContext.Response</span></span>

<span data-ttu-id="c44e6-248">**HttpContext.Response.Status**と**HttpContext.Response.StatusDescription**に変換します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-248">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="c44e6-249">**HttpContext.Response.ContentEncoding**と**HttpContext.Response.ContentType**に変換します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-249">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="c44e6-250">**HttpContext.Response.ContentType**でそれ自体にも変換します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-250">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="c44e6-251">**HttpContext.Response.Output**に変換されます。</span><span class="sxs-lookup"><span data-stu-id="c44e6-251">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="c44e6-252">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="c44e6-252">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="c44e6-253">説明、ファイルを提供している[ここ](../fundamentals/request-features.md#middleware-and-request-features)です。</span><span class="sxs-lookup"><span data-stu-id="c44e6-253">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="c44e6-254">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="c44e6-254">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="c44e6-255">応答ヘッダーの送信が複雑にする設定した場合、応答本文に何も書き込まれた後、それらは送信されません。</span><span class="sxs-lookup"><span data-stu-id="c44e6-255">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="c44e6-256">ソリューションでは、右、応答が開始に書き込む前に呼び出されるコールバック メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="c44e6-256">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="c44e6-257">開始時にこれは、最適な`Invoke`ミドルウェア内でのメソッドです。</span><span class="sxs-lookup"><span data-stu-id="c44e6-257">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="c44e6-258">このコールバック メソッド、応答ヘッダーを設定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c44e6-258">It is this callback method that sets your response headers.</span></span>

<span data-ttu-id="c44e6-259">次のコードが呼び出されるコールバック メソッドを設定`SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="c44e6-259">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c44e6-260">`SetHeaders`コールバック メソッドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-260">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="c44e6-261">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c44e6-261">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="c44e6-262">Cookie をブラウザーに移動、 *Set-cookie*応答ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="c44e6-262">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="c44e6-263">その結果、cookie を送信する必要があります使用したのと同じコールバック応答ヘッダーを送信するため。</span><span class="sxs-lookup"><span data-stu-id="c44e6-263">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="c44e6-264">`SetCookies`コールバック メソッドは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c44e6-264">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="c44e6-265">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="c44e6-265">Additional Resources</span></span>

* [<span data-ttu-id="c44e6-266">HTTP ハンドラーと HTTP モジュールの概要</span><span class="sxs-lookup"><span data-stu-id="c44e6-266">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="c44e6-267">構成</span><span class="sxs-lookup"><span data-stu-id="c44e6-267">Configuration</span></span>](../fundamentals/configuration.md)

* [<span data-ttu-id="c44e6-268">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="c44e6-268">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="c44e6-269">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="c44e6-269">Middleware</span></span>](../fundamentals/middleware.md)
