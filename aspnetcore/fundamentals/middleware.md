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
uid: fundamentals/middleware
ms.openlocfilehash: 697a3a8e475dda0d48a11dbfad8fbb0603aa1bf8
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="9dd4d-103">ASP.NET Core のミドルウェアの基礎</span><span class="sxs-lookup"><span data-stu-id="9dd4d-103">ASP.NET Core Middleware Fundamentals</span></span>

<a name="fundamentals-middleware"></a>

<span data-ttu-id="9dd4d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9dd4d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9dd4d-105">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="9dd4d-106">ミドルウェアとは</span><span class="sxs-lookup"><span data-stu-id="9dd4d-106">What is middleware?</span></span>

<span data-ttu-id="9dd4d-107">ミドルウェアは、要求と応答を処理するアプリケーション パイプラインとして構成されたソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="9dd4d-108">各コンポーネントで次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-108">Each component:</span></span>

* <span data-ttu-id="9dd4d-109">要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="9dd4d-110">パイプラインの次のコンポーネントが呼び出される前または呼び出された後に作業を実行できます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="9dd4d-111">要求デリゲートは、要求パイプラインの構築に使用されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="9dd4d-112">要求デリゲートは、各 HTTP 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="9dd4d-113">要求デリゲートは、[Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)、[Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) の各拡張メソッドを使って構成されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="9dd4d-114">個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="9dd4d-115">これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" または "*ミドルウェア コンポーネント*" です。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="9dd4d-116">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートサーキットします。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="9dd4d-117">[HTTP モジュールのミドルウェアへの移行](../migration/http-modules.md)に関する記事では、ASP.NET Core および以前のバージョンの要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-117">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="9dd4d-118">IApplicationBuilder を使用したミドルウェア パイプラインの作成</span><span class="sxs-lookup"><span data-stu-id="9dd4d-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="9dd4d-119">ASP.NET Core の要求パイプラインは、次の図で示すように (実行のスレッドは黒い矢印に従います)、順番に呼び出される要求デリゲートのシーケンスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![要求の到着、3 つのミドルウェアによる処理、アプリケーションからの応答の送信を示す要求処理パターン。](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="9dd4d-123">各デリゲートは、次のデリゲートの前と後に操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="9dd4d-124">また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、要求パイプラインのショートサーキットと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="9dd4d-125">不要な処理を回避するために、ショートサーキットが望ましい場合がよくあります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="9dd4d-126">たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートサーキットすることができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="9dd4d-127">例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出される必要があります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="9dd4d-128">考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="9dd4d-129">この場合、実際の要求パイプラインは含まれません。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="9dd4d-130">代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

<span data-ttu-id="9dd4d-131">最初の [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) デリゲートは、パイプラインを終了します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="9dd4d-132">[app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) を使用して、複数の要求デリゲートをチェーン化することができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="9dd4d-133">`next` パラメーターは、パイプラインの次のデリゲートを表します </span><span class="sxs-lookup"><span data-stu-id="9dd4d-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="9dd4d-134">(*next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートサーキットできることに注意してください)。次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="9dd4d-135">応答がクライアントに送信された後では、`next.Invoke` を呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="9dd4d-136">応答が開始した後で `HttpResponse` を変更すると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="9dd4d-137">たとえば、ヘッダーや状態コードの設定といった変化を行うと、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="9dd4d-138">`next` を呼び出した後で応答本文に書き込むと、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="9dd4d-139">プロトコル違反が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-139">May cause a protocol violation.</span></span> <span data-ttu-id="9dd4d-140">たとえば、示されている `content-length` より多くを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="9dd4d-141">本文の形式が破損する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-141">May corrupt the body format.</span></span> <span data-ttu-id="9dd4d-142">たとえば、CSS ファイルに HTML のフッターを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="9dd4d-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) は、ヘッダーが送信されたかどうかや本文が書き込まれたかどうかを示すために役立つヒントです。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="9dd4d-144">並べ替え</span><span class="sxs-lookup"><span data-stu-id="9dd4d-144">Ordering</span></span>

