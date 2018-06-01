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
ms.openlocfilehash: 0433ddb3bf1ef0074c683903ad4553cd6a0b4741
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687819"
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="0f467-103">ASP.NET Core で HTTPS を適用します。</span><span class="sxs-lookup"><span data-stu-id="0f467-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="0f467-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0f467-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0f467-105">このドキュメントでは次の方法について説明します:</span><span class="sxs-lookup"><span data-stu-id="0f467-105">This document shows how to:</span></span>

- <span data-ttu-id="0f467-106">すべての要求に HTTPS を必要とさせる。</span><span class="sxs-lookup"><span data-stu-id="0f467-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="0f467-107">すべての HTTP 要求を HTTPS にリダイレクトさせる。</span><span class="sxs-lookup"><span data-stu-id="0f467-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="0f467-108">機密情報を受信する Web Api で `RequireHttpsAttribute` を使用**しない**でください。</span><span class="sxs-lookup"><span data-stu-id="0f467-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0f467-109">`RequireHttpsAttribute` はブラウザーを HTTP から HTTPS へリダイレクトするために HTTP ステータス コードを使用します。</span><span class="sxs-lookup"><span data-stu-id="0f467-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0f467-110">API クライアントはこれを理解しない場合や、HTTP から HTTPS へのリダイレクトに従わない場合があります。</span><span class="sxs-lookup"><span data-stu-id="0f467-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0f467-111">このようなクライアントは、HTTP 経由で情報を送信することがあります。</span><span class="sxs-lookup"><span data-stu-id="0f467-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0f467-112">Web API は次のいずれかの対策を講じるべきです:</span><span class="sxs-lookup"><span data-stu-id="0f467-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="0f467-113">HTTP をリッスンしない。</span><span class="sxs-lookup"><span data-stu-id="0f467-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="0f467-114">ステータス コード 400 (無効な要求) で接続を閉じ、要求に応答しない。
</span><span class="sxs-lookup"><span data-stu-id="0f467-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="0f467-115">HTTPS が必要</span><span class="sxs-lookup"><span data-stu-id="0f467-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="0f467-116">Web アプリの呼び出しをすべての ASP.NET Core お勧め`UseHttpsRedirection`すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0f467-116">We recommend all ASP.NET Core web apps call `UseHttpsRedirection` to redirect all HTTP requests to HTTPS.</span></span> <span data-ttu-id="0f467-117">場合`UseHsts`と呼ばれますが、アプリで呼び出す必要がある前に`UseHttpsRedirection`です。</span><span class="sxs-lookup"><span data-stu-id="0f467-117">If `UseHsts` is called in the app, it must be called before `UseHttpsRedirection`.</span></span>

<span data-ttu-id="0f467-118">次のコード呼び出し`UseHttpsRedirection`で、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="0f467-118">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="0f467-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="0f467-119">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>


<span data-ttu-id="0f467-120">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0f467-120">The following code:</span></span>

<span data-ttu-id="0f467-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="0f467-121">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

* <span data-ttu-id="0f467-122">セット`RedirectStatusCode`です。</span><span class="sxs-lookup"><span data-stu-id="0f467-122">Sets `RedirectStatusCode`.</span></span>
* <span data-ttu-id="0f467-123">5001 を HTTPS ポートを設定します。</span><span class="sxs-lookup"><span data-stu-id="0f467-123">Sets the HTTPS port to 5001.</span></span>

::: moniker-end


