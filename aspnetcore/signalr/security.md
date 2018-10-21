---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477541"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="6f8d0-103">ASP.NET Core SignalR でのセキュリティに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="6f8d0-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="6f8d0-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="6f8d0-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="6f8d0-105">この記事では、SignalR のセキュリティ保護についてを説明します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="6f8d0-106">クロス オリジン リソース共有</span><span class="sxs-lookup"><span data-stu-id="6f8d0-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="6f8d0-107">[クロス オリジン リソース共有 (CORS)](https://www.w3.org/TR/cors/)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="6f8d0-108">JavaScript コードは、SignalR アプリから別のドメインでホストされている場合[CORS ミドルウェア](xref:security/cors)SignalR アプリへの接続に JavaScript を許可する有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="6f8d0-109">クロス オリジン要求を信頼するドメインまたはコントロールからのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="6f8d0-110">例えば:</span><span class="sxs-lookup"><span data-stu-id="6f8d0-110">For example:</span></span>

* <span data-ttu-id="6f8d0-111">サイトがホストされています。 `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="6f8d0-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="6f8d0-112">SignalR アプリがホストされています。 `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="6f8d0-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="6f8d0-113">のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="6f8d0-114">CORS の構成の詳細については、次を参照してください。[を有効にするクロス オリジン要求 (CORS)](xref:security/cors)します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="6f8d0-115">SignalR**必要**以下の CORS ポリシー。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="6f8d0-116">特定の想定されるオリジンを許可します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-116">Allow the specific expected origins.</span></span> <span data-ttu-id="6f8d0-117">任意のオリジンを許可することができますが、**いない**セキュリティで保護されたかをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="6f8d0-118">HTTP メソッド`GET`と`POST`許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="6f8d0-119">認証を使用しない場合でも、資格情報を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="6f8d0-120">たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます`http://example.com`でホストされている SignalR アプリにアクセスする`http://signalr.example.com`:。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-120">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access the SignalR app hosted on `http://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="6f8d0-121">SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="6f8d0-122">WebSocket の配信元の制限</span><span class="sxs-lookup"><span data-stu-id="6f8d0-122">WebSocket Origin Restriction</span></span>

<span data-ttu-id="6f8d0-123">CORS で提供される保護は、Websocket に適用されません。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="6f8d0-124">ブラウザー操作を実行**いない**:</span><span class="sxs-lookup"><span data-stu-id="6f8d0-124">Browsers do **not**:</span></span>

* <span data-ttu-id="6f8d0-125">CORS の事前要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-125">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="6f8d0-126">指定された制限を尊重`Access-Control`WebSocket 要求を行うときにヘッダー。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-126">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="6f8d0-127">ただし、ブラウザーを送信しないでください、 `Origin` WebSocket 要求を発行するときにヘッダー。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-127">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="6f8d0-128">Websocket の予期される配信元からのみが許可されていることを確認するこれらのヘッダーを検証するアプリケーションを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-128">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="6f8d0-129">配置のカスタム ミドルウェアを使用してヘッダーの検証を実現できる ASP.NET Core 2.1 以降では、**する前に`UseSignalR`、認証ミドルウェアと**で`Configure`:</span><span class="sxs-lookup"><span data-stu-id="6f8d0-129">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="6f8d0-130">`Origin`ヘッダーは、クライアントと同様、制御、`Referer`ヘッダーを送んだことができます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-130">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="6f8d0-131">これらのヘッダーにする必要があります**いない**認証メカニズムとして使用します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-131">These headers should **not** be used as an authentication mechanism.</span></span>

## <a name="access-token-logging"></a><span data-ttu-id="6f8d0-132">アクセス トークンのログ記録</span><span class="sxs-lookup"><span data-stu-id="6f8d0-132">Access token logging</span></span>

<span data-ttu-id="6f8d0-133">Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-133">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="6f8d0-134">標準を使用してとして一般に安全ではクエリ文字列を使用してアクセス トークンを受信`Authorization`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-134">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="6f8d0-135">ただし、多くの web サーバーは、クエリ文字列を含む、各要求の URL をログインします。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-135">However, many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="6f8d0-136">Url をログには、アクセス トークンがログに記録可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-136">Logging the URLs may log the access token.</span></span> <span data-ttu-id="6f8d0-137">Web ログ アクセス トークンを防ぐためにサーバーのログ記録の設定を設定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-137">A best practice is to set the web server's logging settings to prevent logging access tokens.</span></span>

## <a name="exceptions"></a><span data-ttu-id="6f8d0-138">例外</span><span class="sxs-lookup"><span data-stu-id="6f8d0-138">Exceptions</span></span>

<span data-ttu-id="6f8d0-139">例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-139">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="6f8d0-140">既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-140">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="6f8d0-141">代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-141">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="6f8d0-142">クライアントに例外メッセージを配信 (たとえば開発またはテスト) でオーバーライドできる[ `EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-142">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="6f8d0-143">例外メッセージは、運用アプリでクライアントに公開されませんする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-143">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="6f8d0-144">バッファー管理</span><span class="sxs-lookup"><span data-stu-id="6f8d0-144">Buffer management</span></span>

<span data-ttu-id="6f8d0-145">SignalR では、接続ごとのバッファーを使用して、受信および送信メッセージを管理します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-145">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="6f8d0-146">既定では、SignalR は、これらを 32 KB のバッファーを制限します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-146">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="6f8d0-147">クライアントまたはサーバーに送信できる最大のメッセージは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-147">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="6f8d0-148">メッセージの接続によって消費される最大メモリは、32 KB です。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-148">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="6f8d0-149">メッセージには、32 KB 未満が常に場合、制限値を減らすことができますが。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-149">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="6f8d0-150">クライアントがより大きなメッセージを送信できるようにされたりするを防止します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-150">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="6f8d0-151">サーバーは、メッセージを受信する大きなバッファーを割り当てる必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-151">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="6f8d0-152">メッセージが 32 KB を超える場合は、制限を増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-152">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="6f8d0-153">この制限を増やすことを意味します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-153">Increasing this limit means:</span></span>

* <span data-ttu-id="6f8d0-154">クライアントには、サーバーを大規模メモリ バッファーを割り当てる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-154">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="6f8d0-155">ラージ バッファーの割り当てをサーバーには、同時接続数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-155">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="6f8d0-156">受信および送信メッセージの制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:</span><span class="sxs-lookup"><span data-stu-id="6f8d0-156">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="6f8d0-157">`ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-157">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="6f8d0-158">クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-158">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="6f8d0-159">`TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-159">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="6f8d0-160">サーバーがこの制限を超えるメッセージ (ハブ メソッドからの含む戻り値の値) を送信しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-160">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="6f8d0-161">制限値を設定`0`制限を無効にします。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-161">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="6f8d0-162">制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-162">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="6f8d0-163">悪意のあるクライアントがサイズの大きいメッセージを送信するには、過度のメモリを割り当てることがあります。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-163">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="6f8d0-164">過度のメモリ使用量は、同時接続数を大幅に短縮できます。</span><span class="sxs-lookup"><span data-stu-id="6f8d0-164">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
