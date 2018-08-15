---
title: HTTP 要求の開始
author: stevejgordon
description: IHttpClientFactory インターフェイスを使用して、ASP.NET Core の論理 HttpClient インスタンスを管理する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/07/2018
uid: fundamentals/http-requests
ms.openlocfilehash: dd217cfed230ea92c31eeed64ec19838032dd224
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655233"
---
# <a name="initiate-http-requests"></a><span data-ttu-id="83965-103">HTTP 要求の開始</span><span class="sxs-lookup"><span data-stu-id="83965-103">Initiate HTTP requests</span></span>

<span data-ttu-id="83965-104">寄稿者: [Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak)、[Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="83965-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="83965-105">[IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) を登録し、アプリ内で [HttpClient](/dotnet/api/system.net.http.httpclient) インスタンスを構成して作成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-105">An [IHttpClientFactory](/dotnet/api/system.net.http.ihttpclientfactory) can be registered and used to configure and create [HttpClient](/dotnet/api/system.net.http.httpclient) instances in an app.</span></span> <span data-ttu-id="83965-106">次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="83965-106">It offers the following benefits:</span></span>

* <span data-ttu-id="83965-107">論理 `HttpClient` インスタンスの名前付けと構成を一元化します。</span><span class="sxs-lookup"><span data-stu-id="83965-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="83965-108">たとえば、*github* クライアントを登録して、GitHub にアクセスするように構成できます。</span><span class="sxs-lookup"><span data-stu-id="83965-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="83965-109">既定のクライアントは、他の目的に登録できます。</span><span class="sxs-lookup"><span data-stu-id="83965-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="83965-110">`HttpClient` でのハンドラーのデリゲートにより送信ミドルウェアの概念を体系化し、Polly ベースのミドルウェアでそれを利用するための拡張機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="83965-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="83965-111">基になっている `HttpClientMessageHandler` インスタンスのプールと有効期間を管理し、`HttpClient` の有効期間を手動で管理するときに発生する一般的な DNS の問題を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="83965-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="83965-112">ファクトリによって作成されたクライアントから送信されるすべての要求に対し、(`ILogger` によって) 構成可能なログ エクスペリエンスを追加します。</span><span class="sxs-lookup"><span data-stu-id="83965-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="83965-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="83965-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83965-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="83965-114">Prerequisites</span></span>

<span data-ttu-id="83965-115">.NET Framework をターゲットとするプロジェクトでは、[Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet パッケージのインストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="83965-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="83965-116">.NET Core をターゲットとし、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)を参照するプロジェクトには、既に `Microsoft.Extensions.Http` パッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="83965-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="83965-117">利用パターン</span><span class="sxs-lookup"><span data-stu-id="83965-117">Consumption patterns</span></span>

<span data-ttu-id="83965-118">アプリで `IHttpClientFactory` を使用するには複数の方法があります。</span><span class="sxs-lookup"><span data-stu-id="83965-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="83965-119">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="83965-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="83965-120">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="83965-121">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="83965-122">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="83965-123">これらの間に厳密な優劣はありません。</span><span class="sxs-lookup"><span data-stu-id="83965-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="83965-124">どの方法が最善かは、アプリの制約に依存します。</span><span class="sxs-lookup"><span data-stu-id="83965-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="83965-125">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="83965-125">Basic usage</span></span>

<span data-ttu-id="83965-126">`IHttpClientFactory` は、`Startup.ConfigureServices` メソッドの内部で `IServiceCollection` の `AddHttpClient` 拡張メソッドを呼び出すことによって登録できます。</span><span class="sxs-lookup"><span data-stu-id="83965-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="83965-127">登録が済むと、コードは、[依存関係の挿入](xref:fundamentals/dependency-injection) (DI) を使用してサービスを挿入できる任意の場所で、`IHttpClientFactory` を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="83965-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="83965-128">`IHttpClientFactory` を使用して、`HttpClient` インスタンスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="83965-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,20)]

<span data-ttu-id="83965-129">このように `IHttpClientFactory` を使用するのは、既存のアプリをリファクタリングする優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="83965-129">Using `IHttpClientFactory` in this fashion is a great way to refactor an existing app.</span></span> <span data-ttu-id="83965-130">`HttpClient` の使用方法に影響はありません。</span><span class="sxs-lookup"><span data-stu-id="83965-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="83965-131">現在 `HttpClient` インスタンスが作成されている場所で、それを [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient) の呼び出しに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="83965-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to [CreateClient](/dotnet/api/system.net.http.ihttpclientfactory.createclient).</span></span>

### <a name="named-clients"></a><span data-ttu-id="83965-132">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-132">Named clients</span></span>

<span data-ttu-id="83965-133">それぞれ構成が異なる多数の `HttpClient` をアプリで個別に使用する必要がある場合は、**名前付きクライアント**を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="83965-134">名前付き `HttpClient` の構成は、`Startup.ConfigureServices` での登録時に指定できます。</span><span class="sxs-lookup"><span data-stu-id="83965-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet2)]

<span data-ttu-id="83965-135">上記のコードでは、*github* という名前を指定して `AddHttpClient` を呼び出しています。</span><span class="sxs-lookup"><span data-stu-id="83965-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="83965-136">このクライアントには既定の構成がいくつか適用されています。つまり、GitHub API を使用するために必要なベース アドレスと 2 つのヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="83965-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="83965-137">`CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが作成されて、構成アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="83965-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="83965-138">名前付きクライアントを使用するには、文字列パラメーターを `CreateClient` に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="83965-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="83965-139">作成するクライアントの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="83965-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=20)]

<span data-ttu-id="83965-140">上記のコードでは、要求でホスト名を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="83965-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="83965-141">クライアントに構成されているベース アドレスが使用されるため、パスだけを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="83965-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="83965-142">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-142">Typed clients</span></span>

<span data-ttu-id="83965-143">型指定されたクライアントは、キーとして文字列を使用する必要なしに、名前付きクライアントと同じ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="83965-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="83965-144">型指定されたクライアントの方法では、クライアントを使用するときに、IntelliSense とコンパイラのヘルプが提供されます。</span><span class="sxs-lookup"><span data-stu-id="83965-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="83965-145">型指定されたクライアントは、特定の `HttpClient` を構成してそれと対話する 1 つの場所を提供します。</span><span class="sxs-lookup"><span data-stu-id="83965-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="83965-146">たとえば、単一の型指定されたクライアントを単一のバックエンド エンドポイントに対して使用し、そのエンドポイントを操作するすべてのロジックをカプセル化できます。</span><span class="sxs-lookup"><span data-stu-id="83965-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="83965-147">もう 1 つの利点は、型指定されたクライアントは DI に対応しており、アプリ内の必要な場所に挿入できることです。</span><span class="sxs-lookup"><span data-stu-id="83965-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="83965-148">型指定されたクライアントは、コンストラクターで `HttpClient` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="83965-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="83965-149">上記のコードでは、構成が型指定されたクライアントに移動されています。</span><span class="sxs-lookup"><span data-stu-id="83965-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="83965-150">`HttpClient` オブジェクトは、パブリック プロパティとして公開されます。</span><span class="sxs-lookup"><span data-stu-id="83965-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="83965-151">`HttpClient` 機能を公開する API 固有のメソッドを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="83965-152">`GetAspNetDocsIssues` メソッドは、GitHub リポジトリで最新の未解決の問題をクエリして解析するために必要なコードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="83965-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="83965-153">型指定されたクライアントを登録するには、ジェネリック `AddHttpClient` 拡張メソッドを `Startup.ConfigureServices` 内で使用して、型指定されたクライアントのクラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="83965-153">To register a typed client, the generic `AddHttpClient` extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet3)]

<span data-ttu-id="83965-154">型指定されたクライアントは、DI で一時的として登録されます。</span><span class="sxs-lookup"><span data-stu-id="83965-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="83965-155">型指定されたクライアントは、直接挿入して使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="83965-156">好みに応じて、型指定されたクライアントのコンストラクターではなく、`Startup.ConfigureServices` での登録時に型指定されたクライアントの構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="83965-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet4)]

<span data-ttu-id="83965-157">型指定されたクライアントの内部に `HttpClient` を完全にカプセル化することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="83965-158">プロパティとして公開するのではなく、`HttpClient` インスタンスを内部的に呼び出すパブリック メソッドとして提供することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/GitHub/RepoService.cs?name=snippet1&highlight=3)]

<span data-ttu-id="83965-159">上記のコードでは、`HttpClient` はプライベート フィールドとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="83965-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="83965-160">外部呼び出しを行うすべてのアクセスは、`GetRepos` メソッドを通して行われます。</span><span class="sxs-lookup"><span data-stu-id="83965-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="83965-161">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="83965-161">Generated clients</span></span>

<span data-ttu-id="83965-162">`IHttpClientFactory` は、[Refit](https://github.com/paulcbetts/refit) などの他のサードパーティ製ライブラリと組み合わせて使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="83965-163">Refit は、.NET 用の REST ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="83965-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="83965-164">REST API をライブ インターフェイスに変換します。</span><span class="sxs-lookup"><span data-stu-id="83965-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="83965-165">インターフェイスの実装は `RestService` によって動的に生成され、`HttpClient` を使用して外部 HTTP の呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="83965-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="83965-166">インターフェイスと応答は、外部の API とその応答を表すように定義されます。</span><span class="sxs-lookup"><span data-stu-id="83965-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="83965-167">型指定されたクライアントを追加し、Refit を使用して実装を生成できます。</span><span class="sxs-lookup"><span data-stu-id="83965-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="83965-168">定義されたインターフェイスは、DI および Refit によって提供される実装で必要に応じて使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="83965-169">送信要求ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="83965-169">Outgoing request middleware</span></span>

<span data-ttu-id="83965-170">`HttpClient` は既に、送信 HTTP 要求用にリンクできるハンドラーのデリゲートの概念を備えています。</span><span class="sxs-lookup"><span data-stu-id="83965-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="83965-171">`IHttpClientFactory` を使用すると、各名前付きクライアントに適用するハンドラーを簡単に定義できます。</span><span class="sxs-lookup"><span data-stu-id="83965-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="83965-172">複数のハンドラーを登録してチェーン化し、送信要求ミドルウェア パイプラインを構築できます。</span><span class="sxs-lookup"><span data-stu-id="83965-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="83965-173">各ハンドラーは、送信要求の前と後に処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="83965-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="83965-174">このパターンは、ASP.NET Core での受信ミドルウェア パイプラインに似ています。</span><span class="sxs-lookup"><span data-stu-id="83965-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="83965-175">このパターンは、キャッシュ、エラー処理、シリアル化、ログ記録など、HTTP 要求に関する横断的関心事を管理するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="83965-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="83965-176">ハンドラーを作成するには、`DelegatingHandler` の派生クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="83965-176">To create a handler, define a class deriving from `DelegatingHandler`.</span></span> <span data-ttu-id="83965-177">パイプライン内の次のハンドラーに要求を渡す前にコードを実行するように、`SendAsync` メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="83965-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="83965-178">上記のコードでは、基本的なハンドラーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="83965-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="83965-179">このコードは、`X-API-KEY` ヘッダーが要求に含まれていたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="83965-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="83965-180">ヘッダーがない場合、HTTP 呼び出しを行わずに適切な応答を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="83965-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="83965-181">登録の間に、1 つ以上のハンドラーを `HttpClient` の構成に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="83965-182">このタスクは、[IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder) の拡張メソッドを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="83965-182">This task is accomplished via extension methods on the [IHttpClientBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.ihttpclientbuilder).</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet5)]

<span data-ttu-id="83965-183">上記のコードでは、`ValidateHeaderHandler` が DI で登録されます。</span><span class="sxs-lookup"><span data-stu-id="83965-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="83965-184">ハンドラーは、DI で一時的として登録される**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="83965-184">The handler **must** be registered in DI as transient.</span></span> <span data-ttu-id="83965-185">登録が済むと、ハンドラーの型を渡して [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler) を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="83965-185">Once registered, [AddHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.addhttpmessagehandler) can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="83965-186">実行する順序で、複数のハンドラーを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="83965-186">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="83965-187">最後の `HttpClientHandler` が要求を実行するまで、各ハンドラーは次のハンドラーをラップします。</span><span class="sxs-lookup"><span data-stu-id="83965-187">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/Startup.cs?name=snippet6)]

## <a name="use-polly-based-handlers"></a><span data-ttu-id="83965-188">Polly ベースのハンドラーを使用する</span><span class="sxs-lookup"><span data-stu-id="83965-188">Use Polly-based handlers</span></span>

<span data-ttu-id="83965-189">`IHttpClientFactory` は、[Polly](https://github.com/App-vNext/Polly) という名前の人気のあるサードパーティ製ライブラリと統合します。</span><span class="sxs-lookup"><span data-stu-id="83965-189">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="83965-190">Polly は、.NET 用の包括的な回復力および一時的エラー処理ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="83965-190">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="83965-191">開発者は、自然でスレッド セーフな方法を使用して、Retry、Circuit Breaker、Timeout、Bulkhead Isolation、Fallback などのポリシーを表現できます。</span><span class="sxs-lookup"><span data-stu-id="83965-191">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="83965-192">構成されている `HttpClient` インスタンスで Polly ポリシーを使用できるようにするための、拡張メソッドが提供されています。</span><span class="sxs-lookup"><span data-stu-id="83965-192">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="83965-193">ポリー拡張機能は、"[Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/)" という NuGet パッケージで利用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-193">The Polly extensions are available in the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="83965-194">このパッケージは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)に含まれていません。</span><span class="sxs-lookup"><span data-stu-id="83965-194">This package isn't included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="83965-195">拡張を使用するには、プロジェクトに明示的な `<PackageReference />` を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="83965-195">To use the extensions, an explicit `<PackageReference />` should be included in the project.</span></span>

[!code-csharp[](http-requests/samples/HttpClientFactorySample.csproj?highlight=9)]

<span data-ttu-id="83965-196">このパッケージを復元した後は、拡張メソッドを使用してクライアントに Polly ベースのハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="83965-196">After restoring this package, extension methods are available to support adding Polly-based handlers to clients.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="83965-197">一時的な障害を処理する</span><span class="sxs-lookup"><span data-stu-id="83965-197">Handle transient faults</span></span>

<span data-ttu-id="83965-198">外部 HTTP を呼び出す際に発生する一般的な障害のほとんどは、一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="83965-198">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="83965-199">`AddTransientHttpErrorPolicy` という便利な拡張メソッドが含まれており、一時的なエラーを処理するためのポリシーを定義できます。</span><span class="sxs-lookup"><span data-stu-id="83965-199">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="83965-200">この拡張メソッドで構成されたポリシーは、`HttpRequestException`、HTTP 5xx 応答、および HTTP 408 応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="83965-200">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="83965-201">`AddTransientHttpErrorPolicy` 拡張メソッドは、`Startup.ConfigureServices` 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-201">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="83965-202">この拡張メソッドは、可能性のある一時的障害を表すエラーを処理するように構成された `PolicyBuilder` オブジェクトへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="83965-202">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet7)]

<span data-ttu-id="83965-203">上記のコードでは、`WaitAndRetryAsync` ポリシーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="83965-203">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="83965-204">失敗した要求は最大 3 回再試行され、再試行の間には 600 ミリ秒の遅延が設けられます。</span><span class="sxs-lookup"><span data-stu-id="83965-204">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="83965-205">ポリシーを動的に選択する</span><span class="sxs-lookup"><span data-stu-id="83965-205">Dynamically select policies</span></span>

<span data-ttu-id="83965-206">Polly ベースのハンドラーを追加するために使用できる追加の拡張メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="83965-206">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="83965-207">そのような拡張メソッドの 1 つは `AddPolicyHandler` であり、複数のオーバーロードを持ちます。</span><span class="sxs-lookup"><span data-stu-id="83965-207">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="83965-208">1 つのオーバーロードでは、適用するポリシーを定義するときに要求を検査できます。</span><span class="sxs-lookup"><span data-stu-id="83965-208">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet8)]

<span data-ttu-id="83965-209">上記のコードでは、送信要求が GET の場合は、10 秒のタイムアウトが適用されます。</span><span class="sxs-lookup"><span data-stu-id="83965-209">In the preceding code, if the outgoing request is a GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="83965-210">他の HTTP メソッドの場合は、30 秒のタイムアウトが使用されます。</span><span class="sxs-lookup"><span data-stu-id="83965-210">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="83965-211">複数の Polly ハンドラーを追加する</span><span class="sxs-lookup"><span data-stu-id="83965-211">Add multiple Polly handlers</span></span>

<span data-ttu-id="83965-212">Polly ポリシーを入れ子にして拡張機能を提供するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="83965-212">It is common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet9)]

<span data-ttu-id="83965-213">前の例では、2 つのハンドラーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="83965-213">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="83965-214">最初のハンドラーは、`AddTransientHttpErrorPolicy` 拡張メソッドを使用して再試行ポリシーを追加します。</span><span class="sxs-lookup"><span data-stu-id="83965-214">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="83965-215">失敗した要求は 3 回まで再試行されます。</span><span class="sxs-lookup"><span data-stu-id="83965-215">Failed requests are retried up to three times.</span></span> <span data-ttu-id="83965-216">`AddTransientHttpErrorPolicy` の 2 番目の呼び出しでは、サーキット ブレーカー ポリシーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="83965-216">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="83965-217">試行が連続して 5 回失敗した場合、それ以上の外部要求は 30 秒間ブロックされます。</span><span class="sxs-lookup"><span data-stu-id="83965-217">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="83965-218">サーキット ブレーカー ポリシーはステートフルです。</span><span class="sxs-lookup"><span data-stu-id="83965-218">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="83965-219">このクライアントからのすべての呼び出しは、同じサーキット状態を共有します。</span><span class="sxs-lookup"><span data-stu-id="83965-219">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="83965-220">Polly レジストリからポリシーを追加する</span><span class="sxs-lookup"><span data-stu-id="83965-220">Add policies from the Polly registry</span></span>

<span data-ttu-id="83965-221">定期的に使用されるポリシーを管理するには、ポリシーを 1 回定義した後、`PolicyRegistry` でポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="83965-221">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="83965-222">提供されている拡張メソッドを使用することで、レジストリからポリシーを使用してハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="83965-222">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet10)]

<span data-ttu-id="83965-223">上記のコードでは、`PolicyRegistry` が `ServiceCollection` に追加されるときに 2 つのポリシーが登録されています。</span><span class="sxs-lookup"><span data-stu-id="83965-223">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="83965-224">レジストリからポリシーを使用するには、適用するポリシーの名前を渡して `AddPolicyHandlerFromRegistry` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="83965-224">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="83965-225">`IHttpClientFactory` および Polly の統合について詳しくは、[Polly の Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="83965-225">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="83965-226">HttpClient と有効期間の管理</span><span class="sxs-lookup"><span data-stu-id="83965-226">HttpClient and lifetime management</span></span>

<span data-ttu-id="83965-227">`IHttpClientFactory` で `CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが返されます。</span><span class="sxs-lookup"><span data-stu-id="83965-227">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="83965-228">名前の付いたクライアントごとに [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) があります。</span><span class="sxs-lookup"><span data-stu-id="83965-228">There's an [HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler) per named client.</span></span> <span data-ttu-id="83965-229">`IHttpClientFactory` は、リソースの消費量を減らすため、ファクトリによって作成された `HttpMessageHandler` のインスタンスをプールします。</span><span class="sxs-lookup"><span data-stu-id="83965-229">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="83965-230">新しい `HttpClient` インスタンスを作成するときに、プールの `HttpMessageHandler` インスタンスの有効期間が切れていない場合はそれを再利用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="83965-230">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="83965-231">通常、各ハンドラーでは基になる HTTP 接続が独自に管理されるため、ハンドラーはプールすることが望まれます。</span><span class="sxs-lookup"><span data-stu-id="83965-231">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="83965-232">必要以上のハンドラーを作成すると、接続に遅延が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="83965-232">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="83965-233">また、一部のハンドラーは接続を無期限に開いており、DNS の変更にハンドラーが対応できないことがあります。</span><span class="sxs-lookup"><span data-stu-id="83965-233">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="83965-234">ハンドラーの既定の有効期間は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="83965-234">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="83965-235">名前付きクライアントごとに、既定値をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="83965-235">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="83965-236">オーバーライドするには、クライアント作成時に返された `IHttpClientBuilder` で [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="83965-236">To override it, call [SetHandlerLifetime](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.sethandlerlifetime) on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet11)]

<span data-ttu-id="83965-237">クライアントを破棄する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="83965-237">Disposal of the client isn't required.</span></span> <span data-ttu-id="83965-238">破棄すると送信要求がキャンセルされ、[Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose) 呼び出し後には指定された `HttpClient` インスタンスを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="83965-238">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling [Dispose](/dotnet/api/system.idisposable.dispose#System_IDisposable_Dispose).</span></span> <span data-ttu-id="83965-239">`IHttpClientFactory` は、`HttpClient` インスタンスによって使用されたリソースの追跡と破棄を行います。</span><span class="sxs-lookup"><span data-stu-id="83965-239">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="83965-240">通常、`HttpClient` インスタンスは、破棄を必要としない .NET オブジェクトとして扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="83965-240">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="83965-241">`IHttpClientFactory` の登場以前は、1 つの `HttpClient` インスタンスを長い期間使い続けるパターンが一般的に使用されていました。</span><span class="sxs-lookup"><span data-stu-id="83965-241">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="83965-242">`IHttpClientFactory` に移行した後は、このパターンは不要になります。</span><span class="sxs-lookup"><span data-stu-id="83965-242">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="83965-243">ログの記録</span><span class="sxs-lookup"><span data-stu-id="83965-243">Logging</span></span>

<span data-ttu-id="83965-244">`IHttpClientFactory` によって作成されたクライアントは、すべての要求のログ メッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="83965-244">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="83965-245">既定のログ メッセージを見るには、ログの構成で適切な情報レベルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="83965-245">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="83965-246">要求ヘッダーのログなどの追加ログは、トレース レベルでのみ含まれます。</span><span class="sxs-lookup"><span data-stu-id="83965-246">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="83965-247">各クライアントに使用されるログのカテゴリには、クライアントの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="83965-247">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="83965-248">たとえば、*MyNamedClient* という名前のクライアントは、カテゴリ `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` でメッセージをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="83965-248">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="83965-249">*LogicalHandler* というサフィックスが付いたメッセージが、要求ハンドラーのパイプラインの外部で発生します。</span><span class="sxs-lookup"><span data-stu-id="83965-249">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="83965-250">要求では、パイプライン内の他のハンドラーが要求を処理する前に、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="83965-250">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="83965-251">応答では、他のパイプライン ハンドラーが応答を受け取った後で、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="83965-251">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="83965-252">ログ記録は、要求ハンドラーのパイプラインの内部でも行われます。</span><span class="sxs-lookup"><span data-stu-id="83965-252">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="83965-253">*MyNamedClient* の例では、それらのメッセージはログ カテゴリ `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` に対して記録されます。</span><span class="sxs-lookup"><span data-stu-id="83965-253">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="83965-254">要求では、他のすべてのハンドラーが実行した後、要求がネットワークに送信される直前に、これが行われます。</span><span class="sxs-lookup"><span data-stu-id="83965-254">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="83965-255">応答では、このログ記録には、ハンドラー パイプラインを通じ戻される前の応答の状態が含まれます。</span><span class="sxs-lookup"><span data-stu-id="83965-255">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="83965-256">パイプラインの外部と内部でログを有効にすると、他のパイプライン ハンドラーによる変更を検査できます。</span><span class="sxs-lookup"><span data-stu-id="83965-256">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="83965-257">これには、たとえば、要求ヘッダーや応答状態コードへの変更を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="83965-257">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="83965-258">ログのカテゴリにクライアントの名前を含めると、必要に応じて、特定の名前付きクライアントでログをフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="83965-258">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="83965-259">HttpMessageHandler を構成する</span><span class="sxs-lookup"><span data-stu-id="83965-259">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="83965-260">クライアントによって使用される内部 `HttpMessageHandler` の構成を制御することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="83965-260">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="83965-261">名前付きクライアントまたは型指定されたクライアントを追加すると、`IHttpClientBuilder` が返されます。</span><span class="sxs-lookup"><span data-stu-id="83965-261">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="83965-262">デリゲートを定義するために [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) 拡張メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="83965-262">The [ConfigurePrimaryHttpMessageHandler](/dotnet/api/microsoft.extensions.dependencyinjection.httpclientbuilderextensions.configureprimaryhttpmessagehandler) extension method can be used to define a delegate.</span></span> <span data-ttu-id="83965-263">デリゲートは、そのクライアントによって使用されるプライマリ `HttpMessageHandler` の作成と構成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="83965-263">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/Startup.cs?name=snippet12)]
