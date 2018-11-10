---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: be1dd24c40327d9a0d8f91bf75300128d3d52725
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225370"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="dbcfa-103">ASP.NET Core SignalR でのセキュリティに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="dbcfa-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="dbcfa-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="dbcfa-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="dbcfa-105">この記事では、SignalR のセキュリティ保護についてを説明します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="dbcfa-106">クロス オリジン リソース共有</span><span class="sxs-lookup"><span data-stu-id="dbcfa-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="dbcfa-107">[クロス オリジン リソース共有 (CORS)](https://www.w3.org/TR/cors/)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="dbcfa-108">JavaScript コードは、SignalR アプリから別のドメインでホストされている場合[CORS ミドルウェア](xref:security/cors)SignalR アプリへの接続に JavaScript を許可する有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="dbcfa-109">クロス オリジン要求を信頼するドメインまたはコントロールからのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="dbcfa-110">例えば:</span><span class="sxs-lookup"><span data-stu-id="dbcfa-110">For example:</span></span>

* <span data-ttu-id="dbcfa-111">サイトがホストされています。 `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="dbcfa-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="dbcfa-112">SignalR アプリがホストされています。 `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="dbcfa-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="dbcfa-113">のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="dbcfa-114">CORS の構成の詳細については、次を参照してください。[を有効にするクロス オリジン要求 (CORS)](xref:security/cors)します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="dbcfa-115">SignalR**必要**以下の CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="dbcfa-116">特定の想定されるオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-116">Allow the specific expected origins.</span></span> <span data-ttu-id="dbcfa-117">任意のオリジンを許可することができますが、**いない**セキュリティで保護されたかをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="dbcfa-118">HTTP メソッド`GET`と`POST`許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="dbcfa-119">認証を使用しない場合でも、資格情報を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="dbcfa-120">たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます`http://example.com`でホストされている SignalR アプリにアクセスする`http://signalr.example.com`:。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="dbcfa-121">SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="dbcfa-122">WebSocket の配信元の制限</span><span class="sxs-lookup"><span data-stu-id="dbcfa-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="dbcfa-123">CORS で提供される保護は、Websocket に適用されません。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="dbcfa-124">Websocket を配信元の制限、読み取り[Websocket 原点制限](xref:fundamentals/websockets#websocket-origin-restriction)します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="dbcfa-125">CORS で提供される保護は、Websocket に適用されません。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="dbcfa-126">ブラウザー操作を実行**いない**:</span><span class="sxs-lookup"><span data-stu-id="dbcfa-126">Browsers do **not**:</span></span>

* <span data-ttu-id="dbcfa-127">CORS の事前要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="dbcfa-128">指定された制限を尊重`Access-Control`WebSocket 要求を行うときにヘッダー。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="dbcfa-129">ただし、ブラウザーを送信しないでください、 `Origin` WebSocket 要求を発行するときにヘッダー。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="dbcfa-130">Websocket の予期される配信元からのみが許可されていることを確認するこれらのヘッダーを検証するアプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="dbcfa-131">配置のカスタム ミドルウェアを使用してヘッダーの検証を実現できる ASP.NET Core 2.1 以降では、**する前に`UseSignalR`、認証ミドルウェアと**で`Configure`:</span><span class="sxs-lookup"><span data-stu-id="dbcfa-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="dbcfa-132">`Origin`ヘッダーは、クライアントと同様、制御、`Referer`ヘッダーを送んだことができます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="dbcfa-133">これらのヘッダーにする必要があります**いない**認証メカニズムとして使用します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="dbcfa-134">アクセス トークンのログ記録</span><span class="sxs-lookup"><span data-stu-id="dbcfa-134">Access token logging</span></span>

<span data-ttu-id="dbcfa-135">Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="dbcfa-136">標準を使用してとして一般に安全ではクエリ文字列を使用してアクセス トークンを受信`Authorization`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="dbcfa-137">ただし、多くの web サーバーは、クエリ文字列を含む、各要求の URL をログインします。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-137">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="dbcfa-138">Url をログには、アクセス トークンがログに記録可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-138">Logging the URLs may log the access token.</span></span> <span data-ttu-id="dbcfa-139">Web ログ アクセス トークンを防ぐためにサーバーのログ記録の設定を設定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-139">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="dbcfa-140">例外</span><span class="sxs-lookup"><span data-stu-id="dbcfa-140">Exceptions</span></span>

<span data-ttu-id="dbcfa-141">例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-141">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="dbcfa-142">既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-142">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="dbcfa-143">代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-143">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="dbcfa-144">クライアントに例外メッセージを配信 (たとえば開発またはテスト) でオーバーライドできる[ `EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-144">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="dbcfa-145">例外メッセージは、運用アプリでクライアントに公開されませんする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-145">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="dbcfa-146">バッファー管理</span><span class="sxs-lookup"><span data-stu-id="dbcfa-146">Buffer management</span></span>

<span data-ttu-id="dbcfa-147">SignalR では、接続ごとのバッファーを使用して、受信および送信メッセージを管理します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-147">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="dbcfa-148">既定では、SignalR は、これらを 32 KB のバッファーを制限します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-148">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="dbcfa-149">クライアントまたはサーバーに送信できる最大のメッセージは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-149">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="dbcfa-150">メッセージの接続によって消費される最大メモリは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-150">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="dbcfa-151">メッセージには、32 KB 未満が常に場合、制限値を減らすことができますが。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-151">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="dbcfa-152">クライアントがより大きなメッセージを送信できるようにされたりするを防止します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-152">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="dbcfa-153">サーバーは、メッセージを受信する大きなバッファーを割り当てる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-153">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="dbcfa-154">メッセージが 32 KB を超える場合は、制限を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-154">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="dbcfa-155">この制限を増やすことを意味します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-155">Increasing this limit means:</span></span>

* <span data-ttu-id="dbcfa-156">クライアントには、サーバーを大規模メモリ バッファーを割り当てる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-156">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="dbcfa-157">ラージ バッファーの割り当てをサーバーには、同時接続数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-157">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="dbcfa-158">受信および送信メッセージの制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:</span><span class="sxs-lookup"><span data-stu-id="dbcfa-158">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="dbcfa-159">`ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-159">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="dbcfa-160">クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-160">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="dbcfa-161">`TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-161">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="dbcfa-162">サーバーがこの制限を超えるメッセージ (ハブ メソッドからの含む戻り値の値) を送信しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-162">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="dbcfa-163">制限値を設定`0`制限を無効にします。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-163">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="dbcfa-164">制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-164">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="dbcfa-165">悪意のあるクライアントがサイズの大きいメッセージを送信するには、過度のメモリを割り当てることがあります。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-165">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="dbcfa-166">過度のメモリ使用量は、同時接続数を大幅に短縮できます。</span><span class="sxs-lookup"><span data-stu-id="dbcfa-166">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
