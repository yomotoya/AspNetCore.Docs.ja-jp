---
title: "ASP.NET Core ミドルウェア"
author: rick-anderson
description: "ASP.NET Core ミドルウェアと要求パイプラインについて説明します。"
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 84f386db4ab96a82011ee2fc0b6c20a1a05b5e4b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="42920-103">ASP.NET Core ミドルウェアの基本事項</span><span class="sxs-lookup"><span data-stu-id="42920-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="42920-104">によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="42920-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="42920-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="42920-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="42920-106">ミドルウェアとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="42920-106">What is middleware?</span></span>

<span data-ttu-id="42920-107">ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="42920-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="42920-108">各コンポーネントには:</span><span class="sxs-lookup"><span data-stu-id="42920-108">Each component:</span></span>

* <span data-ttu-id="42920-109">要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。</span><span class="sxs-lookup"><span data-stu-id="42920-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="42920-110">前に、と、パイプラインの次のコンポーネントが呼び出された後、作業を実行できます。</span><span class="sxs-lookup"><span data-stu-id="42920-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="42920-111">要求のデリゲートは、要求パイプラインの構築に使用されます。</span><span class="sxs-lookup"><span data-stu-id="42920-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="42920-112">要求のデリゲートでは、各 HTTP 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="42920-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="42920-113">デリゲートを使用して構成を要求[実行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[マップ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、および[使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="42920-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="42920-114">個々 の要求はデリゲートがインラインで指定された (インライン ミドルウェアと呼ばれます)、匿名メソッド、としてまたは再利用可能なクラスで定義されていることができます。</span><span class="sxs-lookup"><span data-stu-id="42920-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="42920-115">これらの再利用可能なクラスとインラインの匿名メソッドは*ミドルウェア*、または*ミドルウェア コンポーネント*です。</span><span class="sxs-lookup"><span data-stu-id="42920-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="42920-116">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインでは、次のコンポーネントを呼び出すか、該当する場合、チェーンをショート サーキットします。</span><span class="sxs-lookup"><span data-stu-id="42920-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="42920-117">[ミドルウェアを移行する HTTP モジュール](../migration/http-modules.md)ASP.NET Core での要求パイプラインと、以前のバージョンの違いについて説明し、他のミドルウェア サンプルを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="42920-118">IApplicationBuilder のミドルウェア パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="42920-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="42920-119">ASP.NET Core 要求パイプラインは、この図に示す (実行次のように黒の矢印のスレッド) 後に、他のいずれかと呼ばれる、要求のデリゲートのシーケンスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="42920-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![要求の処理パターンが到着の場合、次の 3 つの middlewares とアプリケーションの応答を通じて処理要求を表示します。](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="42920-123">各デリゲートは、次のデリゲートの前後に、操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="42920-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="42920-124">デリゲートは、要求パイプラインをショート サーキットと呼ばれる次のデリゲートに要求を渡しませんすることもできます。</span><span class="sxs-lookup"><span data-stu-id="42920-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="42920-125">不要な処理を回避しているので、ショート サーキット望ましいがよくあります。</span><span class="sxs-lookup"><span data-stu-id="42920-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="42920-126">たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返すし、残りのパイプラインをショート サーキットします。</span><span class="sxs-lookup"><span data-stu-id="42920-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="42920-127">例外処理のデリゲートは、パイプラインの後の段階で発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="42920-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="42920-128">考えられる最も簡単な ASP.NET Core アプリは、すべての要求を処理する 1 つの要求のデリゲートを設定します。</span><span class="sxs-lookup"><span data-stu-id="42920-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="42920-129">この場合に、実際の要求パイプラインが含まれていません。</span><span class="sxs-lookup"><span data-stu-id="42920-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="42920-130">代わりに、各 HTTP 要求に対する応答で単一の匿名関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="42920-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="42920-131">最初の[アプリ。実行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)デリゲートは、パイプラインを終了します。</span><span class="sxs-lookup"><span data-stu-id="42920-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="42920-132">と共に複数の要求の代理人をチェーンする[アプリ。使用して](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)です。</span><span class="sxs-lookup"><span data-stu-id="42920-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="42920-133">`next`パラメーターはパイプラインの次のデリゲートを表します。</span><span class="sxs-lookup"><span data-stu-id="42920-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="42920-134">(でパイプラインをショート サーキットできることに注意してください*いない*を呼び出す、*次*パラメーターです)。通常を実行できますアクション前に、と後、次のデリゲートでは、この例で示すように。</span><span class="sxs-lookup"><span data-stu-id="42920-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="42920-135">呼び出さない`next.Invoke`応答がクライアントに送信された後です。</span><span class="sxs-lookup"><span data-stu-id="42920-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="42920-136">変更`HttpResponse`応答が開始された後に例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="42920-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="42920-137">たとえば、ヘッダー、ステータス コードなど、設定などの変化例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="42920-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="42920-138">呼び出した後、応答本文に書き込む`next`:</span><span class="sxs-lookup"><span data-stu-id="42920-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="42920-139">プロトコル違反が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="42920-139">May cause a protocol violation.</span></span> <span data-ttu-id="42920-140">たとえば、書き込み、示されたより`content-length`です。</span><span class="sxs-lookup"><span data-stu-id="42920-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="42920-141">本文の形式が破損する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="42920-141">May corrupt the body format.</span></span> <span data-ttu-id="42920-142">たとえば、CSS ファイルを HTML のフッターを書き込んでいます。</span><span class="sxs-lookup"><span data-stu-id="42920-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="42920-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)ヘッダーが送信されたかどうかや、本体に書き込まれたを示すために役立ちますヒントします。</span><span class="sxs-lookup"><span data-stu-id="42920-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="42920-144">並べ替え</span><span class="sxs-lookup"><span data-stu-id="42920-144">Ordering</span></span>

<span data-ttu-id="42920-145">ミドルウェア コンポーネントを追加する順序、`Configure`メソッドは、要求で呼び出されるしている順序と応答の逆の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="42920-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="42920-146">この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。</span><span class="sxs-lookup"><span data-stu-id="42920-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="42920-147">Configure メソッド (下図参照) は、次のミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="42920-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="42920-148">例外とエラー処理</span><span class="sxs-lookup"><span data-stu-id="42920-148">Exception/error handling</span></span>
2. <span data-ttu-id="42920-149">静的なファイル サーバー</span><span class="sxs-lookup"><span data-stu-id="42920-149">Static file server</span></span>
3. <span data-ttu-id="42920-150">認証</span><span class="sxs-lookup"><span data-stu-id="42920-150">Authentication</span></span>
4. <span data-ttu-id="42920-151">MVC</span><span class="sxs-lookup"><span data-stu-id="42920-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42920-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42920-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42920-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42920-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="42920-154">、上記のコードで`UseExceptionHandler`は、最初のミドルウェア コンポーネントをパイプラインに追加: そのため、その後の呼び出しで発生する例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="42920-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="42920-155">静的ファイル ミドルウェアがパイプラインの早い段階で呼び出されるは、要求を処理し、残りのコンポーネントを通過せずショート サーキットためです。</span><span class="sxs-lookup"><span data-stu-id="42920-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="42920-156">静的ファイル ミドルウェアを提供**ありません**承認チェックします。</span><span class="sxs-lookup"><span data-stu-id="42920-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="42920-157">下にあるものを含め、すべてのファイルが処理される*wwwroot*、一般に公開されます。</span><span class="sxs-lookup"><span data-stu-id="42920-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="42920-158">参照してください[静的ファイルで作業](xref:fundamentals/static-files)の静的なファイルをセキュリティで保護する方法です。</span><span class="sxs-lookup"><span data-stu-id="42920-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="42920-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="42920-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="42920-160">要求は、静的ファイル ミドルウェアによって処理されない場合、に、Identity ミドルウェア渡されます (`app.UseAuthentication`)、認証を実行します。</span><span class="sxs-lookup"><span data-stu-id="42920-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="42920-161">Id は、認証されていない要求をショート サーキットしません。</span><span class="sxs-lookup"><span data-stu-id="42920-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="42920-162">Identity は、要求を認証、承認 (および却下) 実行されますが、MVC は、固有の Razor ページまたはコント ローラーとアクションを選択した後のみです。</span><span class="sxs-lookup"><span data-stu-id="42920-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="42920-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="42920-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="42920-164">要求は、静的ファイル ミドルウェアによって処理されない場合、に、Identity ミドルウェア渡されます (`app.UseIdentity`)、認証を実行します。</span><span class="sxs-lookup"><span data-stu-id="42920-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="42920-165">Id は、認証されていない要求をショート サーキットしません。</span><span class="sxs-lookup"><span data-stu-id="42920-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="42920-166">Identity は、要求を認証、承認 (および却下) 実行されますが、MVC は、特定のコント ローラーとアクションを選択した後のみです。</span><span class="sxs-lookup"><span data-stu-id="42920-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="42920-167">次の例では、応答の圧縮のミドルウェアの前に、静的ファイル ミドルウェアによって静的ファイルに対する要求を処理する、順序付けミドルウェアを示します。</span><span class="sxs-lookup"><span data-stu-id="42920-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="42920-168">このミドルウェアの順序では、静的なファイルは圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="42920-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="42920-169">MVC 応答から[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)圧縮することができます。</span><span class="sxs-lookup"><span data-stu-id="42920-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="42920-170">使用して、実行、およびマップ</span><span class="sxs-lookup"><span data-stu-id="42920-170">Use, Run, and Map</span></span>

<span data-ttu-id="42920-171">HTTP パイプラインを使用して、構成する`Use`、 `Run`、および`Map`です。</span><span class="sxs-lookup"><span data-stu-id="42920-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="42920-172">`Use`メソッドは、パイプラインをショート サーキットことができます (呼び出しません場合に、`next`要求デリゲート)。</span><span class="sxs-lookup"><span data-stu-id="42920-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="42920-173">`Run`規則は、一部のミドルウェア コンポーネントが公開される`Run[Middleware]`パイプラインの最後に実行されるメソッド。</span><span class="sxs-lookup"><span data-stu-id="42920-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="42920-174">`Map*`拡張機能は、パイプラインを分岐に、規則として使用されます。</span><span class="sxs-lookup"><span data-stu-id="42920-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="42920-175">[マップ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)指定された要求パスの一致に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="42920-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="42920-176">要求パスに指定されたパスで起動する場合、分岐が実行されます。</span><span class="sxs-lookup"><span data-stu-id="42920-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="42920-177">次の表は、要求および応答から`http://localhost:1234`前のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="42920-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="42920-178">要求</span><span class="sxs-lookup"><span data-stu-id="42920-178">Request</span></span> | <span data-ttu-id="42920-179">応答</span><span class="sxs-lookup"><span data-stu-id="42920-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="42920-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="42920-180">localhost:1234</span></span> | <span data-ttu-id="42920-181">非マップ デリゲートからこんにちはです。</span><span class="sxs-lookup"><span data-stu-id="42920-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="42920-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="42920-182">localhost:1234/map1</span></span> | <span data-ttu-id="42920-183">マップのテスト 1</span><span class="sxs-lookup"><span data-stu-id="42920-183">Map Test 1</span></span> |
| <span data-ttu-id="42920-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="42920-184">localhost:1234/map2</span></span> | <span data-ttu-id="42920-185">マップのテスト 2</span><span class="sxs-lookup"><span data-stu-id="42920-185">Map Test 2</span></span> |
| <span data-ttu-id="42920-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="42920-186">localhost:1234/map3</span></span> | <span data-ttu-id="42920-187">非マップ デリゲートからこんにちはです。</span><span class="sxs-lookup"><span data-stu-id="42920-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="42920-188">ときに`Map`はこれを使用すると、一致したパス セグメントはから削除されます`HttpRequest.Path`に付加して`HttpRequest.PathBase`要求ごとにします。</span><span class="sxs-lookup"><span data-stu-id="42920-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="42920-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)指定の述語の結果に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="42920-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="42920-190">型の任意の述語`Func<HttpContext, bool>`パイプラインの新しい分岐に要求をマップするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="42920-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="42920-191">次の例では、述語はクエリ文字列変数の存在を検出する使用`branch`:</span><span class="sxs-lookup"><span data-stu-id="42920-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="42920-192">次の表は、要求および応答から`http://localhost:1234`前のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="42920-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="42920-193">要求</span><span class="sxs-lookup"><span data-stu-id="42920-193">Request</span></span> | <span data-ttu-id="42920-194">応答</span><span class="sxs-lookup"><span data-stu-id="42920-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="42920-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="42920-195">localhost:1234</span></span> | <span data-ttu-id="42920-196">非マップ デリゲートからこんにちはです。</span><span class="sxs-lookup"><span data-stu-id="42920-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="42920-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="42920-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="42920-198">分岐を使用するマスターを =</span><span class="sxs-lookup"><span data-stu-id="42920-198">Branch used = master</span></span>|

<span data-ttu-id="42920-199">`Map`たとえば、入れ子をサポートします。</span><span class="sxs-lookup"><span data-stu-id="42920-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="42920-200">`Map`できますと一致しても複数のセグメントを一度に例。</span><span class="sxs-lookup"><span data-stu-id="42920-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="42920-201">組み込みのミドルウェア</span><span class="sxs-lookup"><span data-stu-id="42920-201">Built-in middleware</span></span>

<span data-ttu-id="42920-202">ASP.NET Core は、追加する順序の説明と同様に、次のミドルウェア コンポーネントに付属します。</span><span class="sxs-lookup"><span data-stu-id="42920-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="42920-203">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="42920-203">Middleware</span></span> | <span data-ttu-id="42920-204">説明</span><span class="sxs-lookup"><span data-stu-id="42920-204">Description</span></span> | <span data-ttu-id="42920-205">順番</span><span class="sxs-lookup"><span data-stu-id="42920-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="42920-206">認証</span><span class="sxs-lookup"><span data-stu-id="42920-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="42920-207">認証のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-207">Provides authentication support.</span></span> | <span data-ttu-id="42920-208">前に`HttpContext.User`が必要です。</span><span class="sxs-lookup"><span data-stu-id="42920-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="42920-209">OAuth コールバックのターミナル。</span><span class="sxs-lookup"><span data-stu-id="42920-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="42920-210">CORS</span><span class="sxs-lookup"><span data-stu-id="42920-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="42920-211">クロス オリジン リソース共有を構成します。</span><span class="sxs-lookup"><span data-stu-id="42920-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="42920-212">コンポーネントの前に、CORS を使用します。</span><span class="sxs-lookup"><span data-stu-id="42920-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="42920-213">診断</span><span class="sxs-lookup"><span data-stu-id="42920-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="42920-214">診断を構成します。</span><span class="sxs-lookup"><span data-stu-id="42920-214">Configures diagnostics.</span></span> | <span data-ttu-id="42920-215">コンポーネントの前に、エラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="42920-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="42920-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="42920-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="42920-217">現在の要求にプロキシのヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="42920-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="42920-218">更新されたフィールドを使用するコンポーネントの前に (例: スキーム、ホスト、ClientIP、メソッド)。</span><span class="sxs-lookup"><span data-stu-id="42920-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="42920-219">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="42920-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="42920-220">応答のキャッシュのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-220">Provides support for caching responses.</span></span> | <span data-ttu-id="42920-221">コンポーネントの前にキャッシュする必要があります。</span><span class="sxs-lookup"><span data-stu-id="42920-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="42920-222">応答の圧縮</span><span class="sxs-lookup"><span data-stu-id="42920-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="42920-223">応答の圧縮のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="42920-224">コンポーネントの前に圧縮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="42920-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="42920-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="42920-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="42920-226">ローカライズ サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-226">Provides localization support.</span></span> | <span data-ttu-id="42920-227">ローカリゼーションの重要なコンポーネント前にです。</span><span class="sxs-lookup"><span data-stu-id="42920-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="42920-228">ルーティング</span><span class="sxs-lookup"><span data-stu-id="42920-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="42920-229">定義し、要求のルートを制約します。</span><span class="sxs-lookup"><span data-stu-id="42920-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="42920-230">一致するルートのターミナル。</span><span class="sxs-lookup"><span data-stu-id="42920-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="42920-231">セッション</span><span class="sxs-lookup"><span data-stu-id="42920-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="42920-232">ユーザー セッションを管理するためのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="42920-233">コンポーネントの前に、セッションを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="42920-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="42920-234">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="42920-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="42920-235">静的ファイルとディレクトリの参照を提供しているは、サポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="42920-236">ターミナル場合、要求では、ファイルと一致します。</span><span class="sxs-lookup"><span data-stu-id="42920-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="42920-237">URL 書き換え</span><span class="sxs-lookup"><span data-stu-id="42920-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="42920-238">Url の書き換えと、要求をリダイレクトするサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="42920-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="42920-239">コンポーネントの前に URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="42920-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="42920-240">WebSocket</span><span class="sxs-lookup"><span data-stu-id="42920-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="42920-241">Websocket プロトコルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="42920-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="42920-242">コンポーネントの前に、WebSocket 要求を受け入れることが必要です。</span><span class="sxs-lookup"><span data-stu-id="42920-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="42920-243">書き込みミドルウェア</span><span class="sxs-lookup"><span data-stu-id="42920-243">Writing middleware</span></span>

<span data-ttu-id="42920-244">ミドルウェアは一般に、クラスにカプセル化し、拡張メソッドを公開します。</span><span class="sxs-lookup"><span data-stu-id="42920-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="42920-245">次のミドルウェアは、クエリ文字列からの現在の要求のカルチャの設定を考慮してください。</span><span class="sxs-lookup"><span data-stu-id="42920-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="42920-246">注: 上記のサンプル コードは、ミドルウェア コンポーネントの作成を示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="42920-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="42920-247">参照してください[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)ASP.NET Core の組み込みのローカリゼーションをサポートします。</span><span class="sxs-lookup"><span data-stu-id="42920-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="42920-248">たとえば、カルチャに渡すことによって、ミドルウェアをテストする`http://localhost:7997/?culture=no`です。</span><span class="sxs-lookup"><span data-stu-id="42920-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="42920-249">次のコードは、ミドルウェア デリゲートをクラスに移動します。</span><span class="sxs-lookup"><span data-stu-id="42920-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="42920-250">次の拡張メソッドを公開、ミドルウェアを介して[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="42920-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="42920-251">次のコードを呼び出すからミドルウェア`Configure`:</span><span class="sxs-lookup"><span data-stu-id="42920-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="42920-252">ミドルウェアが従う必要があります、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)のコンス トラクターにその依存関係を公開することによりします。</span><span class="sxs-lookup"><span data-stu-id="42920-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="42920-253">1 回あたりのミドルウェアが構築*アプリケーションの有効期間*です。</span><span class="sxs-lookup"><span data-stu-id="42920-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="42920-254">参照してください*要求ごとの依存関係*場合は、次は、要求内のミドルウェアとサービスを共有する必要があります。</span><span class="sxs-lookup"><span data-stu-id="42920-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="42920-255">ミドルウェア コンポーネントには、コンス トラクターのパラメーターを使用して依存関係の挿入からその依存関係を解決できます。</span><span class="sxs-lookup"><span data-stu-id="42920-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="42920-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)追加のパラメーターを直接受け入れるもできます。</span><span class="sxs-lookup"><span data-stu-id="42920-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="42920-257">要求ごとの依存関係</span><span class="sxs-lookup"><span data-stu-id="42920-257">Per-request dependencies</span></span>

<span data-ttu-id="42920-258">ミドルウェアがない要求ごとの、アプリの起動時に構築されたため*スコープ*ミドルウェア コンス トラクターによって使用される有効期間サービスは各要求での他の依存関係挿入の種類と共有されません。</span><span class="sxs-lookup"><span data-stu-id="42920-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="42920-259">共有する場合は、*スコープ*ミドルウェアとその他の種類のサービス、これらのサービスを追加、`Invoke`メソッドのシグネチャ。</span><span class="sxs-lookup"><span data-stu-id="42920-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="42920-260">`Invoke`メソッドは、依存関係の挿入によって設定される追加のパラメーターを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="42920-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="42920-261">例:</span><span class="sxs-lookup"><span data-stu-id="42920-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="42920-262">リソース</span><span class="sxs-lookup"><span data-stu-id="42920-262">Resources</span></span>

* [<span data-ttu-id="42920-263">このドキュメントで使用するサンプル コード</span><span class="sxs-lookup"><span data-stu-id="42920-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="42920-264">ミドルウェアへの HTTP モジュールの移行</span><span class="sxs-lookup"><span data-stu-id="42920-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="42920-265">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="42920-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="42920-266">要求機能</span><span class="sxs-lookup"><span data-stu-id="42920-266">Request Features</span></span>](request-features.md)
