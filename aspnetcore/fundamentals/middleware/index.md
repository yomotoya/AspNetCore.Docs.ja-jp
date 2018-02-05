---
title: "ASP.NET Core のミドルウェア"
author: rick-anderson
description: "ASP.NET Core のミドルウェアと要求パイプラインについて説明します。"
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 887ba1a4742821226a7ebecfd238c97627d6c5f7
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="f1567-103">ASP.NET Core のミドルウェア</span><span class="sxs-lookup"><span data-stu-id="f1567-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="f1567-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f1567-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f1567-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="f1567-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="f1567-106">ミドルウェアとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="f1567-106">What is middleware?</span></span>

<span data-ttu-id="f1567-107">ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="f1567-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="f1567-108">各コンポーネントで次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="f1567-108">Each component:</span></span>

* <span data-ttu-id="f1567-109">要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。</span><span class="sxs-lookup"><span data-stu-id="f1567-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="f1567-110">パイプラインの次のコンポーネントが呼び出される前または呼び出された後に作業を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f1567-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="f1567-111">要求パイプラインの構築には、要求デリゲートが使われます。</span><span class="sxs-lookup"><span data-stu-id="f1567-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="f1567-112">要求デリゲートは、各 HTTP 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="f1567-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="f1567-113">要求デリゲートは、[Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、[Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) の各拡張メソッドを使って構成されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="f1567-114">個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="f1567-115">これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" または "*ミドルウェア コンポーネント*" です。</span><span class="sxs-lookup"><span data-stu-id="f1567-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="f1567-116">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。</span><span class="sxs-lookup"><span data-stu-id="f1567-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="f1567-117">[HTTP モジュールのミドルウェアへの移行](xref:migration/http-modules)に関するページでは、ASP.NET Core と ASP.NET 4.x の要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。</span><span class="sxs-lookup"><span data-stu-id="f1567-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="f1567-118">IApplicationBuilder を使用したミドルウェア パイプラインの作成</span><span class="sxs-lookup"><span data-stu-id="f1567-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="f1567-119">ASP.NET Core の要求パイプラインは、次の図で示すように (実行のスレッドは黒い矢印に沿って進みます)、順番に呼び出される要求デリゲートのシーケンスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![要求の到着、3 つのミドルウェアによる処理、アプリケーションからの応答の送信を示す要求処理パターン。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="f1567-123">各デリゲートは、次のデリゲートの前と後に操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f1567-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="f1567-124">また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、要求パイプラインのショートサーキットと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="f1567-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="f1567-125">不要な処理を回避するために、ショートさせることが望ましい場合がよくあります。</span><span class="sxs-lookup"><span data-stu-id="f1567-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="f1567-126">たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートさせることができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="f1567-127">例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1567-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="f1567-128">考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。</span><span class="sxs-lookup"><span data-stu-id="f1567-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="f1567-129">この場合、実際の要求パイプラインは含まれません。</span><span class="sxs-lookup"><span data-stu-id="f1567-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="f1567-130">代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="f1567-131">最初の [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) デリゲートが、パイプラインを終了します。</span><span class="sxs-lookup"><span data-stu-id="f1567-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="f1567-132">複数の要求デリゲートを [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) と一緒にチェーンすることができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="f1567-133">`next` パラメーターは、パイプラインの次のデリゲートを表します </span><span class="sxs-lookup"><span data-stu-id="f1567-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="f1567-134">(*next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートさせることができることに注意してください)。次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="f1567-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="f1567-135">応答がクライアントに送信された後に、`next.Invoke` を呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="f1567-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="f1567-136">応答が開始した後で `HttpResponse` を変更すると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f1567-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="f1567-137">たとえば、ヘッダーや状態コードの設定といった変化を行うと、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="f1567-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="f1567-138">`next` を呼び出した後で応答本文に書き込むと、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f1567-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="f1567-139">プロトコル違反が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f1567-139">May cause a protocol violation.</span></span> <span data-ttu-id="f1567-140">たとえば、示されている `content-length` より多くを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="f1567-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="f1567-141">本文の形式が破損する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f1567-141">May corrupt the body format.</span></span> <span data-ttu-id="f1567-142">たとえば、CSS ファイルに HTML フッターを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="f1567-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="f1567-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) は、ヘッダーが送信されたかどうかや、本文が書き込まれたかどうかを示す役に立つヒントです。</span><span class="sxs-lookup"><span data-stu-id="f1567-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="f1567-144">並べ替え</span><span class="sxs-lookup"><span data-stu-id="f1567-144">Ordering</span></span>

