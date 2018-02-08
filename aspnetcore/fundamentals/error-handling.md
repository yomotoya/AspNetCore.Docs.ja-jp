---
title: "ASP.NET Core のエラー処理"
author: ardalis
description: "ASP.NET Core アプリケーションでエラーを処理する方法について説明します。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="09a3c-103">ASP.NET Core でのエラー処理の概要</span><span class="sxs-lookup"><span data-stu-id="09a3c-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="09a3c-104">執筆: [Steve Smith](https://ardalis.com/)、[Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="09a3c-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="09a3c-105">この記事では、ASP.NET Core アプリでエラーを処理するための一般的な手法について取り上げます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="09a3c-106">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="09a3c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="09a3c-107">開発者例外ページ</span><span class="sxs-lookup"><span data-stu-id="09a3c-107">The developer exception page</span></span>

<span data-ttu-id="09a3c-108">例外に関する詳細を表示するページを表示するようにアプリを構成するには、`Microsoft.AspNetCore.Diagnostics` NuGet パッケージをインストールし、[Startup クラスの Configure メソッド](startup.md)に次の行を追加します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="09a3c-109">`app.UseMvc` のように、例外をキャッチするミドルウェアの前に `UseDeveloperExceptionPage` を挿入します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="09a3c-110">**アプリを開発環境で実行するときにのみ**、開発者例外ページを有効にします。</span><span class="sxs-lookup"><span data-stu-id="09a3c-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="09a3c-111">アプリを実稼働環境で実行するときは、詳細な例外情報を公開しません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="09a3c-112">[環境の構成についてはこちらをご覧ください](environments.md)。</span><span class="sxs-lookup"><span data-stu-id="09a3c-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="09a3c-113">開発者例外ページを表示するには、環境を `Development` に設定してサンプル アプリケーションを実行し、アプリの基礎 URL に `?throw=true` を追加します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="09a3c-114">このページには、例外と要求に関する情報を含むタブがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="09a3c-115">最初のタブにはスタック トレースがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-115">The first tab includes a stack trace.</span></span> 

![スタック トレース](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="09a3c-117">次のタブには、クエリ文字列パラメーターがあればそれが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-117">The next tab shows the query string parameters, if any.</span></span>

![クエリ文字列のパラメーター](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="09a3c-119">この要求には Cookie が与えられませんでしたが、与えられた場合、**Cookies** タブに表示されます。最後のタブに渡されたヘッダーを表示できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![ヘッダー](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="09a3c-121">カスタム例外処理ページを構成する</span><span class="sxs-lookup"><span data-stu-id="09a3c-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="09a3c-122">アプリを `Development` 環境で実行しないときに使用する例外ハンドラー ページを構成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="09a3c-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="09a3c-123">MVC アプリでは、`HttpGet` など、HTTP メソッド属性でエラー ハンドラー アクション メソッドを明示的に修飾しないでください。</span><span class="sxs-lookup"><span data-stu-id="09a3c-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="09a3c-124">明示的な動詞を使用すると、要求がメソッドに届かないことがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="09a3c-125">ステータス コード ページを構成する</span><span class="sxs-lookup"><span data-stu-id="09a3c-125">Configuring status code pages</span></span>

<span data-ttu-id="09a3c-126">アプリは既定で、500 (内部サーバー エラー) や 404 (見つかりません) など、HTTP ステータス コードをリッチ ページで表示しません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="09a3c-127">`Configure` メソッドに次の行を追加することで `StatusCodePagesMiddleware` を構成できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="09a3c-128">既定では、このミドルウェアは、404 など、一般的なステータス コードに単純なテキストのみのハンドラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 ページ](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="09a3c-130">ミドルウェアは、さまざまな拡張メソッドに対応しています。</span><span class="sxs-lookup"><span data-stu-id="09a3c-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="09a3c-131">ラムダ式を受け取るものや、コンテンツの種類と書式設定文字列を受け取るものがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="09a3c-132">リダイレクト拡張メソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-132">There are also redirect extension methods.</span></span> <span data-ttu-id="09a3c-133">302 ステータス コードをクライアントに送信するものや、元のステータス コードをクライアントに返すが、リダイレクト URL のハンドラーを実行するものがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="09a3c-134">特定の要求に対してステータス コード ページを無効にする場合、次の方法で可能です。</span><span class="sxs-lookup"><span data-stu-id="09a3c-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="09a3c-135">例外処理コード</span><span class="sxs-lookup"><span data-stu-id="09a3c-135">Exception-handling code</span></span>

<span data-ttu-id="09a3c-136">例外処理ページのコードは例外をスローすることがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="09a3c-137">実稼働のエラー ページは純粋に静的なコンテンツで構成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="09a3c-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="09a3c-138">また、応答のヘッダーの送信後、応答のステータス コードを変更できなくなり、例外ページやハンドラーを実行できなくなることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="09a3c-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="09a3c-139">応答は完了している必要があります。あるいは、接続が中止となっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="09a3c-140">サーバー例外処理</span><span class="sxs-lookup"><span data-stu-id="09a3c-140">Server exception handling</span></span>

<span data-ttu-id="09a3c-141">アプリの例外処理ロジックに加え、アプリをホストしている[サーバー](servers/index.md)がいくつかの例外処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="09a3c-142">サーバーがヘッダーの送信前に例外をキャッチすると、サーバーは 500 内部サーバー エラーを本文なしで送信します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="09a3c-143">サーバーがヘッダーの送信後に例外をキャッチすると、サーバーは接続を閉じます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="09a3c-144">アプリで処理されない要求はサーバーで処理されます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="09a3c-145">例外が発生すると、サーバーの例外処理で処理されます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="09a3c-146">カスタムのエラー ページ、例外処理ミドルウェア、フィルターを構成しても、この動作は変わりません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="09a3c-147">起動時の例外処理</span><span class="sxs-lookup"><span data-stu-id="09a3c-147">Startup exception handling</span></span>

<span data-ttu-id="09a3c-148">アプリの起動中に起こる例外はホスティング層だけが処理できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="09a3c-149">`captureStartupErrors` と `detailedErrors` キーを利用し、[起動中のエラーに対するホストの動作を構成](hosting.md#detailed-errors)できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="09a3c-150">ホスティングでは、ホスト アドレス/ポート バインディング後にエラーが発生した場合のみ、キャプチャされた起動時エラーに対してエラー ページを表示できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="09a3c-151">何らかの理由でバインディングに失敗した場合、ホスティング層は重要な例外をログに記録し、dotnet プロセスがクラッシュします。エラー ページは表示されません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="09a3c-152">ASP.NET MVC エラー処理</span><span class="sxs-lookup"><span data-stu-id="09a3c-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="09a3c-153">[MVC](xref:mvc/overview) アプリには、エラー処理の追加オプションがいくつかあります。例外フィルターの構成やモデル検証の実行などです。</span><span class="sxs-lookup"><span data-stu-id="09a3c-153">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="09a3c-154">例外フィルター</span><span class="sxs-lookup"><span data-stu-id="09a3c-154">Exception Filters</span></span>

<span data-ttu-id="09a3c-155">例外フィルターはグローバルに構成するか、MVC アプリのコントローラーまたはアクション単位で構成できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="09a3c-156">このようなフィルターはコントローラー アクションや別のフィルターの実行中に発生した例外が未処理のときにそれを処理し、それ以外では呼び出されません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="09a3c-157">例外フィルターの詳細は[フィルター](../mvc/controllers/filters.md)で確認できます。</span><span class="sxs-lookup"><span data-stu-id="09a3c-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="09a3c-158">例外フィルターは、MVC アクション内で発生する例外をトラップするのには適していますが、エラー処理ミドルウェアほどの柔軟性はありません。</span><span class="sxs-lookup"><span data-stu-id="09a3c-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="09a3c-159">一般的なケースにはミドルウェアを選択し、MVC アクションで選択されたのとは*異なる*方法でエラー処理を行う必要がある場合にのみフィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="09a3c-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="09a3c-160">モデル状態エラーの処理</span><span class="sxs-lookup"><span data-stu-id="09a3c-160">Handling Model State Errors</span></span>

<span data-ttu-id="09a3c-161">[モデル検証](../mvc/models/validation.md)は各コントローラー アクションを呼び出す前に行われます。`ModelState.IsValid` を検査し、適切に対処するのはアクション メソッドの仕事です。</span><span class="sxs-lookup"><span data-stu-id="09a3c-161">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="09a3c-162">一部のアプリでは、モデル検証エラーを処理するとき、標準的な規則に従います。その場合、そのようなポリシーを実装する場所として[フィルター](../mvc/controllers/filters.md)が適していることがあります。</span><span class="sxs-lookup"><span data-stu-id="09a3c-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="09a3c-163">無効なモデル状態でアクションの動作をテストしてください。</span><span class="sxs-lookup"><span data-stu-id="09a3c-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="09a3c-164">詳細については、[コントローラー ロジックのテスト](../mvc/controllers/testing.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="09a3c-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



