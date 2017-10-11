---
title: "URL の ASP.NET Core のミドルウェアの書き換え"
author: guardrex
description: "URL 書き換えおよび ASP.NET Core アプリケーションの URL 書き換えミドルウェアにリダイレクトすることについて説明します。"
keywords: "ASP.NET Core、URL 書き換え、URL 書き換え URL をリダイレクトする、URL リダイレクト、ミドルウェア apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 0a4024edf13651e2ed7e0f87e554e8ba8d895619
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="4f7dd-104">URL の ASP.NET Core のミドルウェアの書き換え</span><span class="sxs-lookup"><span data-stu-id="4f7dd-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="4f7dd-105">によって[Luke Latham](https://github.com/guardrex)と[Mikael メンギストゥ](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="4f7dd-105">By [Luke Latham](https://github.com/guardrex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

<span data-ttu-id="4f7dd-106">[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4f7dd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4f7dd-107">URL の書き換えは 1 つまたは複数の事前定義された規則に基づいて、Url 要求を変更することです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="4f7dd-108">URL 書き換え、場所とアドレスが緊密にリンクされていないように、リソースの場所とその住所間の抽象化を作成します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="4f7dd-109">URL 書き換えが役に立ちますいくつかのシナリオはあります。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="4f7dd-110">移動またはそれらのリソースの安定したロケーターを維持しながら、サーバー リソースを一時的または永続的に交換</span><span class="sxs-lookup"><span data-stu-id="4f7dd-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="4f7dd-111">要求処理でアプリを 1 つの区分にわたってまたは別のアプリ間での分割</span><span class="sxs-lookup"><span data-stu-id="4f7dd-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="4f7dd-112">削除、追加、または受信した要求の URL セグメントを再編成</span><span class="sxs-lookup"><span data-stu-id="4f7dd-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="4f7dd-113">検索エンジン最適化 (SEO) のパブリック Url を最適化します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="4f7dd-114">次のリンクを検索できるが、コンテンツの予測を手助けするフレンドリなパブリック Url の使用を許可します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="4f7dd-115">エンドポイントをセキュリティで保護するセキュリティ保護されていない要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="4f7dd-116">イメージ hotlinking の防止</span><span class="sxs-lookup"><span data-stu-id="4f7dd-116">Preventing image hotlinking</span></span>

<span data-ttu-id="4f7dd-117">いくつかの方法で URL を変更する regex、Apache mod_rewrite モジュール ルール、IIS の書き換え規則など、カスタムの規則のロジックを使用するための規則を定義できます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="4f7dd-118">このドキュメントでは、ASP.NET Core アプリケーションで URL 書き換えミドルウェアを使用する方法について記載された URL の書き換えが導入されています。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="4f7dd-119">URL の書き換えと、アプリのパフォーマンスが低下することができます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="4f7dd-120">可能であれば、規則の複雑さと数を制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="4f7dd-121">URL リダイレクトと URL の書き換え</span><span class="sxs-lookup"><span data-stu-id="4f7dd-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="4f7dd-122">表現の間に違い*URL リダイレクト*と*URL 書き換え*に微妙なように見えますが最初のクライアントにリソースを提供するための重要な意味を含んでいます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="4f7dd-123">ASP.NET Core URL 書き換えミドルウェアは、両方のニーズを満たせるです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="4f7dd-124">A *URL リダイレクト*クライアント側の操作は、クライアントが別のアドレスにあるリソースにアクセスするよう指示します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="4f7dd-125">サーバーへのラウンド トリップが必要ですされ、クライアントが、リソースに対する新しい要求を行うときに、ブラウザーのアドレス バーに、クライアントに返され、リダイレクト URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="4f7dd-126">場合`/resource`は*リダイレクト*に`/different-resource`、クライアントからの要求`/resource`、サーバーは、クライアントがリソースをリソースを取得する必要があります、応答と`/different-resource`リダイレクトがであることを示すステータス コードで一時的または永続的です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="4f7dd-127">クライアントは、リダイレクト URL にあるリソースに対する新しい要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-127">The client executes a new request for the resource at the redirect URL.</span></span>

![WebAPI サービス エンドポイントは、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から一時的に変更されましたが。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="4f7dd-133">要求を別の URL にリダイレクトする場合は、リダイレクトが永続的または一時的であるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="4f7dd-134">301 (完全な移動) 状態コードには、リソースのすべての要求が新しい URL を使用する必要がありますをクライアントに指示する、新しい永続的な URL は、リソースは、するは使用されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="4f7dd-135">*301 ステータス コードを受信すると、クライアントは応答をキャッシュ可能性があります。*</span><span class="sxs-lookup"><span data-stu-id="4f7dd-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="4f7dd-136">302 (検出) ステータス コードを使用、リダイレクトが一時的または一般にサブジェクトが変更するべきではありません、クライアントの格納し、リダイレクト URL を後で再になるようにします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="4f7dd-137">詳細については、次を参照してください。 [RFC 2616: 状態コード定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="4f7dd-138">A *URL 書き換え*はサーバー側の操作を別のリソースのアドレスからのリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="4f7dd-139">URL の書き換えと、サーバーへのラウンド トリップが必要としません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="4f7dd-140">書き換えられた URL では、クライアントに返されません。 と、ブラウザーのアドレス バーに表示されません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="4f7dd-141">ときに`/resource`は*書き直す*に`/different-resource`、クライアントからの要求`/resource`とサーバー*内部的に*にあるリソースをフェッチ`/different-resource`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="4f7dd-142">クライアントは、書き換えられた URL にあるリソースを取得できる場合がありますがその要求を行う応答を受信して、ときに、書き換えられた URL にリソースが存在するクライアントが通知されません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![WebAPI サービス エンドポイントが、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から変更されました。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="4f7dd-147">URL 書き換えのサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="4f7dd-147">URL rewriting sample app</span></span>
<span data-ttu-id="4f7dd-148">URL 書き換えミドルウェアの機能を調べることができます、 [URL 書き換えのサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="4f7dd-149">アプリが書き換えを適用し、リダイレクト ルールし、書き換えられたかリダイレクトされる URL を表示します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="4f7dd-150">URL 書き換えミドルウェアを使用する場合</span><span class="sxs-lookup"><span data-stu-id="4f7dd-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="4f7dd-151">使用できない場合は、URL 書き換えミドルウェアを使用して、 [URL Rewrite モジュール](https://www.iis.net/downloads/microsoft/url-rewrite)Windows Server 上の IIS で、 [Apache mod_rewrite モジュール](https://httpd.apache.org/docs/2.4/rewrite/)Apache サーバーで、 [NginxでURLの書き換え](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)でホストされているアプリまたは[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="4f7dd-152">テクノロジを使用して、サーバー ベース URL 書き換え IIS、Apache、または Nginx の主な理由ではこれらのモジュールの全機能をサポートしていないミドルウェア ミドルウェアのパフォーマンス可能性とも一致しません、モジュールの。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="4f7dd-153">ただし、ASP.NET Core プロジェクトなどを操作しないサーバー モジュールの一部の機能がある、`IsFile`と`IsDirectory`IIS Rewrite モジュールの制約。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="4f7dd-154">これらのシナリオでは、代わりに、ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="4f7dd-155">Package</span><span class="sxs-lookup"><span data-stu-id="4f7dd-155">Package</span></span>
<span data-ttu-id="4f7dd-156">ミドルウェアをプロジェクトに含めるへの参照を追加、 [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="4f7dd-157">この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="4f7dd-158">拡張機能とオプション</span><span class="sxs-lookup"><span data-stu-id="4f7dd-158">Extension and options</span></span>
<span data-ttu-id="4f7dd-159">URL 書き換えを確立しのインスタンスを作成することでルールをリダイレクト、`RewriteOptions`クラスでは、ルールの各拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="4f7dd-160">ようにした場合に処理された順序で複数のルールを連結します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="4f7dd-161">`RewriteOptions`と要求パイプラインに追加されるように、URL 書き換えミドルウェアに渡される`app.UseRewriter(options);`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]

---

### <a name="url-redirect"></a><span data-ttu-id="4f7dd-164">URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="4f7dd-164">URL redirect</span></span>
<span data-ttu-id="4f7dd-165">使用して`AddRedirect`要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-165">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="4f7dd-166">最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-166">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="4f7dd-167">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-167">The second parameter is the replacement string.</span></span> <span data-ttu-id="4f7dd-168">3 番目のパラメーターでは、存在する場合は、ステータス コードを指定します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-168">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="4f7dd-169">ステータス コードを指定しない場合、既定値 302 (検出)、リソースが一時的に移動または置き換えることを示すです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-169">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]

---

<span data-ttu-id="4f7dd-172">ブラウザーで有効になっている開発者ツールで、パスのサンプル アプリに要求を行う`/redirect-rule/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-172">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="4f7dd-173">正規表現の要求パスに一致する`redirect-rule/(.*)`、パスが置き換え`/redirected/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-173">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="4f7dd-174">リダイレクト URL は 302 (検出) のステータス コードでクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-174">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="4f7dd-175">ブラウザーでは、新しい要求をブラウザーのアドレス バーに表示されるリダイレクト URL でです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-175">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="4f7dd-176">2 番目の要求は、アプリから 200 (OK) 応答を受信し、リダイレクト URL のサンプル アプリの規則が一致しないため、応答の本体はリダイレクト URL が表示できます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-176">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="4f7dd-177">URL がサーバーとのやり取りが行われた*リダイレクト*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-177">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="4f7dd-178">リダイレクト ルールを確立するときに注意します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-178">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="4f7dd-179">リダイレクト後など、アプリへの各要求では、リダイレクト ルールが評価されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-179">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="4f7dd-180">簡単に誤って無限のリダイレクトのループを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-180">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="4f7dd-181">元の要求:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-181">Original Request: `/redirect-rule/1234/5678`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="4f7dd-183">かっこ内に含まれる式の一部と呼ばれる、*キャプチャ グループ*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-183">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="4f7dd-184">ドット (`.`) 式の意味*任意の文字を一致*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-184">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="4f7dd-185">アスタリスク (`*`) を示す*直前の文字に 0 回以上一致*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-185">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="4f7dd-186">URL の最後の 2 つのパス セグメントではそのため、 `1234/5678`、キャプチャ グループによってキャプチャされた`(.*)`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-186">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="4f7dd-187">任意の値の後に、要求 URL で指定する`redirect-rule/`はこの単一のキャプチャ グループによってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-187">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="4f7dd-188">置換文字列にキャプチャされたグループが挿入され、ドル記号の文字列 (`$`) に続けてのキャプチャのシーケンス番号。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-188">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="4f7dd-189">最初のキャプチャ グループ値を取得`$1`、2 番目は`$2`の正規表現内のキャプチャ グループのシーケンスで続行されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-189">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="4f7dd-190">1 つだけのキャプチャ グループにあるリダイレクト ルール regex サンプル アプリでは置換文字列に 1 つだけの挿入されたグループがあるため`$1`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-190">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="4f7dd-191">ルールが適用されるときに、URL は次のようになります。`/redirected/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-191">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="4f7dd-192">セキュリティで保護されたエンドポイントへの URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="4f7dd-192">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="4f7dd-193">使用して`AddRedirectToHttps`HTTP 要求を同じホストと HTTPS を使用して、パスにリダイレクト (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-193">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="4f7dd-194">ステータス コードが指定されていない場合、ミドルウェアを 302 (検出) に既定値です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-194">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="4f7dd-195">ポートが指定されていない場合は、ミドルウェア既定`null`、つまり、プロトコルに変更`https://`クライアント ポート 443 上のリソースにアクセスするとします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-195">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="4f7dd-196">この例では、301 (完全な移動) に、状態コードを設定し、5001 にポートを変更する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-196">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="4f7dd-197">使用して`AddRedirectToHttpsPermanent`同じホストのパスとセキュリティで保護された HTTPS プロトコルを使用するセキュリティ保護されていない要求をリダイレクトする (`https://`ポート 443)。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-197">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="4f7dd-198">ミドルウェアは、ステータス コードを 301 (完全な移動) に設定します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-198">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="4f7dd-199">サンプル アプリが使用する方法を示す可能`AddRedirectToHttps`または`AddRedirectToHttpsPermanent`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-199">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="4f7dd-200">拡張メソッドを追加、`RewriteOptions`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-200">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="4f7dd-201">任意の URL にアプリをセキュリティ保護されていない要求を行います。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-201">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="4f7dd-202">自己署名証明書が信頼できることを警告するブラウザーのセキュリティを消去します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-202">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="4f7dd-203">元の要求を使用して`AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-203">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="4f7dd-205">元の要求を使用して`AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-205">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="4f7dd-207">URL 書き換え</span><span class="sxs-lookup"><span data-stu-id="4f7dd-207">URL rewrite</span></span>
<span data-ttu-id="4f7dd-208">使用して`AddRewrite`Url の書き換えの規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-208">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="4f7dd-209">最初のパラメーターには、着信 URL パスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-209">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="4f7dd-210">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-210">The second parameter is the replacement string.</span></span> <span data-ttu-id="4f7dd-211">3 番目のパラメーターでは、`skipRemainingRules: {true|false}`ミドルウェアに示す現在のルールが適用されている場合は、追加の書き換えルールをスキップするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-211">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-212">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-212">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]

---

<span data-ttu-id="4f7dd-214">元の要求:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-214">Original Request: `/rewrite-rule/1234/5678`</span></span>

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="4f7dd-216">最初、正規表現で注目するは、キャレット (`^`) 式の先頭にします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-216">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="4f7dd-217">これは、URL パスの先頭で一致が開始されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-217">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="4f7dd-218">リダイレクト ルールでは、先ほどの例で`redirect-rule/(.*)`、正規表現の先頭のキャレットはありません。 したがって、任意の文字の前可能性があります`redirect-rule/`に成功した一致のパスにします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-218">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="4f7dd-219">パス</span><span class="sxs-lookup"><span data-stu-id="4f7dd-219">Path</span></span>                               | <span data-ttu-id="4f7dd-220">一致したもの</span><span class="sxs-lookup"><span data-stu-id="4f7dd-220">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="4f7dd-221">はい</span><span class="sxs-lookup"><span data-stu-id="4f7dd-221">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="4f7dd-222">はい</span><span class="sxs-lookup"><span data-stu-id="4f7dd-222">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="4f7dd-223">はい</span><span class="sxs-lookup"><span data-stu-id="4f7dd-223">Yes</span></span>   |

<span data-ttu-id="4f7dd-224">書き換えルール`^rewrite-rule/(\d+)/(\d+)`、のみで開始される場合、パスと一致する`rewrite-rule/`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-224">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="4f7dd-225">書き換えルールの下と上のリダイレクト ルール間の一致の違いに注意してください。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-225">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="4f7dd-226">パス</span><span class="sxs-lookup"><span data-stu-id="4f7dd-226">Path</span></span>                              | <span data-ttu-id="4f7dd-227">一致したもの</span><span class="sxs-lookup"><span data-stu-id="4f7dd-227">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="4f7dd-228">はい</span><span class="sxs-lookup"><span data-stu-id="4f7dd-228">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="4f7dd-229">いいえ</span><span class="sxs-lookup"><span data-stu-id="4f7dd-229">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="4f7dd-230">いいえ</span><span class="sxs-lookup"><span data-stu-id="4f7dd-230">No</span></span>    |

<span data-ttu-id="4f7dd-231">次の`^rewrite-rule/`部分式の 2 つのキャプチャ グループが`(\d+)/(\d+)`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-231">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="4f7dd-232">`\d`ことを示します*digit (数) を一致*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-232">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="4f7dd-233">プラス記号 (`+`) ことを意味*直前の文字の 1 つ以上の一致*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-233">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="4f7dd-234">そのため、URL では、スラッシュを他の数値の後に続く数値を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-234">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="4f7dd-235">これらのキャプチャ グループが挿入され、書き換えられた URL として`$1`と`$2`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-235">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="4f7dd-236">書き換えルールの置換文字列は、クエリ文字列に、キャプチャされたグループを配置します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-236">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="4f7dd-237">要求されたパスの`/rewrite-rule/1234/5678`にあるリソースを取得する書き直す`/rewritten?var1=1234&var2=5678`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-237">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="4f7dd-238">クエリ文字列が元の要求に存在する場合、URL は再作成時に保持します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-238">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="4f7dd-239">リソースを取得するためにサーバーとのやり取りはありません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-239">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="4f7dd-240">リソースが存在する場合は、フェッチされ、200 (OK) ステータス コードをクライアントに返されることがされます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-240">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="4f7dd-241">クライアントがリダイレクトされていないために、ブラウザーのアドレス バーの URL は変更されません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-241">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="4f7dd-242">クライアントに関しては、限りことはありません、URL 書き換え操作が発生しました。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-242">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="4f7dd-243">使用して`skipRemainingRules: true`可能な限り、コストのかかるプロセスは、アプリの応答時間を短縮の規則に一致するためです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-243">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="4f7dd-244">最も高速のアプリの応答。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-244">For the fastest app response:</span></span>
> * <span data-ttu-id="4f7dd-245">最もよく一致するルールを最も頻繁に一致したルールから、書き換えルールを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-245">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="4f7dd-246">一致が発生し、その他のルールの処理は必要ありません、残りのルールの処理をスキップします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-246">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="4f7dd-247">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="4f7dd-247">Apache mod_rewrite</span></span>
<span data-ttu-id="4f7dd-248">Apache mod_rewrite ルールを適用`AddApacheModRewrite`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-248">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="4f7dd-249">アプリの規則ファイルが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-249">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="4f7dd-250">詳細と mod_rewrite 規則の例については、次を参照してください。 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-250">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4f7dd-252">A`StreamReader`からルールの読み取りに使用される、 *ApacheModRewrite.txt*ルール ファイルです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-252">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4f7dd-254">最初のパラメーターには、 `IFileProvider`、経由で提供されています[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-254">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="4f7dd-255">`IHostingEnvironment`を提供する挿入、`ContentRootFileProvider`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-255">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="4f7dd-256">2 番目のパラメーターは、ファイルへのパスの規則、これは*ApacheModRewrite.txt*サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-256">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]

---

<span data-ttu-id="4f7dd-257">サンプル アプリからの要求をリダイレクトする`/apache-mod-rules-redirect/(.\*)`に`/redirected?id=$1`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-257">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="4f7dd-258">応答状態コードは、302 (検出) です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-258">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="4f7dd-259">元の要求:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-259">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="4f7dd-261">サポートされているサーバー変数</span><span class="sxs-lookup"><span data-stu-id="4f7dd-261">Supported server variables</span></span>
<span data-ttu-id="4f7dd-262">ミドルウェアは、次の Apache mod_rewrite サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-262">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="4f7dd-263">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-263">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="4f7dd-264">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-264">HTTP_ACCEPT</span></span>
* <span data-ttu-id="4f7dd-265">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="4f7dd-265">HTTP_CONNECTION</span></span>
* <span data-ttu-id="4f7dd-266">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="4f7dd-266">HTTP_COOKIE</span></span>
* <span data-ttu-id="4f7dd-267">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="4f7dd-267">HTTP_FORWARDED</span></span>
* <span data-ttu-id="4f7dd-268">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="4f7dd-268">HTTP_HOST</span></span>
* <span data-ttu-id="4f7dd-269">ボタン</span><span class="sxs-lookup"><span data-stu-id="4f7dd-269">HTTP_REFERER</span></span>
* <span data-ttu-id="4f7dd-270">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-270">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="4f7dd-271">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4f7dd-271">HTTPS</span></span>
* <span data-ttu-id="4f7dd-272">IPV6</span><span class="sxs-lookup"><span data-stu-id="4f7dd-272">IPV6</span></span>
* <span data-ttu-id="4f7dd-273">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="4f7dd-273">QUERY_STRING</span></span>
* <span data-ttu-id="4f7dd-274">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-274">REMOTE_ADDR</span></span>
* <span data-ttu-id="4f7dd-275">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-275">REMOTE_PORT</span></span>
* <span data-ttu-id="4f7dd-276">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="4f7dd-276">REQUEST_FILENAME</span></span>
* <span data-ttu-id="4f7dd-277">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="4f7dd-277">REQUEST_METHOD</span></span>
* <span data-ttu-id="4f7dd-278">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="4f7dd-278">REQUEST_SCHEME</span></span>
* <span data-ttu-id="4f7dd-279">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="4f7dd-279">REQUEST_URI</span></span>
* <span data-ttu-id="4f7dd-280">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="4f7dd-280">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="4f7dd-281">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-281">SERVER_ADDR</span></span>
* <span data-ttu-id="4f7dd-282">これらの用語</span><span class="sxs-lookup"><span data-stu-id="4f7dd-282">SERVER_PORT</span></span>
* <span data-ttu-id="4f7dd-283">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="4f7dd-283">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="4f7dd-284">時間</span><span class="sxs-lookup"><span data-stu-id="4f7dd-284">TIME</span></span>
* <span data-ttu-id="4f7dd-285">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="4f7dd-285">TIME_DAY</span></span>
* <span data-ttu-id="4f7dd-286">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-286">TIME_HOUR</span></span>
* <span data-ttu-id="4f7dd-287">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="4f7dd-287">TIME_MIN</span></span>
* <span data-ttu-id="4f7dd-288">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="4f7dd-288">TIME_MON</span></span>
* <span data-ttu-id="4f7dd-289">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="4f7dd-289">TIME_SEC</span></span>
* <span data-ttu-id="4f7dd-290">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="4f7dd-290">TIME_WDAY</span></span>
* <span data-ttu-id="4f7dd-291">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-291">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="4f7dd-292">IIS URL Rewrite モジュール ルール</span><span class="sxs-lookup"><span data-stu-id="4f7dd-292">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="4f7dd-293">IIS URL Rewrite モジュールに適用される規則を使用する`AddIISUrlRewrite`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-293">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="4f7dd-294">アプリの規則ファイルが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-294">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="4f7dd-295">使用するミドルウェアを直接しない、 *web.config*ファイルの Windows Server IIS で実行されているときにします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-295">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="4f7dd-296">外部で IIS にこれらの規則を保存するか、 *web.config* IIS Rewrite モジュールと競合しないようにします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-296">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="4f7dd-297">詳細と IIS URL Rewrite モジュールの規則の例については、次を参照してください。 [Url Rewrite モジュール 2.0 の使用](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)と[URL 書き換えモジュール構成の参照](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-297">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4f7dd-299">A`StreamReader`からルールの読み取りに使用される、 *IISUrlRewrite.xml*ルール ファイルです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-299">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-300">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-300">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4f7dd-301">最初のパラメーターには、 `IFileProvider`、2 番目のパラメーターはこれは、XML ルール ファイルのパスを*IISUrlRewrite.xml*サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-301">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]

---

<span data-ttu-id="4f7dd-302">サンプル アプリからの要求をリライト`/iis-rules-rewrite/(.*)`に`/rewritten?id=$1`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-302">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="4f7dd-303">応答は、200 (OK) ステータス コードでクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-303">The response is sent to the client with a 200 (OK) status code.</span></span>

[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]

<span data-ttu-id="4f7dd-304">元の要求:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-304">Original Request: `/iis-rules-rewrite/1234`</span></span>

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="4f7dd-306">、望ましくない方法でアプリに影響するように構成されたサーバー レベル ルールにアクティブな IIS Rewrite モジュールがある場合は、アプリの IIS Rewrite モジュールを無効にできます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-306">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="4f7dd-307">詳細については、次を参照してください。[を無効にする IIS モジュール](xref:hosting/iis-modules#disabling-iis-modules)です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-307">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="4f7dd-308">サポートされていない機能</span><span class="sxs-lookup"><span data-stu-id="4f7dd-308">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-309">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-309">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4f7dd-310">リリース ミドルウェアと ASP.NET Core 2.x IIS URL Rewrite モジュール機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-310">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="4f7dd-311">送信の規則</span><span class="sxs-lookup"><span data-stu-id="4f7dd-311">Outbound Rules</span></span>
* <span data-ttu-id="4f7dd-312">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="4f7dd-312">Custom Server Variables</span></span>
* <span data-ttu-id="4f7dd-313">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="4f7dd-313">Wildcards</span></span>
* <span data-ttu-id="4f7dd-314">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="4f7dd-314">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4f7dd-316">リリース ミドルウェアと ASP.NET Core 1.x IIS URL Rewrite モジュール機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-316">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="4f7dd-317">グローバル ルール</span><span class="sxs-lookup"><span data-stu-id="4f7dd-317">Global Rules</span></span>
* <span data-ttu-id="4f7dd-318">送信の規則</span><span class="sxs-lookup"><span data-stu-id="4f7dd-318">Outbound Rules</span></span>
* <span data-ttu-id="4f7dd-319">マップを書き直してください。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-319">Rewrite Maps</span></span>
* <span data-ttu-id="4f7dd-320">CustomResponse アクション</span><span class="sxs-lookup"><span data-stu-id="4f7dd-320">CustomResponse Action</span></span>
* <span data-ttu-id="4f7dd-321">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="4f7dd-321">Custom Server Variables</span></span>
* <span data-ttu-id="4f7dd-322">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="4f7dd-322">Wildcards</span></span>
* <span data-ttu-id="4f7dd-323">アクション: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="4f7dd-323">Action:CustomResponse</span></span>
* <span data-ttu-id="4f7dd-324">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="4f7dd-324">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="4f7dd-325">サポートされているサーバー変数</span><span class="sxs-lookup"><span data-stu-id="4f7dd-325">Supported server variables</span></span>
<span data-ttu-id="4f7dd-326">ミドルウェアは、次の URL Rewrite モジュールを IIS サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-326">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="4f7dd-327">多く</span><span class="sxs-lookup"><span data-stu-id="4f7dd-327">CONTENT_LENGTH</span></span>
* <span data-ttu-id="4f7dd-328">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="4f7dd-328">CONTENT_TYPE</span></span>
* <span data-ttu-id="4f7dd-329">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-329">HTTP_ACCEPT</span></span>
* <span data-ttu-id="4f7dd-330">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="4f7dd-330">HTTP_CONNECTION</span></span>
* <span data-ttu-id="4f7dd-331">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="4f7dd-331">HTTP_COOKIE</span></span>
* <span data-ttu-id="4f7dd-332">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="4f7dd-332">HTTP_HOST</span></span>
* <span data-ttu-id="4f7dd-333">ボタン</span><span class="sxs-lookup"><span data-stu-id="4f7dd-333">HTTP_REFERER</span></span>
* <span data-ttu-id="4f7dd-334">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="4f7dd-334">HTTP_URL</span></span>
* <span data-ttu-id="4f7dd-335">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-335">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="4f7dd-336">HTTPS</span><span class="sxs-lookup"><span data-stu-id="4f7dd-336">HTTPS</span></span>
* <span data-ttu-id="4f7dd-337">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-337">LOCAL_ADDR</span></span>
* <span data-ttu-id="4f7dd-338">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="4f7dd-338">QUERY_STRING</span></span>
* <span data-ttu-id="4f7dd-339">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="4f7dd-339">REMOTE_ADDR</span></span>
* <span data-ttu-id="4f7dd-340">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="4f7dd-340">REMOTE_PORT</span></span>
* <span data-ttu-id="4f7dd-341">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="4f7dd-341">REQUEST_FILENAME</span></span>
* <span data-ttu-id="4f7dd-342">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="4f7dd-342">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="4f7dd-343">取得することも、`IFileProvider`を介して、`PhysicalFileProvider`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-343">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="4f7dd-344">このアプローチは可能性があります規則ファイルを書き換えの場所をより柔軟に提供します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-344">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="4f7dd-345">書き換え規則ファイルを指定したパスにあるサーバーにデプロイされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-345">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="4f7dd-346">メソッド ベースのルール</span><span class="sxs-lookup"><span data-stu-id="4f7dd-346">Method-based rule</span></span>
<span data-ttu-id="4f7dd-347">使用して`Add(Action<RewriteContext> applyRule)`メソッドで、独自の規則ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-347">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="4f7dd-348">`RewriteContext`公開、`HttpContext`メソッドで使用するためです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-348">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="4f7dd-349">`context.Result`パイプラインを追加する方法を決定する処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-349">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="4f7dd-350">コンテキスト。結果</span><span class="sxs-lookup"><span data-stu-id="4f7dd-350">context.Result</span></span>                       | <span data-ttu-id="4f7dd-351">操作</span><span class="sxs-lookup"><span data-stu-id="4f7dd-351">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="4f7dd-352">`RuleResult.ContinueRules` (既定値)</span><span class="sxs-lookup"><span data-stu-id="4f7dd-352">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="4f7dd-353">ルールの適用を続行します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-353">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="4f7dd-354">規則の適用を停止し、応答を送信</span><span class="sxs-lookup"><span data-stu-id="4f7dd-354">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="4f7dd-355">ルールの適用を停止し、次のミドルウェアにコンテキストを送信</span><span class="sxs-lookup"><span data-stu-id="4f7dd-355">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-356">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-356">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-357">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-357">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]

---

<span data-ttu-id="4f7dd-358">サンプル アプリで終了するパスの要求をリダイレクトするメソッドに示します*.xml*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-358">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="4f7dd-359">要求を行う場合`/file.xml`にリダイレクトされます`/xmlfiles/file.xml`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-359">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="4f7dd-360">ステータス コードが 301 (完全な移動) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-360">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="4f7dd-361">リダイレクトは、応答のステータス コード明示的に設定する必要があります。それ以外の場合は 200 (OK) のステータス コードが返され、リダイレクトはクライアントで発生しません。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-361">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-362">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-362">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]

---

<span data-ttu-id="4f7dd-364">元の要求:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-364">Original Request: `/file.xml`</span></span>

![ブラウザー ウィンドウで、要求および file.xml の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="4f7dd-366">IRule ベースのルール</span><span class="sxs-lookup"><span data-stu-id="4f7dd-366">IRule-based rule</span></span>
<span data-ttu-id="4f7dd-367">使用して`Add(IRule)`から派生したクラスに、独自の規則ロジックを実装する`IRule`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-367">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="4f7dd-368">使用して、`IRule`メソッド ベースのルールのアプローチを使用する場合より高い柔軟性が実現します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-368">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="4f7dd-369">派生クラスでのパラメーターに渡すことができます、コンス トラクターを含めることがあります、`ApplyRule`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-369">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-370">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-370">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-371">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-371">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]

---

<span data-ttu-id="4f7dd-372">用のサンプル アプリケーションでは、パラメーターの値、`extension`と`newPath`をいくつかの条件を満たすためにチェックされます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-372">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="4f7dd-373">`extension` 、値を格納する必要があります値を指定する必要があります*.png*、 *.jpg*、または*.gif*です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-373">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="4f7dd-374">場合、`newPath`が有効でない、`ArgumentException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-374">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="4f7dd-375">要求を行う場合*image.png*にリダイレクトされます`/png-images/image.png`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-375">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="4f7dd-376">要求を行う場合*image.jpg*にリダイレクトされます`/jpg-images/image.jpg`です。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-376">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="4f7dd-377">ステータス コードが 301 (完全な移動) に設定され、`context.Result`規則の処理を停止し、応答の送信に設定されています。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-377">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4f7dd-378">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-378">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4f7dd-379">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4f7dd-379">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]

---

<span data-ttu-id="4f7dd-380">元の要求:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-380">Original Request: `/image.png`</span></span>

![ブラウザー ウィンドウで、要求および image.png の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="4f7dd-382">元の要求:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-382">Original Request: `/image.jpg`</span></span>

![ブラウザー ウィンドウで、要求および image.jpg の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="4f7dd-384">正規表現の例</span><span class="sxs-lookup"><span data-stu-id="4f7dd-384">Regex examples</span></span>

| <span data-ttu-id="4f7dd-385">Goal</span><span class="sxs-lookup"><span data-stu-id="4f7dd-385">Goal</span></span> | <span data-ttu-id="4f7dd-386">Regex 文字列 (& a)</span><span class="sxs-lookup"><span data-stu-id="4f7dd-386">Regex String &</span></span><br><span data-ttu-id="4f7dd-387">一致の例</span><span class="sxs-lookup"><span data-stu-id="4f7dd-387">Match Example</span></span> | <span data-ttu-id="4f7dd-388">置換後の文字列 (& a)</span><span class="sxs-lookup"><span data-stu-id="4f7dd-388">Replacement String &</span></span><br><span data-ttu-id="4f7dd-389">出力例</span><span class="sxs-lookup"><span data-stu-id="4f7dd-389">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="4f7dd-390">クエリ文字列にパスを書き直してください。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-390">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="4f7dd-391">末尾のスラッシュを除去します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-391">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="4f7dd-392">末尾のスラッシュを適用します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-392">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="4f7dd-393">特定の要求を再作成を回避します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-393">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="4f7dd-394">うん：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-394">Yes: `/resource.htm`</span></span><br><span data-ttu-id="4f7dd-395">違います：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="4f7dd-395">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="4f7dd-396">URL セグメントを再配置します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-396">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="4f7dd-397">URL セグメントを置き換えます</span><span class="sxs-lookup"><span data-stu-id="4f7dd-397">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="4f7dd-398">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="4f7dd-398">Additional resources</span></span>
* [<span data-ttu-id="4f7dd-399">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="4f7dd-399">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="4f7dd-400">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="4f7dd-400">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="4f7dd-401">.NET の正規表現</span><span class="sxs-lookup"><span data-stu-id="4f7dd-401">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="4f7dd-402">正規表現言語 - クイック リファレンス</span><span class="sxs-lookup"><span data-stu-id="4f7dd-402">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="4f7dd-403">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="4f7dd-403">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="4f7dd-404">(IIS) の Url 書き換えモジュール 2.0 を使用してください。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-404">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="4f7dd-405">URL 書き換えモジュール構成のリファレンス</span><span class="sxs-lookup"><span data-stu-id="4f7dd-405">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="4f7dd-406">IIS URL 書き換えモジュール フォーラム</span><span class="sxs-lookup"><span data-stu-id="4f7dd-406">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="4f7dd-407">単純な URL 構造を保持します。</span><span class="sxs-lookup"><span data-stu-id="4f7dd-407">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="4f7dd-408">10 URL 書き換えヒントし、テクニック</span><span class="sxs-lookup"><span data-stu-id="4f7dd-408">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="4f7dd-409">スラッシュまたは円記号が</span><span class="sxs-lookup"><span data-stu-id="4f7dd-409">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