<span data-ttu-id="f1567-145">`Configure` メソッドでミドルウェア コンポーネントを追加する順序は、要求でそれらが呼び出される順序および応答での逆の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="f1567-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="f1567-146">この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。</span><span class="sxs-lookup"><span data-stu-id="f1567-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="f1567-147">Configure メソッド (下図参照) は、次のミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f1567-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="f1567-148">例外/エラー処理</span><span class="sxs-lookup"><span data-stu-id="f1567-148">Exception/error handling</span></span>
2. <span data-ttu-id="f1567-149">静的ファイル サーバー</span><span class="sxs-lookup"><span data-stu-id="f1567-149">Static file server</span></span>
3. <span data-ttu-id="f1567-150">認証</span><span class="sxs-lookup"><span data-stu-id="f1567-150">Authentication</span></span>
4. <span data-ttu-id="f1567-151">MVC</span><span class="sxs-lookup"><span data-stu-id="f1567-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f1567-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f1567-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f1567-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f1567-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="f1567-154">上記のコードでは、`UseExceptionHandler` がパイプラインに追加される最初のミドルウェア コンポーネントです。そのため、後続の呼び出しで発生するすべての例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="f1567-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="f1567-155">静的ファイル ミドルウェアはパイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショートさせることができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="f1567-156">静的ファイル ミドルウェアでは、承認チェックは提供**されません**。</span><span class="sxs-lookup"><span data-stu-id="f1567-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="f1567-157">*wwwroot* の下にあるものも含め、このミドルウェアによって提供されるすべてのファイルは、一般に公開されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="f1567-158">静的なファイルをセキュリティで保護する方法については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f1567-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f1567-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f1567-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="f1567-160">要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (`app.UseAuthentication`) に渡されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="f1567-161">ID は、認証されない要求をショートさせません。</span><span class="sxs-lookup"><span data-stu-id="f1567-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f1567-162">ID は要求を認証しますが、承認 (および却下) は、MVC が特定の Razor ページまたはコントローラーとアクションを選んだ後でのみ行われます。</span><span class="sxs-lookup"><span data-stu-id="f1567-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f1567-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f1567-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f1567-164">要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (`app.UseIdentity`) に渡されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="f1567-165">ID は、認証されない要求をショートさせません。</span><span class="sxs-lookup"><span data-stu-id="f1567-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="f1567-166">ID は要求を認証しますが、承認 (および却下) は、MVC が特定のコントローラーとアクションを選んだ後でのみ行われます。</span><span class="sxs-lookup"><span data-stu-id="f1567-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="f1567-167">次の例では、静的ファイルの要求が応答圧縮ミドルウェアの前に静的ファイル ミドルウェアによって処理される、ミドルウェアの順序を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="f1567-168">このミドルウェアの順序では、静的ファイルは圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="f1567-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="f1567-169">[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) からの MVC の応答は、圧縮することができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="f1567-170">Use、Run、Map</span><span class="sxs-lookup"><span data-stu-id="f1567-170">Use, Run, and Map</span></span>