<span data-ttu-id="9dd4d-145">`Configure` メソッドでミドルウェア コンポーネントを追加する順序は、要求でそれらが呼び出される順序および応答での逆の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="9dd4d-146">この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="9dd4d-147">Configure メソッド (下図参照) は、次のミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="9dd4d-148">例外/エラー処理</span><span class="sxs-lookup"><span data-stu-id="9dd4d-148">Exception/error handling</span></span>
2. <span data-ttu-id="9dd4d-149">静的ファイル サーバー</span><span class="sxs-lookup"><span data-stu-id="9dd4d-149">Static file server</span></span>
3. <span data-ttu-id="9dd4d-150">認証</span><span class="sxs-lookup"><span data-stu-id="9dd4d-150">Authentication</span></span>
4. <span data-ttu-id="9dd4d-151">MVC</span><span class="sxs-lookup"><span data-stu-id="9dd4d-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd4d-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd4d-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd4d-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd4d-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="9dd4d-154">上記のコードでは、`UseExceptionHandler` が、パイプラインに追加される最初のミドルウェア コンポーネントなので、その後の呼び出しで発生する例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="9dd4d-155">静的ファイル ミドルウェアは、パイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショート サーキットします。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="9dd4d-156">静的ファイル ミドルウェアは、承認チェックを提供**しません**。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="9dd4d-157">*wwwroot* の下にあるファイルを含め、処理されるすべてファイルが一般に公開されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="9dd4d-158">静的ファイルを保護する方法については、「[Working with static files](xref:fundamentals/static-files)」(静的ファイルの操作) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9dd4d-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9dd4d-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="9dd4d-160">要求が静的ファイル ミドルウェアによって処理されない場合、要求が Identity ミドルウェア (`app.UseAuthentication`) に渡され、そこで認証が実行されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="9dd4d-161">Identity は、認証されていない要求をショート サーキットしません。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="9dd4d-162">Identity は要求を承認しますが、承認 (および却下) は、MVC が固有の Razor ページまたはコント ローラーとアクションを選択した後にのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9dd4d-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9dd4d-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9dd4d-164">要求が静的ファイル ミドルウェアによって処理されない場合、要求が Identity ミドルウェア (`app.UseIdentity`) に渡され、そこで認証が実行されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="9dd4d-165">Identity は、認証されていない要求をショート サーキットしません。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="9dd4d-166">Identity は要求を承認しますが、承認 (および却下) は、MVC が固有のコント ローラーとアクションを選択した後にのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="9dd4d-167">次の例では、応答の圧縮のミドルウェアの前に、静的ファイル ミドルウェアによって静的ファイルの要求を処理する、ミドルウェアの順序付けを示します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="9dd4d-168">このミドルウェアの順序では、静的なファイルは圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="9dd4d-169">[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) からの MVC 応答を圧縮することができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="9dd4d-170">Use、Run、および Map</span><span class="sxs-lookup"><span data-stu-id="9dd4d-170">Use, Run, and Map</span></span>

<span data-ttu-id="9dd4d-171">HTTP パイプラインは、`Use`、`Run`、および `Map` を使用して構成します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="9dd4d-172">`Use` メソッドは、パイプラインをショートサーキットすることができます (つまり `next` 要求デリゲートを呼び出さない場合)。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="9dd4d-173">`Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="9dd4d-174">`Map*` 拡張機能は、パイプラインを分岐する規則として使用されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="9dd4d-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) は、指定された要求パスの一致に基づいて要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="9dd4d-176">要求パスが、指定されたパスで始まる場合、分岐が実行されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="9dd4d-177">次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="9dd4d-178">要求</span><span class="sxs-lookup"><span data-stu-id="9dd4d-178">Request</span></span> | <span data-ttu-id="9dd4d-179">応答</span><span class="sxs-lookup"><span data-stu-id="9dd4d-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="9dd4d-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="9dd4d-180">localhost:1234</span></span> | <span data-ttu-id="9dd4d-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="9dd4d-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="9dd4d-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="9dd4d-182">localhost:1234/map1</span></span> | <span data-ttu-id="9dd4d-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="9dd4d-183">Map Test 1</span></span> |
| <span data-ttu-id="9dd4d-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="9dd4d-184">localhost:1234/map2</span></span> | <span data-ttu-id="9dd4d-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="9dd4d-185">Map Test 2</span></span> |
| <span data-ttu-id="9dd4d-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="9dd4d-186">localhost:1234/map3</span></span> | <span data-ttu-id="9dd4d-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="9dd4d-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="9dd4d-188">`Map` を使用すると、一致したパス セグメントが `HttpRequest.Path` から削除され、要求ごとに `HttpRequest.PathBase` に付加されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="9dd4d-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="9dd4d-190">`Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="9dd4d-191">次の例では、クエリ文字列変数 `branch` の存在を検出するために術後が使用されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="9dd4d-192">次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="9dd4d-193">要求</span><span class="sxs-lookup"><span data-stu-id="9dd4d-193">Request</span></span> | <span data-ttu-id="9dd4d-194">応答</span><span class="sxs-lookup"><span data-stu-id="9dd4d-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="9dd4d-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="9dd4d-195">localhost:1234</span></span> | <span data-ttu-id="9dd4d-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="9dd4d-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="9dd4d-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="9dd4d-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="9dd4d-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="9dd4d-198">Branch used = master</span></span>|

<span data-ttu-id="9dd4d-199">たとえば次のように、`Map` は入れ子をサポートします。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="9dd4d-200">たとえば次のように、`Map` は複数のセグメントと一度に一致させることもできます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="9dd4d-201">組み込みミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9dd4d-201">Built-in middleware</span></span>

