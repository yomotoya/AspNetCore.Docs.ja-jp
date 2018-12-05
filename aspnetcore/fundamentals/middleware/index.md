---
title: ASP.NET Core のミドルウェア
author: rick-anderson
description: ASP.NET Core のミドルウェアと要求パイプラインについて説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 4e5da1036b77e876899ccdea48bdec69454e1657
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861486"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="7892a-103">ASP.NET Core のミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7892a-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="7892a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)、[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7892a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7892a-105">ミドルウェアとは、要求と応答を処理するために、アプリのパイプラインに組み込まれたソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="7892a-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="7892a-106">各コンポーネントで次の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="7892a-106">Each component:</span></span>

* <span data-ttu-id="7892a-107">要求をパイプライン内の次のコンポーネントに渡すかどうかを選択します。</span><span class="sxs-lookup"><span data-stu-id="7892a-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="7892a-108">パイプラインの次のコンポーネントが呼び出される前または呼び出された後に作業を実行できます。</span><span class="sxs-lookup"><span data-stu-id="7892a-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="7892a-109">要求デリゲートは、要求パイプラインの構築に使用されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="7892a-110">要求デリゲートは、各 HTTP 要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="7892a-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="7892a-111">要求デリゲートは、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>、<xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> の各拡張メソッドを使って構成されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="7892a-112">個々の要求デリゲートは、匿名メソッドとしてインラインで指定するか (インライン ミドルウェアと呼ばれます)、または再利用可能なクラスで定義することができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="7892a-113">これらの再利用可能なクラスとインラインの匿名メソッドは、"*ミドルウェア*" です。"*ミドルウェア コンポーネント*" とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="7892a-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="7892a-114">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、パイプラインをショートさせます。</span><span class="sxs-lookup"><span data-stu-id="7892a-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="7892a-115">「<xref:migration/http-modules>」では、ASP.NET Core と ASP.NET 4.x の要求パイプラインの違いについての説明と、他のミドルウェア サンプルが提供されています。</span><span class="sxs-lookup"><span data-stu-id="7892a-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="7892a-116">IApplicationBuilder を使用したミドルウェア パイプラインの作成</span><span class="sxs-lookup"><span data-stu-id="7892a-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="7892a-117">ASP.NET Core 要求パイプラインは、順番に呼び出される一連の要求デリゲートで構成されています。</span><span class="sxs-lookup"><span data-stu-id="7892a-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="7892a-118">次の図は、その概念を示しています。</span><span class="sxs-lookup"><span data-stu-id="7892a-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="7892a-119">実行のスレッドは黒い矢印をたどります。</span><span class="sxs-lookup"><span data-stu-id="7892a-119">The thread of execution follows the black arrows.</span></span>

![要求の到着、3 つのミドルウェアによる処理、アプリからの応答の送信を示す要求処理パターン。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="7892a-123">各デリゲートは、次のデリゲートの前と後に操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="7892a-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="7892a-124">また、デリゲートは要求を次のデリゲートに渡さないこともできます。これは、*要求パイプラインのショートサーキット*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="7892a-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="7892a-125">不要な処理を回避するために、ショートさせることが望ましい場合がよくあります。</span><span class="sxs-lookup"><span data-stu-id="7892a-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="7892a-126">たとえば、静的ファイル ミドルウェアは、静的ファイルの要求を返して、残りのパイプラインをショートさせることができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-126">For example, Static File Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="7892a-127">例外処理デリゲートは、パイプラインの後のステージで発生する例外をキャッチできるように、パイプラインの早い段階で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="7892a-128">考えられる最も簡単な ASP.NET Core アプリは、1 つの要求デリゲートを設定してすべての要求を処理するものです。</span><span class="sxs-lookup"><span data-stu-id="7892a-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="7892a-129">この場合、実際の要求パイプラインは含まれません。</span><span class="sxs-lookup"><span data-stu-id="7892a-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="7892a-130">代わりに、すべての HTTP 要求に対応して単一の匿名関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="7892a-131">最初の <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> デリゲートが、パイプラインを終了します。</span><span class="sxs-lookup"><span data-stu-id="7892a-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="7892a-132">複数の要求デリゲートを <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> と一緒にチェーンします。</span><span class="sxs-lookup"><span data-stu-id="7892a-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="7892a-133">`next` パラメーターは、パイプラインの次のデリゲートを表します </span><span class="sxs-lookup"><span data-stu-id="7892a-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="7892a-134">*next* パラメーターを "*呼び出さない*" ことで、パイプラインをショートさせることができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="7892a-135">次の例で示すように、通常は、次のデリゲートの前と後の両方でアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="7892a-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="7892a-136">応答がクライアントに送信された後に、`next.Invoke` を呼び出さないでください。</span><span class="sxs-lookup"><span data-stu-id="7892a-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="7892a-137">応答が開始した後で <xref:Microsoft.AspNetCore.Http.HttpResponse> を変更すると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="7892a-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="7892a-138">たとえば、ヘッダーや状態コードの設定などの変更があると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="7892a-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="7892a-139">`next` を呼び出した後で応答本文に書き込むと、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="7892a-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="7892a-140">プロトコル違反が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7892a-140">May cause a protocol violation.</span></span> <span data-ttu-id="7892a-141">たとえば、示されている `Content-Length` より多くを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="7892a-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="7892a-142">本文の形式が破損する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7892a-142">May corrupt the body format.</span></span> <span data-ttu-id="7892a-143">たとえば、CSS ファイルに HTML フッターを書き込んだ場合。</span><span class="sxs-lookup"><span data-stu-id="7892a-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="7892a-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> は、ヘッダーが送信されたかどうかや本文が書き込まれたかどうかを示すために役立つヒントです。</span><span class="sxs-lookup"><span data-stu-id="7892a-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="7892a-145">順番</span><span class="sxs-lookup"><span data-stu-id="7892a-145">Order</span></span>

<span data-ttu-id="7892a-146">`Startup.Configure` メソッドでミドルウェア コンポーネントを追加する順序は、要求でミドルウェア コンポーネントが呼び出される順序および応答での逆の順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="7892a-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="7892a-147">この順序は、セキュリティ、パフォーマンス、および機能にとって重要です。</span><span class="sxs-lookup"><span data-stu-id="7892a-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="7892a-148">次の `Startup.Configure` メソッドにより、共通アプリ シナリオのためのミドルウェア コンポーネントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-148">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="7892a-149">例外/エラー処理</span><span class="sxs-lookup"><span data-stu-id="7892a-149">Exception/error handling</span></span>
1. <span data-ttu-id="7892a-150">HTTP Strict Transport Security プロトコル</span><span class="sxs-lookup"><span data-stu-id="7892a-150">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="7892a-151">HTTPS リダイレクト</span><span class="sxs-lookup"><span data-stu-id="7892a-151">HTTPS redirection</span></span>
1. <span data-ttu-id="7892a-152">静的ファイル サーバー</span><span class="sxs-lookup"><span data-stu-id="7892a-152">Static file server</span></span>
1. <span data-ttu-id="7892a-153">Cookie ポリシーの適用</span><span class="sxs-lookup"><span data-stu-id="7892a-153">Cookie policy enforcement</span></span>
1. <span data-ttu-id="7892a-154">認証</span><span class="sxs-lookup"><span data-stu-id="7892a-154">Authentication</span></span>
1. <span data-ttu-id="7892a-155">セッション</span><span class="sxs-lookup"><span data-stu-id="7892a-155">Session</span></span>
1. <span data-ttu-id="7892a-156">MVC</span><span class="sxs-lookup"><span data-stu-id="7892a-156">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="7892a-157">例外/エラー処理</span><span class="sxs-lookup"><span data-stu-id="7892a-157">Exception/error handling</span></span>
1. <span data-ttu-id="7892a-158">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="7892a-158">Static files</span></span>
1. <span data-ttu-id="7892a-159">認証</span><span class="sxs-lookup"><span data-stu-id="7892a-159">Authentication</span></span>
1. <span data-ttu-id="7892a-160">セッション</span><span class="sxs-lookup"><span data-stu-id="7892a-160">Session</span></span>
1. <span data-ttu-id="7892a-161">MVC</span><span class="sxs-lookup"><span data-stu-id="7892a-161">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="7892a-162">前のコード例では、各ミドルウェアの拡張メソッドが <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 名前空間を通じて <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> で公開されています。</span><span class="sxs-lookup"><span data-stu-id="7892a-162">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7892a-163">パイプラインに追加された最初のミドルウェア コンポーネントは <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> です。</span><span class="sxs-lookup"><span data-stu-id="7892a-163"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="7892a-164">そのため、例外ハンドラー ミドルウェアでは、以降の呼び出しで発生するすべての例外がキャッチされます。</span><span class="sxs-lookup"><span data-stu-id="7892a-164">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="7892a-165">静的ファイル ミドルウェアはパイプラインの早い段階で呼び出されるので、要求を処理し、残りのコンポーネントを通過せずにショートさせることができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-165">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="7892a-166">静的ファイル ミドルウェアでは、承認チェックは提供**されません**。</span><span class="sxs-lookup"><span data-stu-id="7892a-166">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="7892a-167">*wwwroot* の下にあるものも含め、このミドルウェアによって提供されるすべてのファイルは、一般に公開されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-167">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="7892a-168">静的ファイルを保護する方法については、「<xref:fundamentals/static-files>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7892a-168">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7892a-169">要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する認証ミドルウェア (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) に渡されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-169">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="7892a-170">認証は、認証されない要求をショートさせません。</span><span class="sxs-lookup"><span data-stu-id="7892a-170">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="7892a-171">認証ミドルウェアは要求を認証しますが、承認 (および却下) は、MVC が特定の Razor ページまたは MVC コントローラーとアクションを選んだ後でのみ行われます。</span><span class="sxs-lookup"><span data-stu-id="7892a-171">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7892a-172">要求が静的ファイル ミドルウェアによって処理されない場合、要求は認証を実行する ID ミドルウェア (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>) に渡されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-172">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="7892a-173">ID は、認証されない要求をショートさせません。</span><span class="sxs-lookup"><span data-stu-id="7892a-173">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="7892a-174">ID は要求を認証しますが、承認 (および却下) は、MVC が特定のコントローラーとアクションを選んだ後でのみ行われます。</span><span class="sxs-lookup"><span data-stu-id="7892a-174">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="7892a-175">次の例は、静的ファイルの要求が応答圧縮ミドルウェアの前に静的ファイル ミドルウェアによって処理される、ミドルウェアの順序を示します。</span><span class="sxs-lookup"><span data-stu-id="7892a-175">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="7892a-176">静的ファイルは、このミドルウェアの順序では圧縮されません。</span><span class="sxs-lookup"><span data-stu-id="7892a-176">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="7892a-177"><xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> からの MVC 応答を圧縮することができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-177">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="7892a-178">Use、Run、および Map</span><span class="sxs-lookup"><span data-stu-id="7892a-178">Use, Run, and Map</span></span>

<span data-ttu-id="7892a-179">HTTP パイプラインを構成するには、`Use`、`Run`、`Map` を使います。</span><span class="sxs-lookup"><span data-stu-id="7892a-179">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="7892a-180">`Use` メソッドは、パイプラインをショートさせることができます (つまり `next` 要求デリゲートを呼び出さない場合)。</span><span class="sxs-lookup"><span data-stu-id="7892a-180">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="7892a-181">`Run` は規則であり、一部のミドルウェア コンポーネントは、パイプラインの最後に実行される `Run[Middleware]` メソッドを公開することがあります。</span><span class="sxs-lookup"><span data-stu-id="7892a-181">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="7892a-182"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 拡張メソッドは、パイプラインを分岐する規則として使われます。</span><span class="sxs-lookup"><span data-stu-id="7892a-182"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="7892a-183">`Map*` は、指定された要求パスの一致に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="7892a-183">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="7892a-184">要求パスが指定されたパスで開始する場合、分岐が実行されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-184">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="7892a-185">次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。</span><span class="sxs-lookup"><span data-stu-id="7892a-185">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="7892a-186">要求</span><span class="sxs-lookup"><span data-stu-id="7892a-186">Request</span></span>             | <span data-ttu-id="7892a-187">応答</span><span class="sxs-lookup"><span data-stu-id="7892a-187">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="7892a-188">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="7892a-188">localhost:1234</span></span>      | <span data-ttu-id="7892a-189">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7892a-189">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="7892a-190">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="7892a-190">localhost:1234/map1</span></span> | <span data-ttu-id="7892a-191">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="7892a-191">Map Test 1</span></span>                   |
| <span data-ttu-id="7892a-192">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="7892a-192">localhost:1234/map2</span></span> | <span data-ttu-id="7892a-193">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="7892a-193">Map Test 2</span></span>                   |
| <span data-ttu-id="7892a-194">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="7892a-194">localhost:1234/map3</span></span> | <span data-ttu-id="7892a-195">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7892a-195">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="7892a-196">`Map` を使用すると、一致したパス セグメントが `HttpRequest.Path` から削除され、要求ごとに `HttpRequest.PathBase` に付加されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-196">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="7892a-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) は、指定された述語の結果に基づいて、要求パイプラインを分岐します。</span><span class="sxs-lookup"><span data-stu-id="7892a-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="7892a-198">`Func<HttpContext, bool>` という型の任意の述語を使って、要求をパイプラインの新しい分岐にマップできます。</span><span class="sxs-lookup"><span data-stu-id="7892a-198">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="7892a-199">次の例では、クエリ文字列変数 `branch` の存在を検出するために術後が使用されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-199">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="7892a-200">次の表は、前のコードを使用した `http://localhost:1234` からの要求および応答を示しています。</span><span class="sxs-lookup"><span data-stu-id="7892a-200">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="7892a-201">要求</span><span class="sxs-lookup"><span data-stu-id="7892a-201">Request</span></span>                       | <span data-ttu-id="7892a-202">応答</span><span class="sxs-lookup"><span data-stu-id="7892a-202">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="7892a-203">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="7892a-203">localhost:1234</span></span>                | <span data-ttu-id="7892a-204">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7892a-204">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="7892a-205">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="7892a-205">localhost:1234/?branch=master</span></span> | <span data-ttu-id="7892a-206">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="7892a-206">Branch used = master</span></span>         |

<span data-ttu-id="7892a-207">`Map` は入れ子をサポートします。次にその例を示します。</span><span class="sxs-lookup"><span data-stu-id="7892a-207">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="7892a-208">`Map` では、次のように一度に複数のセグメントを照合できます。</span><span class="sxs-lookup"><span data-stu-id="7892a-208">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="7892a-209">組み込みミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7892a-209">Built-in middleware</span></span>

<span data-ttu-id="7892a-210">ASP.NET Core には、次のミドルウェア コンポーネントが付属しています。</span><span class="sxs-lookup"><span data-stu-id="7892a-210">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="7892a-211">「*順番*」列には、要求パイプライン内のミドルウェアの配置と、ミドルウェアが要求を中断し、他のミドルウェアが要求を処理できなくなる条件に関する注意が記載されています。</span><span class="sxs-lookup"><span data-stu-id="7892a-211">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="7892a-212">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="7892a-212">Middleware</span></span> | <span data-ttu-id="7892a-213">説明</span><span class="sxs-lookup"><span data-stu-id="7892a-213">Description</span></span> | <span data-ttu-id="7892a-214">順番</span><span class="sxs-lookup"><span data-stu-id="7892a-214">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="7892a-215">認証</span><span class="sxs-lookup"><span data-stu-id="7892a-215">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="7892a-216">認証のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-216">Provides authentication support.</span></span> | <span data-ttu-id="7892a-217">`HttpContext.User` が必要な場所の前。</span><span class="sxs-lookup"><span data-stu-id="7892a-217">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="7892a-218">OAuth コールバックの終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-218">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="7892a-219">Cookie のポリシー</span><span class="sxs-lookup"><span data-stu-id="7892a-219">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="7892a-220">個人情報の保存に関してユーザーからの同意を追跡し、`secure` や `SameSite` など、Cookie フィールドの最小要件を適用します。</span><span class="sxs-lookup"><span data-stu-id="7892a-220">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="7892a-221">Cookie を発行するミドルウェアの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-221">Before middleware that issues cookies.</span></span> <span data-ttu-id="7892a-222">例: 認証、セッション、MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="7892a-222">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="7892a-223">CORS</span><span class="sxs-lookup"><span data-stu-id="7892a-223">CORS</span></span>](xref:security/cors) | <span data-ttu-id="7892a-224">クロス オリジン リソース共有を構成します。</span><span class="sxs-lookup"><span data-stu-id="7892a-224">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="7892a-225">CORS を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-225">Before components that use CORS.</span></span> |
| [<span data-ttu-id="7892a-226">診断</span><span class="sxs-lookup"><span data-stu-id="7892a-226">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="7892a-227">診断を構成します。</span><span class="sxs-lookup"><span data-stu-id="7892a-227">Configures diagnostics.</span></span> | <span data-ttu-id="7892a-228">エラーを生成するコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-228">Before components that generate errors.</span></span> |
| [<span data-ttu-id="7892a-229">転送されるヘッダー</span><span class="sxs-lookup"><span data-stu-id="7892a-229">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="7892a-230">プロキシされたヘッダーを現在の要求に転送します。</span><span class="sxs-lookup"><span data-stu-id="7892a-230">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="7892a-231">更新されたフィールドを使用するコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-231">Before components that consume the updated fields.</span></span> <span data-ttu-id="7892a-232">例: スキーム、ホスト、クライアント IP、メソッド。</span><span class="sxs-lookup"><span data-stu-id="7892a-232">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="7892a-233">正常性チェック</span><span class="sxs-lookup"><span data-stu-id="7892a-233">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="7892a-234">ASP.NET Core アプリとその依存関係の正常性を、データベースの可用性などで確認します。</span><span class="sxs-lookup"><span data-stu-id="7892a-234">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="7892a-235">要求が正常性チェックのエンドポイントと一致した場合の終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-235">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="7892a-236">HTTP メソッドのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="7892a-236">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="7892a-237">メソッドをオーバーライドする受信 POST 要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="7892a-237">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="7892a-238">更新されたメソッドを使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-238">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="7892a-239">HTTPS リダイレクト</span><span class="sxs-lookup"><span data-stu-id="7892a-239">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="7892a-240">すべての HTTP 要求を HTTPS (ASP.NET Core 2.1 以降) にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="7892a-240">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="7892a-241">URL を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-241">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="7892a-242">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="7892a-242">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="7892a-243">特殊な応答ヘッダーを追加するセキュリティ拡張機能のミドルウェア (ASP.NET Core 2.1 以降)。</span><span class="sxs-lookup"><span data-stu-id="7892a-243">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="7892a-244">応答が送信される前と要求を変更するコンポーネントの後。</span><span class="sxs-lookup"><span data-stu-id="7892a-244">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="7892a-245">例: 転送されるヘッダー、URL リライト。</span><span class="sxs-lookup"><span data-stu-id="7892a-245">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="7892a-246">MVC</span><span class="sxs-lookup"><span data-stu-id="7892a-246">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="7892a-247">MVC/Razor Pages (ASP.NET Core 2.0 以降) で要求を処理します。</span><span class="sxs-lookup"><span data-stu-id="7892a-247">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="7892a-248">要求がルートと一致した場合の終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-248">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="7892a-249">OWIN</span><span class="sxs-lookup"><span data-stu-id="7892a-249">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="7892a-250">OWIN ベースのアプリ、サーバー、およびミドルウェアと相互運用します。</span><span class="sxs-lookup"><span data-stu-id="7892a-250">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="7892a-251">OWIN ミドルウェアが要求を完全に処理した場合の終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-251">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="7892a-252">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="7892a-252">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="7892a-253">応答のキャッシュのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-253">Provides support for caching responses.</span></span> | <span data-ttu-id="7892a-254">キャッシュが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-254">Before components that require caching.</span></span> |
| [<span data-ttu-id="7892a-255">応答圧縮</span><span class="sxs-lookup"><span data-stu-id="7892a-255">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="7892a-256">応答の圧縮のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-256">Provides support for compressing responses.</span></span> | <span data-ttu-id="7892a-257">圧縮が必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-257">Before components that require compression.</span></span> |
| [<span data-ttu-id="7892a-258">要求のローカライズ</span><span class="sxs-lookup"><span data-stu-id="7892a-258">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="7892a-259">ローカライズのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-259">Provides localization support.</span></span> | <span data-ttu-id="7892a-260">ローカリゼーションが重要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-260">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="7892a-261">ルーティング</span><span class="sxs-lookup"><span data-stu-id="7892a-261">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="7892a-262">要求のルートを定義および制約します。</span><span class="sxs-lookup"><span data-stu-id="7892a-262">Defines and constrains request routes.</span></span> | <span data-ttu-id="7892a-263">一致するルートの終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-263">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="7892a-264">セッション</span><span class="sxs-lookup"><span data-stu-id="7892a-264">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="7892a-265">ユーザー セッションの管理のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-265">Provides support for managing user sessions.</span></span> | <span data-ttu-id="7892a-266">セッションが必要なコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-266">Before components that require Session.</span></span> |
| [<span data-ttu-id="7892a-267">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="7892a-267">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="7892a-268">静的ファイルとディレクトリ参照に対応するサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-268">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="7892a-269">要求がファイルと一致した場合の終端。</span><span class="sxs-lookup"><span data-stu-id="7892a-269">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="7892a-270">URL リライト</span><span class="sxs-lookup"><span data-stu-id="7892a-270">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="7892a-271">URL の書き換えと要求のリダイレクトのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="7892a-271">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="7892a-272">URL を使うコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-272">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="7892a-273">WebSocket</span><span class="sxs-lookup"><span data-stu-id="7892a-273">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="7892a-274">WebSocket プロトコルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="7892a-274">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="7892a-275">WebSocket 要求を受け入れる必要があるコンポーネントの前。</span><span class="sxs-lookup"><span data-stu-id="7892a-275">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="7892a-276">ミドルウェアの作成</span><span class="sxs-lookup"><span data-stu-id="7892a-276">Write middleware</span></span>

<span data-ttu-id="7892a-277">ミドルウェアは一般に、クラスにカプセル化され、拡張メソッドを使用して公開されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-277">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="7892a-278">クエリ文字列から現在の要求のカルチャを設定する次のようなミドルウェアを考慮します。</span><span class="sxs-lookup"><span data-stu-id="7892a-278">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="7892a-279">上のサンプル コードを使って、ミドルウェア コンポーネントの作成方法を示します。</span><span class="sxs-lookup"><span data-stu-id="7892a-279">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="7892a-280">ASP.NET Core に組み込まれているローカライズのサポートについては、「<xref:fundamentals/localization>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7892a-280">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="7892a-281">カルチャを渡すことによって、ミドルウェアをテストできます (例: `http://localhost:7997/?culture=no`)。</span><span class="sxs-lookup"><span data-stu-id="7892a-281">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="7892a-282">次のコードは、ミドルウェアのデリゲートをクラスに移動します。</span><span class="sxs-lookup"><span data-stu-id="7892a-282">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="7892a-283">ミドルウェア `Task` メソッドの名前は `Invoke` である必要があります。</span><span class="sxs-lookup"><span data-stu-id="7892a-283">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="7892a-284">ASP.NET Core 2.0 以降では、名前は `Invoke` でも `InvokeAsync` でも構いません。</span><span class="sxs-lookup"><span data-stu-id="7892a-284">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="7892a-285">次の拡張メソッドは、<xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> を介してミドルウェアを公開します。</span><span class="sxs-lookup"><span data-stu-id="7892a-285">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="7892a-286">次のコードは、`Startup.Configure` からミドルウェアを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7892a-286">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7892a-287">ミドルウェアは、コンストラクターで依存関係を公開することによって、[明示的な依存関係の原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="7892a-287">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="7892a-288">ミドルウェアは、"*アプリケーションの有効期間*" ごとに 1 回構築されます。</span><span class="sxs-lookup"><span data-stu-id="7892a-288">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="7892a-289">要求内でミドルウェアとサービスを共有する必要がある場合は、「[要求ごとの依存関係](#per-request-dependencies)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7892a-289">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="7892a-290">ミドルウェア コンポーネントは、コンストラクター パラメーターにより、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) から依存関係を解決できます。</span><span class="sxs-lookup"><span data-stu-id="7892a-290">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="7892a-291">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) は、追加パラメーターを直接受け入れることもできます。</span><span class="sxs-lookup"><span data-stu-id="7892a-291">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="7892a-292">要求ごとの依存関係</span><span class="sxs-lookup"><span data-stu-id="7892a-292">Per-request dependencies</span></span>

<span data-ttu-id="7892a-293">ミドルウェアは要求ごとではなくアプリの起動時に構築されるため、ミドルウェアのコンストラクターによって使われる "*スコープ*" 有効期間のサービスは、各要求の間に、依存関係によって挿入される他の種類と共有されません。</span><span class="sxs-lookup"><span data-stu-id="7892a-293">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="7892a-294">ミドルウェアとその他の種類の間で "*スコープ*" サービスを共有する必要がある場合は、これらのサービスを `Invoke` メソッドのシグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="7892a-294">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="7892a-295">`Invoke` メソッドは、DI によって設定される追加のパラメーターを受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="7892a-295">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="7892a-296">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="7892a-296">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
