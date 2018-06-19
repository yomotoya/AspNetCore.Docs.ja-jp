---
title: HTTP 要求の開始
author: stevejgordon
description: IHttpClientFactory インターフェイスを使用して、ASP.NET Core の論理 HttpClient インスタンスを管理する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/http-requests
ms.openlocfilehash: 1f2c7522a10220cd9520d78846d2e897115447c2
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838762"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="2702a-103">HTTP 要求の開始</span><span class="sxs-lookup"><span data-stu-id="2702a-103">Initiate HTTP requests</span></span>

<span data-ttu-id="2702a-104">寄稿者: [Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak)、[Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="2702a-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="2702a-105">`IHttpClientFactory` を登録し、アプリ内で [HttpClient](/dotnet/api/system.net.http.httpclient) インスタンスを構成して作成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-105">An `IHttpClientFactory` can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="2702a-106">次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="2702a-106">It offers the following benefits:</span></span>

* <span data-ttu-id="2702a-107">論理 `HttpClient` インスタンスの名前付けと構成を一元化します。</span><span class="sxs-lookup"><span data-stu-id="2702a-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="2702a-108">たとえば、"github" クライアントを登録して、GitHub にアクセスするように構成できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-108">For example, a "github" client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="2702a-109">既定のクライアントは、他の目的に登録できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="2702a-110">`HttpClient` でのハンドラーのデリゲートにより送信ミドルウェアの概念を体系化し、Polly ベースのミドルウェアでそれを利用するための拡張機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2702a-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="2702a-111">基になっている `HttpClientMessageHandler` インスタンスのプールと有効期間を管理し、`HttpClient` の有効期間を手動で管理するときに発生する一般的な DNS の問題を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="2702a-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="2702a-112">ファクトリによって作成されたクライアントから送信されるすべての要求に対し、(`ILogger` によって) 構成可能なログ エクスペリエンスを追加します。</span><span class="sxs-lookup"><span data-stu-id="2702a-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="2702a-113">利用パターン</span><span class="sxs-lookup"><span data-stu-id="2702a-113">Consumption patterns</span></span>

<span data-ttu-id="2702a-114">アプリで `IHttpClientFactory` を使用するには複数の方法があります。</span><span class="sxs-lookup"><span data-stu-id="2702a-114">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="2702a-115">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="2702a-115">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="2702a-116">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-116">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="2702a-117">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-117">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="2702a-118">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-118">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="2702a-119">これらの間に厳密な優劣はありません。</span><span class="sxs-lookup"><span data-stu-id="2702a-119">None of them are strictly superior to another.</span></span> <span data-ttu-id="2702a-120">どの方法が最善かは、アプリの制約に依存します。</span><span class="sxs-lookup"><span data-stu-id="2702a-120">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="2702a-121">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="2702a-121">Basic usage</span></span>

<span data-ttu-id="2702a-122">`IHttpClientFactory` は、Startup.cs の `ConfigureServices` メソッドの内部で `IServiceCollection` の `AddHttpClient` 拡張メソッドを呼び出すことによって登録できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-122">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `ConfigureServices` method in Startup.cs.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="2702a-123">登録が済むと、コードは、[依存関係の挿入](xref:fundamentals/dependency-injection) (DI) を使用してサービスを挿入できる任意の場所で、`IHttpClientFactory` を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-123">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="2702a-124">`IHttpClientFactory` を使用して、`HttpClient` インスタンスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-124">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="2702a-125">このように `IHttpClientFactory` を使用するのは、既存のアプリをリファクタリングする優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="2702a-125">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="2702a-126">`HttpClient` の使用方法に影響はありません。</span><span class="sxs-lookup"><span data-stu-id="2702a-126">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="2702a-127">現在 `HttpClient` インスタンスが作成されている場所で、それを `CreateClient` の呼び出しに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2702a-127">In places where `HttpClient` instances are currently created, replace those occurrences with a call to `CreateClient`.</span></span>

### <a name="named-clients"></a><span data-ttu-id="2702a-128">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-128">Named clients</span></span>

<span data-ttu-id="2702a-129">それぞれ構成が異なる複数の `HttpClient` をアプリで個別に使用する必要がある場合のオプションは、**名前付きクライアント**を使用することです。</span><span class="sxs-lookup"><span data-stu-id="2702a-129">If an app requires multiple distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="2702a-130">名前付き `HttpClient` の構成は、`ConfigureServices` での登録時に指定できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-130">Configuration for a named `HttpClient` can be specified during registration in `ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="2702a-131">上記のコードでは、`AddHttpClient` を呼び出して "github" という名前を提供しています。</span><span class="sxs-lookup"><span data-stu-id="2702a-131">In the preceding code, `AddHttpClient` is called, providing the name "github".</span></span> <span data-ttu-id="2702a-132">このクライアントには既定の構成がいくつか適用されています。つまり、GitHub API を使用するために必要なベース アドレスと 2 つのヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="2702a-132">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="2702a-133">`CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが作成されて、構成アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-133">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="2702a-134">名前付きクライアントを使用するには、文字列パラメーターを `CreateClient` に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-134">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="2702a-135">作成するクライアントの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="2702a-135">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="2702a-136">上記のコードでは、要求でホスト名を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2702a-136">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="2702a-137">クライアントに構成されているベース アドレスが使用されるため、パスだけを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-137">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="2702a-138">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-138">Typed clients</span></span>

<span data-ttu-id="2702a-139">型指定されたクライアントは、キーとして文字列を使用する必要なしに、名前付きクライアントと同じ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2702a-139">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="2702a-140">型指定されたクライアントの方法では、クライアントを使用するときに、IntelliSense とコンパイラのヘルプが提供されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-140">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="2702a-141">型指定されたクライアントは、特定の `HttpClient` を構成してそれと対話する 1 つの場所を提供します。</span><span class="sxs-lookup"><span data-stu-id="2702a-141">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="2702a-142">たとえば、単一の型指定されたクライアントを単一のバックエンド エンドポイントに対して使用し、そのエンドポイントを操作するすべてのロジックをカプセル化できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-142">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="2702a-143">もう 1 つの利点は、型指定されたクライアントは DI に対応しており、アプリ内の必要な場所に挿入できることです。</span><span class="sxs-lookup"><span data-stu-id="2702a-143">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="2702a-144">型指定されたクライアントは、コンストラクターで `HttpClient` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="2702a-144">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="2702a-145">上記のコードでは、構成が型指定されたクライアントに移動されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-145">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="2702a-146">`HttpClient` オブジェクトは、パブリック プロパティとして公開されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-146">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="2702a-147">`HttpClient` 機能を公開する API 固有のメソッドを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-147">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="2702a-148">`GetAspNetDocsIssues` メソッドは、GitHub リポジトリで最新の未解決の問題をクエリして解析するために必要なコードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="2702a-148">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="2702a-149">型指定されたクライアントを登録するには、ジェネリック `AddHttpClient` 拡張メソッドを `ConfigureServices` 内で使用して、型指定されたクライアントのクラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="2702a-149">To register a typed client, the generic `AddHttpClient` extension method can be used within `ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="2702a-150">型指定されたクライアントは、DI で一時的として登録されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-150">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="2702a-151">型指定されたクライアントは、直接挿入して使用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-151">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="2702a-152">好みに応じて、型指定されたクライアントのコンストラクターではなく、`ConfigureServices` での登録時に型指定されたクライアントの構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-152">If preferred, the configuration for a typed client can be specified during registration in `ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="2702a-153">型指定されたクライアントの内部に `HttpClient` を完全にカプセル化することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-153">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="2702a-154">プロパティとして公開するのではなく、`HttpClient` インスタンスを内部的に呼び出すパブリック メソッドとして提供することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-154">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="2702a-155">上記のコードでは、`HttpClient` はプライベート フィールドとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-155">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="2702a-156">外部呼び出しを行うすべてのアクセスは、`GetRepos` メソッドを通して行われます。</span><span class="sxs-lookup"><span data-stu-id="2702a-156">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="2702a-157">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="2702a-157">Generated clients</span></span>

<span data-ttu-id="2702a-158">`IHttpClientFactory` は、[Refit](https://github.com/paulcbetts/refit) などの他のサードパーティ製ライブラリと組み合わせて使用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-158">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="2702a-159">Refit は、.NET 用の REST ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="2702a-159">Refit is a REST library for .NET.</span></span> <span data-ttu-id="2702a-160">REST API をライブ インターフェイスに変換します。</span><span class="sxs-lookup"><span data-stu-id="2702a-160">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="2702a-161">インターフェイスの実装は `RestService` によって動的に生成され、`HttpClient` を使用して外部 HTTP の呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="2702a-161">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="2702a-162">インターフェイスと応答は、外部の API とその応答を表すように定義されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-162">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="2702a-163">型指定されたクライアントを追加し、Refit を使用して実装を生成できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-163">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="2702a-164">定義されたインターフェイスは、DI および Refit によって提供される実装で必要に応じて使用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-164">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="2702a-165">送信要求ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="2702a-165">Outgoing request middleware</span></span>

<span data-ttu-id="2702a-166">`HttpClient` は既に、送信 HTTP 要求用にリンクできるハンドラーのデリゲートの概念を備えています。</span><span class="sxs-lookup"><span data-stu-id="2702a-166">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="2702a-167">`IHttpClientFactory` を使用すると、各名前付きクライアントに適用するハンドラーを簡単に定義できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-167">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="2702a-168">複数のハンドラーを登録してチェーン化し、送信要求ミドルウェア パイプラインを構築できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-168">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="2702a-169">各ハンドラーは、送信要求の前と後に処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-169">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="2702a-170">このパターンは、ASP.NET Core での受信ミドルウェア パイプラインに似ています。</span><span class="sxs-lookup"><span data-stu-id="2702a-170">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="2702a-171">このパターンは、キャッシュ、エラー処理、シリアル化、ログ記録など、HTTP 要求に関する横断的関心事を管理するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="2702a-171">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="2702a-172">ハンドラーを作成するには、`DelegatingHandler` の派生クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="2702a-172">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="2702a-173">パイプライン内の次のハンドラーに要求を渡す前にコードを実行するように、`SendAsync` メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="2702a-173">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="2702a-174">上記のコードでは、基本的なハンドラーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-174">The preceding code defines a basic handler.</span></span> <span data-ttu-id="2702a-175">このコードは、X-API-KEY ヘッダーが要求に含まれていたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="2702a-175">It checks to see if an X-API-KEY header has been included on the request.</span></span> <span data-ttu-id="2702a-176">ヘッダーがない場合、HTTP 呼び出しを行わずに適切な応答を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-176">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="2702a-177">登録の間に、1 つ以上のハンドラーを `HttpClient` の構成に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-177">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="2702a-178">このタスクは、`IHttpClientBuilder` の拡張メソッドを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-178">This task is accomplished via extension methods on the `IHttpClientBuilder`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="2702a-179">上記のコードでは、`ValidateHeaderHandler` が DI で登録されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-179">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="2702a-180">ハンドラーは、DI で一時的として登録される**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="2702a-180">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="2702a-181">登録が済むと、`AddHttpMessageHandler` を呼び出してハンドラーの型を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-181">Once registered, `AddHttpMessageHandler` can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="2702a-182">実行する順序で、複数のハンドラーを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-182">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="2702a-183">最後の `HttpClientHandler` が要求を実行するまで、各ハンドラーは次のハンドラーをラップします。</span><span class="sxs-lookup"><span data-stu-id="2702a-183">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="2702a-184">Polly ベースのハンドラーを使用する</span><span class="sxs-lookup"><span data-stu-id="2702a-184">Use Polly-based handlers</span></span>

<span data-ttu-id="2702a-185">`IHttpClientFactory` は、[Polly](https://github.com/App-vNext/Polly) という名前の人気のあるサードパーティ製ライブラリと統合します。</span><span class="sxs-lookup"><span data-stu-id="2702a-185">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="2702a-186">Polly は、.NET 用の包括的な回復力および一時的エラー処理ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="2702a-186">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="2702a-187">開発者は、自然でスレッド セーフな方法を使用して、Retry、Circuit Breaker、Timeout、Bulkhead Isolation、Fallback などのポリシーを表現できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-187">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="2702a-188">構成されている `HttpClient` インスタンスで Polly ポリシーを使用できるようにするための、拡張メソッドが提供されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-188">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="2702a-189">ポリー拡張機能は、"Microsoft.Extensions.Http.Polly" と呼ばれる NuGet パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-189">The Polly extensions are available in a NuGet package called 'Microsoft.Extensions.Http.Polly'.</span></span> <span data-ttu-id="2702a-190">このパッケージは、"Microsoft.AspNetCore.App" メタパッケージには既定では含まれません。</span><span class="sxs-lookup"><span data-stu-id="2702a-190">This package is not included by default by the 'Microsoft.AspNetCore.App' metapackage.</span></span> <span data-ttu-id="2702a-191">この拡張機能を使用するには、プロジェクトに PackageReference を明示的に含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="2702a-191">To use the extensions, a PackageReference should be explicitly included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="2702a-192">このパッケージを復元した後は、拡張メソッドを使用してクライアントに Polly ベースのハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-192">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="2702a-193">一時的な障害を処理する</span><span class="sxs-lookup"><span data-stu-id="2702a-193">Handle transient faults</span></span>

<span data-ttu-id="2702a-194">外部 HTTP 呼び出しを行うときに発生する可能性のある最も一般的な障害は、一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="2702a-194">The most common faults you may expect to occur when making external HTTP calls will be transient.</span></span> <span data-ttu-id="2702a-195">`AddTransientHttpErrorPolicy` という便利な拡張メソッドが含まれており、一時的なエラーを処理するためのポリシーを定義できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-195">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="2702a-196">この拡張メソッドで構成されたポリシーは、`HttpRequestException`、HTTP 5xx 応答、および HTTP 408 応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="2702a-196">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="2702a-197">`AddTransientHttpErrorPolicy` 拡張メソッドは、`ConfigureServices` 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-197">The `AddTransientHttpErrorPolicy` extension can be used within `ConfigureServices`.</span></span> <span data-ttu-id="2702a-198">この拡張メソッドは、可能性のある一時的障害を表すエラーを処理するように構成された `PolicyBuilder` オブジェクトへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="2702a-198">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="2702a-199">上記のコードでは、`WaitAndRetryAsync` ポリシーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-199">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="2702a-200">失敗した要求は最大 3 回再試行され、再試行の間には 600 ミリ秒の遅延が設けられます。</span><span class="sxs-lookup"><span data-stu-id="2702a-200">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="2702a-201">ポリシーを動的に選択する</span><span class="sxs-lookup"><span data-stu-id="2702a-201">Dynamically select policies</span></span>

<span data-ttu-id="2702a-202">Polly ベースのハンドラーを追加するために使用できる追加の拡張メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="2702a-202">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="2702a-203">そのような拡張メソッドの 1 つは `AddPolicyHandler` であり、複数のオーバーロードを持ちます。</span><span class="sxs-lookup"><span data-stu-id="2702a-203">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="2702a-204">1 つのオーバーロードでは、適用するポリシーを定義するときに要求を検査できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-204">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="2702a-205">上記のコードでは、送信要求が GET の場合は、10 秒のタイムアウトが適用されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-205">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="2702a-206">他の HTTP メソッドの場合は、30 秒のタイムアウトが使用されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-206">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="2702a-207">複数の Polly ハンドラーを追加する</span><span class="sxs-lookup"><span data-stu-id="2702a-207">Add multiple Polly handlers</span></span>

<span data-ttu-id="2702a-208">Polly ポリシーを入れ子にして拡張機能を提供するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="2702a-208">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="2702a-209">前の例では、2 つのハンドラーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-209">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="2702a-210">最初のハンドラーは、`AddTransientHttpErrorPolicy` 拡張メソッドを使用して再試行ポリシーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2702a-210">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="2702a-211">失敗した要求は 3 回まで再試行されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-211">Failed requests are retried up to three times.</span></span> <span data-ttu-id="2702a-212">`AddTransientHttpErrorPolicy` の 2 番目の呼び出しでは、サーキット ブレーカー ポリシーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-212">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="2702a-213">試行が連続して 5 回失敗した場合、それ以上の外部要求は 30 秒間ブロックされます。</span><span class="sxs-lookup"><span data-stu-id="2702a-213">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="2702a-214">サーキット ブレーカー ポリシーはステートフルです。</span><span class="sxs-lookup"><span data-stu-id="2702a-214">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="2702a-215">このクライアントからのすべての呼び出しは、同じサーキット状態を共有します。</span><span class="sxs-lookup"><span data-stu-id="2702a-215">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="2702a-216">Polly レジストリからポリシーを追加する</span><span class="sxs-lookup"><span data-stu-id="2702a-216">Add policies from the Polly registry</span></span>

<span data-ttu-id="2702a-217">定期的に使用されるポリシーを管理するには、ポリシーを 1 回定義した後、`PolicyRegistry` でポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="2702a-217">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="2702a-218">提供されている拡張メソッドを使用することで、レジストリからポリシーを使用してハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-218">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="2702a-219">上記のコードでは、PolicyRegistry が `ServiceCollection` に追加され、2 つのポリシーがそれに登録されています。</span><span class="sxs-lookup"><span data-stu-id="2702a-219">In the preceding code, a PolicyRegistry is added to the `ServiceCollection` and two policies are registered with it.</span></span> <span data-ttu-id="2702a-220">レジストリからポリシーを使用するには、`AddPolicyHandlerFromRegistry` メソッドを使用して、適用するポリシーの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="2702a-220">In order to use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="2702a-221">`IHttpClientFactory` および Polly の統合について詳しくは、[Polly の Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="2702a-221">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="2702a-222">HttpClient と有効期間の管理</span><span class="sxs-lookup"><span data-stu-id="2702a-222">HttpClient and lifetime management</span></span>

<span data-ttu-id="2702a-223">`IHttpClientFactory` で `CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが返されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-223">Each time `CreateClient` is called on the `IHttpClientFactory`, a new instance of a `HttpClient` is returned.</span></span> <span data-ttu-id="2702a-224">名前付きクライアントごとに `HttpMessageHandler` が存在するようになります。</span><span class="sxs-lookup"><span data-stu-id="2702a-224">There will be a `HttpMessageHandler` per named client.</span></span> <span data-ttu-id="2702a-225">`IHttpClientFactory` は、リソースの消費量を減らすため、ファクトリによって作成された `HttpMessageHandler` のインスタンスをプールします。</span><span class="sxs-lookup"><span data-stu-id="2702a-225">`IHttpClientFactory` will pool the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="2702a-226">新しい `HttpClient` インスタンスを作成するとき、プールの `HttpMessageHandler` インスタンスの有効期間が切れていない場合はそれを再利用できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-226">A `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span> 

<span data-ttu-id="2702a-227">通常各ハンドラーは基になる HTTP 接続を独自に管理しており、必要以上に多くのハンドラーを作成すると接続が遅延する可能性があるため、ハンドラーをプールするのは望ましい方法です。</span><span class="sxs-lookup"><span data-stu-id="2702a-227">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections; creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="2702a-228">また、一部のハンドラーは接続を無期限に開いており、DNS の変更にハンドラーが対応できないことがあります。</span><span class="sxs-lookup"><span data-stu-id="2702a-228">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="2702a-229">ハンドラーの既定の有効期間は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="2702a-229">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="2702a-230">名前付きクライアントごとに、既定値をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-230">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="2702a-231">オーバーライドするには、クライアント作成時に返された `IHttpClientBuilder` で `SetHandlerLifetime` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2702a-231">To override it, call `SetHandlerLifetime` on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

## <a name="logging"></a><span data-ttu-id="2702a-232">ログの記録</span><span class="sxs-lookup"><span data-stu-id="2702a-232">Logging</span></span>

<span data-ttu-id="2702a-233">`IHttpClientFactory` によって作成されたクライアントは、すべての要求のログ メッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="2702a-233">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="2702a-234">既定のログ メッセージを見るには、ログの構成で適切な情報レベルを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2702a-234">You'll need to enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="2702a-235">要求ヘッダーのログなどの追加ログは、トレース レベルでのみ含まれます。</span><span class="sxs-lookup"><span data-stu-id="2702a-235">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="2702a-236">各クライアントに使用されるログのカテゴリには、クライアントの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2702a-236">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="2702a-237">たとえば、"MyNamedClient" という名前のクライアントは、カテゴリ `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` でメッセージをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="2702a-237">A client named "MyNamedClient", for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="2702a-238">"LogicalHandler" というサフィックスのメッセージは、要求ハンドラー パイプラインの外側で発生します。</span><span class="sxs-lookup"><span data-stu-id="2702a-238">Messages with the suffix of "LogicalHandler" occur on the outside of request handler pipeline.</span></span> <span data-ttu-id="2702a-239">要求では、パイプライン内の他のハンドラーが要求を処理する前に、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-239">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="2702a-240">応答では、他のパイプライン ハンドラーが応答を受け取った後で、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-240">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="2702a-241">ログ記録は、要求ハンドラー パイプラインの内部でも行われます。</span><span class="sxs-lookup"><span data-stu-id="2702a-241">Logging also occurs on the inside of the request handler pipeline.</span></span> <span data-ttu-id="2702a-242">"MyNamedClient" の例の場合、それらのメッセージはログ カテゴリ `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` に対して記録されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-242">In the case of the "MyNamedClient" example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="2702a-243">要求では、他のすべてのハンドラーが実行した後、要求がネットワークに送信される直前に、これが行われます。</span><span class="sxs-lookup"><span data-stu-id="2702a-243">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="2702a-244">応答では、このログ記録には、ハンドラー パイプラインを通じ戻される前の応答の状態が含まれます。</span><span class="sxs-lookup"><span data-stu-id="2702a-244">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="2702a-245">パイプラインの外側および内部でログを有効にすると、他のパイプライン ハンドラーによって行われた変更を検査できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-245">Enabling logging on the outside and inside of the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="2702a-246">これには、たとえば、要求ヘッダーや応答状態コードへの変更を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-246">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="2702a-247">ログのカテゴリにクライアントの名前を含めると、必要に応じて、特定の名前付きクライアントでログをフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="2702a-247">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="2702a-248">HttpMessageHandler を構成する</span><span class="sxs-lookup"><span data-stu-id="2702a-248">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="2702a-249">クライアントによって使用される内部 `HttpMessageHandler` の構成を制御することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="2702a-249">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="2702a-250">名前付きクライアントまたは型指定されたクライアントを追加すると、`IHttpClientBuilder` が返されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-250">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="2702a-251">`ConfigurePrimaryHttpMessageHandler` 拡張メソッドを使用して、デリゲートを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="2702a-251">The `ConfigurePrimaryHttpMessageHandler` extension method can be used to define a delegate.</span></span> <span data-ttu-id="2702a-252">デリゲートは、そのクライアントによって使用されるプライマリ `HttpMessageHandler` の作成と構成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2702a-252">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