<span data-ttu-id="9dd4d-202">ASP.NET Core には、次のミドルウェア コンポーネントおよびそれらを追加する順序の説明が付属しています。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="9dd4d-203">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="9dd4d-203">Middleware</span></span> | <span data-ttu-id="9dd4d-204">説明</span><span class="sxs-lookup"><span data-stu-id="9dd4d-204">Description</span></span> | <span data-ttu-id="9dd4d-205">順番</span><span class="sxs-lookup"><span data-stu-id="9dd4d-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="9dd4d-206">認証</span><span class="sxs-lookup"><span data-stu-id="9dd4d-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="9dd4d-207">認証のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-207">Provides authentication support.</span></span> | <span data-ttu-id="9dd4d-208">`HttpContext.User` が必要な場所の前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="9dd4d-209">OAuth コールバックの終端です。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="9dd4d-210">CORS</span><span class="sxs-lookup"><span data-stu-id="9dd4d-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="9dd4d-211">クロス オリジン リソース共有を構成します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="9dd4d-212">CORS を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="9dd4d-213">診断</span><span class="sxs-lookup"><span data-stu-id="9dd4d-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="9dd4d-214">診断を構成します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-214">Configures diagnostics.</span></span> | <span data-ttu-id="9dd4d-215">エラーを生成するコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="9dd4d-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="9dd4d-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="9dd4d-217">プロキシされたヘッダーを現在の要求に転送します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="9dd4d-218">更新されたフィールド (例: Scheme、Host、ClientIP、Method) を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="9dd4d-219">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="9dd4d-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="9dd4d-220">応答のキャッシュのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-220">Provides support for caching responses.</span></span> | <span data-ttu-id="9dd4d-221">キャッシュが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="9dd4d-222">応答圧縮</span><span class="sxs-lookup"><span data-stu-id="9dd4d-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="9dd4d-223">応答の圧縮のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="9dd4d-224">圧縮が必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="9dd4d-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="9dd4d-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="9dd4d-226">ローカライズのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-226">Provides localization support.</span></span> | <span data-ttu-id="9dd4d-227">ローカリゼーションが重要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="9dd4d-228">ルーティング</span><span class="sxs-lookup"><span data-stu-id="9dd4d-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="9dd4d-229">要求のルートを定義および制約します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="9dd4d-230">一致するルートの最後。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="9dd4d-231">セッション</span><span class="sxs-lookup"><span data-stu-id="9dd4d-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="9dd4d-232">ユーザー セッションの管理のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="9dd4d-233">セッションが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="9dd4d-234">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="9dd4d-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="9dd4d-235">静的ファイルとディレクトリ参照に対応するサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="9dd4d-236">要求がファイルと一致した場合の最後。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="9dd4d-237">URL リライト</span><span class="sxs-lookup"><span data-stu-id="9dd4d-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="9dd4d-238">URL の書き換えと要求のリダイレクトのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="9dd4d-239">URL を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="9dd4d-240">WebSocket</span><span class="sxs-lookup"><span data-stu-id="9dd4d-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="9dd4d-241">WebSocket プロトコルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="9dd4d-242">WebSocket 要求を受け入れる必要があるコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="9dd4d-243">ミドルウェアの作成</span><span class="sxs-lookup"><span data-stu-id="9dd4d-243">Writing middleware</span></span>

<span data-ttu-id="9dd4d-244">ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="9dd4d-245">クエリ文字列からの現在の要求のカルチャを設定する次のミドルウェアについて考えてください。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="9dd4d-246">注: 上記のサンプル コードは、ミドルウェア コンポーネントの作成を示すために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="9dd4d-247">ASP.NET Core の組み込みのローカリゼーション サポートについては、「[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="9dd4d-248">たとえば `http://localhost:7997/?culture=no` のように、カルチャで渡すことによってミドルウェアをテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="9dd4d-249">次のコードは、ミドルウェア デリゲートをクラスに移動します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="9dd4d-250">次の拡張メソッドは、[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) を介してミドルウェアを公開します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="9dd4d-251">次のコードは、`Configure` からミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="9dd4d-252">ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="9dd4d-253">ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="9dd4d-254">要求内でミドルウェアとサービスを共有する必要がある場合は、後の「*要求ごとの依存関係*」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="9dd4d-255">ミドルウェア コンポーネントは、コンストラクター パラメーターにより、依存関係の挿入から依存関係を解決できます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="9dd4d-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) は、追加パラメーターを直接受け入れることもできます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="9dd4d-257">要求ごとの依存関係</span><span class="sxs-lookup"><span data-stu-id="9dd4d-257">Per-request dependencies</span></span>

<span data-ttu-id="9dd4d-258">ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="9dd4d-259">ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="9dd4d-260">`Invoke` メソッドは、依存関係の挿入によって設定される追加のパラメーターを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="9dd4d-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="9dd4d-261">例:</span><span class="sxs-lookup"><span data-stu-id="9dd4d-261">For example:</span></span>

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

## <a name="resources"></a><span data-ttu-id="9dd4d-262">リソース</span><span class="sxs-lookup"><span data-stu-id="9dd4d-262">Resources</span></span>

* [<span data-ttu-id="9dd4d-263">このドキュメントで使用するサンプル コード</span><span class="sxs-lookup"><span data-stu-id="9dd4d-263">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="9dd4d-264">ミドルウェアへの HTTP モジュールの移行</span><span class="sxs-lookup"><span data-stu-id="9dd4d-264">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="9dd4d-265">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="9dd4d-265">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="9dd4d-266">要求機能</span><span class="sxs-lookup"><span data-stu-id="9dd4d-266">Request Features</span></span>](request-features.md)
