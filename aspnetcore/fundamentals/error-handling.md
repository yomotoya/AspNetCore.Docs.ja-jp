---
title: "ASP.NET Core でのエラー処理"
author: ardalis
description: "ASP.NET Core アプリケーションのエラーを処理する方法について説明します"
keywords: "ASP.NET Core、エラーを処理する例外を処理します。"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5898892c63e978adfabf9939394fef4ea1848d49
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="5dab0-104">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="5dab0-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="5dab0-105">によって[Steve Smith](http://ardalis.com)と[Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="5dab0-105">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="5dab0-106">この記事では、ASP.NET Core アプリケーションでエラーを処理する一般的な appoaches について説明します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="5dab0-107">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="5dab0-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="5dab0-108">開発者の例外ページ</span><span class="sxs-lookup"><span data-stu-id="5dab0-108">The developer exception page</span></span>

<span data-ttu-id="5dab0-109">例外に関する詳細情報を表示するページを表示するアプリを構成するのには、インストール、 `Microsoft.AspNetCore.Diagnostics` NuGet パッケージ化し、行を追加して、[スタートアップ クラスでメソッドを構成する](startup.md):</span><span class="sxs-lookup"><span data-stu-id="5dab0-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

<span data-ttu-id="5dab0-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="5dab0-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span></span>

<span data-ttu-id="5dab0-111">Put`UseDeveloperExceptionPage`など、例外をキャッチするすべてのミドルウェアの前に`app.UseMvc`です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-111">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="5dab0-112">開発者の例外のページを有効にする**、アプリが開発環境で実行されている場合にのみ**です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-112">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="5dab0-113">実稼働環境でアプリを実行するときに、パブリックに詳細な例外情報を共有します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="5dab0-114">[環境の構成について](environments.md)です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-114">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="5dab0-115">開発者の例外のページを表示する設定した環境でサンプル アプリケーションを実行`Development`、し、追加`?throw=true`アプリの基本 URL にします。</span><span class="sxs-lookup"><span data-stu-id="5dab0-115">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="5dab0-116">このページには、例外と、要求に関する情報をいくつかのタブが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-116">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="5dab0-117">最初のタブには、スタック トレースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5dab0-117">The first tab includes a stack trace.</span></span> 

