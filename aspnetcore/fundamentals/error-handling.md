---
title: ASP.NET Core のエラーを処理する
author: tdykstra
description: ASP.NET Core アプリでエラーを処理する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345496"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="ed94b-103">ASP.NET Core のエラーを処理する</span><span class="sxs-lookup"><span data-stu-id="ed94b-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="ed94b-104">作成者: [Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex)、[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ed94b-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ed94b-105">この記事では、ASP.NET Core アプリでエラーを処理するための一般的な手法について取り上げます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="ed94b-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ed94b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="ed94b-107">開発者例外ページ</span><span class="sxs-lookup"><span data-stu-id="ed94b-107">Developer Exception Page</span></span>

<span data-ttu-id="ed94b-108">例外に関する詳細情報を表示するページを表示するアプリを構成するには、*開発者例外ページ*を使用します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="ed94b-109">このページは [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) 内で利用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="ed94b-110">`Startup.Configure` メソッドに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-110">Add a line to the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

<span data-ttu-id="ed94b-111">例外をキャッチしたい任意のミドルウェアの前に <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> への呼び出しを配置します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="ed94b-112">**アプリを開発環境で実行するときにのみ**、開発者例外ページを有効にします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="ed94b-113">アプリを実稼働環境で実行するときは、詳細な例外情報を公開しません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="ed94b-114">環境の構成について詳しくは、「<xref:fundamentals/environments>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="ed94b-115">開発者例外ページを表示するには、環境を `Development` に設定してサンプル アプリを実行し、アプリのベース URL に `?throw=true` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="ed94b-116">このページには、例外と要求に関する次の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed94b-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="ed94b-117">スタック トレース</span><span class="sxs-lookup"><span data-stu-id="ed94b-117">Stack trace</span></span>
* <span data-ttu-id="ed94b-118">クエリ文字列のパラメーター (ある場合)</span><span class="sxs-lookup"><span data-stu-id="ed94b-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="ed94b-119">Cookie (ある場合)</span><span class="sxs-lookup"><span data-stu-id="ed94b-119">Cookies (if any)</span></span>
* <span data-ttu-id="ed94b-120">ヘッダー</span><span class="sxs-lookup"><span data-stu-id="ed94b-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="ed94b-121">カスタム例外処理ページを構成する</span><span class="sxs-lookup"><span data-stu-id="ed94b-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="ed94b-122">アプリを開発環境で実行していない場合は、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 拡張メソッドを呼び出して例外処理ミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="ed94b-123">ミドルウェアでは次を行います。</span><span class="sxs-lookup"><span data-stu-id="ed94b-123">The middleware:</span></span>

* <span data-ttu-id="ed94b-124">例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-124">Catches exceptions.</span></span>
* <span data-ttu-id="ed94b-125">例外をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-125">Logs exceptions.</span></span>
* <span data-ttu-id="ed94b-126">ページ用の、またはコントローラーが指定した別のパイプラインで、要求を再実行します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="ed94b-127">応答が始まっていた場合、要求は再実行されません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="ed94b-128">サンプル アプリの次の例では、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> により非開発環境に例外処理ミドルウェアを追加しています。</span><span class="sxs-lookup"><span data-stu-id="ed94b-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="ed94b-129">この拡張メソッドでは、例外がキャッチされてログに記録された後、再実行された要求用に `/Error` エンドポイントにあるエラー ページまたはコントローラーを指定します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

<span data-ttu-id="ed94b-130">Razor Pages アプリのテンプレートには、エラー ページ (*.cshtml*) と <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> クラス (`ErrorModel`) が Pages フォルダー内に用意されています。</span><span class="sxs-lookup"><span data-stu-id="ed94b-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="ed94b-131">MVC アプリでは、次のエラー処理メソッドが MVC アプリ テンプレートに含まれ、ホーム コントローラーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="ed94b-132">`HttpGet` などの HTTP メソッド属性を使ってエラー ハンドラー アクション メソッドを修飾しないでください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="ed94b-133">明示的な動詞を使用すると、要求がメソッドに届かないことがあります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="ed94b-134">認証されていないユーザーがエラー ビューを受信できるように、メソッドへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="ed94b-135">状態コード ページを構成する</span><span class="sxs-lookup"><span data-stu-id="ed94b-135">Configure status code pages</span></span>

<span data-ttu-id="ed94b-136">ASP.NET Core アプリでは、既定で、"*404 - 見つかりません*" などの HTTP 状態コードの状態コード ページが表示されません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-136">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="ed94b-137">アプリでは、状態コードと空の応答本文が返されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-137">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="ed94b-138">状態コード ページを提供するには、状態コード ページ ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="ed94b-139">このミドルウェアは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内で使用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ed94b-140">`Startup.Configure` メソッドに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-140">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="ed94b-141">要求処理ミドルウェア (たとえば、静的ファイル ミドルウェアや MVC ミドルウェア) の前に <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-141">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="ed94b-142">既定では、状態コード ページ ミドルウェアにより、一般的な状態コード ("*404 - 見つかりません*" など) に対してテキストのみのハンドラーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-142">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="ed94b-143">ミドルウェアではいくつかの拡張メソッドがサポートされていて、それを使ってその動作をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-143">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="ed94b-144"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはラムダ式を受け取ります。これを使って、カスタム エラー処理ロジックを処理し、手動で応答を書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-144">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="ed94b-145"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはコンテンツの種類と書式文字列を受け取ります。これを使って、コンテンツの種類と応答テキストをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-145">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="ed94b-146">拡張メソッドをリダイレクトして再実行する</span><span class="sxs-lookup"><span data-stu-id="ed94b-146">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="ed94b-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="ed94b-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="ed94b-148">クライアントに *302 - Found* 状態コードを送信します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-148">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="ed94b-149">URL テンプレートで指定された場所にクライアントをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-149">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="ed94b-150">アプリが次の条件を満たす場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> が一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="ed94b-151">クライアントを別のエンドポイントにリダイレクトする必要がある場合 (通常は、別のアプリがエラーを処理する場合)。</span><span class="sxs-lookup"><span data-stu-id="ed94b-151">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="ed94b-152">Web アプリの場合は、クライアントのブラウザーのアドレス バーにリダイレクトされたエンドポイントが反映されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-152">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="ed94b-153">元の状態コードを保持し、初回のリダイレクト応答で返してはいけない場合。</span><span class="sxs-lookup"><span data-stu-id="ed94b-153">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="ed94b-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="ed94b-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="ed94b-155">元の状態コードをクライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-155">Returns the original status code to the client.</span></span>
* <span data-ttu-id="ed94b-156">代替パスを使用して要求パイプラインを再実行することで、応答本文を生成します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-156">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="ed94b-157">アプリで次を行う必要がある場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> が一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="ed94b-158">別のエンドポイントにリダイレクトすることなく要求を処理する。</span><span class="sxs-lookup"><span data-stu-id="ed94b-158">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="ed94b-159">Web アプリの場合は、クライアントのブラウザーのアドレス バーに、初めに要求されていたエンドポイントが反映されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-159">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="ed94b-160">元の状態コードを保持し、応答で返す。</span><span class="sxs-lookup"><span data-stu-id="ed94b-160">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="ed94b-161">テンプレートには、状態コード用のプレースホルダー (`{0}`) が含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-161">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="ed94b-162">テンプレートには、最初にスラッシュ (`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-162">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="ed94b-163">プレースホルダーを使用する場合は、エンドポイント (ページまたはコントローラー) でパスのセグメントを処理できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-163">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="ed94b-164">たとえば、エラー用の Razor ページでは、`@page` ディレクティブの付いた省略可能なパスのセグメント値を受け入れる必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-164">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="ed94b-165">状態コード ページは、Razor ページ ハンドラー メソッドまたは MVC コントローラー内の特定の要求に対して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-165">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="ed94b-166">状態コード ページを無効にするには、要求の [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) コレクションから <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> を取得してみて、それが使用可能だった場合は機能を無効にします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-166">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="ed94b-167">アプリ内のエンドポイントを指す <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> オーバーロードを使用するには、そのエンドポイントの MVC ビューまたは Razor ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-167">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="ed94b-168">たとえば、Razor Pages アプリのテンプレートでは、次のページとページ モデル クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-168">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="ed94b-169">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ed94b-169">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="ed94b-170">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed94b-170">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="ed94b-171">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed94b-171">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="ed94b-172">例外処理コード</span><span class="sxs-lookup"><span data-stu-id="ed94b-172">Exception-handling code</span></span>

<span data-ttu-id="ed94b-173">例外処理ページのコードは例外をスローすることがあります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-173">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="ed94b-174">実稼働のエラー ページは純粋に静的なコンテンツで構成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-174">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="ed94b-175">また、応答のヘッダーが送信されたら、次のことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-175">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="ed94b-176">アプリで応答の状態コードを変更できません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-176">The app can't change the response's status code.</span></span>
* <span data-ttu-id="ed94b-177">すべての例外ページやハンドラーを実行できません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-177">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="ed94b-178">応答は完了している必要があります。あるいは、接続が中止となっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-178">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="ed94b-179">サーバー例外処理</span><span class="sxs-lookup"><span data-stu-id="ed94b-179">Server exception handling</span></span>

<span data-ttu-id="ed94b-180">アプリ内の例外処理ロジックに加えて、[サーバー実装](xref:fundamentals/servers/index)でも一部の例外を処理できます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-180">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="ed94b-181">応答ヘッダーの送信前にサーバーで例外がキャッチされると、サーバーによって "*500 - 内部サーバー エラーです*" 応答が応答本文なしで送信されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-181">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="ed94b-182">応答ヘッダーの送信後にサーバーで例外がキャッチされた場合、サーバーは接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-182">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="ed94b-183">アプリで処理されない要求はサーバーで処理されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-183">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="ed94b-184">例外が発生すると、サーバーの例外処理で処理されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-184">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="ed94b-185">カスタムのエラー ページ、例外処理ミドルウェア、フィルターを構成しても、この動作は変わりません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-185">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="ed94b-186">起動時の例外処理</span><span class="sxs-lookup"><span data-stu-id="ed94b-186">Startup exception handling</span></span>

<span data-ttu-id="ed94b-187">アプリの起動中に起こる例外はホスティング層だけが処理できます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-187">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="ed94b-188">[Web ホスト](xref:fundamentals/host/web-host)を使うと、`captureStartupErrors` と `detailedErrors` キーを使って、[起動中のエラーに対するホストの動作方法を構成する](xref:fundamentals/host/web-host#detailed-errors)ことができます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-188">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="ed94b-189">ホスティングでは、ホスト アドレス/ポート バインディング後にエラーが発生した場合のみ、キャプチャされた起動時エラーに対してエラー ページを表示できます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-189">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="ed94b-190">なんらかの理由でいずれかのバインドが失敗した場合:</span><span class="sxs-lookup"><span data-stu-id="ed94b-190">If any binding fails for any reason:</span></span>

* <span data-ttu-id="ed94b-191">ホスティング レイヤーにより重大な例外がログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-191">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="ed94b-192">dotnet プロセスがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-192">The dotnet process crashes.</span></span>
* <span data-ttu-id="ed94b-193">アプリを [Kestrel](xref:fundamentals/servers/kestrel) サーバー上で実行している場合、エラー ページが表示されません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-193">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="ed94b-194">[IIS](/iis) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上で実行している場合、プロセスを開始できなければ、[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)から "*502.5 - 処理エラー*" が返されます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-194">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="ed94b-195">詳細については、「<xref:host-and-deploy/iis/troubleshoot>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-195">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="ed94b-196">Azure App Service での起動の問題のトラブルシューティングについては、<xref:host-and-deploy/azure-apps/troubleshoot> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-196">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="ed94b-197">ASP.NET Core MVC エラー処理</span><span class="sxs-lookup"><span data-stu-id="ed94b-197">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="ed94b-198">[MVC](xref:mvc/overview) アプリには、エラー処理の追加オプションがいくつかあります。例外フィルターの構成やモデル検証の実行などです。</span><span class="sxs-lookup"><span data-stu-id="ed94b-198">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="ed94b-199">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="ed94b-199">Exception filters</span></span>

<span data-ttu-id="ed94b-200">例外フィルターはグローバルに構成するか、MVC アプリのコントローラーまたはアクション単位で構成できます。</span><span class="sxs-lookup"><span data-stu-id="ed94b-200">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="ed94b-201">このようなフィルターはコントローラー アクションや別のフィルターの実行中に発生する未処理の例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-201">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="ed94b-202">これらのフィルターは、それ以外の場合には呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-202">These filters aren't called otherwise.</span></span> <span data-ttu-id="ed94b-203">詳細については、<xref:mvc/controllers/filters> をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-203">To learn more, see <xref:mvc/controllers/filters>.</span></span>

> [!TIP]
> <span data-ttu-id="ed94b-204">例外フィルターは、MVC アクション内で発生した例外をトラップする場合は便利ですが、エラー処理ミドルウェアほど柔軟ではありません。</span><span class="sxs-lookup"><span data-stu-id="ed94b-204">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="ed94b-205">ミドルウェアの使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ed94b-205">We recommend the use of middleware.</span></span> <span data-ttu-id="ed94b-206">選択された MVC アクションに応じて "*異なる方法で*" エラー処理を実行する必要がある場合にのみ、フィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed94b-206">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="ed94b-207">モデルの状態エラーを処理する</span><span class="sxs-lookup"><span data-stu-id="ed94b-207">Handle model state errors</span></span>

<span data-ttu-id="ed94b-208">[モデル検証](xref:mvc/models/validation)は各コントローラー アクションを呼び出す前に発生します。[ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) を検査して適切に対処するのは、アクション メソッドの仕事です。</span><span class="sxs-lookup"><span data-stu-id="ed94b-208">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="ed94b-209">一部のアプリでは、[モデル検証](xref:mvc/models/validation)エラーを処理するための標準的な規則に従うことを選択します。その場合、そのようなポリシーを実装する場所として[フィルター](xref:mvc/controllers/filters)が適していることがあります。</span><span class="sxs-lookup"><span data-stu-id="ed94b-209">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="ed94b-210">無効なモデル状態でアクションの動作をテストしてください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-210">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="ed94b-211">詳細については、「<xref:mvc/controllers/testing>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ed94b-211">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed94b-212">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ed94b-212">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
