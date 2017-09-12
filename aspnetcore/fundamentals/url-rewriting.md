---
title: "URL の ASP.NET Core のミドルウェアの書き換え"
author: guardrex
description: "URL 書き換えおよび ASP.NET Core アプリケーションで URL 書き換えミドルウェアを使用する方法について記載されたリダイレクトの概要です。"
keywords: "ASP.NET Core、URL 書き換え、URL 書き換え URL をリダイレクトする、URL リダイレクト、ミドルウェア apache_mod"
ms.author: riande
manager: wpickett
ms.date: 08/17/2017
ms.topic: article
ms.assetid: e6130638-c410-4161-9921-b658ce988bd1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/url-rewriting
ms.openlocfilehash: 05a92c4eee6b26e49831c11e1251aedba87ed717
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="url-rewriting-middleware-in-aspnet-core"></a><span data-ttu-id="c0f9f-104">URL の ASP.NET Core のミドルウェアの書き換え</span><span class="sxs-lookup"><span data-stu-id="c0f9f-104">URL Rewriting Middleware in ASP.NET Core</span></span>

<span data-ttu-id="c0f9f-105">によって[Luke Latham](https://github.com/GuardRex)と[Mikael メンギストゥ](https://github.com/mikaelm12)</span><span class="sxs-lookup"><span data-stu-id="c0f9f-105">By [Luke Latham](https://github.com/GuardRex) and [Mikael Mengistu](https://github.com/mikaelm12)</span></span>

[<span data-ttu-id="c0f9f-106">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="c0f9f-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)

<span data-ttu-id="c0f9f-107">URL の書き換えは 1 つまたは複数の事前定義された規則に基づいて、Url 要求を変更することです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-107">URL rewriting is the act of modifying request URLs based on one or more predefined rules.</span></span> <span data-ttu-id="c0f9f-108">URL 書き換え、場所とアドレスが緊密にリンクされていないように、リソースの場所とその住所間の抽象化を作成します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-108">URL rewriting creates an abstraction between resource locations and their addresses so that the locations and addresses are not tightly linked.</span></span> <span data-ttu-id="c0f9f-109">URL 書き換えが役に立ちますいくつかのシナリオはあります。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-109">There are several scenarios where URL rewriting is valuable:</span></span>
* <span data-ttu-id="c0f9f-110">移動またはそれらのリソースの安定したロケーターを維持しながら、サーバー リソースを一時的または永続的に交換</span><span class="sxs-lookup"><span data-stu-id="c0f9f-110">Moving or replacing server resources temporarily or permanently while maintaining stable locators for those resources</span></span>
* <span data-ttu-id="c0f9f-111">要求処理でアプリを 1 つの区分にわたってまたは別のアプリ間での分割</span><span class="sxs-lookup"><span data-stu-id="c0f9f-111">Splitting request processing across different apps or across areas of one app</span></span>
* <span data-ttu-id="c0f9f-112">削除、追加、または受信した要求の URL セグメントを再編成</span><span class="sxs-lookup"><span data-stu-id="c0f9f-112">Removing, adding, or reorganizing URL segments on incoming requests</span></span>
* <span data-ttu-id="c0f9f-113">検索エンジン最適化 (SEO) のパブリック Url を最適化します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-113">Optimizing public URLs for Search Engine Optimization (SEO)</span></span>
* <span data-ttu-id="c0f9f-114">次のリンクを検索できるが、コンテンツの予測を手助けするフレンドリなパブリック Url の使用を許可します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-114">Permitting the use of friendly public URLs to help people predict the content they will find by following a link</span></span>
* <span data-ttu-id="c0f9f-115">エンドポイントをセキュリティで保護するセキュリティ保護されていない要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-115">Redirecting insecure requests to secure endpoints</span></span>
* <span data-ttu-id="c0f9f-116">イメージ hotlinking の防止</span><span class="sxs-lookup"><span data-stu-id="c0f9f-116">Preventing image hotlinking</span></span>

<span data-ttu-id="c0f9f-117">いくつかの方法で URL を変更する regex、Apache mod_rewrite モジュール ルール、IIS の書き換え規則など、カスタムの規則のロジックを使用するための規則を定義できます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-117">You can define rules for changing the URL in several ways, including regex, Apache mod_rewrite module rules, IIS Rewrite Module rules, and using custom rule logic.</span></span> <span data-ttu-id="c0f9f-118">このドキュメントでは、ASP.NET Core アプリケーションで URL 書き換えミドルウェアを使用する方法について記載された URL の書き換えが導入されています。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-118">This document introduces URL rewriting with instructions on how to use URL Rewriting Middleware in ASP.NET Core apps.</span></span>

> [!NOTE]
> <span data-ttu-id="c0f9f-119">URL の書き換えと、アプリのパフォーマンスが低下することができます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-119">URL rewriting can reduce the performance of an app.</span></span> <span data-ttu-id="c0f9f-120">可能であれば、規則の複雑さと数を制限する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-120">Where feasible, you should limit the number and complexity of rules.</span></span>

## <a name="url-redirect-and-url-rewrite"></a><span data-ttu-id="c0f9f-121">URL リダイレクトと URL の書き換え</span><span class="sxs-lookup"><span data-stu-id="c0f9f-121">URL redirect and URL rewrite</span></span>
<span data-ttu-id="c0f9f-122">表現の間に違い*URL リダイレクト*と*URL 書き換え*に微妙なように見えますが最初のクライアントにリソースを提供するための重要な意味を含んでいます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-122">The difference in wording between *URL redirect* and *URL rewrite* may seem subtle at first but has important implications for providing resources to clients.</span></span> <span data-ttu-id="c0f9f-123">ASP.NET Core URL 書き換えミドルウェアは、両方のニーズを満たせるです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-123">ASP.NET Core's URL Rewriting Middleware is capable of meeting the need for both.</span></span>

<span data-ttu-id="c0f9f-124">A *URL リダイレクト*クライアント側の操作は、クライアントが別のアドレスにあるリソースにアクセスするよう指示します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-124">A *URL redirect* is a client-side operation, where the client is instructed to access a resource at another address.</span></span> <span data-ttu-id="c0f9f-125">サーバーへのラウンド トリップが必要ですされ、クライアントが、リソースに対する新しい要求を行うときに、ブラウザーのアドレス バーに、クライアントに返され、リダイレクト URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-125">This requires a round-trip to the server, and the redirect URL returned to the client appears in the browser's address bar when the client makes a new request for the resource.</span></span> <span data-ttu-id="c0f9f-126">場合`/resource`は*リダイレクト*に`/different-resource`、クライアントからの要求`/resource`、サーバーは、クライアントがリソースをリソースを取得する必要があります、応答と`/different-resource`リダイレクトがであることを示すステータス コードで一時的または永続的です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-126">If `/resource` is *redirected* to `/different-resource`, the client requests `/resource`, and the server responds that the client should obtain the resource at `/different-resource` with a status code indicating that the redirect is either temporary or permanent.</span></span> <span data-ttu-id="c0f9f-127">クライアントは、リダイレクト URL にあるリソースに対する新しい要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-127">The client executes a new request for the resource at the redirect URL.</span></span>

![WebAPI サービス エンドポイントは、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から一時的に変更されましたが。](url-rewriting/_static/url_redirect.png)

<span data-ttu-id="c0f9f-133">要求を別の URL にリダイレクトする場合は、リダイレクトが永続的または一時的であるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-133">When redirecting requests to a different URL, you indicate whether the redirect is permanent or temporary.</span></span> <span data-ttu-id="c0f9f-134">301 (完全な移動) 状態コードには、リソースのすべての要求が新しい URL を使用する必要がありますをクライアントに指示する、新しい永続的な URL は、リソースは、するは使用されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-134">The 301 (Moved Permanently) status code is used where the resource has a new, permanent URL and you wish to instruct the client that all future requests for the resource should use the new URL.</span></span> <span data-ttu-id="c0f9f-135">*301 ステータス コードを受信すると、クライアントは応答をキャッシュ可能性があります。*</span><span class="sxs-lookup"><span data-stu-id="c0f9f-135">*The client may cache the response when a 301 status code is received.*</span></span> <span data-ttu-id="c0f9f-136">302 (検出) ステータス コードを使用、リダイレクトが一時的または一般にサブジェクトが変更するべきではありません、クライアントの格納し、リダイレクト URL を後で再になるようにします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-136">The 302 (Found) status code is used where the redirection is temporary or generally subject to change, such that the client shouldn't store and reuse the redirect URL in the future.</span></span> <span data-ttu-id="c0f9f-137">詳細については、次を参照してください。 [RFC 2616: 状態コード定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-137">For more information, see [RFC 2616: Status Code Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

<span data-ttu-id="c0f9f-138">A *URL 書き換え*はサーバー側の操作を別のリソースのアドレスからのリソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-138">A *URL rewrite* is a server-side operation to provide a resource from a different resource address.</span></span> <span data-ttu-id="c0f9f-139">URL の書き換えと、サーバーへのラウンド トリップが必要としません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-139">Rewriting a URL doesn't require a round-trip to the server.</span></span> <span data-ttu-id="c0f9f-140">書き換えられた URL では、クライアントに返されません。 と、ブラウザーのアドレス バーに表示されません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-140">The rewritten URL isn't returned to the client and won't appear in the browser's address bar.</span></span> <span data-ttu-id="c0f9f-141">ときに`/resource`は*書き直す*に`/different-resource`、クライアントからの要求`/resource`とサーバー*内部的に*にあるリソースをフェッチ`/different-resource`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-141">When `/resource` is *rewritten* to `/different-resource`, the client requests `/resource`, and the server *internally* fetches the resource at `/different-resource`.</span></span> <span data-ttu-id="c0f9f-142">クライアントは、書き換えられた URL にあるリソースを取得できる場合がありますがその要求を行う応答を受信して、ときに、書き換えられた URL にリソースが存在するクライアントが通知されません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-142">Although the client might be able to retrieve the resource at the rewritten URL, the client won't be informed that the resource exists at the rewritten URL when it makes its request and receives the response.</span></span>

![WebAPI サービス エンドポイントが、サーバー上のバージョン 2 (v2) をバージョン 1 (v1) から変更されました。](url-rewriting/_static/url_rewrite.png)

## <a name="url-rewriting-sample-app"></a><span data-ttu-id="c0f9f-147">URL 書き換えのサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="c0f9f-147">URL rewriting sample app</span></span>
<span data-ttu-id="c0f9f-148">URL 書き換えミドルウェアの機能を調べることができます、 [URL 書き換えのサンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-148">You can explore the features of the URL Rewriting Middleware with the [URL rewriting sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/).</span></span> <span data-ttu-id="c0f9f-149">アプリが書き換えを適用し、リダイレクト ルールし、書き換えられたかリダイレクトされる URL を表示します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-149">The app applies rewrite and redirect rules and shows the rewritten or redirected URL.</span></span>

## <a name="when-to-use-url-rewriting-middleware"></a><span data-ttu-id="c0f9f-150">URL 書き換えミドルウェアを使用する場合</span><span class="sxs-lookup"><span data-stu-id="c0f9f-150">When to use URL Rewriting Middleware</span></span>
<span data-ttu-id="c0f9f-151">使用できない場合は、URL 書き換えミドルウェアを使用して、 [URL Rewrite モジュール](https://www.iis.net/downloads/microsoft/url-rewrite)Windows Server 上の IIS で、 [Apache mod_rewrite モジュール](https://httpd.apache.org/docs/2.4/rewrite/)Apache サーバーで、 [NginxでURLの書き換え](https://www.nginx.com/blog/creating-nginx-rewrite-rules/)でホストされているアプリまたは[HTTP.sys サーバー](xref:fundamentals/servers/httpsys) (旧称[WebListener](xref:fundamentals/servers/weblistener))。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-151">Use URL Rewriting Middleware when you are unable to use the [URL Rewrite module](https://www.iis.net/downloads/microsoft/url-rewrite) with IIS on Windows Server, the [Apache mod_rewrite module](https://httpd.apache.org/docs/2.4/rewrite/) on Apache Server, [URL rewriting on Nginx](https://www.nginx.com/blog/creating-nginx-rewrite-rules/), or your app is hosted on [HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="c0f9f-152">テクノロジを使用して、サーバー ベース URL 書き換え IIS、Apache、または Nginx の主な理由ではこれらのモジュールの全機能をサポートしていないミドルウェア ミドルウェアのパフォーマンス可能性とも一致しません、モジュールの。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-152">The main reasons to use the server-based URL rewriting technologies in IIS, Apache, or Nginx are that the middleware doesn't support the full features of these modules and the performance of the middleware probably won't match that of the modules.</span></span> <span data-ttu-id="c0f9f-153">ただし、ASP.NET Core プロジェクトなどを操作しないサーバー モジュールの一部の機能がある、`IsFile`と`IsDirectory`IIS Rewrite モジュールの制約。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-153">However, there are some features of the server modules that don't work with ASP.NET Core projects, such as the `IsFile` and `IsDirectory` constraints of the IIS Rewrite module.</span></span> <span data-ttu-id="c0f9f-154">これらのシナリオでは、代わりに、ミドルウェアを使用します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-154">In these scenarios, use the middleware instead.</span></span>

## <a name="package"></a><span data-ttu-id="c0f9f-155">Package</span><span class="sxs-lookup"><span data-stu-id="c0f9f-155">Package</span></span>
<span data-ttu-id="c0f9f-156">ミドルウェアをプロジェクトに含めるへの参照を追加、 [ `Microsoft.AspNetCore.Rewrite` ](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-156">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.Rewrite`](https://www.nuget.org/packages/Microsoft.AspNetCore.Rewrite/) package.</span></span> <span data-ttu-id="c0f9f-157">この機能は、ASP.NET Core 1.1 を対象とするアプリの使用可能な以降です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-157">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="extension-and-options"></a><span data-ttu-id="c0f9f-158">拡張機能とオプション</span><span class="sxs-lookup"><span data-stu-id="c0f9f-158">Extension and options</span></span>
<span data-ttu-id="c0f9f-159">URL 書き換えを確立しのインスタンスを作成することでルールをリダイレクト、`RewriteOptions`クラスでは、ルールの各拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-159">Establish your URL rewrite and redirect rules by creating an instance of the `RewriteOptions` class with extension methods for each of your rules.</span></span> <span data-ttu-id="c0f9f-160">ようにした場合に処理された順序で複数のルールを連結します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-160">Chain multiple rules in the order that you would like them processed.</span></span> <span data-ttu-id="c0f9f-161">`RewriteOptions`と要求パイプラインに追加されるように、URL 書き換えミドルウェアに渡される`app.UseRewriter(options);`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-161">The `RewriteOptions` are passed into the URL Rewriting Middleware as it's added to the request pipeline with `app.UseRewriter(options);`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-163">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-163">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-164">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-164">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-165">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-165">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1)]</span></span>

---

### <a name="url-redirect"></a><span data-ttu-id="c0f9f-166">URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="c0f9f-166">URL redirect</span></span>
<span data-ttu-id="c0f9f-167">使用して`AddRedirect`要求をリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-167">Use `AddRedirect` to redirect requests.</span></span> <span data-ttu-id="c0f9f-168">最初のパラメーターには、着信 URL のパスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-168">The first parameter contains your regex for matching on the path of the incoming URL.</span></span> <span data-ttu-id="c0f9f-169">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-169">The second parameter is the replacement string.</span></span> <span data-ttu-id="c0f9f-170">3 番目のパラメーターでは、存在する場合は、ステータス コードを指定します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-170">The third parameter, if present, specifies the status code.</span></span> <span data-ttu-id="c0f9f-171">ステータス コードを指定しない場合、既定値 302 (検出)、リソースが一時的に移動または置き換えることを示すです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-171">If you don't specify the status code, it defaults to 302 (Found), which indicates that the resource is temporarily moved or replaced.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-173">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-173">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=5)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-175">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-175">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=2)]</span></span>

---

<span data-ttu-id="c0f9f-176">ブラウザーで有効になっている開発者ツールで、パスのサンプル アプリに要求を行う`/redirect-rule/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-176">In a browser with developer tools enabled, make a request to the sample app with the path `/redirect-rule/1234/5678`.</span></span> <span data-ttu-id="c0f9f-177">正規表現の要求パスに一致する`redirect-rule/(.*)`、パスが置き換え`/redirected/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-177">The regex matches the request path on `redirect-rule/(.*)`, and the path is replaced with `/redirected/1234/5678`.</span></span> <span data-ttu-id="c0f9f-178">リダイレクト URL は 302 (検出) のステータス コードでクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-178">The redirect URL is sent back to the client with a 302 (Found) status code.</span></span> <span data-ttu-id="c0f9f-179">ブラウザーでは、新しい要求をブラウザーのアドレス バーに表示されるリダイレクト URL でです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-179">The browser makes a new request at the redirect URL, which appears in the browser's address bar.</span></span> <span data-ttu-id="c0f9f-180">2 番目の要求は、アプリから 200 (OK) 応答を受信し、リダイレクト URL のサンプル アプリの規則が一致しないため、応答の本体はリダイレクト URL が表示できます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-180">Since no rules in the sample app match on the redirect URL, the second request receives a 200 (OK) response from the app and the body of the response shows the redirect URL.</span></span> <span data-ttu-id="c0f9f-181">URL がサーバーとのやり取りが行われた*リダイレクト*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-181">A roundtrip is made to the server when a URL is *redirected*.</span></span>

> [!WARNING]
> <span data-ttu-id="c0f9f-182">リダイレクト ルールを確立するときに注意します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-182">Be cautious when establishing your redirect rules.</span></span> <span data-ttu-id="c0f9f-183">リダイレクト後など、アプリへの各要求では、リダイレクト ルールが評価されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-183">Your redirect rules are evaluated on each request to the app, including after a redirect.</span></span> <span data-ttu-id="c0f9f-184">簡単に誤って無限のリダイレクトのループを作成します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-184">It's easy to accidently create a loop of infinite redirects.</span></span>

<span data-ttu-id="c0f9f-185">元の要求:`/redirect-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-185">Original Request: `/redirect-rule/1234/5678`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect.png)

<span data-ttu-id="c0f9f-187">かっこ内に含まれる式の一部と呼ばれる、*キャプチャ グループ*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-187">The part of the expression contained within parentheses is called a *capture group*.</span></span> <span data-ttu-id="c0f9f-188">ドット (`.`) 式の意味*任意の文字を一致*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-188">The dot (`.`) of the expression means *match any character*.</span></span> <span data-ttu-id="c0f9f-189">アスタリスク (`*`) を示す*直前の文字に 0 回以上一致*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-189">The asterisk (`*`) indicates *match the preceding character zero or more times*.</span></span> <span data-ttu-id="c0f9f-190">URL の最後の 2 つのパス セグメントではそのため、 `1234/5678`、キャプチャ グループによってキャプチャされた`(.*)`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-190">Therefore, the last two path segments of the URL, `1234/5678`, are captured by capture group `(.*)`.</span></span> <span data-ttu-id="c0f9f-191">任意の値の後に、要求 URL で指定する`redirect-rule/`はこの単一のキャプチャ グループによってキャプチャされます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-191">Any value you provide in the request URL after `redirect-rule/` is captured by this single capture group.</span></span>

<span data-ttu-id="c0f9f-192">置換文字列にキャプチャされたグループが挿入され、ドル記号の文字列 (`$`) に続けてのキャプチャのシーケンス番号。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-192">In the replacement string, captured groups are injected into the string with the dollar sign (`$`) followed by the sequence number of the capture.</span></span> <span data-ttu-id="c0f9f-193">最初のキャプチャ グループ値を取得`$1`、2 番目は`$2`の正規表現内のキャプチャ グループのシーケンスで続行されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-193">The first capture group value is obtained with `$1`, the second with `$2`, and they continue in sequence for the capture groups in your regex.</span></span> <span data-ttu-id="c0f9f-194">1 つだけのキャプチャ グループにあるリダイレクト ルール regex サンプル アプリでは置換文字列に 1 つだけの挿入されたグループがあるため`$1`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-194">There's only one captured group in the redirect rule regex in the sample app, so there's only one injected group in the replacement string, which is `$1`.</span></span> <span data-ttu-id="c0f9f-195">ルールが適用されるときに、URL は次のようになります。`/redirected/1234/5678`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-195">When the rule is applied, the URL becomes `/redirected/1234/5678`.</span></span>

<a name=url-redirect-to-secure-endpoint></a>
### <a name="url-redirect-to-a-secure-endpoint"></a><span data-ttu-id="c0f9f-196">セキュリティで保護されたエンドポイントへの URL リダイレクト</span><span class="sxs-lookup"><span data-stu-id="c0f9f-196">URL redirect to a secure endpoint</span></span>
<span data-ttu-id="c0f9f-197">使用して`AddRedirectToHttps`HTTP 要求を同じホストと HTTPS を使用して、パスにリダイレクト (`https://`)。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-197">Use `AddRedirectToHttps` to redirect HTTP requests to the same host and path using HTTPS (`https://`).</span></span> <span data-ttu-id="c0f9f-198">ステータス コードが指定されていない場合、ミドルウェアを 302 (検出) に既定値です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-198">If the status code isn't supplied, the middleware defaults to 302 (Found).</span></span> <span data-ttu-id="c0f9f-199">ポートが指定されていない場合は、ミドルウェア既定`null`、つまり、プロトコルに変更`https://`クライアント ポート 443 上のリソースにアクセスするとします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-199">If the port isn't supplied, the middleware defaults to `null`, which means the protocol changes to `https://` and the client accesses the resource on port 443.</span></span> <span data-ttu-id="c0f9f-200">この例では、301 (完全な移動) に、状態コードを設定し、5001 にポートを変更する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-200">The example shows how to set the status code to 301 (Moved Permanently) and change the port to 5001.</span></span>
```csharp
var options = new RewriteOptions()
    .AddRedirectToHttps(301, 5001);

app.UseRewriter(options);
```
<span data-ttu-id="c0f9f-201">使用して`AddRedirectToHttpsPermanent`同じホストのパスとセキュリティで保護された HTTPS プロトコルを使用するセキュリティ保護されていない要求をリダイレクトする (`https://`ポート 443)。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-201">Use `AddRedirectToHttpsPermanent` to redirect insecure requests to the same host and path with secure HTTPS protocol (`https://` on port 443).</span></span> <span data-ttu-id="c0f9f-202">ミドルウェアは、ステータス コードを 301 (完全な移動) に設定します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-202">The middleware sets the status code to 301 (Moved Permanently).</span></span>

<span data-ttu-id="c0f9f-203">サンプル アプリが使用する方法を示す可能`AddRedirectToHttps`または`AddRedirectToHttpsPermanent`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-203">The sample app is capable of demonstrating how to use `AddRedirectToHttps` or `AddRedirectToHttpsPermanent`.</span></span> <span data-ttu-id="c0f9f-204">拡張メソッドを追加、`RewriteOptions`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-204">Add the extension method to the `RewriteOptions`.</span></span> <span data-ttu-id="c0f9f-205">任意の URL にアプリをセキュリティ保護されていない要求を行います。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-205">Make an insecure request to the app at any URL.</span></span> <span data-ttu-id="c0f9f-206">自己署名証明書が信頼できることを警告するブラウザーのセキュリティを消去します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-206">Dismiss the browser security warning that the self-signed certificate is untrusted.</span></span>

<span data-ttu-id="c0f9f-207">元の要求を使用して`AddRedirectToHttps(301, 5001)`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-207">Original Request using `AddRedirectToHttps(301, 5001)`: `/secure`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https.png)

<span data-ttu-id="c0f9f-209">元の要求を使用して`AddRedirectToHttpsPermanent`:`/secure`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-209">Original Request using `AddRedirectToHttpsPermanent`: `/secure`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_redirect_to_https_permanent.png)

### <a name="url-rewrite"></a><span data-ttu-id="c0f9f-211">URL 書き換え</span><span class="sxs-lookup"><span data-stu-id="c0f9f-211">URL rewrite</span></span>
<span data-ttu-id="c0f9f-212">使用して`AddRewrite`Url の書き換えの規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-212">Use `AddRewrite` to create a rules for rewriting URLs.</span></span> <span data-ttu-id="c0f9f-213">最初のパラメーターには、着信 URL パスに一致する正規表現が含まれています。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-213">The first parameter contains your regex for matching on the incoming URL path.</span></span> <span data-ttu-id="c0f9f-214">2 番目のパラメーターは、置換文字列です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-214">The second parameter is the replacement string.</span></span> <span data-ttu-id="c0f9f-215">3 番目のパラメーターでは、`skipRemainingRules: {true|false}`ミドルウェアに示す現在のルールが適用されている場合は、追加の書き換えルールをスキップするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-215">The third parameter, `skipRemainingRules: {true|false}`, indicates to the middleware whether or not to skip additional rewrite rules if the current rule is applied.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-217">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-217">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=6)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-219">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-219">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=3)]</span></span>

---

<span data-ttu-id="c0f9f-220">元の要求:`/rewrite-rule/1234/5678`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-220">Original Request: `/rewrite-rule/1234/5678`</span></span>

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_rewrite.png)

<span data-ttu-id="c0f9f-222">最初、正規表現で注目するは、キャレット (`^`) 式の先頭にします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-222">The first thing you notice in the regex is the carat (`^`) at the beginning of the expression.</span></span> <span data-ttu-id="c0f9f-223">これは、URL パスの先頭で一致が開始されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-223">This means that matching starts at the beginning of the URL path.</span></span>

<span data-ttu-id="c0f9f-224">リダイレクト ルールでは、先ほどの例で`redirect-rule/(.*)`、正規表現の先頭のキャレットはありません。 したがって、任意の文字の前可能性があります`redirect-rule/`に成功した一致のパスにします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-224">In the earlier example with the redirect rule, `redirect-rule/(.*)`, there's no carat at the start of the regex; therefore, any characters may precede `redirect-rule/` in the path for a successful match.</span></span>

| <span data-ttu-id="c0f9f-225">パス</span><span class="sxs-lookup"><span data-stu-id="c0f9f-225">Path</span></span>                               | <span data-ttu-id="c0f9f-226">一致したもの</span><span class="sxs-lookup"><span data-stu-id="c0f9f-226">Match</span></span> |
| ---------------------------------- | :---: |
| `/redirect-rule/1234/5678`         | <span data-ttu-id="c0f9f-227">はい</span><span class="sxs-lookup"><span data-stu-id="c0f9f-227">Yes</span></span>   |
| `/my-cool-redirect-rule/1234/5678` | <span data-ttu-id="c0f9f-228">はい</span><span class="sxs-lookup"><span data-stu-id="c0f9f-228">Yes</span></span>   |
| `/anotherredirect-rule/1234/5678`  | <span data-ttu-id="c0f9f-229">はい</span><span class="sxs-lookup"><span data-stu-id="c0f9f-229">Yes</span></span>   |

<span data-ttu-id="c0f9f-230">書き換えルール`^rewrite-rule/(\d+)/(\d+)`、のみで開始される場合、[パスと一致する`rewrite-rule/`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-230">The rewrite rule, `^rewrite-rule/(\d+)/(\d+)`, only matches paths if they start with `rewrite-rule/`.</span></span> <span data-ttu-id="c0f9f-231">書き換えルールの下と上のリダイレクト ルール間の一致の違いに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-231">Notice the difference in matching between the rewrite rule below and the redirect rule above.</span></span>

| <span data-ttu-id="c0f9f-232">パス</span><span class="sxs-lookup"><span data-stu-id="c0f9f-232">Path</span></span>                              | <span data-ttu-id="c0f9f-233">一致したもの</span><span class="sxs-lookup"><span data-stu-id="c0f9f-233">Match</span></span> |
| --------------------------------- | :---: |
| `/rewrite-rule/1234/5678`         | <span data-ttu-id="c0f9f-234">はい</span><span class="sxs-lookup"><span data-stu-id="c0f9f-234">Yes</span></span>   |
| `/my-cool-rewrite-rule/1234/5678` | <span data-ttu-id="c0f9f-235">いいえ</span><span class="sxs-lookup"><span data-stu-id="c0f9f-235">No</span></span>    |
| `/anotherrewrite-rule/1234/5678`  | <span data-ttu-id="c0f9f-236">いいえ</span><span class="sxs-lookup"><span data-stu-id="c0f9f-236">No</span></span>    |

<span data-ttu-id="c0f9f-237">次の`^rewrite-rule/`部分式の 2 つのキャプチャ グループが`(\d+)/(\d+)`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-237">Following the `^rewrite-rule/` portion of the expression, there are two capture groups, `(\d+)/(\d+)`.</span></span> <span data-ttu-id="c0f9f-238">`\d`ことを示します*digit (数) を一致*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-238">The `\d` signifies *match a digit (number)*.</span></span> <span data-ttu-id="c0f9f-239">プラス記号 (`+`) ことを意味*直前の文字の 1 つ以上の一致*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-239">The plus sign (`+`) means *match one or more of the preceding character*.</span></span> <span data-ttu-id="c0f9f-240">そのため、URL では、スラッシュを他の数値の後に続く数値を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-240">Therefore, the URL must contain a number followed by a forward-slash followed by another number.</span></span> <span data-ttu-id="c0f9f-241">これらのキャプチャ グループが挿入され、書き換えられた URL として`$1`と`$2`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-241">These capture groups are injected into the rewritten URL as `$1` and `$2`.</span></span> <span data-ttu-id="c0f9f-242">書き換えルールの置換文字列は、クエリ文字列に、キャプチャされたグループを配置します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-242">The rewrite rule replacement string places the captured groups into the querystring.</span></span> <span data-ttu-id="c0f9f-243">要求されたパスの`/rewrite-rule/1234/5678`にあるリソースを取得する書き直す`/rewritten?var1=1234&var2=5678`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-243">The requested path of `/rewrite-rule/1234/5678` is rewritten to obtain the resource at `/rewritten?var1=1234&var2=5678`.</span></span> <span data-ttu-id="c0f9f-244">クエリ文字列が元の要求に存在する場合、URL は再作成時に保持します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-244">If a querystring is present on the original request, it's preserved when the URL is rewritten.</span></span>

<span data-ttu-id="c0f9f-245">リソースを取得するためにサーバーとのやり取りはありません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-245">There's no roundtrip to the server to obtain the resource.</span></span> <span data-ttu-id="c0f9f-246">リソースが存在する場合は、フェッチされ、200 (OK) ステータス コードをクライアントに返されることがされます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-246">If the resource exists, it's fetched and returned to the client with a 200 (OK) status code.</span></span> <span data-ttu-id="c0f9f-247">クライアントがリダイレクトされていないために、ブラウザーのアドレス バーの URL は変更されません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-247">Because the client isn't redirected, the URL in the browser address bar doesn't change.</span></span> <span data-ttu-id="c0f9f-248">クライアントに関しては、限りことはありません、URL 書き換え操作が発生しました。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-248">As far as the client is concerned, the URL rewrite operation never occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="c0f9f-249">使用して`skipRemainingRules: true`可能な限り、コストのかかるプロセスは、アプリの応答時間を短縮の規則に一致するためです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-249">Use `skipRemainingRules: true` whenever possible, because matching rules is an expensive process and reduces app response time.</span></span> <span data-ttu-id="c0f9f-250">最も高速のアプリの応答。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-250">For the fastest app response:</span></span>
> * <span data-ttu-id="c0f9f-251">最もよく一致するルールを最も頻繁に一致したルールから、書き換えルールを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-251">Order your rewrite rules from the most frequently matched rule to the least frequently matched rule.</span></span>
> * <span data-ttu-id="c0f9f-252">一致が発生し、その他のルールの処理は必要ありません、残りのルールの処理をスキップします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-252">Skip the processing of the remaining rules when a match occurs and no additional rule processing is required.</span></span>

### <a name="apache-modrewrite"></a><span data-ttu-id="c0f9f-253">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="c0f9f-253">Apache mod_rewrite</span></span>
<span data-ttu-id="c0f9f-254">Apache mod_rewrite ルールを適用`AddApacheModRewrite`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-254">Apply Apache mod_rewrite rules with `AddApacheModRewrite`.</span></span> <span data-ttu-id="c0f9f-255">アプリの規則ファイルが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-255">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="c0f9f-256">詳細と mod_rewrite 規則の例については、次を参照してください。 [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-256">For more information and examples of mod_rewrite rules, see [Apache mod_rewrite](https://httpd.apache.org/docs/2.4/rewrite/).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-257">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-257">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-258">A`StreamReader`からルールの読み取りに使用される、 *ApacheModRewrite.txt*ルール ファイルです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-258">A `StreamReader` is used to read the rules from the *ApacheModRewrite.txt* rules file.</span></span>

<span data-ttu-id="c0f9f-259">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-259">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=1,7)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-260">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-260">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-261">最初のパラメーターには、 `IFileProvider`、経由で提供されています[依存性の注入](dependency-injection.md)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-261">The first parameter takes an `IFileProvider`, which is provided via [Dependency Injection](dependency-injection.md).</span></span> <span data-ttu-id="c0f9f-262">`IHostingEnvironment`を提供する挿入、`ContentRootFileProvider`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-262">The `IHostingEnvironment` is injected to provide the `ContentRootFileProvider`.</span></span> <span data-ttu-id="c0f9f-263">2 番目のパラメーターは、ファイルへのパスの規則、これは*ApacheModRewrite.txt*サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-263">The second parameter is the path to your rules file, which is *ApacheModRewrite.txt* in the sample app.</span></span>

<span data-ttu-id="c0f9f-264">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-264">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=4)]</span></span>

---

<span data-ttu-id="c0f9f-265">サンプル アプリからの要求をリダイレクトする`/apache-mod-rules-redirect/(.\*)`に`/redirected?id=$1`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-265">The sample app redirects requests from `/apache-mod-rules-redirect/(.\*)` to `/redirected?id=$1`.</span></span> <span data-ttu-id="c0f9f-266">応答状態コードは、302 (検出) です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-266">The response status code is 302 (Found).</span></span>

[!code[Main](url-rewriting/samples/2.x/ApacheModRewrite.txt)]

<span data-ttu-id="c0f9f-267">元の要求:`/apache-mod-rules-redirect/1234`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-267">Original Request: `/apache-mod-rules-redirect/1234`</span></span>

![ブラウザー ウィンドウで、要求および応答を追跡開発者ツール](url-rewriting/_static/add_apache_mod_redirect.png)

##### <a name="supported-server-variables"></a><span data-ttu-id="c0f9f-269">サポートされているサーバー変数</span><span class="sxs-lookup"><span data-stu-id="c0f9f-269">Supported server variables</span></span>
<span data-ttu-id="c0f9f-270">ミドルウェアは、次の Apache mod_rewrite サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-270">The middleware supports the following Apache mod_rewrite server variables:</span></span>
* <span data-ttu-id="c0f9f-271">CONN_REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-271">CONN_REMOTE_ADDR</span></span>
* <span data-ttu-id="c0f9f-272">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-272">HTTP_ACCEPT</span></span>
* <span data-ttu-id="c0f9f-273">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="c0f9f-273">HTTP_CONNECTION</span></span>
* <span data-ttu-id="c0f9f-274">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="c0f9f-274">HTTP_COOKIE</span></span>
* <span data-ttu-id="c0f9f-275">HTTP_FORWARDED</span><span class="sxs-lookup"><span data-stu-id="c0f9f-275">HTTP_FORWARDED</span></span>
* <span data-ttu-id="c0f9f-276">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="c0f9f-276">HTTP_HOST</span></span>
* <span data-ttu-id="c0f9f-277">ボタン</span><span class="sxs-lookup"><span data-stu-id="c0f9f-277">HTTP_REFERER</span></span>
* <span data-ttu-id="c0f9f-278">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-278">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="c0f9f-279">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c0f9f-279">HTTPS</span></span>
* <span data-ttu-id="c0f9f-280">IPV6</span><span class="sxs-lookup"><span data-stu-id="c0f9f-280">IPV6</span></span>
* <span data-ttu-id="c0f9f-281">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="c0f9f-281">QUERY_STRING</span></span>
* <span data-ttu-id="c0f9f-282">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-282">REMOTE_ADDR</span></span>
* <span data-ttu-id="c0f9f-283">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-283">REMOTE_PORT</span></span>
* <span data-ttu-id="c0f9f-284">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c0f9f-284">REQUEST_FILENAME</span></span>
* <span data-ttu-id="c0f9f-285">REQUEST_METHOD</span><span class="sxs-lookup"><span data-stu-id="c0f9f-285">REQUEST_METHOD</span></span>
* <span data-ttu-id="c0f9f-286">REQUEST_SCHEME</span><span class="sxs-lookup"><span data-stu-id="c0f9f-286">REQUEST_SCHEME</span></span>
* <span data-ttu-id="c0f9f-287">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="c0f9f-287">REQUEST_URI</span></span>
* <span data-ttu-id="c0f9f-288">SCRIPT_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c0f9f-288">SCRIPT_FILENAME</span></span>
* <span data-ttu-id="c0f9f-289">SERVER_ADDR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-289">SERVER_ADDR</span></span>
* <span data-ttu-id="c0f9f-290">これらの用語</span><span class="sxs-lookup"><span data-stu-id="c0f9f-290">SERVER_PORT</span></span>
* <span data-ttu-id="c0f9f-291">SERVER_PROTOCOL</span><span class="sxs-lookup"><span data-stu-id="c0f9f-291">SERVER_PROTOCOL</span></span>
* <span data-ttu-id="c0f9f-292">時間</span><span class="sxs-lookup"><span data-stu-id="c0f9f-292">TIME</span></span>
* <span data-ttu-id="c0f9f-293">TIME_DAY</span><span class="sxs-lookup"><span data-stu-id="c0f9f-293">TIME_DAY</span></span>
* <span data-ttu-id="c0f9f-294">TIME_HOUR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-294">TIME_HOUR</span></span>
* <span data-ttu-id="c0f9f-295">TIME_MIN</span><span class="sxs-lookup"><span data-stu-id="c0f9f-295">TIME_MIN</span></span>
* <span data-ttu-id="c0f9f-296">TIME_MON</span><span class="sxs-lookup"><span data-stu-id="c0f9f-296">TIME_MON</span></span>
* <span data-ttu-id="c0f9f-297">TIME_SEC</span><span class="sxs-lookup"><span data-stu-id="c0f9f-297">TIME_SEC</span></span>
* <span data-ttu-id="c0f9f-298">TIME_WDAY</span><span class="sxs-lookup"><span data-stu-id="c0f9f-298">TIME_WDAY</span></span>
* <span data-ttu-id="c0f9f-299">TIME_YEAR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-299">TIME_YEAR</span></span>

### <a name="iis-url-rewrite-module-rules"></a><span data-ttu-id="c0f9f-300">IIS URL Rewrite モジュール ルール</span><span class="sxs-lookup"><span data-stu-id="c0f9f-300">IIS URL Rewrite Module rules</span></span>
<span data-ttu-id="c0f9f-301">IIS URL Rewrite モジュールに適用される規則を使用する`AddIISUrlRewrite`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-301">To use rules that apply to the IIS URL Rewrite Module, use `AddIISUrlRewrite`.</span></span> <span data-ttu-id="c0f9f-302">アプリの規則ファイルが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-302">Make sure that the rules file is deployed with the app.</span></span> <span data-ttu-id="c0f9f-303">使用するミドルウェアを直接しない、 *web.config*ファイルの Windows Server IIS で実行されているときにします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-303">Don't direct the middleware to use your *web.config* file when running on Windows Server IIS.</span></span> <span data-ttu-id="c0f9f-304">外部で IIS にこれらの規則を保存するか、 *web.config* IIS Rewrite モジュールと競合しないようにします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-304">With IIS, these rules should be stored outside of your *web.config* to avoid conflicts with the IIS Rewrite module.</span></span> <span data-ttu-id="c0f9f-305">詳細と IIS URL Rewrite モジュールの規則の例については、次を参照してください。 [Url Rewrite モジュール 2.0 の使用](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)と[URL 書き換えモジュール構成の参照](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-305">For more information and examples of IIS URL Rewrite Module rules, see [Using Url Rewrite Module 2.0](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20) and [URL Rewrite Module Configuration Reference](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-306">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-306">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-307">A`StreamReader`からルールの読み取りに使用される、 *IISUrlRewrite.xml*ルール ファイルです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-307">A `StreamReader` is used to read the rules from the *IISUrlRewrite.xml* rules file.</span></span>

<span data-ttu-id="c0f9f-308">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-308">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=2,8)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-310">最初のパラメーターには、 `IFileProvider`、2 番目のパラメーターはこれは、XML ルール ファイルのパスを*IISUrlRewrite.xml*サンプル アプリでします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-310">The first parameter takes an `IFileProvider`, while the second parameter is the path to your XML rules file, which is *IISUrlRewrite.xml* in the sample app.</span></span>

<span data-ttu-id="c0f9f-311">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-311">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=5)]</span></span>

---

<span data-ttu-id="c0f9f-312">サンプル アプリからの要求をリライト`/iis-rules-rewrite/(.*)`に`/rewritten?id=$1`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-312">The sample app rewrites requests from `/iis-rules-rewrite/(.*)` to `/rewritten?id=$1`.</span></span> <span data-ttu-id="c0f9f-313">応答は、200 (OK) ステータス コードでクライアントに送信されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-313">The response is sent to the client with a 200 (OK) status code.</span></span>

<span data-ttu-id="c0f9f-314">[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-314">[!code-xml[Main](url-rewriting/samples/2.x/IISUrlRewrite.xml)]</span></span>

<span data-ttu-id="c0f9f-315">元の要求:`/iis-rules-rewrite/1234`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-315">Original Request: `/iis-rules-rewrite/1234`</span></span>

![ブラウザー ウィンドウで、要求と応答を追跡開発者ツール](url-rewriting/_static/add_iis_url_rewrite.png)

<span data-ttu-id="c0f9f-317">、望ましくない方法でアプリに影響するように構成されたサーバー レベル ルールにアクティブな IIS Rewrite モジュールがある場合は、アプリの IIS Rewrite モジュールを無効にできます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-317">If you have an active IIS Rewrite Module with server-level rules configured that would impact your app in undesirable ways, you can disable the IIS Rewrite Module for an app.</span></span> <span data-ttu-id="c0f9f-318">詳細については、次を参照してください。[を無効にする IIS モジュール](xref:hosting/iis-modules#disabling-iis-modules)です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-318">For more information, see [Disabling IIS modules](xref:hosting/iis-modules#disabling-iis-modules).</span></span>

#### <a name="unsupported-features"></a><span data-ttu-id="c0f9f-319">サポートされていない機能</span><span class="sxs-lookup"><span data-stu-id="c0f9f-319">Unsupported features</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-320">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-320">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-321">リリース ミドルウェアと ASP.NET Core 2.x IIS URL Rewrite モジュール機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-321">The middleware released with ASP.NET Core 2.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="c0f9f-322">送信の規則</span><span class="sxs-lookup"><span data-stu-id="c0f9f-322">Outbound Rules</span></span>
* <span data-ttu-id="c0f9f-323">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="c0f9f-323">Custom Server Variables</span></span>
* <span data-ttu-id="c0f9f-324">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="c0f9f-324">Wildcards</span></span>
* <span data-ttu-id="c0f9f-325">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="c0f9f-325">LogRewrittenUrl</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-326">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-326">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-327">リリース ミドルウェアと ASP.NET Core 1.x IIS URL Rewrite モジュール機能をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-327">The middleware released with ASP.NET Core 1.x doesn't support the following IIS URL Rewrite Module features:</span></span>
* <span data-ttu-id="c0f9f-328">グローバル ルール</span><span class="sxs-lookup"><span data-stu-id="c0f9f-328">Global Rules</span></span>
* <span data-ttu-id="c0f9f-329">送信の規則</span><span class="sxs-lookup"><span data-stu-id="c0f9f-329">Outbound Rules</span></span>
* <span data-ttu-id="c0f9f-330">マップを書き直してください。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-330">Rewrite Maps</span></span>
* <span data-ttu-id="c0f9f-331">CustomResponse アクション</span><span class="sxs-lookup"><span data-stu-id="c0f9f-331">CustomResponse Action</span></span>
* <span data-ttu-id="c0f9f-332">カスタム サーバー変数</span><span class="sxs-lookup"><span data-stu-id="c0f9f-332">Custom Server Variables</span></span>
* <span data-ttu-id="c0f9f-333">ワイルドカード</span><span class="sxs-lookup"><span data-stu-id="c0f9f-333">Wildcards</span></span>
* <span data-ttu-id="c0f9f-334">アクション: CustomResponse</span><span class="sxs-lookup"><span data-stu-id="c0f9f-334">Action:CustomResponse</span></span>
* <span data-ttu-id="c0f9f-335">LogRewrittenUrl</span><span class="sxs-lookup"><span data-stu-id="c0f9f-335">LogRewrittenUrl</span></span>

---

#### <a name="supported-server-variables"></a><span data-ttu-id="c0f9f-336">サポートされているサーバー変数</span><span class="sxs-lookup"><span data-stu-id="c0f9f-336">Supported server variables</span></span>
<span data-ttu-id="c0f9f-337">ミドルウェアは、次の URL Rewrite モジュールを IIS サーバー変数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-337">The middleware supports the following IIS URL Rewrite Module server variables:</span></span>
* <span data-ttu-id="c0f9f-338">多く</span><span class="sxs-lookup"><span data-stu-id="c0f9f-338">CONTENT_LENGTH</span></span>
* <span data-ttu-id="c0f9f-339">CONTENT_TYPE</span><span class="sxs-lookup"><span data-stu-id="c0f9f-339">CONTENT_TYPE</span></span>
* <span data-ttu-id="c0f9f-340">HTTP_ACCEPT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-340">HTTP_ACCEPT</span></span>
* <span data-ttu-id="c0f9f-341">HTTP_CONNECTION</span><span class="sxs-lookup"><span data-stu-id="c0f9f-341">HTTP_CONNECTION</span></span>
* <span data-ttu-id="c0f9f-342">HTTP_COOKIE</span><span class="sxs-lookup"><span data-stu-id="c0f9f-342">HTTP_COOKIE</span></span>
* <span data-ttu-id="c0f9f-343">HTTP_HOST</span><span class="sxs-lookup"><span data-stu-id="c0f9f-343">HTTP_HOST</span></span>
* <span data-ttu-id="c0f9f-344">ボタン</span><span class="sxs-lookup"><span data-stu-id="c0f9f-344">HTTP_REFERER</span></span>
* <span data-ttu-id="c0f9f-345">HTTP_URL</span><span class="sxs-lookup"><span data-stu-id="c0f9f-345">HTTP_URL</span></span>
* <span data-ttu-id="c0f9f-346">HTTP_USER_AGENT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-346">HTTP_USER_AGENT</span></span>
* <span data-ttu-id="c0f9f-347">HTTPS</span><span class="sxs-lookup"><span data-stu-id="c0f9f-347">HTTPS</span></span>
* <span data-ttu-id="c0f9f-348">LOCAL_ADDR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-348">LOCAL_ADDR</span></span>
* <span data-ttu-id="c0f9f-349">QUERY_STRING</span><span class="sxs-lookup"><span data-stu-id="c0f9f-349">QUERY_STRING</span></span>
* <span data-ttu-id="c0f9f-350">REMOTE_ADDR</span><span class="sxs-lookup"><span data-stu-id="c0f9f-350">REMOTE_ADDR</span></span>
* <span data-ttu-id="c0f9f-351">REMOTE_PORT</span><span class="sxs-lookup"><span data-stu-id="c0f9f-351">REMOTE_PORT</span></span>
* <span data-ttu-id="c0f9f-352">REQUEST_FILENAME</span><span class="sxs-lookup"><span data-stu-id="c0f9f-352">REQUEST_FILENAME</span></span>
* <span data-ttu-id="c0f9f-353">REQUEST_URI</span><span class="sxs-lookup"><span data-stu-id="c0f9f-353">REQUEST_URI</span></span>

> [!NOTE]
> <span data-ttu-id="c0f9f-354">取得することも、`IFileProvider`を介して、`PhysicalFileProvider`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-354">You can also obtain an `IFileProvider` via a `PhysicalFileProvider`.</span></span> <span data-ttu-id="c0f9f-355">このアプローチは可能性があります規則ファイルを書き換えの場所をより柔軟に提供します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-355">This approach may provide greater flexibility for the location of your rewrite rules files.</span></span> <span data-ttu-id="c0f9f-356">書き換え規則ファイルを指定したパスにあるサーバーにデプロイされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-356">Make sure that your rewrite rules files are deployed to the server at the path you provide.</span></span>
> ```csharp
> PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
> ```

### <a name="method-based-rule"></a><span data-ttu-id="c0f9f-357">メソッド ベースのルール</span><span class="sxs-lookup"><span data-stu-id="c0f9f-357">Method-based rule</span></span>
<span data-ttu-id="c0f9f-358">使用して`Add(Action<RewriteContext> applyRule)`メソッドで、独自の規則ロジックを実装します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-358">Use `Add(Action<RewriteContext> applyRule)` to implement your own rule logic in a method.</span></span> <span data-ttu-id="c0f9f-359">`RewriteContext`公開、`HttpContext`メソッドで使用するためです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-359">The `RewriteContext` exposes the `HttpContext` for use in your method.</span></span> <span data-ttu-id="c0f9f-360">`context.Result`パイプラインを追加する方法を決定する処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-360">The `context.Result` determines how additional pipeline processing is handled.</span></span>

| <span data-ttu-id="c0f9f-361">コンテキスト。結果</span><span class="sxs-lookup"><span data-stu-id="c0f9f-361">context.Result</span></span>                       | <span data-ttu-id="c0f9f-362">操作</span><span class="sxs-lookup"><span data-stu-id="c0f9f-362">Action</span></span>                                                          |
| ------------------------------------ | --------------------------------------------------------------- |
| <span data-ttu-id="c0f9f-363">`RuleResult.ContinueRules` (既定値)</span><span class="sxs-lookup"><span data-stu-id="c0f9f-363">`RuleResult.ContinueRules` (default)</span></span> | <span data-ttu-id="c0f9f-364">ルールの適用を続行します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-364">Continue applying rules</span></span>                                         |
| `RuleResult.EndResponse`             | <span data-ttu-id="c0f9f-365">規則の適用を停止し、応答を送信</span><span class="sxs-lookup"><span data-stu-id="c0f9f-365">Stop applying rules and send the response</span></span>                       |
| `RuleResult.SkipRemainingRules`      | <span data-ttu-id="c0f9f-366">ルールの適用を停止し、次のミドルウェアにコンテキストを送信</span><span class="sxs-lookup"><span data-stu-id="c0f9f-366">Stop applying rules and send the context to the next middleware</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-367">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-367">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-368">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-368">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=9)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-369">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-369">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-370">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-370">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=6)]</span></span>

---

<span data-ttu-id="c0f9f-371">サンプル アプリで終了するパスの要求をリダイレクトするメソッドに示します*.xml*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-371">The sample app demonstrates a method that redirects requests for paths that end with *.xml*.</span></span> <span data-ttu-id="c0f9f-372">要求を行う場合`/file.xml`にリダイレクトされます`/xmlfiles/file.xml`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-372">If you make a request for `/file.xml`, it's redirected to `/xmlfiles/file.xml`.</span></span> <span data-ttu-id="c0f9f-373">ステータス コードが 301 (完全な移動) に設定されます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-373">The status code is set to 301 (Moved Permanently).</span></span> <span data-ttu-id="c0f9f-374">リダイレクトは、応答のステータス コード明示的に設定する必要があります。それ以外の場合は 200 (OK) のステータス コードが返され、リダイレクトはクライアントで発生しません。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-374">For a redirect, you must explicitly set the status code of the response; otherwise, a 200 (OK) status code is returned and the redirect won't occur on the client.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-375">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-375">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-376">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-376">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet1)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-377">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-377">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-378">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-378">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet2)]</span></span>

---

<span data-ttu-id="c0f9f-379">元の要求:`/file.xml`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-379">Original Request: `/file.xml`</span></span>

![ブラウザー ウィンドウで、要求および file.xml の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_xml_requests.png)

### <a name="irule-based-rule"></a><span data-ttu-id="c0f9f-381">IRule ベースのルール</span><span class="sxs-lookup"><span data-stu-id="c0f9f-381">IRule-based rule</span></span>
<span data-ttu-id="c0f9f-382">使用して`Add(IRule)`から派生したクラスに、独自の規則ロジックを実装する`IRule`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-382">Use `Add(IRule)` to implement your own rule logic in a class that derives from `IRule`.</span></span> <span data-ttu-id="c0f9f-383">使用して、`IRule`メソッド ベースのルールのアプローチを使用する場合より高い柔軟性が実現します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-383">Using an `IRule` provides greater flexibility over using the method-based rule approach.</span></span> <span data-ttu-id="c0f9f-384">派生クラスでのパラメーターに渡すことができます、コンス トラクターを含めることがあります、`ApplyRule`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-384">Your derived class may include a constructor, where you can pass in parameters for the `ApplyRule` method.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-385">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-385">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-386">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-386">[!code-csharp[Main](url-rewriting/samples/2.x/Program.cs?name=snippet1&highlight=10-11)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-387">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-387">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-388">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-388">[!code-csharp[Main](url-rewriting/samples/1.x/Startup.cs?name=snippet1&highlight=7-8)]</span></span>

---

<span data-ttu-id="c0f9f-389">用のサンプル アプリケーションでは、パラメーターの値、`extension`と`newPath`をいくつかの条件を満たすためにチェックされます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-389">The values of the parameters in the sample app for the `extension` and the `newPath` are checked to meet several conditions.</span></span> <span data-ttu-id="c0f9f-390">`extension` 、値を格納する必要があります値を指定する必要があります*.png*、 *.jpg*、または*.gif*です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-390">The `extension` must contain a value, and the value must be *.png*, *.jpg*, or *.gif*.</span></span> <span data-ttu-id="c0f9f-391">場合、`newPath`が有効でない、`ArgumentException`がスローされます。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-391">If the `newPath` isn't valid, an `ArgumentException` is thrown.</span></span> <span data-ttu-id="c0f9f-392">要求を行う場合*image.png*にリダイレクトされます`/png-images/image.png`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-392">If you make a request for *image.png*, it's redirected to `/png-images/image.png`.</span></span> <span data-ttu-id="c0f9f-393">要求を行う場合*image.jpg*にリダイレクトされます`/jpg-images/image.jpg`です。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-393">If you make a request for *image.jpg*, it's redirected to `/jpg-images/image.jpg`.</span></span> <span data-ttu-id="c0f9f-394">ステータス コードが 301 (完全な移動) に設定され、`context.Result`規則の処理を停止し、応答の送信に設定されています。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-394">The status code is set to 301 (Moved Permanently), and the `context.Result` is set to stop processing rules and send the response.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c0f9f-395">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-395">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c0f9f-396">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-396">[!code-csharp[Main](url-rewriting/samples/2.x/RewriteRules.cs?name=snippet2)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c0f9f-397">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c0f9f-397">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c0f9f-398">[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="c0f9f-398">[!code-csharp[Main](url-rewriting/samples/1.x/RewriteRule.cs?name=snippet1)]</span></span>

---

<span data-ttu-id="c0f9f-399">元の要求:`/image.png`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-399">Original Request: `/image.png`</span></span>

![ブラウザー ウィンドウで、要求および image.png の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_png_requests.png)

<span data-ttu-id="c0f9f-401">元の要求:`/image.jpg`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-401">Original Request: `/image.jpg`</span></span>

![ブラウザー ウィンドウで、要求および image.jpg の応答を追跡開発者ツール](url-rewriting/_static/add_redirect_jpg_requests.png)

## <a name="regex-examples"></a><span data-ttu-id="c0f9f-403">正規表現の例</span><span class="sxs-lookup"><span data-stu-id="c0f9f-403">Regex examples</span></span>

| <span data-ttu-id="c0f9f-404">Goal</span><span class="sxs-lookup"><span data-stu-id="c0f9f-404">Goal</span></span> | <span data-ttu-id="c0f9f-405">Regex 文字列 (& a)</span><span class="sxs-lookup"><span data-stu-id="c0f9f-405">Regex String &</span></span><br><span data-ttu-id="c0f9f-406">一致の例</span><span class="sxs-lookup"><span data-stu-id="c0f9f-406">Match Example</span></span> | <span data-ttu-id="c0f9f-407">置換後の文字列 (& a)</span><span class="sxs-lookup"><span data-stu-id="c0f9f-407">Replacement String &</span></span><br><span data-ttu-id="c0f9f-408">出力例</span><span class="sxs-lookup"><span data-stu-id="c0f9f-408">Output Example</span></span> |
| ---- | :-----------------------------: | :------------------------------------: |
| <span data-ttu-id="c0f9f-409">クエリ文字列にパスを書き直してください。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-409">Rewrite path into querystring</span></span> | `^path/(.*)/(.*)`<br>`/path/abc/123` | `path?var1=$1&var2=$2`<br>`/path?var1=abc&var2=123` |
| <span data-ttu-id="c0f9f-410">末尾のスラッシュを除去します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-410">Strip trailing slash</span></span> | `(.*)/$`<br>`/path/` | `$1`<br>`/path` |
| <span data-ttu-id="c0f9f-411">末尾のスラッシュを適用します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-411">Enforce trailing slash</span></span> | `(.*[^/])$`<br>`/path` | `$1/`<br>`/path/` |
| <span data-ttu-id="c0f9f-412">特定の要求を再作成を回避します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-412">Avoid rewriting specific requests</span></span> | `(.*[^(\.axd)])$`<br><span data-ttu-id="c0f9f-413">うん：`/resource.htm`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-413">Yes: `/resource.htm`</span></span><br><span data-ttu-id="c0f9f-414">違います：`/resource.axd`</span><span class="sxs-lookup"><span data-stu-id="c0f9f-414">No: `/resource.axd`</span></span> | `rewritten/$1`<br>`/rewritten/resource.htm`<br>`/resource.axd` |
| <span data-ttu-id="c0f9f-415">URL セグメントを再配置します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-415">Rearrange URL segments</span></span> | `path/(.*)/(.*)/(.*)`<br>`path/1/2/3` | `path/$3/$2/$1`<br>`path/3/2/1` |
| <span data-ttu-id="c0f9f-416">URL セグメントを置き換えます</span><span class="sxs-lookup"><span data-stu-id="c0f9f-416">Replace a URL segment</span></span> | `^(.*)/segment2/(.*)`<br>`/segment1/segment2/segment3` | `$1/replaced/$2`<br>`/segment1/replaced/segment3` |

## <a name="additional-resources"></a><span data-ttu-id="c0f9f-417">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c0f9f-417">Additional resources</span></span>
* [<span data-ttu-id="c0f9f-418">アプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="c0f9f-418">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="c0f9f-419">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="c0f9f-419">Middleware</span></span>](middleware.md)
* [<span data-ttu-id="c0f9f-420">.NET の正規表現</span><span class="sxs-lookup"><span data-stu-id="c0f9f-420">Regular expressions in .NET</span></span>](/dotnet/articles/standard/base-types/regular-expressions)
* [<span data-ttu-id="c0f9f-421">正規表現言語 - クイック リファレンス</span><span class="sxs-lookup"><span data-stu-id="c0f9f-421">Regular expression language - quick reference</span></span>](/dotnet/articles/standard/base-types/quick-ref)
* [<span data-ttu-id="c0f9f-422">Apache mod_rewrite</span><span class="sxs-lookup"><span data-stu-id="c0f9f-422">Apache mod_rewrite</span></span>](https://httpd.apache.org/docs/2.4/rewrite/)
* [<span data-ttu-id="c0f9f-423">(IIS) の Url 書き換えモジュール 2.0 を使用してください。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-423">Using Url Rewrite Module 2.0 (for IIS)</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/using-url-rewrite-module-20)
* [<span data-ttu-id="c0f9f-424">URL 書き換えモジュール構成のリファレンス</span><span class="sxs-lookup"><span data-stu-id="c0f9f-424">URL Rewrite Module Configuration Reference</span></span>](https://docs.microsoft.com/iis/extensions/url-rewrite-module/url-rewrite-module-configuration-reference)
* [<span data-ttu-id="c0f9f-425">IIS URL 書き換えモジュール フォーラム</span><span class="sxs-lookup"><span data-stu-id="c0f9f-425">IIS URL Rewrite Module Forum</span></span>](https://forums.iis.net/1152.aspx)
* [<span data-ttu-id="c0f9f-426">単純な URL 構造を保持します。</span><span class="sxs-lookup"><span data-stu-id="c0f9f-426">Keep a simple URL structure</span></span>](https://support.google.com/webmasters/answer/76329?hl=en)
* [<span data-ttu-id="c0f9f-427">10 URL 書き換えヒントし、テクニック</span><span class="sxs-lookup"><span data-stu-id="c0f9f-427">10 URL Rewriting Tips and Tricks</span></span>](http://ruslany.net/2009/04/10-url-rewriting-tips-and-tricks/)
* [<span data-ttu-id="c0f9f-428">スラッシュまたは円記号が</span><span class="sxs-lookup"><span data-stu-id="c0f9f-428">To slash or not to slash</span></span>](https://webmasters.googleblog.com/2010/04/to-slash-or-not-to-slash.html)
