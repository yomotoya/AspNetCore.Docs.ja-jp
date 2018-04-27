---
title: ASP.NET Core で HTTPS を適用します。
author: rick-anderson
description: Web アプリで ASP.NET Core HTTPS/TLS を必要とする方法を示します。
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 0509bebe430c6ba213031a2cb7cb91bb7a39566d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="f1ef1-103">ASP.NET Core で HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="f1ef1-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1ef1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1ef1-105">このドキュメントでは次の方法について説明します:</span><span class="sxs-lookup"><span data-stu-id="f1ef1-105">This document shows how to:</span></span>

- <span data-ttu-id="f1ef1-106">すべての要求に HTTPS を必要とさせる。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="f1ef1-107">すべての HTTP 要求を HTTPS にリダイレクトさせる。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="f1ef1-108">機密情報を受信する Web Api で `RequireHttpsAttribute` を使用**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="f1ef1-109">`RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="f1ef1-110">API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="f1ef1-111">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="f1ef1-112">Web API は次のいずれかの対策を講じるべきです:</span><span class="sxs-lookup"><span data-stu-id="f1ef1-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="f1ef1-113">HTTP をリッスンしない。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="f1ef1-114">ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。
</span><span class="sxs-lookup"><span data-stu-id="f1ef1-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="f1ef1-115">HTTPS が必要</span><span class="sxs-lookup"><span data-stu-id="f1ef1-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="f1ef1-117">Web アプリの呼び出しをすべての ASP.NET Core お勧め`UseHttpsRedirection`すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-117">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="f1ef1-118">場合`UseHsts`と呼ばれますが、アプリで呼び出す必要がある前に`UseHttpsRedirection`です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-118">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="f1ef1-119">次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-119">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="f1ef1-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-120">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="f1ef1-121">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-121">The following code:</span></span>

<span data-ttu-id="f1ef1-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-122">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="f1ef1-123">セット`RedirectStatusCode`です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-123">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="f1ef1-124">5001 を HTTPS ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-124">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="f1ef1-125">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) HTTPS を要求するために使用します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-125">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="f1ef1-126">`[RequireHttpsAttribute]` 装飾できるは、メソッドまたはコント ローラーまたはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-126">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="f1ef1-127">属性をグローバルに適用するには、次のコードを追加`ConfigureServices`で`Startup`:</span><span class="sxs-lookup"><span data-stu-id="f1ef1-127">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="f1ef1-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-128">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="f1ef1-129">前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-129">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="f1ef1-130">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-130">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="f1ef1-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-131">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="f1ef1-132">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-132">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="f1ef1-133">HTTPS をグローバルに必要とする (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-133">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="f1ef1-134">適用する、`[RequireHttps]`属性をすべてのコント ローラー/Razor ページされていないグローバルに HTTPS を必要とすると、セキュリティで保護されたと見なされます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-134">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="f1ef1-135">保証できません、`[RequireHttps]`属性は、新しいコント ローラーおよび Razor ページが追加されたときに適用します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-135">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="f1ef1-136">HTTP 厳密なトランスポート セキュリティ プロトコル (HSTS)</span><span class="sxs-lookup"><span data-stu-id="f1ef1-136">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="f1ef1-137">あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP 厳密なトランスポート セキュリティ (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)オプトイン セキュリティ拡張機能を利用、特別な応答ヘッダーを使用して web アプリケーションによって指定されています。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-137">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="f1ef1-138">サポートされているブラウザーがこのヘッダーを受け取るし、そのブラウザーから、指定したドメインに HTTP 経由で送信されるすべての通信を防ぐ HTTPS 経由ですべての通信を代わりに送信されます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-138">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="f1ef1-139">ブラウザーでのプロンプトの HTTPS クリックスルーも回避されます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-139">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="f1ef1-140">ASP.NET 2.1 preview1 のコアまたは後で HSTS を実装する、`UseHsts`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-140">ASP.NET Core 2.1 preview1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="f1ef1-141">次のコード呼び出し`UseHsts`にアプリがないとき[開発モード](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="f1ef1-141">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="f1ef1-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-142">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="f1ef1-143">`UseHsts` 開発のことをお勧めためには HSTS ヘッダーがブラウザーでキャッシュ可能な高度です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-143">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="f1ef1-144">既定では、UseHsts はローカル ループバック アドレスを除外します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-144">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="f1ef1-145">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-145">The following code:</span></span>

<span data-ttu-id="f1ef1-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="f1ef1-146">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="f1ef1-147">Strict トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-147">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="f1ef1-148">プリロードされていないの一部、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)が新規インストールで HSTS サイトをプリロードする web ブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-148">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="f1ef1-149">詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-149">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="f1ef1-150">により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)、HSTS ポリシー サブドメインをホストに適用されます。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-150">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="f1ef1-151">明示的に 60 日間にする高レベルのトランスポートのセキュリティ ヘッダーの最大継続期間パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-151">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="f1ef1-152">設定されていない場合、既定値は 30 日間です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-152">If not set, defaults to 30 days.</span></span> <span data-ttu-id="f1ef1-153">参照してください、[最大継続期間ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-153">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="f1ef1-154">追加`example.com`を除外するホストの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-154">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="f1ef1-155">`UseHsts` 次のループバックのホストを除外します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-155">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="f1ef1-156">`localhost` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-156">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="f1ef1-157">`127.0.0.1` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-157">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="f1ef1-158">`[::1]` : IPv6 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-158">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="f1ef1-159">前の例では、他のホストを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-159">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="f1ef1-160">オプトアウト HTTPS でプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="f1ef1-160">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="f1ef1-161">ASP.NET Core 2.1 とそれ以降の web アプリケーション テンプレート (Visual Studio dotnet コマンドラインから) を有効にする[HTTPS リダイレクト](#require)と[HSTS](#hsts)です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-161">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="f1ef1-162">HTTPS は必要ありません、展開にすることができますオプトアウト HTTPS です。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-162">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="f1ef1-163">たとえば、ここで HTTPS を処理している外部で、エッジに各ノードで HTTPS を使用して一部のバックエンド サービスは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-163">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="f1ef1-164">無効にするは、HTTPS:</span><span class="sxs-lookup"><span data-stu-id="f1ef1-164">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f1ef1-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1ef1-165">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="f1ef1-166">オフにして、 **HTTPS の構成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-166">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![エンティティ図](enforcing-ssl/_static/out.png)


#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f1ef1-168">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f1ef1-168">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="f1ef1-169">`--no-https` オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-169">Use the `--no-https` option.</span></span> <span data-ttu-id="f1ef1-170">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="f1ef1-170">For example</span></span>

```cli
dotnet new razor --no-https
```

------

::: moniker-end