<span data-ttu-id="f1567-171">HTTP パイプラインを構成するには、`Use`、`Run`、`Map` を使います。</span><span class="sxs-lookup"><span data-stu-id="f1567-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="f1567-172">`Use` メソッドは、パイプラインをショートさせることができます (つまり `next` 要求デリゲートを呼び出さない場合)。</span><span class="sxs-lookup"><span data-stu-id="f1567-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="f1567-173">`Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。</span><span class="sxs-lookup"><span data-stu-id="f1567-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="f1567-174">`Map*` 拡張メソッドは、パイプラインを分岐する規則として使われます。</span><span class="sxs-lookup"><span data-stu-id="f1567-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="f1567-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) は、指定された要求パスの一致に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="f1567-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="f1567-176">要求パスが指定されたパスで開始する場合、分岐が実行されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="f1567-177">次の表に、前記のコードを使ったときの、要求と `http://localhost:1234` からの応答を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="f1567-178">要求</span><span class="sxs-lookup"><span data-stu-id="f1567-178">Request</span></span> | <span data-ttu-id="f1567-179">応答</span><span class="sxs-lookup"><span data-stu-id="f1567-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="f1567-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f1567-180">localhost:1234</span></span> | <span data-ttu-id="f1567-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f1567-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="f1567-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="f1567-182">localhost:1234/map1</span></span> | <span data-ttu-id="f1567-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="f1567-183">Map Test 1</span></span> |
| <span data-ttu-id="f1567-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="f1567-184">localhost:1234/map2</span></span> | <span data-ttu-id="f1567-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="f1567-185">Map Test 2</span></span> |
| <span data-ttu-id="f1567-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="f1567-186">localhost:1234/map3</span></span> | <span data-ttu-id="f1567-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f1567-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="f1567-188">`Map` を使うと、一致したパス セグメントが、`HttpRequest.Path` から削除されて各要求の `HttpRequest.PathBase` に追加されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="f1567-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="f1567-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="f1567-190">`Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。</span><span class="sxs-lookup"><span data-stu-id="f1567-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="f1567-191">次の例では、述語を使って、クエリ文字列変数 `branch` の存在を検出しています。</span><span class="sxs-lookup"><span data-stu-id="f1567-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="f1567-192">次の表に、前記のコードを使ったときの、要求と `http://localhost:1234` からの応答を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="f1567-193">要求</span><span class="sxs-lookup"><span data-stu-id="f1567-193">Request</span></span> | <span data-ttu-id="f1567-194">応答</span><span class="sxs-lookup"><span data-stu-id="f1567-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="f1567-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="f1567-195">localhost:1234</span></span> | <span data-ttu-id="f1567-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="f1567-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="f1567-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="f1567-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="f1567-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="f1567-198">Branch used = master</span></span>|

<span data-ttu-id="f1567-199">`Map` は入れ子をサポートします。次にその例を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="f1567-200">`Map` では、一度に複数のセグメントを照合できます。次にその例を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="f1567-201">組み込みミドルウェア</span><span class="sxs-lookup"><span data-stu-id="f1567-201">Built-in middleware</span></span>

<span data-ttu-id="f1567-202">次の表では、ASP.NET Core に付属するミドルウェア コンポーネントと、それを追加する順序について説明します。</span><span class="sxs-lookup"><span data-stu-id="f1567-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="f1567-203">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="f1567-203">Middleware</span></span> | <span data-ttu-id="f1567-204">説明</span><span class="sxs-lookup"><span data-stu-id="f1567-204">Description</span></span> | <span data-ttu-id="f1567-205">順番</span><span class="sxs-lookup"><span data-stu-id="f1567-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="f1567-206">認証</span><span class="sxs-lookup"><span data-stu-id="f1567-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="f1567-207">認証のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-207">Provides authentication support.</span></span> | <span data-ttu-id="f1567-208">`HttpContext.User` が必要な場所の前。</span><span class="sxs-lookup"><span data-stu-id="f1567-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="f1567-209">OAuth コールバックの終端。</span><span class="sxs-lookup"><span data-stu-id="f1567-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="f1567-210">CORS</span><span class="sxs-lookup"><span data-stu-id="f1567-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="f1567-211">クロス オリジン リソース共有を構成します。</span><span class="sxs-lookup"><span data-stu-id="f1567-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="f1567-212">CORS を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="f1567-213">診断</span><span class="sxs-lookup"><span data-stu-id="f1567-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="f1567-214">診断を構成します。</span><span class="sxs-lookup"><span data-stu-id="f1567-214">Configures diagnostics.</span></span> | <span data-ttu-id="f1567-215">エラーを生成するコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="f1567-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="f1567-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="f1567-217">プロキシされたヘッダーを現在の要求に転送します。</span><span class="sxs-lookup"><span data-stu-id="f1567-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="f1567-218">更新されたフィールド (例: Scheme、Host、ClientIP、Method) を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="f1567-219">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="f1567-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="f1567-220">応答のキャッシュのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-220">Provides support for caching responses.</span></span> | <span data-ttu-id="f1567-221">キャッシュが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="f1567-222">応答圧縮</span><span class="sxs-lookup"><span data-stu-id="f1567-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="f1567-223">応答の圧縮のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="f1567-224">圧縮が必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="f1567-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="f1567-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="f1567-226">ローカライズのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-226">Provides localization support.</span></span> | <span data-ttu-id="f1567-227">ローカライズの必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="f1567-228">ルーティング</span><span class="sxs-lookup"><span data-stu-id="f1567-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="f1567-229">要求のルートを定義および制約します。</span><span class="sxs-lookup"><span data-stu-id="f1567-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="f1567-230">一致するルートの終端。</span><span class="sxs-lookup"><span data-stu-id="f1567-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="f1567-231">セッション</span><span class="sxs-lookup"><span data-stu-id="f1567-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="f1567-232">ユーザー セッションの管理のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="f1567-233">セッションが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="f1567-234">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="f1567-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="f1567-235">静的ファイルとディレクトリ参照に対応するサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="f1567-236">要求がファイルと一致した場合の終端。</span><span class="sxs-lookup"><span data-stu-id="f1567-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="f1567-237">URL リライト</span><span class="sxs-lookup"><span data-stu-id="f1567-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="f1567-238">URL の書き換えと要求のリダイレクトのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="f1567-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="f1567-239">URL を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="f1567-240">WebSocket</span><span class="sxs-lookup"><span data-stu-id="f1567-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="f1567-241">WebSocket プロトコルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="f1567-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="f1567-242">WebSocket 要求を受け入れる必要があるコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="f1567-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="f1567-243">ミドルウェアの作成</span><span class="sxs-lookup"><span data-stu-id="f1567-243">Writing middleware</span></span>