::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="0f467-124">[RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) は HTTPS を必須とさせるために使用します。</span><span class="sxs-lookup"><span data-stu-id="0f467-124">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="0f467-125">`[RequireHttpsAttribute]` はメソッドまたはコントローラーを装飾するか、またはグローバルに適用することができます。</span><span class="sxs-lookup"><span data-stu-id="0f467-125">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="0f467-126">属性をグローバルに適用するには、`ConfigureServices` の `Startup` に次のコードを追加します:</span><span class="sxs-lookup"><span data-stu-id="0f467-126">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="0f467-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="0f467-127">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="0f467-128">前の強調表示されたコードでは、すべての要求を使用して`HTTPS`です。 そのため、HTTP 要求は無視されます。</span><span class="sxs-lookup"><span data-stu-id="0f467-128">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="0f467-129">次の強調表示されたコードは、すべての HTTP 要求を HTTPS にリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="0f467-129">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="0f467-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="0f467-130">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="0f467-131">さらに詳しい情報は、[URL 書き換えミドルウェア](xref:fundamentals/url-rewriting) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f467-131">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="0f467-132">グローバルに HTTPS を必須とさせること (`options.Filters.Add(new RequireHttpsAttribute());`) は、セキュリティのベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="0f467-132">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="0f467-133">`[RequireHttps]`属性をすべてのコントローラー/Razor ページ に適用することは、グローバルに HTTPS を必須とさせることほど安全性が高いとは考えられていません。</span><span class="sxs-lookup"><span data-stu-id="0f467-133">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="0f467-134">新しいコントローラー または Razor ページが追加されたときに `[RequireHttps]` 属性が適用されるとは限らないためです。</span><span class="sxs-lookup"><span data-stu-id="0f467-134">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0f467-135">HTTP 厳密なトランスポート セキュリティ プロトコル (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0f467-135">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0f467-136">あたり[OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project)、 [HTTP 厳密なトランスポート セキュリティ (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)オプトイン セキュリティ拡張機能を利用、特別な応答ヘッダーを使用して web アプリケーションによって指定されています。</span><span class="sxs-lookup"><span data-stu-id="0f467-136">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="0f467-137">サポートされているブラウザーがこのヘッダーを受け取るし、そのブラウザーから、指定したドメインに HTTP 経由で送信されるすべての通信を防ぐ HTTPS 経由ですべての通信を代わりに送信されます。</span><span class="sxs-lookup"><span data-stu-id="0f467-137">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="0f467-138">ブラウザーでのプロンプトの HTTPS クリックスルーも回避されます。</span><span class="sxs-lookup"><span data-stu-id="0f467-138">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="0f467-139">ASP.NET Core 2.1 以降と HSTS を実装して、`UseHsts`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="0f467-139">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0f467-140">次のコード呼び出し`UseHsts`にアプリがないとき[開発モード](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0f467-140">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="0f467-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="0f467-141">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="0f467-142">`UseHsts` 開発のことをお勧めためには HSTS ヘッダーがブラウザーでキャッシュ可能な高度です。</span><span class="sxs-lookup"><span data-stu-id="0f467-142">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="0f467-143">既定では、UseHsts はローカル ループバック アドレスを除外します。</span><span class="sxs-lookup"><span data-stu-id="0f467-143">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="0f467-144">コード例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0f467-144">The following code:</span></span>

<span data-ttu-id="0f467-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="0f467-145">[!code-csharp[sample](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="0f467-146">Strict トランスポート セキュリティ ヘッダーのプリロード パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="0f467-146">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0f467-147">プリロードされていないの一部、 [RFC HSTS 仕様](https://tools.ietf.org/html/rfc6797)が新規インストールで HSTS サイトをプリロードする web ブラウザーでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0f467-147">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0f467-148">詳細については、「[https://hstspreload.org/](https://hstspreload.org/)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0f467-148">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0f467-149">により、 [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2)、HSTS ポリシー サブドメインをホストに適用されます。</span><span class="sxs-lookup"><span data-stu-id="0f467-149">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="0f467-150">明示的に 60 日間にする高レベルのトランスポートのセキュリティ ヘッダーの最大継続期間パラメーターを設定します。</span><span class="sxs-lookup"><span data-stu-id="0f467-150">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="0f467-151">設定されていない場合、既定値は 30 日間です。</span><span class="sxs-lookup"><span data-stu-id="0f467-151">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0f467-152">参照してください、[最大継続期間ディレクティブ](https://tools.ietf.org/html/rfc6797#section-6.1.1)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="0f467-152">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0f467-153">追加`example.com`を除外するホストの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="0f467-153">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0f467-154">`UseHsts` 次のループバックのホストを除外します。</span><span class="sxs-lookup"><span data-stu-id="0f467-154">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0f467-155">`localhost` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="0f467-155">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0f467-156">`127.0.0.1` : IPv4 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="0f467-156">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0f467-157">`[::1]` : IPv6 ループバック アドレス。</span><span class="sxs-lookup"><span data-stu-id="0f467-157">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="0f467-158">前の例では、他のホストを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0f467-158">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1"
<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="0f467-159">オプトアウト HTTPS でプロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="0f467-159">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="0f467-160">ASP.NET Core 2.1 とそれ以降の web アプリケーション テンプレート (Visual Studio dotnet コマンドラインから) を有効にする[HTTPS リダイレクト](#require)と[HSTS](#hsts)です。</span><span class="sxs-lookup"><span data-stu-id="0f467-160">The ASP.NET Core 2.1 and later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="0f467-161">HTTPS は必要ありません、展開にすることができますオプトアウト HTTPS です。</span><span class="sxs-lookup"><span data-stu-id="0f467-161">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="0f467-162">たとえば、ここで HTTPS を処理している外部で、エッジに各ノードで HTTPS を使用して一部のバックエンド サービスは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0f467-162">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="0f467-163">無効にするは、HTTPS:</span><span class="sxs-lookup"><span data-stu-id="0f467-163">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0f467-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0f467-164">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0f467-165">オフにして、 **HTTPS の構成**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="0f467-165">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![エンティティ図](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0f467-167">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="0f467-167">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0f467-168">`--no-https` オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="0f467-168">Use the `--no-https` option.</span></span> <span data-ttu-id="0f467-169">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0f467-169">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="0f467-170">Docker の開発者の証明書をセットアップする方法</span><span class="sxs-lookup"><span data-stu-id="0f467-170">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="0f467-171">参照してください[この GitHub 問題](https://github.com/aspnet/Docs/issues/6199)です。</span><span class="sxs-lookup"><span data-stu-id="0f467-171">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
