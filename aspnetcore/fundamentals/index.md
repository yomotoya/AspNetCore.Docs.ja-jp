---
title: "ASP.NET Core の基礎"
author: rick-anderson
description: "この記事では、ASP.NET Core アプリケーションを構築する場合に理解しておく必要がある基本概念の概要を示します。"
keywords: "ASP.NET Core,基礎,概要"
ms.author: riande
manager: wpickett
ms.date: 08/18/2017
ms.topic: get-started-article
ms.assetid: a19b7836-63e4-44e8-8250-50d426dd1070
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/index
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99fbe0e02be27a0fbbb7ff65bc15713aab58c003
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="aspnet-core-fundamentals-overview"></a><span data-ttu-id="e4701-104">ASP.NET Core の基礎の概要</span><span class="sxs-lookup"><span data-stu-id="e4701-104">ASP.NET Core fundamentals overview</span></span>

<span data-ttu-id="e4701-105">ASP.NET Core アプリケーションは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="e4701-105">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e4701-106">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e4701-106">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e4701-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span><span class="sxs-lookup"><span data-stu-id="e4701-107">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs)]</span></span>

<span data-ttu-id="e4701-108">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHost.CreateDefaultBuilder` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e4701-108">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="e4701-109">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="e4701-109">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="e4701-110">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e4701-110">In the preceding example, a [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="e4701-111">ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。</span><span class="sxs-lookup"><span data-stu-id="e4701-111">ASP.NET Core's web host will attempt to run on IIS, if it is available.</span></span> <span data-ttu-id="e4701-112">[HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e4701-112">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e4701-113">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="e4701-113">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="e4701-114">`IWebHostBuilder` (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-114">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="e4701-115">これらのメソッドの一部には、HTTP.sys でアプリケーションをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e4701-115">Some of these methods include `UseHttpSys` for hosting the application in HTTP.sys, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="e4701-116">`Build` および `Run` メソッドは、アプリケーションをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e4701-116">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e4701-117">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e4701-117">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e4701-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="e4701-118">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs)]</span></span>

<span data-ttu-id="e4701-119">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHostBuilder` を使用します。</span><span class="sxs-lookup"><span data-stu-id="e4701-119">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="e4701-120">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="e4701-120">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="e4701-121">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="e4701-121">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="e4701-122">[WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e4701-122">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="e4701-123">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="e4701-123">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="e4701-124">`WebHostBuilder` では、IIS および IIS Express でホストするための `UseIISIntegration` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` を含む、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-124">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express, and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="e4701-125">`Build` および `Run` メソッドは、アプリケーションをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e4701-125">The `Build` and `Run` methods build the `IWebHost` object that will host the application and begin listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="e4701-126">スタートアップ</span><span class="sxs-lookup"><span data-stu-id="e4701-126">Startup</span></span>

<span data-ttu-id="e4701-127">`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="e4701-127">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e4701-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e4701-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e4701-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="e4701-129">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e4701-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e4701-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e4701-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span><span class="sxs-lookup"><span data-stu-id="e4701-131">[!code-csharp[Main](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]</span></span>

---

<span data-ttu-id="e4701-132">`Startup` クラスでは、要求処理パイプラインを定義し、アプリケーションで必要なサービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-132">The `Startup` class is where you define the request handling pipeline and where any services needed by the application are configured.</span></span> <span data-ttu-id="e4701-133">`Startup` クラスはパブリックであり、次のメソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="e4701-133">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

* <span data-ttu-id="e4701-134">`ConfigureServices` は、アプリケーションで使用される[サービス](#services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。</span><span class="sxs-lookup"><span data-stu-id="e4701-134">`ConfigureServices` defines the [Services](#services) used by your application (such as ASP.NET Core MVC, Entity Framework Core, Identity, etc.).</span></span>

* <span data-ttu-id="e4701-135">`Configure` は、要求パイプラインで[ミドルウェア](xref:fundamentals/middleware)を定義します。</span><span class="sxs-lookup"><span data-stu-id="e4701-135">`Configure` defines the [middleware](xref:fundamentals/middleware) in the request pipeline.</span></span>

<span data-ttu-id="e4701-136">詳細については、[アプリケーションの起動](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-136">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="services"></a><span data-ttu-id="e4701-137">サービス</span><span class="sxs-lookup"><span data-stu-id="e4701-137">Services</span></span>

<span data-ttu-id="e4701-138">サービスは、アプリケーションでの一般的な使用を想定されているコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="e4701-138">A service is a component that is intended for common consumption in an application.</span></span> <span data-ttu-id="e4701-139">サービスは[依存性の注入](xref:fundamentals/dependency-injection) (DI) を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4701-139">Services are made available through [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="e4701-140">ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (IoC) コンテナーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e4701-140">ASP.NET Core includes a native inversion of control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="e4701-141">ネイティブ コンテナーは任意のコンテナーに置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="e4701-141">The native container can be replaced with your container of choice.</span></span> <span data-ttu-id="e4701-142">その疎結合の利点に加え、DI はアプリケーション全体でサービスを使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="e4701-142">In addition to its loose coupling benefit, DI makes services available throughout your application.</span></span> <span data-ttu-id="e4701-143">たとえば、[ログ](xref:fundamentals/logging)はアプリケーション全体で使用できます。</span><span class="sxs-lookup"><span data-stu-id="e4701-143">For example, [logging](xref:fundamentals/logging) is available throughout your application.</span></span>

<span data-ttu-id="e4701-144">詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-144">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="e4701-145">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="e4701-145">Middleware</span></span>

<span data-ttu-id="e4701-146">ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware)を使用して要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="e4701-146">In ASP.NET Core, you compose your request pipeline using [Middleware](xref:fundamentals/middleware).</span></span> <span data-ttu-id="e4701-147">ASP.NET Core ミドルウェアは `HttpContext` で非同期ロジックを実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="e4701-147">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="e4701-148">"XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出して追加されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-148">A middleware component called "XYZ" is added by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="e4701-149">ASP.NET Core には、豊富な組み込みミドルウェアのセットが付属しています。</span><span class="sxs-lookup"><span data-stu-id="e4701-149">ASP.NET Core comes with a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="e4701-150">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="e4701-150">Static files</span></span>](xref:fundamentals/static-files)

* [<span data-ttu-id="e4701-151">ルーティング</span><span class="sxs-lookup"><span data-stu-id="e4701-151">Routing</span></span>](xref:fundamentals/routing)

* [<span data-ttu-id="e4701-152">認証</span><span class="sxs-lookup"><span data-stu-id="e4701-152">Authentication</span></span>](xref:security/authentication/index)

<span data-ttu-id="e4701-153">ASP.NET Core で [OWIN](http://owin.org) ベースのミドルウェアを使用することができます。また、独自のカスタム ミドルウェアを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="e4701-153">You can use any [OWIN](http://owin.org)-based middleware with ASP.NET Core, and you can write your own custom middleware.</span></span>

<span data-ttu-id="e4701-154">詳細については、[ミドルウェア](xref:fundamentals/middleware)と [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-154">For more information, see [Middleware](xref:fundamentals/middleware) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="servers"></a><span data-ttu-id="e4701-155">サーバー</span><span class="sxs-lookup"><span data-stu-id="e4701-155">Servers</span></span>

<span data-ttu-id="e4701-156">ASP.NET Core ホスティング モデルは、要求を直接リッスンするのではなく、HTTP サーバー実装を介して要求をアプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="e4701-156">The ASP.NET Core hosting model does not directly listen for requests; rather, it relies on an HTTP server implementation to forward the request to the application.</span></span> <span data-ttu-id="e4701-157">転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。</span><span class="sxs-lookup"><span data-stu-id="e4701-157">The forwarded request is wrapped as a set of feature objects that you can access through interfaces.</span></span> <span data-ttu-id="e4701-158">アプリケーションはこのセットを `HttpContext` に構成します。</span><span class="sxs-lookup"><span data-stu-id="e4701-158">The application composes this set into an `HttpContext`.</span></span> <span data-ttu-id="e4701-159">ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e4701-159">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="e4701-160">Kestrel は通常、[IIS](https://iis.net) や [nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-160">Kestrel is typically run behind a production web server like [IIS](https://iis.net) or [nginx](http://nginx.org).</span></span>

<span data-ttu-id="e4701-161">詳細については、[サーバー](xref:fundamentals/servers/index)と[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-161">For more information, see [Servers](xref:fundamentals/servers/index) and [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="content-root"></a><span data-ttu-id="e4701-162">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="e4701-162">Content root</span></span>

<span data-ttu-id="e4701-163">コンテンツ ルートは、ビュー、[Razor ページ](xref:mvc/razor-pages/index)、および静的資産など、アプリで使用されるコンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="e4701-163">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="e4701-164">既定では、コンテンツ ルートは、アプリケーションをホストする実行可能ファイルのアプリケーション基本パスと同じです。</span><span class="sxs-lookup"><span data-stu-id="e4701-164">By default, the content root is the same as application base path for the executable hosting the application.</span></span> <span data-ttu-id="e4701-165">コンテンツ ルートの別の場所は `WebHostBuilder` で指定されます。</span><span class="sxs-lookup"><span data-stu-id="e4701-165">An alternative location for content root is specified with `WebHostBuilder`.</span></span>

## <a name="web-root"></a><span data-ttu-id="e4701-166">Web ルート</span><span class="sxs-lookup"><span data-stu-id="e4701-166">Web root</span></span>

<span data-ttu-id="e4701-167">アプリケーションの Web ルートは、CSS、JavaScript、およびイメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="e4701-167">The web root of an application is the directory in the project containing public, static resources like CSS, JavaScript, and image files.</span></span> <span data-ttu-id="e4701-168">既定では、静的ファイル ミドルウェアは、Web ルート ディレクトリとそのサブディレクトリからのファイルのみを提供します。</span><span class="sxs-lookup"><span data-stu-id="e4701-168">By default, the static files middleware will only serve files from the web root directory and its sub-directories.</span></span> <span data-ttu-id="e4701-169">詳細については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-169">See [working with static files](xref:fundamentals/static-files) for more info.</span></span> <span data-ttu-id="e4701-170">Web ルート パスは既定で */wwwroot* に設定されますが、`WebHostBuilder` を使用して別の場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="e4701-170">The web root path defaults to */wwwroot*, but you can specify a different location using the `WebHostBuilder`.</span></span>

## <a name="configuration"></a><span data-ttu-id="e4701-171">構成</span><span class="sxs-lookup"><span data-stu-id="e4701-171">Configuration</span></span>

<span data-ttu-id="e4701-172">ASP.NET Core は、単純な名前と値のペアを処理するために新しい構成モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="e4701-172">ASP.NET Core uses a new configuration model for handling simple name-value pairs.</span></span> <span data-ttu-id="e4701-173">この新しい構成モデルは `System.Configuration` や *web.config* に基づくものではなく、構成プロバイダーの順序付けされたセットから取得します。</span><span class="sxs-lookup"><span data-stu-id="e4701-173">The new configuration model is not based on `System.Configuration` or *web.config*; rather, it pulls from an ordered set of configuration providers.</span></span> <span data-ttu-id="e4701-174">組み込みの構成プロバイダーではさまざまなファイル形式 (XML、JSON、INI) と環境変数がサポートされ、これにより環境ベースの構成が可能になります。</span><span class="sxs-lookup"><span data-stu-id="e4701-174">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="e4701-175">独自のカスタム構成プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="e4701-175">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="e4701-176">詳細については、[構成](xref:fundamentals/configuration)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-176">For more information, see [Configuration](xref:fundamentals/configuration).</span></span>

## <a name="environments"></a><span data-ttu-id="e4701-177">環境</span><span class="sxs-lookup"><span data-stu-id="e4701-177">Environments</span></span>

<span data-ttu-id="e4701-178">"開発" や "実稼働" などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数を使用して設定できます。</span><span class="sxs-lookup"><span data-stu-id="e4701-178">Environments, like "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="e4701-179">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-179">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="e4701-180">.NET Core と .NET Framework ランタイム</span><span class="sxs-lookup"><span data-stu-id="e4701-180">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="e4701-181">ASP.NET Core アプリケーションは、.NET Core または .NET Framework ランタイムを対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="e4701-181">An ASP.NET Core application can target the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="e4701-182">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-182">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="additional-information"></a><span data-ttu-id="e4701-183">追加情報</span><span class="sxs-lookup"><span data-stu-id="e4701-183">Additional information</span></span>

<span data-ttu-id="e4701-184">次のトピックも参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4701-184">See also the following topics:</span></span>

- [<span data-ttu-id="e4701-185">エラー処理</span><span class="sxs-lookup"><span data-stu-id="e4701-185">Error Handling</span></span>](xref:fundamentals/error-handling)
- [<span data-ttu-id="e4701-186">ファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="e4701-186">File Providers</span></span>](xref:fundamentals/file-providers)
- [<span data-ttu-id="e4701-187">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="e4701-187">Globalization and localization</span></span>](xref:fundamentals/localization)
- [<span data-ttu-id="e4701-188">ログ</span><span class="sxs-lookup"><span data-stu-id="e4701-188">Logging</span></span>](xref:fundamentals/logging)
- [<span data-ttu-id="e4701-189">アプリケーション状態の管理</span><span class="sxs-lookup"><span data-stu-id="e4701-189">Managing Application State</span></span>](xref:fundamentals/app-state)