<span data-ttu-id="f1567-244">ミドルウェアは一般に、クラス内にカプセル化され、拡張メソッドで公開されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="f1567-245">クエリ文字列から現在の要求のカルチャを設定する次のようなミドルウェアを考慮します。</span><span class="sxs-lookup"><span data-stu-id="f1567-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="f1567-246">注: 上のサンプル コードを使って、ミドルウェア コンポーネントの作成方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f1567-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="f1567-247">ASP.NET Core に組み込まれているローカリゼーションのサポートについては、「[ASP.NET Core のグローバリゼーションおよびローカリゼーション](xref:fundamentals/localization)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f1567-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="f1567-248">カルチャを渡すことによって、ミドルウェアをテストできます (例: `http://localhost:7997/?culture=no`)。</span><span class="sxs-lookup"><span data-stu-id="f1567-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="f1567-249">次のコードは、ミドルウェアのデリゲートをクラスに移動します。</span><span class="sxs-lookup"><span data-stu-id="f1567-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="f1567-250">次の拡張メソッドは、[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) を介してミドルウェアを公開します。</span><span class="sxs-lookup"><span data-stu-id="f1567-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="f1567-251">次のコードは、`Configure` からミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="f1567-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="f1567-252">ミドルウェアは、コンストラクターで依存関係を公開することにより、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="f1567-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="f1567-253">ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。</span><span class="sxs-lookup"><span data-stu-id="f1567-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="f1567-254">要求内でミドルウェアとサービスを共有する必要がある場合は、下記の「*要求ごとの依存関係*」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f1567-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="f1567-255">ミドルウェア コンポーネントは、コンストラクター パラメーターにより、依存関係の挿入から依存関係を解決できます。</span><span class="sxs-lookup"><span data-stu-id="f1567-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="f1567-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) は、追加パラメーターを直接受け入れることもできます。</span><span class="sxs-lookup"><span data-stu-id="f1567-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="f1567-257">要求ごとの依存関係</span><span class="sxs-lookup"><span data-stu-id="f1567-257">Per-request dependencies</span></span>

<span data-ttu-id="f1567-258">ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。</span><span class="sxs-lookup"><span data-stu-id="f1567-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="f1567-259">ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="f1567-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="f1567-260">`Invoke` メソッドは、依存関係の挿入によって設定される追加のパラメーターを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="f1567-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="f1567-261">例:</span><span class="sxs-lookup"><span data-stu-id="f1567-261">For example:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="f1567-262">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f1567-262">Additional resources</span></span>

* [<span data-ttu-id="f1567-263">ミドルウェアへの HTTP モジュールの移行</span><span class="sxs-lookup"><span data-stu-id="f1567-263">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="f1567-264">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="f1567-264">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="f1567-265">要求機能</span><span class="sxs-lookup"><span data-stu-id="f1567-265">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="f1567-266">ファクトリ ベースのミドルウェアのアクティブ化</span><span class="sxs-lookup"><span data-stu-id="f1567-266">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
