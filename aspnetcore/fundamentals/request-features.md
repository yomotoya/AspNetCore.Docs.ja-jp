---
title: "ASP.NET Core での要求機能"
author: ardalis
description: "HTTP 要求と ASP.NET Core のインターフェイスで定義されている応答に関連する web サーバーの実装の詳細情報について説明します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="a015a-104">ASP.NET Core での要求機能</span><span class="sxs-lookup"><span data-stu-id="a015a-104">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="a015a-105">によって[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a015a-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a015a-106">HTTP の要求と応答に関連する Web サーバーの実装の詳細が、インターフェイスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="a015a-106">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="a015a-107">これらのインターフェイスは、サーバーの実装とミドルウェアによって作成および変更、アプリケーションのホスティング パイプラインに使用されます。</span><span class="sxs-lookup"><span data-stu-id="a015a-107">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="a015a-108">機能のインターフェイス</span><span class="sxs-lookup"><span data-stu-id="a015a-108">Feature interfaces</span></span>

<span data-ttu-id="a015a-109">ASP.NET Core での HTTP 機能のインターフェイスの数を定義する`Microsoft.AspNetCore.Http.Features`サポートされる機能を識別するサーバーで使用されています。</span><span class="sxs-lookup"><span data-stu-id="a015a-109">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="a015a-110">次の機能のインターフェイスは、要求を処理し、応答を返します。</span><span class="sxs-lookup"><span data-stu-id="a015a-110">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="a015a-111">`IHttpRequestFeature`プロトコル、パス、クエリ文字列、ヘッダー、および本文を含む、HTTP 要求の構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-111">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="a015a-112">`IHttpResponseFeature`ステータス コード、ヘッダー、および応答の本文を含む、HTTP 応答の構造を定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-112">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="a015a-113">`IHttpAuthenticationFeature`基づくユーザーを識別するためのサポートを定義、`ClaimsPrincipal`認証ハンドラーを指定するとします。</span><span class="sxs-lookup"><span data-stu-id="a015a-113">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="a015a-114">`IHttpUpgradeFeature`サポートを定義[HTTP アップグレード](https://tools.ietf.org/html/rfc2616.html#section-14.42)サーバーがプロトコルの切り替えたい場合に使用するように、その他のプロトコルを指定するクライアントを許可します。</span><span class="sxs-lookup"><span data-stu-id="a015a-114">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="a015a-115">`IHttpBufferingFeature`要求と応答のバッファリングを無効にするためのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-115">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="a015a-116">`IHttpConnectionFeature`ローカルおよびリモート アドレスとポートのプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-116">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="a015a-117">`IHttpRequestLifetimeFeature`接続を中止するかどうか、要求は中止されました途中で、このようなクライアント接続が切断を検出するのサポートを定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-117">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="a015a-118">`IHttpSendFileFeature`ファイルを非同期的に送信するためのメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-118">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="a015a-119">`IHttpWebSocketFeature`Web ソケットをサポートするための API を定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-119">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="a015a-120">`IHttpRequestIdentifierFeature`要求を一意に識別する実装できるプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="a015a-120">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="a015a-121">`ISessionFeature`定義`ISessionFactory`と`ISession`ユーザー セッションをサポートするための抽象化します。</span><span class="sxs-lookup"><span data-stu-id="a015a-121">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="a015a-122">`ITlsConnectionFeature`クライアント証明書を取得するための API を定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-122">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="a015a-123">`ITlsTokenBindingFeature`TLS トークンのバインド パラメーターを使用して処理するメソッドを定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-123">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="a015a-124">`ISessionFeature`サーバー機能ではありませんが、によって実装される、 `SessionMiddleware` (を参照してください[アプリケーションの状態を管理](app-state.md))。</span><span class="sxs-lookup"><span data-stu-id="a015a-124">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="a015a-125">機能のコレクション</span><span class="sxs-lookup"><span data-stu-id="a015a-125">Feature collections</span></span>

<span data-ttu-id="a015a-126">`Features`プロパティ`HttpContext`のインターフェイスを取得および設定の現在の要求に対して使用可能な HTTP 機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="a015a-126">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="a015a-127">機能のコレクションは、要求のコンテキスト内でも変更可能であるために、コレクションを変更して、その他の機能のサポートを追加するミドルウェアを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a015a-127">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="a015a-128">ミドルウェアと要求の機能</span><span class="sxs-lookup"><span data-stu-id="a015a-128">Middleware and request features</span></span>

<span data-ttu-id="a015a-129">サーバーは、機能のコレクションの作成を担当するが、ミドルウェアはことができます両方をこのコレクションに追加し、コレクションからの機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="a015a-129">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="a015a-130">たとえば、`StaticFileMiddleware`にアクセスする、`IHttpSendFileFeature`機能します。</span><span class="sxs-lookup"><span data-stu-id="a015a-130">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="a015a-131">機能が存在する場合、物理パスからの要求された静的ファイルの送信に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a015a-131">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="a015a-132">それ以外の場合、低速な代替手段はファイルの送信に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a015a-132">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="a015a-133">利用可能であれば、`IHttpSendFileFeature`オペレーティング システムにファイルを開き、ネットワーク カードに直接カーネル モードのコピーを実行できるようにします。</span><span class="sxs-lookup"><span data-stu-id="a015a-133">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="a015a-134">さらに、ミドルウェアは、サーバーによって確立された機能のコレクションに追加できます。</span><span class="sxs-lookup"><span data-stu-id="a015a-134">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="a015a-135">既存の機能は、サーバーの機能を補完するミドルウェアを許可するミドルウェアによっても置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a015a-135">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="a015a-136">コレクションに追加された機能は他のミドルウェアまたは基になるアプリケーション自体、要求パイプラインの後ですぐに使用できます。</span><span class="sxs-lookup"><span data-stu-id="a015a-136">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="a015a-137">カスタム サーバーの実装と特定のミドルウェアの機能強化を組み合わせることにより、正確なアプリケーションに必要な機能のセットを構築できます。</span><span class="sxs-lookup"><span data-stu-id="a015a-137">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="a015a-138">これにより、サーバーでの変更を必要とせずに追加する機能がなく、攻撃を制限するため、最小限の機能のみが公開されることを確認画面領域とパフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="a015a-138">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="a015a-139">概要</span><span class="sxs-lookup"><span data-stu-id="a015a-139">Summary</span></span>

<span data-ttu-id="a015a-140">機能のインターフェイスは、特定の要求がサポートする可能性がある特定の HTTP 機能を定義します。</span><span class="sxs-lookup"><span data-stu-id="a015a-140">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="a015a-141">サーバーは、機能のコレクションと、そのサーバーでサポートされる機能の初期セットを定義するが、ミドルウェアは、これらの機能を強化するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="a015a-141">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a015a-142">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="a015a-142">Additional Resources</span></span>

* [<span data-ttu-id="a015a-143">サーバー</span><span class="sxs-lookup"><span data-stu-id="a015a-143">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="a015a-144">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="a015a-144">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="a015a-145">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="a015a-145">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