![スタック トレース](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="5dab0-119">次のタブは、存在する場合に、クエリ文字列パラメーターを示します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-119">The next tab shows the query string parameters, if any.</span></span>

![クエリ文字列パラメーター](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="5dab0-121">この要求は、クッキーがありませんでしたが、サポートしていた場合に表示される、 **Cookie**タブです。</span><span class="sxs-lookup"><span data-stu-id="5dab0-121">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab.</span></span> <span data-ttu-id="5dab0-122">最後のタブが渡されましたが、ヘッダーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-122">You can see the headers that were passed in the last tab.</span></span>

![ヘッダー](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="5dab0-124">ページの処理、カスタム例外を構成します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-124">Configuring a custom exception handling page</span></span>

<span data-ttu-id="5dab0-125">例外ハンドラーで、アプリが実行されていないときに使用するページを構成することをお勧め、`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="5dab0-125">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

<span data-ttu-id="5dab0-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="5dab0-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span></span>

<span data-ttu-id="5dab0-127">MVC アプリケーションでない明示的に装飾 HTTP メソッドの属性を持つエラー ハンドラーのアクション メソッドなど`HttpGet`です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-127">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="5dab0-128">明示的な動詞を使用して防ぐことがいくつかの要求メソッド。</span><span class="sxs-lookup"><span data-stu-id="5dab0-128">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="5dab0-129">ステータス コード ページを構成します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-129">Configuring status code pages</span></span>

<span data-ttu-id="5dab0-130">既定では、アプリが提供されません豊富なステータス コード ページの HTTP ステータス コード 500 (内部サーバー エラー) または 404 (Not Found) など。</span><span class="sxs-lookup"><span data-stu-id="5dab0-130">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="5dab0-131">構成することができます、`StatusCodePagesMiddleware`に行を追加して、`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="5dab0-131">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="5dab0-132">既定では、このミドルウェアには、404 などの一般的な状態コードに対して、単純なテキストのみのハンドラーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-132">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 ページ](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="5dab0-134">ミドルウェアは、いくつかの種類の拡張メソッドをサポートします。</span><span class="sxs-lookup"><span data-stu-id="5dab0-134">The middleware supports several different extension methods.</span></span> <span data-ttu-id="5dab0-135">ラムダ式を 1 つは、他方はコンテンツの種類と形式の文字列を使用します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-135">One takes a lambda expression, another takes a content type and format string.</span></span>

<span data-ttu-id="5dab0-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span><span class="sxs-lookup"><span data-stu-id="5dab0-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="5dab0-137">リダイレクトの拡張メソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="5dab0-137">There are also redirect extension methods.</span></span> <span data-ttu-id="5dab0-138">302 ステータス コードをクライアントに送信いずれかと、元の状態コードをクライアントに返しますがもリダイレクト URL のハンドラーを実行いずれか。</span><span class="sxs-lookup"><span data-stu-id="5dab0-138">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

<span data-ttu-id="5dab0-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span><span class="sxs-lookup"><span data-stu-id="5dab0-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="5dab0-140">特定の要求のステータス コード ページを無効にする必要がある場合は、これを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-140">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="5dab0-141">例外処理コード</span><span class="sxs-lookup"><span data-stu-id="5dab0-141">Exception-handling code</span></span>

<span data-ttu-id="5dab0-142">例外処理のページ内のコードは、例外をスローできます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-142">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="5dab0-143">多くの場合、純粋に静的なコンテンツで構成する実稼働のエラー ページのことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5dab0-143">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="5dab0-144">また、なることの応答ヘッダーが送信されると、応答のステータス コードを変更することはできません実行することもできる任意の例外のページやハンドラー。</span><span class="sxs-lookup"><span data-stu-id="5dab0-144">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="5dab0-145">応答を完了する必要があります。 または接続が中止されました。</span><span class="sxs-lookup"><span data-stu-id="5dab0-145">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="5dab0-146">サーバーの例外処理</span><span class="sxs-lookup"><span data-stu-id="5dab0-146">Server exception handling</span></span>

<span data-ttu-id="5dab0-147">アプリでは、ロジックを処理する例外に加え、[サーバー](servers/index.md)アプリをホストするいると、いくつかの例外処理が実行されます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-147">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app will perform some exception handling.</span></span> <span data-ttu-id="5dab0-148">ヘッダーが送信される前に、サーバーが例外をキャッチする場合は、本文なしで 500 内部サーバー エラー応答を送信します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-148">If the server catches an exception before the headers have been sent it sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="5dab0-149">ヘッダーの送信後に、例外をキャッチした場合は、接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-149">If it catches an exception after the headers have been sent, it closes the connection.</span></span> <span data-ttu-id="5dab0-150">アプリでは処理されない要求は、サーバーによって処理され、サーバーの例外によって発生する例外が処理される処理します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-150">Requests that are not handled by your app will be handled by the server, and any exception that occurs will be handled by the server's exception handling.</span></span> <span data-ttu-id="5dab0-151">任意のカスタム エラー ページまたは例外処理のミドルウェアまたはアプリ用に構成したフィルターは、この動作は影響しません。</span><span class="sxs-lookup"><span data-stu-id="5dab0-151">Any custom error pages or exception handling middleware or filters you have configured for your app will not affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="5dab0-152">スタートアップ例外処理</span><span class="sxs-lookup"><span data-stu-id="5dab0-152">Startup exception handling</span></span>

<span data-ttu-id="5dab0-153">ホスト レイヤーだけでは、アプリの起動中に発生する例外を処理できます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-153">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="5dab0-154">サーバーの動作はアプリの起動中に発生する例外の影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-154">Exceptions that occur during app startup can impact server behavior.</span></span> <span data-ttu-id="5dab0-155">呼び出す前に、例外が発生した場合など、 `KestrelServerOptions.UseHttps`、ホスティング レイヤーは、例外をキャッチ、サーバーを開始して、非 SSL ポートでエラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-155">For example, if an exception happens before you call `KestrelServerOptions.UseHttps`, the hosting layer catches the exception, starts the server, and displays an error page on the non-SSL port.</span></span> <span data-ttu-id="5dab0-156">その行の実行後に例外が発生した場合、エラー ページが代わりに HTTPS 経由で処理されます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-156">If an exception happens after that line executes, the error page is served over HTTPS instead.</span></span>

<span data-ttu-id="5dab0-157">実行できます[の起動中にエラーへの応答でのホストの動作方法を構成する](hosting.md#configuring-a-host)を使用して`CaptureStartupErrors`と`detailedErrors`キー。</span><span class="sxs-lookup"><span data-stu-id="5dab0-157">You can [configure how the host will behave in response to errors during startup](hosting.md#configuring-a-host) using `CaptureStartupErrors` and the `detailedErrors` key.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="5dab0-158">ASP.NET MVC のエラー処理</span><span class="sxs-lookup"><span data-stu-id="5dab0-158">ASP.NET MVC error handling</span></span>

<span data-ttu-id="5dab0-159">[MVC](../mvc/index.md)アプリにある例外フィルターを構成して、モデルの検証を実行するなどのエラーを処理するための追加オプション。</span><span class="sxs-lookup"><span data-stu-id="5dab0-159">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="5dab0-160">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="5dab0-160">Exception Filters</span></span>

<span data-ttu-id="5dab0-161">グローバルまたは MVC アプリケーション コント ローラーごとまたは - アクションごとに、例外フィルターを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="5dab0-161">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="5dab0-162">これらのフィルターは、コント ローラーのアクションまたは別のフィルターの実行中に発生する未処理の例外を処理し、それ以外の場合は呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="5dab0-162">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="5dab0-163">例外フィルターの詳細について[フィルター](../mvc/controllers/filters.md)です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-163">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="5dab0-164">例外フィルターは、MVC アクション内で発生する例外をトラップするのに便利ですが取り込まエラー ミドルウェアを処理する柔軟性がありません。</span><span class="sxs-lookup"><span data-stu-id="5dab0-164">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="5dab0-165">[全般] の場合、ミドルウェアを優先し、エラー処理のためにのみ必要があるフィルターを使用して*異なる*する MVC アクションの選択に基づいて。</span><span class="sxs-lookup"><span data-stu-id="5dab0-165">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="5dab0-166">処理のモデル状態エラー</span><span class="sxs-lookup"><span data-stu-id="5dab0-166">Handling Model State Errors</span></span>

<span data-ttu-id="5dab0-167">[モデルの検証](../mvc/models/validation.md)前に呼び出される、各コント ローラー アクションを発生し、検査するアクション メソッドの責任である`ModelState.IsValid`して適切に対処します。</span><span class="sxs-lookup"><span data-stu-id="5dab0-167">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="5dab0-168">一部のアプリは後者モデル検証エラーを処理するため、標準の規則に従うを選択、[フィルター](../mvc/controllers/filters.md)適切な場所をこのようなポリシーを実装する場合があります。</span><span class="sxs-lookup"><span data-stu-id="5dab0-168">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="5dab0-169">無効なモデルの状態を持つユーザーの操作の動作をテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5dab0-169">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="5dab0-170">詳細について[テスト コント ローラー ロジック](../mvc/controllers/testing.md)です。</span><span class="sxs-lookup"><span data-stu-id="5dab0-170">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



