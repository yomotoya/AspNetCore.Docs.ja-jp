---
title: ASP.NET Core で IHttpClientFactory を使用して HTTP 要求を行う
author: stevejgordon
description: IHttpClientFactory インターフェイスを使用して、ASP.NET Core の論理 HttpClient インスタンスを管理する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 540f14ad2b290d276436033a94d4c815888e5a95
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/11/2019
ms.locfileid: "65536003"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="3dcb6-103">ASP.NET Core で IHttpClientFactory を使用して HTTP 要求を行う</span><span class="sxs-lookup"><span data-stu-id="3dcb6-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

<span data-ttu-id="3dcb6-104">寄稿者: [Glenn Condron](https://github.com/glennc)、[Ryan Nowak](https://github.com/rynowak)、[Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="3dcb6-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="3dcb6-105">アプリ内で <xref:System.Net.Http.HttpClient> インスタンスを構成して作成するために、<xref:System.Net.Http.IHttpClientFactory> を登録して使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="3dcb6-106">次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-106">It offers the following benefits:</span></span>

* <span data-ttu-id="3dcb6-107">論理 `HttpClient` インスタンスの名前付けと構成を一元化します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="3dcb6-108">たとえば、*github* クライアントを登録して、GitHub にアクセスするように構成できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-108">For example, a *github* client can be registered and configured to access GitHub.</span></span> <span data-ttu-id="3dcb6-109">既定のクライアントは、他の目的に登録できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-109">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="3dcb6-110">`HttpClient` でのハンドラーのデリゲートにより送信ミドルウェアの概念を体系化し、Polly ベースのミドルウェアでそれを利用するための拡張機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="3dcb6-111">基になっている `HttpClientMessageHandler` インスタンスのプールと有効期間を管理し、`HttpClient` の有効期間を手動で管理するときに発生する一般的な DNS の問題を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-111">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="3dcb6-112">ファクトリによって作成されたクライアントから送信されるすべての要求に対し、(`ILogger` によって) 構成可能なログ エクスペリエンスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-112">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="3dcb6-113">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range="<= aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="3dcb6-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-114">Prerequisites</span></span>

<span data-ttu-id="3dcb6-115">.NET Framework をターゲットとするプロジェクトでは、[Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet パッケージのインストールが必要です。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-115">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="3dcb6-116">.NET Core をターゲットとし、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)を参照するプロジェクトには、既に `Microsoft.Extensions.Http` パッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-116">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

::: moniker-end

## <a name="consumption-patterns"></a><span data-ttu-id="3dcb6-117">利用パターン</span><span class="sxs-lookup"><span data-stu-id="3dcb6-117">Consumption patterns</span></span>

<span data-ttu-id="3dcb6-118">アプリで `IHttpClientFactory` を使用するには複数の方法があります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-118">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="3dcb6-119">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="3dcb6-119">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="3dcb6-120">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-120">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="3dcb6-121">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-121">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="3dcb6-122">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-122">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="3dcb6-123">これらの間に厳密な優劣はありません。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-123">None of them are strictly superior to another.</span></span> <span data-ttu-id="3dcb6-124">どの方法が最善かは、アプリの制約に依存します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-124">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="3dcb6-125">基本的な使用方法</span><span class="sxs-lookup"><span data-stu-id="3dcb6-125">Basic usage</span></span>

<span data-ttu-id="3dcb6-126">`IHttpClientFactory` は、`Startup.ConfigureServices` メソッドの内部で `IServiceCollection` の `AddHttpClient` 拡張メソッドを呼び出すことによって登録できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-126">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="3dcb6-127">登録が済むと、コードは、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) を使用してサービスを挿入できる任意の場所で、`IHttpClientFactory` を受け取ることができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-127">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3dcb6-128">`IHttpClientFactory` を使用して、`HttpClient` インスタンスを作成できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-128">The `IHttpClientFactory` can be used to create a `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="3dcb6-129">このように `IHttpClientFactory` を使用するのは、既存のアプリをリファクタリングする優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-129">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="3dcb6-130">`HttpClient` の使用方法に影響はありません。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-130">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="3dcb6-131">現在 `HttpClient` インスタンスが作成されている場所で、それを <xref:System.Net.Http.IHttpClientFactory.CreateClient*> の呼び出しに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-131">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="3dcb6-132">名前付きクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-132">Named clients</span></span>

<span data-ttu-id="3dcb6-133">それぞれ構成が異なる多数の `HttpClient` をアプリで個別に使用する必要がある場合は、**名前付きクライアント**を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-133">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="3dcb6-134">名前付き `HttpClient` の構成は、`Startup.ConfigureServices` での登録時に指定できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-134">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="3dcb6-135">上記のコードでは、*github* という名前を指定して `AddHttpClient` を呼び出しています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-135">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="3dcb6-136">このクライアントには既定の構成がいくつか適用されています。つまり、GitHub API を使用するために必要なベース アドレスと 2 つのヘッダーです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-136">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="3dcb6-137">`CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが作成されて、構成アクションが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-137">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="3dcb6-138">名前付きクライアントを使用するには、文字列パラメーターを `CreateClient` に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-138">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="3dcb6-139">作成するクライアントの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-139">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="3dcb6-140">上記のコードでは、要求でホスト名を指定する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-140">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="3dcb6-141">クライアントに構成されているベース アドレスが使用されるため、パスだけを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-141">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="3dcb6-142">型指定されたクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-142">Typed clients</span></span>

<span data-ttu-id="3dcb6-143">型指定されたクライアントは、キーとして文字列を使用する必要なしに、名前付きクライアントと同じ機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-143">Typed clients provide the same capabilities as named clients without the need to use strings as keys.</span></span> <span data-ttu-id="3dcb6-144">型指定されたクライアントの方法では、クライアントを使用するときに、IntelliSense とコンパイラのヘルプが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-144">The typed client approach provides IntelliSense and compiler help when consuming clients.</span></span> <span data-ttu-id="3dcb6-145">型指定されたクライアントは、特定の `HttpClient` を構成してそれと対話する 1 つの場所を提供します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-145">They provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="3dcb6-146">たとえば、単一の型指定されたクライアントを単一のバックエンド エンドポイントに対して使用し、そのエンドポイントを操作するすべてのロジックをカプセル化できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-146">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span> <span data-ttu-id="3dcb6-147">もう 1 つの利点は、型指定されたクライアントは DI に対応しており、アプリ内の必要な場所に挿入できることです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-147">Another advantage is that they work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="3dcb6-148">型指定されたクライアントは、コンストラクターで `HttpClient` パラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-148">A typed client accepts a `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="3dcb6-149">上記のコードでは、構成が型指定されたクライアントに移動されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-149">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="3dcb6-150">`HttpClient` オブジェクトは、パブリック プロパティとして公開されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-150">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="3dcb6-151">`HttpClient` 機能を公開する API 固有のメソッドを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-151">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="3dcb6-152">`GetAspNetDocsIssues` メソッドは、GitHub リポジトリで最新の未解決の問題をクエリして解析するために必要なコードをカプセル化します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-152">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="3dcb6-153">型指定されたクライアントを登録するには、ジェネリック <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> 拡張メソッドを `Startup.ConfigureServices` 内で使用して、型指定されたクライアントのクラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-153">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="3dcb6-154">型指定されたクライアントは、DI で一時的として登録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-154">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="3dcb6-155">型指定されたクライアントは、直接挿入して使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-155">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="3dcb6-156">好みに応じて、型指定されたクライアントのコンストラクターではなく、`Startup.ConfigureServices` での登録時に型指定されたクライアントの構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-156">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="3dcb6-157">型指定されたクライアントの内部に `HttpClient` を完全にカプセル化することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-157">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="3dcb6-158">プロパティとして公開するのではなく、`HttpClient` インスタンスを内部的に呼び出すパブリック メソッドとして提供することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-158">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="3dcb6-159">上記のコードでは、`HttpClient` はプライベート フィールドとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-159">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="3dcb6-160">外部呼び出しを行うすべてのアクセスは、`GetRepos` メソッドを通して行われます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-160">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="3dcb6-161">生成されたクライアント</span><span class="sxs-lookup"><span data-stu-id="3dcb6-161">Generated clients</span></span>

<span data-ttu-id="3dcb6-162">`IHttpClientFactory` は、[Refit](https://github.com/paulcbetts/refit) などの他のサードパーティ製ライブラリと組み合わせて使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-162">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="3dcb6-163">Refit は、.NET 用の REST ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-163">Refit is a REST library for .NET.</span></span> <span data-ttu-id="3dcb6-164">REST API をライブ インターフェイスに変換します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-164">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="3dcb6-165">インターフェイスの実装は `RestService` によって動的に生成され、`HttpClient` を使用して外部 HTTP の呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-165">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="3dcb6-166">インターフェイスと応答は、外部の API とその応答を表すように定義されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-166">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="3dcb6-167">型指定されたクライアントを追加し、Refit を使用して実装を生成できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-167">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="3dcb6-168">定義されたインターフェイスは、DI および Refit によって提供される実装で必要に応じて使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-168">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="3dcb6-169">送信要求ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="3dcb6-169">Outgoing request middleware</span></span>

<span data-ttu-id="3dcb6-170">`HttpClient` は既に、送信 HTTP 要求用にリンクできるハンドラーのデリゲートの概念を備えています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-170">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="3dcb6-171">`IHttpClientFactory` を使用すると、各名前付きクライアントに適用するハンドラーを簡単に定義できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-171">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="3dcb6-172">複数のハンドラーを登録してチェーン化し、送信要求ミドルウェア パイプラインを構築できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-172">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="3dcb6-173">各ハンドラーは、送信要求の前と後に処理を実行できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-173">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="3dcb6-174">このパターンは、ASP.NET Core での受信ミドルウェア パイプラインに似ています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-174">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="3dcb6-175">このパターンは、キャッシュ、エラー処理、シリアル化、ログ記録など、HTTP 要求に関する横断的関心事を管理するためのメカニズムを提供します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-175">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="3dcb6-176">ハンドラーを作成するには、<xref:System.Net.Http.DelegatingHandler> の派生クラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-176">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="3dcb6-177">パイプライン内の次のハンドラーに要求を渡す前にコードを実行するように、`SendAsync` メソッドをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-177">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="3dcb6-178">上記のコードでは、基本的なハンドラーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-178">The preceding code defines a basic handler.</span></span> <span data-ttu-id="3dcb6-179">このコードは、`X-API-KEY` ヘッダーが要求に含まれていたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-179">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="3dcb6-180">ヘッダーがない場合、HTTP 呼び出しを行わずに適切な応答を返すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-180">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="3dcb6-181">登録の間に、1 つ以上のハンドラーを `HttpClient` の構成に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-181">During registration, one or more handlers can be added to the configuration for a `HttpClient`.</span></span> <span data-ttu-id="3dcb6-182">このタスクは、<xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> の拡張メソッドを使用して実行されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-182">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3dcb6-183">上記のコードでは、`ValidateHeaderHandler` が DI で登録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-183">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3dcb6-184">`IHttpClientFactory` では、ハンドラーごとに個別の DI スコープが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-184">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="3dcb6-185">ハンドラーは、任意のスコープのサービスに自由に依存することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-185">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="3dcb6-186">ハンドラーが依存するサービスは、ハンドラーが破棄されるときに破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-186">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="3dcb6-187">登録が済むと、<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> を呼び出してハンドラーの型を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-187">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3dcb6-188">上記のコードでは、`ValidateHeaderHandler` が DI で登録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-188">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="3dcb6-189">ハンドラーは、スコープではなく、一時的なサービスとして DI で登録する**必要があります**。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-189">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="3dcb6-190">ハンドラーがスコープ サービスとして登録されていて、ハンドラーが依存するサービスが破棄可能だった場合、ハンドラーがスコープ外に移動する前にハンドラーのサービスが破棄される可能性があります。この場合、ハンドラーは失敗します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-190">If the handler is registered as a scoped service and any services that the handler depends upon are disposable, the handler's services could be disposed before the handler goes out of scope, which would cause the handler to fail.</span></span>

<span data-ttu-id="3dcb6-191">登録が済むと、<xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> を呼び出してハンドラーの型を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-191">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

::: moniker-end

<span data-ttu-id="3dcb6-192">実行する順序で、複数のハンドラーを登録することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-192">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="3dcb6-193">最後の `HttpClientHandler` が要求を実行するまで、各ハンドラーは次のハンドラーをラップします。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-193">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="3dcb6-194">次の方法のいずれかを使って、要求ごとの状態をメッセージ ハンドラーと共有します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-194">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="3dcb6-195">`HttpRequestMessage.Properties` を使ってデータをハンドラーに渡します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-195">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="3dcb6-196">`IHttpContextAccessor` を使って現在の要求にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-196">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="3dcb6-197">データを渡すカスタムの `AsyncLocal` ストレージ オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-197">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="3dcb6-198">Polly ベースのハンドラーを使用する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-198">Use Polly-based handlers</span></span>

<span data-ttu-id="3dcb6-199">`IHttpClientFactory` は、[Polly](https://github.com/App-vNext/Polly) という名前の人気のあるサードパーティ製ライブラリと統合します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-199">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="3dcb6-200">Polly は、.NET 用の包括的な回復力および一時的エラー処理ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-200">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="3dcb6-201">開発者は、自然でスレッド セーフな方法を使用して、Retry、Circuit Breaker、Timeout、Bulkhead Isolation、Fallback などのポリシーを表現できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-201">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="3dcb6-202">構成されている `HttpClient` インスタンスで Polly ポリシーを使用できるようにするための、拡張メソッドが提供されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-202">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="3dcb6-203">Polly の拡張機能は、次のようになっています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-203">The Polly extensions:</span></span>

* <span data-ttu-id="3dcb6-204">クライアントへの Polly ベースのハンドラーの追加がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-204">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="3dcb6-205">[Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet パッケージのインストール後に使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-205">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="3dcb6-206">このパッケージは、ASP.NET Core 共有フレームワークに含まれていません。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-206">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="3dcb6-207">一時的な障害を処理する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-207">Handle transient faults</span></span>

<span data-ttu-id="3dcb6-208">外部 HTTP を呼び出す際に発生する一般的な障害のほとんどは、一時的なものです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-208">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="3dcb6-209">`AddTransientHttpErrorPolicy` という便利な拡張メソッドが含まれており、一時的なエラーを処理するためのポリシーを定義できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-209">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="3dcb6-210">この拡張メソッドで構成されたポリシーは、`HttpRequestException`、HTTP 5xx 応答、および HTTP 408 応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-210">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="3dcb6-211">`AddTransientHttpErrorPolicy` 拡張メソッドは、`Startup.ConfigureServices` 内で使用できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-211">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3dcb6-212">この拡張メソッドは、可能性のある一時的障害を表すエラーを処理するように構成された `PolicyBuilder` オブジェクトへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-212">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="3dcb6-213">上記のコードでは、`WaitAndRetryAsync` ポリシーが定義されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-213">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="3dcb6-214">失敗した要求は最大 3 回再試行され、再試行の間には 600 ミリ秒の遅延が設けられます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-214">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="3dcb6-215">ポリシーを動的に選択する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-215">Dynamically select policies</span></span>

<span data-ttu-id="3dcb6-216">Polly ベースのハンドラーを追加するために使用できる追加の拡張メソッドが存在します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-216">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="3dcb6-217">そのような拡張メソッドの 1 つは `AddPolicyHandler` であり、複数のオーバーロードを持ちます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-217">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="3dcb6-218">1 つのオーバーロードでは、適用するポリシーを定義するときに要求を検査できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-218">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="3dcb6-219">上記のコードでは、送信要求が HTTP GET の場合は、10 秒のタイムアウトが適用されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-219">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="3dcb6-220">他の HTTP メソッドの場合は、30 秒のタイムアウトが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-220">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="3dcb6-221">複数の Polly ハンドラーを追加する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-221">Add multiple Polly handlers</span></span>

<span data-ttu-id="3dcb6-222">Polly ポリシーを入れ子にして拡張機能を提供するのが一般的です。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-222">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="3dcb6-223">前の例では、2 つのハンドラーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-223">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="3dcb6-224">最初のハンドラーは、`AddTransientHttpErrorPolicy` 拡張メソッドを使用して再試行ポリシーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-224">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="3dcb6-225">失敗した要求は 3 回まで再試行されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-225">Failed requests are retried up to three times.</span></span> <span data-ttu-id="3dcb6-226">`AddTransientHttpErrorPolicy` の 2 番目の呼び出しでは、サーキット ブレーカー ポリシーが追加されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-226">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="3dcb6-227">試行が連続して 5 回失敗した場合、それ以上の外部要求は 30 秒間ブロックされます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-227">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="3dcb6-228">サーキット ブレーカー ポリシーはステートフルです。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-228">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="3dcb6-229">このクライアントからのすべての呼び出しは、同じサーキット状態を共有します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-229">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="3dcb6-230">Polly レジストリからポリシーを追加する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-230">Add policies from the Polly registry</span></span>

<span data-ttu-id="3dcb6-231">定期的に使用されるポリシーを管理するには、ポリシーを 1 回定義した後、`PolicyRegistry` でポリシーを登録します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-231">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="3dcb6-232">提供されている拡張メソッドを使用することで、レジストリからポリシーを使用してハンドラーを追加できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-232">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="3dcb6-233">上記のコードでは、`PolicyRegistry` が `ServiceCollection` に追加されるときに 2 つのポリシーが登録されています。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-233">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="3dcb6-234">レジストリからポリシーを使用するには、適用するポリシーの名前を渡して `AddPolicyHandlerFromRegistry` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-234">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="3dcb6-235">`IHttpClientFactory` および Polly の統合について詳しくは、[Polly の Wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-235">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="3dcb6-236">HttpClient と有効期間の管理</span><span class="sxs-lookup"><span data-stu-id="3dcb6-236">HttpClient and lifetime management</span></span>

<span data-ttu-id="3dcb6-237">`IHttpClientFactory` で `CreateClient` を呼び出すたびに、`HttpClient` の新しいインスタンスが返されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-237">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="3dcb6-238">名前付きクライアントごとに <xref:System.Net.Http.HttpMessageHandler> が存在します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-238">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="3dcb6-239">ファクトリによって `HttpMessageHandler` インスタンスの有効期間が管理されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-239">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="3dcb6-240">`IHttpClientFactory` は、リソースの消費量を減らすため、ファクトリによって作成された `HttpMessageHandler` のインスタンスをプールします。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-240">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="3dcb6-241">新しい `HttpClient` インスタンスを作成するときに、プールの `HttpMessageHandler` インスタンスの有効期間が切れていない場合はそれを再利用する場合があります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-241">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="3dcb6-242">通常、各ハンドラーでは基になる HTTP 接続が独自に管理されるため、ハンドラーはプールすることが望まれます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-242">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="3dcb6-243">必要以上のハンドラーを作成すると、接続に遅延が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-243">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="3dcb6-244">また、一部のハンドラーは接続を無期限に開いており、DNS の変更にハンドラーが対応できないことがあります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-244">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="3dcb6-245">ハンドラーの既定の有効期間は 2 分です。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-245">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="3dcb6-246">名前付きクライアントごとに、既定値をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-246">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="3dcb6-247">オーバーライドするには、クライアント作成時に返された `IHttpClientBuilder` で <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-247">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="3dcb6-248">クライアントを破棄する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-248">Disposal of the client isn't required.</span></span> <span data-ttu-id="3dcb6-249">破棄すると送信要求がキャンセルされ、<xref:System.IDisposable.Dispose*> の呼び出し後には指定された `HttpClient` インスタンスを使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-249">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="3dcb6-250">`IHttpClientFactory` は、`HttpClient` インスタンスによって使用されたリソースの追跡と破棄を行います。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-250">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="3dcb6-251">通常、`HttpClient` インスタンスは、破棄を必要としない .NET オブジェクトとして扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-251">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="3dcb6-252">`IHttpClientFactory` の登場以前は、1 つの `HttpClient` インスタンスを長い期間使い続けるパターンが一般的に使用されていました。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-252">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="3dcb6-253">`IHttpClientFactory` に移行した後は、このパターンは不要になります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-253">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

## <a name="logging"></a><span data-ttu-id="3dcb6-254">ログの記録</span><span class="sxs-lookup"><span data-stu-id="3dcb6-254">Logging</span></span>

<span data-ttu-id="3dcb6-255">`IHttpClientFactory` によって作成されたクライアントは、すべての要求のログ メッセージを記録します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-255">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="3dcb6-256">既定のログ メッセージを見るには、ログの構成で適切な情報レベルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-256">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="3dcb6-257">要求ヘッダーのログなどの追加ログは、トレース レベルでのみ含まれます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-257">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="3dcb6-258">各クライアントに使用されるログのカテゴリには、クライアントの名前が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-258">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="3dcb6-259">たとえば、*MyNamedClient* という名前のクライアントは、カテゴリ `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` でメッセージをログに記録します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-259">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="3dcb6-260">*LogicalHandler* というサフィックスが付いたメッセージが、要求ハンドラーのパイプラインの外部で発生します。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-260">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="3dcb6-261">要求では、パイプライン内の他のハンドラーが要求を処理する前に、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-261">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="3dcb6-262">応答では、他のパイプライン ハンドラーが応答を受け取った後で、メッセージがログに記録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-262">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="3dcb6-263">ログ記録は、要求ハンドラーのパイプラインの内部でも行われます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-263">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="3dcb6-264">*MyNamedClient* の例では、それらのメッセージはログ カテゴリ `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` に対して記録されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-264">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="3dcb6-265">要求では、他のすべてのハンドラーが実行した後、要求がネットワークに送信される直前に、これが行われます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-265">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="3dcb6-266">応答では、このログ記録には、ハンドラー パイプラインを通じ戻される前の応答の状態が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-266">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="3dcb6-267">パイプラインの外部と内部でログを有効にすると、他のパイプライン ハンドラーによる変更を検査できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-267">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="3dcb6-268">これには、たとえば、要求ヘッダーや応答状態コードへの変更を含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-268">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="3dcb6-269">ログのカテゴリにクライアントの名前を含めると、必要に応じて、特定の名前付きクライアントでログをフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-269">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="3dcb6-270">HttpMessageHandler を構成する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-270">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="3dcb6-271">クライアントによって使用される内部 `HttpMessageHandler` の構成を制御することが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-271">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="3dcb6-272">名前付きクライアントまたは型指定されたクライアントを追加すると、`IHttpClientBuilder` が返されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-272">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="3dcb6-273"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> 拡張メソッドを使用して、デリゲートを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-273">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="3dcb6-274">デリゲートは、そのクライアントによって使用されるプライマリ `HttpMessageHandler` の作成と構成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="3dcb6-274">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a><span data-ttu-id="3dcb6-275">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3dcb6-275">Additional resources</span></span>

* [<span data-ttu-id="3dcb6-276">HttpClientFactory を使用して回復力の高い HTTP 要求を実装する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-276">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="3dcb6-277">HttpClientFactory ポリシーと Polly ポリシーで指数バックオフを含む HTTP 呼び出しの再試行を実装する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-277">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="3dcb6-278">サーキット ブレーカー パターンを実装する</span><span class="sxs-lookup"><span data-stu-id="3dcb6-278">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)