---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095133"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="10d67-103">ASP.NET Core SignalR でのセキュリティに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="10d67-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="10d67-104">によって[Andrew Stanton-nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="10d67-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="10d67-105">概要</span><span class="sxs-lookup"><span data-stu-id="10d67-105">Overview</span></span>

<span data-ttu-id="10d67-106">SignalR は、既定では、さまざまなセキュリティ保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="10d67-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="10d67-107">このような保護を構成する方法を理解しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d67-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="10d67-108">クロス オリジン リソース共有</span><span class="sxs-lookup"><span data-stu-id="10d67-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="10d67-109">[クロス オリジン リソース共有 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="10d67-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="10d67-110">有効にする必要がある場合は、JavaScript コードは、SignalR アプリから別のドメイン名でホストされているが、 [ASP.NET Core の CORS ミドルウェア](xref:security/cors)接続できるようにします。</span><span class="sxs-lookup"><span data-stu-id="10d67-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="10d67-111">一般に、クロス オリジン要求を制御するドメインからのみを許可します。</span><span class="sxs-lookup"><span data-stu-id="10d67-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="10d67-112">サイトがホストされている場合など、`http://www.example.com`で SignalR アプリケーションがホストされていると`http://signalr.example.com`、のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。</span><span class="sxs-lookup"><span data-stu-id="10d67-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="10d67-113">CORS の構成の詳細については、次を参照してください。 [ASP.NET Core の CORS にドキュメント](xref:security/cors)します。</span><span class="sxs-lookup"><span data-stu-id="10d67-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="10d67-114">SignalR では、正常に動作するために、以下の CORS ポリシーが必要です。</span><span class="sxs-lookup"><span data-stu-id="10d67-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="10d67-115">ポリシーには、期待するか (推奨されません) 任意のオリジンを許可する特定のオリジンを許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d67-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="10d67-116">HTTP メソッド`GET`と`POST`許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d67-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="10d67-117">認証を使用していない場合でも、資格情報を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="10d67-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="10d67-118">たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます。 `http://example.com` SignalR アプリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="10d67-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="10d67-119">SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="10d67-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="10d67-120">アクセス トークンのログ記録</span><span class="sxs-lookup"><span data-stu-id="10d67-120">Access token logging</span></span>

<span data-ttu-id="10d67-121">Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="10d67-121">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="10d67-122">標準を使用してとして一般に安全ではこの`Authorization`ヘッダー、クエリ文字列を含むが、多くの web サーバーが各要求の URL を記録します。</span><span class="sxs-lookup"><span data-stu-id="10d67-122">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="10d67-123">つまり、アクセス トークンをログに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="10d67-123">This means the access token may be included in logs.</span></span> <span data-ttu-id="10d67-124">この情報をログ記録を回避するために、web サーバーのログ設定を確認してください。</span><span class="sxs-lookup"><span data-stu-id="10d67-124">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="10d67-125">例外</span><span class="sxs-lookup"><span data-stu-id="10d67-125">Exceptions</span></span>

<span data-ttu-id="10d67-126">例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。</span><span class="sxs-lookup"><span data-stu-id="10d67-126">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="10d67-127">既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。</span><span class="sxs-lookup"><span data-stu-id="10d67-127">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="10d67-128">代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="10d67-128">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="10d67-129">この動作をオーバーライドするには、設定、 [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options)設定します。</span><span class="sxs-lookup"><span data-stu-id="10d67-129">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="10d67-130">バッファー管理</span><span class="sxs-lookup"><span data-stu-id="10d67-130">Buffer management</span></span>

<span data-ttu-id="10d67-131">SignalR では、受信および送信メッセージを管理するために、接続ごとのバッファーを使用します。</span><span class="sxs-lookup"><span data-stu-id="10d67-131">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="10d67-132">既定では、SignalR は、これらを 32 KB のバッファーを制限します。</span><span class="sxs-lookup"><span data-stu-id="10d67-132">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="10d67-133">これは、クライアントまたはサーバーに送信できる最大の可能なメッセージが 32 KB を意味します。</span><span class="sxs-lookup"><span data-stu-id="10d67-133">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="10d67-134">メッセージの接続によって消費されるメモリの最大量が 32 KB も意味します。</span><span class="sxs-lookup"><span data-stu-id="10d67-134">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="10d67-135">メッセージがこの制限より小さいが常にわかっている場合は、クライアントの大きいメッセージを送信して、そのまま使用するメモリを割り当てるサーバーを強制的にできることを防ぐためには、このサイズを削減できます。</span><span class="sxs-lookup"><span data-stu-id="10d67-135">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="10d67-136">同様に、メッセージがこの制限を超える場合は、増やすことができます。</span><span class="sxs-lookup"><span data-stu-id="10d67-136">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="10d67-137">ただし、注意をこの制限を増やすことつまりクライアントは追加メモリを割り当てるサーバーを起こすことと、アプリが処理できる同時接続数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="10d67-137">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="10d67-138">受信および送信メッセージの個別の制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:</span><span class="sxs-lookup"><span data-stu-id="10d67-138">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="10d67-139">`ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。</span><span class="sxs-lookup"><span data-stu-id="10d67-139">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="10d67-140">クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。</span><span class="sxs-lookup"><span data-stu-id="10d67-140">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="10d67-141">`TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。</span><span class="sxs-lookup"><span data-stu-id="10d67-141">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="10d67-142">サーバーが、メッセージを送信しようとしています。 場合 (ハブ メソッドの戻り値が含まれています)、この制限を超える、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="10d67-142">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="10d67-143">制限値を設定`0`制限を完全に無効にします。</span><span class="sxs-lookup"><span data-stu-id="10d67-143">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="10d67-144">ただし、これは十分注意して行ってください。</span><span class="sxs-lookup"><span data-stu-id="10d67-144">However, this should be done with extreme caution.</span></span> <span data-ttu-id="10d67-145">制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。</span><span class="sxs-lookup"><span data-stu-id="10d67-145">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="10d67-146">これは、アプリがサポートできる同時接続数を大幅に削減できますが、過度のメモリを割り当てられると、悪意のあるクライアントで使用できます。</span><span class="sxs-lookup"><span data-stu-id="10d67-146">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
