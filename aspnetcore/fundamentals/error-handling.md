---
title: ASP.NET Core のエラーを処理する
author: tdykstra
description: ASP.NET Core アプリでエラーを処理する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665364"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="13be8-103">ASP.NET Core のエラーを処理する</span><span class="sxs-lookup"><span data-stu-id="13be8-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="13be8-104">作成者: [Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex)、[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="13be8-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="13be8-105">この記事では、ASP.NET Core アプリでエラーを処理するための一般的な手法について取り上げます。</span><span class="sxs-lookup"><span data-stu-id="13be8-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="13be8-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="13be8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="13be8-107">開発者例外ページ</span><span class="sxs-lookup"><span data-stu-id="13be8-107">Developer Exception Page</span></span>

<span data-ttu-id="13be8-108">アプリを構成して要求の例外に関する詳細情報を示すページを表示させるには、"*開発者例外ページ*" を使用します。</span><span class="sxs-lookup"><span data-stu-id="13be8-108">To configure an app to display a page that shows detailed information about request exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="13be8-109">このページは [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) 内で利用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="13be8-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="13be8-110">アプリを開発[環境](xref:fundamentals/environments)で実行しているときに、`Startup.Configure` メソッドに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="13be8-110">Add a line to the `Startup.Configure` method when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

<span data-ttu-id="13be8-111">例外をキャッチしたい任意のミドルウェアの前に <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> への呼び出しを配置します。</span><span class="sxs-lookup"><span data-stu-id="13be8-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="13be8-112">**アプリを開発環境で実行するときにのみ**、開発者例外ページを有効にします。</span><span class="sxs-lookup"><span data-stu-id="13be8-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="13be8-113">アプリを実稼働環境で実行するときは、詳細な例外情報を公開しません。</span><span class="sxs-lookup"><span data-stu-id="13be8-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="13be8-114">環境の構成について詳しくは、「<xref:fundamentals/environments>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="13be8-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="13be8-115">開発者例外ページを表示するには、環境を `Development` に設定してサンプル アプリを実行し、アプリのベース URL に `?throw=true` を追加します。</span><span class="sxs-lookup"><span data-stu-id="13be8-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="13be8-116">このページには、例外と要求に関する次の情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="13be8-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="13be8-117">スタック トレース</span><span class="sxs-lookup"><span data-stu-id="13be8-117">Stack trace</span></span>
* <span data-ttu-id="13be8-118">クエリ文字列のパラメーター (ある場合)</span><span class="sxs-lookup"><span data-stu-id="13be8-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="13be8-119">Cookie (ある場合)</span><span class="sxs-lookup"><span data-stu-id="13be8-119">Cookies (if any)</span></span>
* <span data-ttu-id="13be8-120">ヘッダー</span><span class="sxs-lookup"><span data-stu-id="13be8-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="13be8-121">カスタム例外処理ページを構成する</span><span class="sxs-lookup"><span data-stu-id="13be8-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="13be8-122">アプリを開発環境で実行していない場合は、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 拡張メソッドを呼び出して例外処理ミドルウェアを追加します。</span><span class="sxs-lookup"><span data-stu-id="13be8-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="13be8-123">ミドルウェアでは次を行います。</span><span class="sxs-lookup"><span data-stu-id="13be8-123">The middleware:</span></span>

* <span data-ttu-id="13be8-124">例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="13be8-124">Catches exceptions.</span></span>
* <span data-ttu-id="13be8-125">例外をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="13be8-125">Logs exceptions.</span></span>
* <span data-ttu-id="13be8-126">ページ用の、またはコントローラーが指定した別のパイプラインで、要求を再実行します。</span><span class="sxs-lookup"><span data-stu-id="13be8-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="13be8-127">応答が始まっていた場合、要求は再実行されません。</span><span class="sxs-lookup"><span data-stu-id="13be8-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="13be8-128">サンプル アプリの次の例では、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> により非開発環境に例外処理ミドルウェアを追加しています。</span><span class="sxs-lookup"><span data-stu-id="13be8-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="13be8-129">この拡張メソッドでは、例外がキャッチされてログに記録された後、再実行された要求用に `/Error` エンドポイントにあるエラー ページまたはコントローラーを指定します。</span><span class="sxs-lookup"><span data-stu-id="13be8-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

<span data-ttu-id="13be8-130">Razor Pages アプリのテンプレートには、エラー ページ (*.cshtml*) と <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> クラス (`ErrorModel`) が Pages フォルダー内に用意されています。</span><span class="sxs-lookup"><span data-stu-id="13be8-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="13be8-131">MVC アプリでは、次のエラー処理メソッドが MVC アプリ テンプレートに含まれ、ホーム コントローラーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="13be8-132">`HttpGet` などの HTTP メソッド属性を使ってエラー ハンドラー アクション メソッドを修飾しないでください。</span><span class="sxs-lookup"><span data-stu-id="13be8-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="13be8-133">明示的な動詞を使用すると、要求がメソッドに届かないことがあります。</span><span class="sxs-lookup"><span data-stu-id="13be8-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="13be8-134">認証されていないユーザーがエラー ビューを受信できるように、メソッドへの匿名アクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="13be8-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="access-the-exception"></a><span data-ttu-id="13be8-135">例外にアクセスする</span><span class="sxs-lookup"><span data-stu-id="13be8-135">Access the exception</span></span>

<span data-ttu-id="13be8-136"><xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> を使って、例外や、コントローラーまたはページ内の元の要求パスにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="13be8-136">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception or the original request path in a controller or page:</span></span>

* <span data-ttu-id="13be8-137">このパスは <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> プロパティから取得できます。</span><span class="sxs-lookup"><span data-stu-id="13be8-137">The path is available from the <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> property.</span></span>
* <span data-ttu-id="13be8-138">継承した [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) プロパティから <xref:System.Exception?displayProperty=fullName> を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="13be8-138">Read the <xref:System.Exception?displayProperty=fullName> from the inherited [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) property.</span></span>

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> <span data-ttu-id="13be8-139"><xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> または <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> からの機密性の高いエラー情報をクライアントに提供**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="13be8-139">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="13be8-140">エラーの提供はセキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="13be8-140">Serving errors is a security risk.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="13be8-141">カスタム例外処理コードを構成する</span><span class="sxs-lookup"><span data-stu-id="13be8-141">Configure custom exception handling code</span></span>

<span data-ttu-id="13be8-142">[カスタム例外処理ページ](#configure-a-custom-exception-handling-page)を使ってエラー用にエンドポイントを提供する方法の代替手段は、<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> にラムダを指定することです。</span><span class="sxs-lookup"><span data-stu-id="13be8-142">An alternative to serving an endpoint for errors with a [custom exception handling page](#configure-a-custom-exception-handling-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="13be8-143"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> と共にラムダを使うと、応答を返す前にエラーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="13be8-143">Using a lambda with <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> allows access to the error before returning the response.</span></span>

<span data-ttu-id="13be8-144">サンプル アプリは、`Startup.Configure` のカスタム例外処理コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="13be8-144">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="13be8-145">Index ページの **[例外のスロー]** リンクを使って例外をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="13be8-145">Trigger an exception with the **Throw Exception** link on the Index page.</span></span> <span data-ttu-id="13be8-146">次のラムダが実行されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-146">The following lambda runs:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> <span data-ttu-id="13be8-147"><xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> または <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> からの機密性の高いエラー情報をクライアントに提供**しないでください**。</span><span class="sxs-lookup"><span data-stu-id="13be8-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="13be8-148">エラーの提供はセキュリティ上のリスクです。</span><span class="sxs-lookup"><span data-stu-id="13be8-148">Serving errors is a security risk.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="13be8-149">状態コード ページを構成する</span><span class="sxs-lookup"><span data-stu-id="13be8-149">Configure status code pages</span></span>

<span data-ttu-id="13be8-150">ASP.NET Core アプリでは、既定で、"*404 - 見つかりません*" などの HTTP 状態コードの状態コード ページが表示されません。</span><span class="sxs-lookup"><span data-stu-id="13be8-150">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="13be8-151">アプリでは、状態コードと空の応答本文が返されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-151">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="13be8-152">状態コード ページを提供するには、状態コード ページ ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="13be8-152">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="13be8-153">このミドルウェアは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)内で使用できる、[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) パッケージによって使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="13be8-153">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="13be8-154">`Startup.Configure` メソッドに次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="13be8-154">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="13be8-155">要求処理ミドルウェア (たとえば、静的ファイル ミドルウェアや MVC ミドルウェア) の前に <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="13be8-155">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="13be8-156">既定では、状態コード ページ ミドルウェアにより、一般的な状態コード ("*404 - 見つかりません*" など) に対してテキストのみのハンドラーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-156">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="13be8-157">ミドルウェアではいくつかの拡張メソッドがサポートされていて、それを使ってその動作をカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="13be8-157">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="13be8-158"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはラムダ式を受け取ります。これを使って、カスタム エラー処理ロジックを処理し、手動で応答を書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="13be8-158">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="13be8-159"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> のオーバーロードはコンテンツの種類と書式文字列を受け取ります。これを使って、コンテンツの種類と応答テキストをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="13be8-159">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="13be8-160">拡張メソッドをリダイレクトして再実行する</span><span class="sxs-lookup"><span data-stu-id="13be8-160">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="13be8-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="13be8-161"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="13be8-162">クライアントに *302 - Found* 状態コードを送信します。</span><span class="sxs-lookup"><span data-stu-id="13be8-162">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="13be8-163">URL テンプレートで指定された場所にクライアントをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="13be8-163">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="13be8-164">アプリが次の条件を満たす場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> が一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="13be8-165">クライアントを別のエンドポイントにリダイレクトする必要がある場合 (通常は、別のアプリがエラーを処理する場合)。</span><span class="sxs-lookup"><span data-stu-id="13be8-165">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="13be8-166">Web アプリの場合は、クライアントのブラウザーのアドレス バーにリダイレクトされたエンドポイントが反映されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-166">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="13be8-167">元の状態コードを保持し、初回のリダイレクト応答で返してはいけない場合。</span><span class="sxs-lookup"><span data-stu-id="13be8-167">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="13be8-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="13be8-168"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="13be8-169">元の状態コードをクライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="13be8-169">Returns the original status code to the client.</span></span>
* <span data-ttu-id="13be8-170">代替パスを使用して要求パイプラインを再実行することで、応答本文を生成します。</span><span class="sxs-lookup"><span data-stu-id="13be8-170">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="13be8-171">アプリで次を行う必要がある場合、<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> が一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-171"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="13be8-172">別のエンドポイントにリダイレクトすることなく要求を処理する。</span><span class="sxs-lookup"><span data-stu-id="13be8-172">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="13be8-173">Web アプリの場合は、クライアントのブラウザーのアドレス バーに、初めに要求されていたエンドポイントが反映されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-173">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="13be8-174">元の状態コードを保持し、応答で返す。</span><span class="sxs-lookup"><span data-stu-id="13be8-174">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="13be8-175">テンプレートには、状態コード用のプレースホルダー (`{0}`) が含まれる場合があります。</span><span class="sxs-lookup"><span data-stu-id="13be8-175">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="13be8-176">テンプレートには、最初にスラッシュ (`/`) を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="13be8-176">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="13be8-177">プレースホルダーを使用する場合は、エンドポイント (ページまたはコントローラー) でパスのセグメントを処理できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="13be8-177">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="13be8-178">たとえば、エラー用の Razor ページでは、`@page` ディレクティブの付いた省略可能なパスのセグメント値を受け入れる必要があります。</span><span class="sxs-lookup"><span data-stu-id="13be8-178">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="13be8-179">状態コード ページは、Razor ページ ハンドラー メソッドまたは MVC コントローラー内の特定の要求に対して無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="13be8-179">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="13be8-180">状態コード ページを無効にするには、要求の [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) コレクションから <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> を取得してみて、それが使用可能だった場合は機能を無効にします。</span><span class="sxs-lookup"><span data-stu-id="13be8-180">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="13be8-181">アプリ内のエンドポイントを指す <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> オーバーロードを使用するには、そのエンドポイントの MVC ビューまたは Razor ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="13be8-181">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="13be8-182">たとえば、Razor Pages アプリのテンプレートでは、次のページとページ モデル クラスが生成されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-182">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="13be8-183">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="13be8-183">*Error.cshtml*:</span></span>

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

<span data-ttu-id="13be8-184">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="13be8-184">*Error.cshtml.cs*:</span></span>

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

<span data-ttu-id="13be8-185">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="13be8-185">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="13be8-186">例外処理コード</span><span class="sxs-lookup"><span data-stu-id="13be8-186">Exception-handling code</span></span>

<span data-ttu-id="13be8-187">例外処理ページのコードは例外をスローすることがあります。</span><span class="sxs-lookup"><span data-stu-id="13be8-187">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="13be8-188">実稼働のエラー ページは純粋に静的なコンテンツで構成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="13be8-188">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="13be8-189">また、応答のヘッダーが送信されたら、次のことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="13be8-189">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="13be8-190">アプリで応答の状態コードを変更できません。</span><span class="sxs-lookup"><span data-stu-id="13be8-190">The app can't change the response's status code.</span></span>
* <span data-ttu-id="13be8-191">すべての例外ページやハンドラーを実行できません。</span><span class="sxs-lookup"><span data-stu-id="13be8-191">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="13be8-192">応答は完了している必要があります。あるいは、接続が中止となっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="13be8-192">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="13be8-193">サーバー例外処理</span><span class="sxs-lookup"><span data-stu-id="13be8-193">Server exception handling</span></span>

<span data-ttu-id="13be8-194">アプリ内の例外処理ロジックに加えて、[サーバー実装](xref:fundamentals/servers/index)でも一部の例外を処理できます。</span><span class="sxs-lookup"><span data-stu-id="13be8-194">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="13be8-195">応答ヘッダーの送信前にサーバーで例外がキャッチされると、サーバーによって "*500 - 内部サーバー エラーです*" 応答が応答本文なしで送信されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-195">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="13be8-196">応答ヘッダーの送信後にサーバーで例外がキャッチされた場合、サーバーは接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="13be8-196">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="13be8-197">アプリで処理されない要求はサーバーで処理されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-197">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="13be8-198">サーバーが要求を処理しているときに発生した例外は、すべてサーバーの例外処理によって処理されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-198">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="13be8-199">この動作は、アプリのカスタム エラー ページ、例外処理ミドルウェア、およびフィルターから影響を受けません。</span><span class="sxs-lookup"><span data-stu-id="13be8-199">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="13be8-200">起動時の例外処理</span><span class="sxs-lookup"><span data-stu-id="13be8-200">Startup exception handling</span></span>

<span data-ttu-id="13be8-201">アプリの起動中に起こる例外はホスティング層だけが処理できます。</span><span class="sxs-lookup"><span data-stu-id="13be8-201">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="13be8-202">[Web ホスト](xref:fundamentals/host/web-host)を使うと、`captureStartupErrors` と `detailedErrors` キーを使って、[起動中のエラーに対するホストの動作方法を構成する](xref:fundamentals/host/web-host#detailed-errors)ことができます。</span><span class="sxs-lookup"><span data-stu-id="13be8-202">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="13be8-203">ホスティングでは、ホスト アドレス/ポート バインディング後にエラーが発生した場合のみ、キャプチャされた起動時エラーに対してエラー ページを表示できます。</span><span class="sxs-lookup"><span data-stu-id="13be8-203">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="13be8-204">なんらかの理由でいずれかのバインドが失敗した場合:</span><span class="sxs-lookup"><span data-stu-id="13be8-204">If any binding fails for any reason:</span></span>

* <span data-ttu-id="13be8-205">ホスティング レイヤーにより重大な例外がログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-205">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="13be8-206">dotnet プロセスがクラッシュします。</span><span class="sxs-lookup"><span data-stu-id="13be8-206">The dotnet process crashes.</span></span>
* <span data-ttu-id="13be8-207">アプリを [Kestrel](xref:fundamentals/servers/kestrel) サーバー上で実行している場合、エラー ページが表示されません。</span><span class="sxs-lookup"><span data-stu-id="13be8-207">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="13be8-208">[IIS](/iis) または [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上で実行している場合、プロセスを開始できなければ、[ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)から "*502.5 - 処理エラー*" が返されます。</span><span class="sxs-lookup"><span data-stu-id="13be8-208">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="13be8-209">詳細については、「<xref:host-and-deploy/iis/troubleshoot>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="13be8-209">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="13be8-210">Azure App Service での起動の問題のトラブルシューティングについては、<xref:host-and-deploy/azure-apps/troubleshoot> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="13be8-210">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="13be8-211">ASP.NET Core MVC エラー処理</span><span class="sxs-lookup"><span data-stu-id="13be8-211">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="13be8-212">[MVC](xref:mvc/overview) アプリには、エラー処理の追加オプションがいくつかあります。例外フィルターの構成やモデル検証の実行などです。</span><span class="sxs-lookup"><span data-stu-id="13be8-212">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="13be8-213">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="13be8-213">Exception filters</span></span>

<span data-ttu-id="13be8-214">例外フィルターはグローバルに構成するか、MVC アプリのコントローラーまたはアクション単位で構成できます。</span><span class="sxs-lookup"><span data-stu-id="13be8-214">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="13be8-215">このようなフィルターはコントローラー アクションや別のフィルターの実行中に発生する未処理の例外を処理します。</span><span class="sxs-lookup"><span data-stu-id="13be8-215">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="13be8-216">これらのフィルターは、それ以外の場合には呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="13be8-216">These filters aren't called otherwise.</span></span> <span data-ttu-id="13be8-217">詳細については、「<xref:mvc/controllers/filters#exception-filters>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="13be8-217">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="13be8-218">例外フィルターは、MVC アクション内で発生した例外をトラップする場合は便利ですが、例外処理ミドルウェアほど柔軟ではありません。</span><span class="sxs-lookup"><span data-stu-id="13be8-218">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="13be8-219">ミドルウェアの使用をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="13be8-219">We recommend using the middleware.</span></span> <span data-ttu-id="13be8-220">選択された MVC アクションに応じて "*異なる方法で*" エラー処理を実行する必要がある場合にのみ、フィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="13be8-220">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="13be8-221">モデルの状態エラーを処理する</span><span class="sxs-lookup"><span data-stu-id="13be8-221">Handle model state errors</span></span>

<span data-ttu-id="13be8-222">[モデル検証](xref:mvc/models/validation)は各コントローラー アクションを呼び出す前に発生します。[ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) を検査して適切に対処するのは、アクション メソッドの仕事です。</span><span class="sxs-lookup"><span data-stu-id="13be8-222">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="13be8-223">一部のアプリでは、[モデル検証](xref:mvc/models/validation)エラーを処理するための標準的な規則に従うことを選択します。その場合、そのようなポリシーを実装する場所として[フィルター](xref:mvc/controllers/filters)が適していることがあります。</span><span class="sxs-lookup"><span data-stu-id="13be8-223">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="13be8-224">無効なモデル状態でアクションの動作をテストしてください。</span><span class="sxs-lookup"><span data-stu-id="13be8-224">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="13be8-225">詳細については、「<xref:mvc/controllers/testing>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="13be8-225">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="13be8-226">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="13be8-226">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
