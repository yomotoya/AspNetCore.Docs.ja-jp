---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: bradygaster
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 6e9f849ed856cf1cbf989b8b16cab5209c465471
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743788"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="fc684-103">ASP.NET Core SignalR でのセキュリティに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="fc684-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="fc684-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="fc684-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="fc684-105">この記事では、SignalR のセキュリティ保護についてを説明します。</span><span class="sxs-lookup"><span data-stu-id="fc684-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="fc684-106">クロス オリジン リソース共有</span><span class="sxs-lookup"><span data-stu-id="fc684-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="fc684-107">[クロス オリジン リソース共有 (CORS)](https://www.w3.org/TR/cors/)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="fc684-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="fc684-108">JavaScript コードは、SignalR アプリから別のドメインでホストされている場合[CORS ミドルウェア](xref:security/cors)SignalR アプリへの接続に JavaScript を許可する有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="fc684-109">クロス オリジン要求を信頼するドメインまたはコントロールからのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="fc684-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="fc684-110">例:</span><span class="sxs-lookup"><span data-stu-id="fc684-110">For example:</span></span>

* <span data-ttu-id="fc684-111">サイトがホストされています。 `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="fc684-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="fc684-112">SignalR アプリがホストされています。 `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="fc684-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="fc684-113">のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。</span><span class="sxs-lookup"><span data-stu-id="fc684-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="fc684-114">CORS の構成の詳細については、[を有効にするクロス オリジン要求 (CORS)](xref:security/cors)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="fc684-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="fc684-115">SignalR**必要**以下の CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="fc684-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="fc684-116">特定の想定されるオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="fc684-116">Allow the specific expected origins.</span></span> <span data-ttu-id="fc684-117">任意のオリジンを許可することができますが、**いない**セキュリティで保護されたかをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fc684-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="fc684-118">HTTP メソッド`GET`と`POST`許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="fc684-119">認証を使用しない場合でも、資格情報を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="fc684-120">たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます`https://example.com`でホストされている SignalR アプリにアクセスする`https://signalr.example.com`:。</span><span class="sxs-lookup"><span data-stu-id="fc684-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="fc684-121">SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="fc684-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="fc684-122">WebSocket の配信元の制限</span><span class="sxs-lookup"><span data-stu-id="fc684-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fc684-123">CORS で提供される保護は、WebSocket には適用されません。</span><span class="sxs-lookup"><span data-stu-id="fc684-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="fc684-124">Websocket を配信元の制限、読み取り[Websocket 原点制限](xref:fundamentals/websockets#websocket-origin-restriction)します。</span><span class="sxs-lookup"><span data-stu-id="fc684-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fc684-125">CORS で提供される保護は、WebSocket には適用されません。</span><span class="sxs-lookup"><span data-stu-id="fc684-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="fc684-126">ブラウザーでは以下を実行**しません**。</span><span class="sxs-lookup"><span data-stu-id="fc684-126">Browsers do **not**:</span></span>

* <span data-ttu-id="fc684-127">CORS の事前要求を実行する。</span><span class="sxs-lookup"><span data-stu-id="fc684-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="fc684-128">WebSocket 要求を行うときに `Access-Control` ヘッダーに指定された制限を考慮する。</span><span class="sxs-lookup"><span data-stu-id="fc684-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="fc684-129">ただし、WebSocket 要求を発行するときにはブラウザーから `Origin` ヘッダーが送信されます。</span><span class="sxs-lookup"><span data-stu-id="fc684-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="fc684-130">予期した配信元からの WebSocket のみが許可されるように、アプリケーションでこれらのヘッダーが検証されるように構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="fc684-131">配置のカスタム ミドルウェアを使用してヘッダーの検証を実現できる ASP.NET Core 2.1 以降では、**する前に`UseSignalR`、認証ミドルウェアと**で`Configure`:</span><span class="sxs-lookup"><span data-stu-id="fc684-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="fc684-132">`Origin` ヘッダーは、クライアントによって制御され、`Referer` のように偽装することができます。</span><span class="sxs-lookup"><span data-stu-id="fc684-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="fc684-133">これらのヘッダーにする必要があります**いない**認証メカニズムとして使用します。</span><span class="sxs-lookup"><span data-stu-id="fc684-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="fc684-134">アクセス トークンのログ記録</span><span class="sxs-lookup"><span data-stu-id="fc684-134">Access token logging</span></span>

<span data-ttu-id="fc684-135">Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="fc684-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="fc684-136">標準を使用してとして一般に安全ではクエリ文字列を使用してアクセス トークンを受信`Authorization`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="fc684-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="fc684-137">クライアントとサーバー間のセキュリティで保護されたエンド ツー エンド接続を確実に常に HTTPS を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="fc684-138">クエリ文字列を含む、各要求の URL を多くの web サーバーにログインします。</span><span class="sxs-lookup"><span data-stu-id="fc684-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="fc684-139">Url をログには、アクセス トークンがログに記録可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="fc684-140">ASP.NET Core では、既定で、クエリ文字列が含まれる各要求の URL を記録します。</span><span class="sxs-lookup"><span data-stu-id="fc684-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="fc684-141">例:</span><span class="sxs-lookup"><span data-stu-id="fc684-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="fc684-142">サーバーのログでこのデータのログ記録に関する懸念がある場合は場合、構成によっては、完全でこのログ記録を無効にできます、`Microsoft.AspNetCore.Hosting`ロガー、`Warning`レベル以上 (でこれらのメッセージが書き込まれます`Info`レベル)。</span><span class="sxs-lookup"><span data-stu-id="fc684-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="fc684-143">上のドキュメントを参照して[ログのフィルター処理](xref:fundamentals/logging/index#log-filtering)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="fc684-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="fc684-144">特定の要求情報を記録する場合は、 [、ミドルウェアを作成する](xref:fundamentals/middleware/write)フィルターで除外、必要なデータをログ記録、 `access_token` (存在する) 場合、クエリ文字列の値。</span><span class="sxs-lookup"><span data-stu-id="fc684-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="fc684-145">例外</span><span class="sxs-lookup"><span data-stu-id="fc684-145">Exceptions</span></span>

<span data-ttu-id="fc684-146">例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fc684-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="fc684-147">既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。</span><span class="sxs-lookup"><span data-stu-id="fc684-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="fc684-148">代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="fc684-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="fc684-149">クライアントに例外メッセージを配信 (たとえば開発またはテスト) でオーバーライドできる[ `EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)します。</span><span class="sxs-lookup"><span data-stu-id="fc684-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="fc684-150">例外メッセージは、運用アプリでクライアントに公開されませんする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="fc684-151">バッファー管理</span><span class="sxs-lookup"><span data-stu-id="fc684-151">Buffer management</span></span>

<span data-ttu-id="fc684-152">SignalR では、接続ごとのバッファーを使用して、受信および送信メッセージを管理します。</span><span class="sxs-lookup"><span data-stu-id="fc684-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="fc684-153">既定では、SignalR は、これらを 32 KB のバッファーを制限します。</span><span class="sxs-lookup"><span data-stu-id="fc684-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="fc684-154">クライアントまたはサーバーに送信できる最大のメッセージは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="fc684-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="fc684-155">メッセージの接続によって消費される最大メモリは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="fc684-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="fc684-156">メッセージには、32 KB 未満が常に場合、制限値を減らすことができますが。</span><span class="sxs-lookup"><span data-stu-id="fc684-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="fc684-157">クライアントがより大きなメッセージを送信できるようにされたりするを防止します。</span><span class="sxs-lookup"><span data-stu-id="fc684-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="fc684-158">サーバーは、メッセージを受信する大きなバッファーを割り当てる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fc684-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="fc684-159">メッセージが 32 KB を超える場合は、制限を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="fc684-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="fc684-160">この制限を増やすことを意味します。</span><span class="sxs-lookup"><span data-stu-id="fc684-160">Increasing this limit means:</span></span>

* <span data-ttu-id="fc684-161">クライアントには、サーバーを大規模メモリ バッファーを割り当てる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="fc684-162">ラージ バッファーの割り当てをサーバーには、同時接続数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="fc684-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="fc684-163">受信および送信メッセージの制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:</span><span class="sxs-lookup"><span data-stu-id="fc684-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="fc684-164">`ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="fc684-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="fc684-165">クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fc684-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="fc684-166">`TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。</span><span class="sxs-lookup"><span data-stu-id="fc684-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="fc684-167">サーバーがこの制限を超えるメッセージ (ハブ メソッドからの含む戻り値の値) を送信しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="fc684-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="fc684-168">制限値を設定`0`制限を無効にします。</span><span class="sxs-lookup"><span data-stu-id="fc684-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="fc684-169">制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。</span><span class="sxs-lookup"><span data-stu-id="fc684-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="fc684-170">悪意のあるクライアントがサイズの大きいメッセージを送信するには、過度のメモリを割り当てることがあります。</span><span class="sxs-lookup"><span data-stu-id="fc684-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="fc684-171">過度のメモリ使用量は、同時接続数を大幅に短縮できます。</span><span class="sxs-lookup"><span data-stu-id="fc684-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
