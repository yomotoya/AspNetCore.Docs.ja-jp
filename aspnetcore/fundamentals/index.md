---
title: "ASP.NET Core の基礎"
author: rick-anderson
description: "ASP.NET Core アプリケーションの構築に関する基本概念について説明します。"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: fundamentals/index
ms.openlocfilehash: be37df7789354ac4ce8e373a1560366be157ffa5
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="d7358-103">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="d7358-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="d7358-104">ASP.NET Core アプリケーションは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="d7358-104">An ASP.NET Core application is a console app that creates a web server in its `Main` method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d7358-105">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d7358-105">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs)]

<span data-ttu-id="d7358-106">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHost.CreateDefaultBuilder` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d7358-106">The `Main` method invokes `WebHost.CreateDefaultBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d7358-107">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="d7358-107">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d7358-108">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="d7358-108">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="d7358-109">ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。</span><span class="sxs-lookup"><span data-stu-id="d7358-109">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="d7358-110">[HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-110">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d7358-111">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d7358-111">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d7358-112">`IWebHostBuilder` (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-112">`IWebHostBuilder`, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="d7358-113">これらのメソッドの一部には、HTTP.sys でアプリをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d7358-113">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d7358-114">`Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d7358-114">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d7358-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d7358-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs)]

<span data-ttu-id="d7358-116">`Main` メソッドは、Web アプリケーション ホストを作成するビルダー パターンに従う、`WebHostBuilder` を使用します。</span><span class="sxs-lookup"><span data-stu-id="d7358-116">The `Main` method uses `WebHostBuilder`, which follows the builder pattern to create a web application host.</span></span> <span data-ttu-id="d7358-117">ビルダーには、Web サーバー (`UseKestrel` など) とスタートアップ クラス (`UseStartup`) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="d7358-117">The builder has methods that define the web server (for example, `UseKestrel`) and the startup class (`UseStartup`).</span></span> <span data-ttu-id="d7358-118">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="d7358-118">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="d7358-119">[WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-119">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="d7358-120">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d7358-120">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="d7358-121">`WebHostBuilder` では、IIS および IIS Express でホストするための `UseIISIntegration` と、ルート コンテンツ ディレクトリを指定するための `UseContentRoot` を含む、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-121">`WebHostBuilder` provides many optional methods, including `UseIISIntegration` for hosting in IIS and IIS Express and `UseContentRoot` for specifying the root content directory.</span></span> <span data-ttu-id="d7358-122">`Build` および `Run` メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する `IWebHost` オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d7358-122">The `Build` and `Run` methods build the `IWebHost` object that hosts the app and begins listening for HTTP requests.</span></span>

---

## <a name="startup"></a><span data-ttu-id="d7358-123">スタートアップ</span><span class="sxs-lookup"><span data-stu-id="d7358-123">Startup</span></span>

<span data-ttu-id="d7358-124">`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="d7358-124">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d7358-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d7358-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program2x.cs?highlight=10&range=6-17)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d7358-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d7358-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](../getting-started/sample/aspnetcoreapp/Program.cs?highlight=7&range=6-17)]

---

<span data-ttu-id="d7358-127">`Startup` クラスでは、要求処理パイプラインを定義し、アプリで必要なサービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-127">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="d7358-128">`Startup` クラスはパブリックであり、次のメソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="d7358-128">The `Startup` class must be public and contain the following methods:</span></span>

```csharp
public class Startup
{
    // This method gets called by the runtime. Use this method
    // to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
    }

    // This method gets called by the runtime. Use this method
    // to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app)
    {
    }
}
```

<span data-ttu-id="d7358-129">`ConfigureServices` は、アプリで使用される[サービス](#dependency-injection-services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。</span><span class="sxs-lookup"><span data-stu-id="d7358-129">`ConfigureServices` defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="d7358-130">`Configure` は、要求パイプラインの[ミドルウェア](xref:fundamentals/middleware/index)を定義します。</span><span class="sxs-lookup"><span data-stu-id="d7358-130">`Configure` defines the [middleware](xref:fundamentals/middleware/index) for the request pipeline.</span></span>

<span data-ttu-id="d7358-131">詳細については、[アプリケーションの起動](xref:fundamentals/startup)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-131">For more information, see [Application startup](xref:fundamentals/startup).</span></span>

## <a name="content-root"></a><span data-ttu-id="d7358-132">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="d7358-132">Content root</span></span>

<span data-ttu-id="d7358-133">コンテンツ ルートは、ビュー、[Razor ページ](xref:mvc/razor-pages/index)、および静的資産など、アプリで使用されるコンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="d7358-133">The content root is the base path to any content used by the app, such as views, [Razor Pages](xref:mvc/razor-pages/index), and static assets.</span></span> <span data-ttu-id="d7358-134">既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルのアプリケーション基本パスと同じです。</span><span class="sxs-lookup"><span data-stu-id="d7358-134">By default, the content root is the same as application base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="d7358-135">Web ルート</span><span class="sxs-lookup"><span data-stu-id="d7358-135">Web root</span></span>

<span data-ttu-id="d7358-136">アプリの Web ルートは、CSS、JavaScript、イメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="d7358-136">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="d7358-137">依存性の注入 (サービス)</span><span class="sxs-lookup"><span data-stu-id="d7358-137">Dependency Injection (Services)</span></span>

<span data-ttu-id="d7358-138">サービスは、アプリでの一般的な使用を想定されているコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="d7358-138">A service is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="d7358-139">サービスは[依存性の注入 (DI)](xref:fundamentals/dependency-injection) を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="d7358-139">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d7358-140">ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (**I**nversion **o**f **C**ontrol、IoC) コンテナーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7358-140">ASP.NET Core includes a native **I**nversion **o**f **C**ontrol (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="d7358-141">必要に応じて、既定のネイティブ コンテナーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-141">You can replace the default native container if you wish.</span></span> <span data-ttu-id="d7358-142">その疎結合の利点に加え、DI はアプリ全体でサービス ([ログ](xref:fundamentals/logging/index)など) を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d7358-142">In addition to its loose coupling benefit, DI makes services available throughout your app (for example, [logging](xref:fundamentals/logging/index)).</span></span>

<span data-ttu-id="d7358-143">詳細については、[依存性の注入](xref:fundamentals/dependency-injection)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-143">For more information, see [Dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="middleware"></a><span data-ttu-id="d7358-144">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="d7358-144">Middleware</span></span>

<span data-ttu-id="d7358-145">ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware/index)を使用して要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="d7358-145">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="d7358-146">ASP.NET Core ミドルウェアは `HttpContext` で非同期ロジックを実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="d7358-146">ASP.NET Core middleware performs asynchronous logic on an `HttpContext` and then either invokes the next middleware in the sequence or terminates the request directly.</span></span> <span data-ttu-id="d7358-147">"XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出して追加されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-147">A middleware component called "XYZ" is added by invoking an `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="d7358-148">ASP.NET Core には、豊富な組み込みミドルウェアのセットが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7358-148">ASP.NET Core includes a rich set of built-in middleware:</span></span>

* [<span data-ttu-id="d7358-149">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="d7358-149">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="d7358-150">ルーティング</span><span class="sxs-lookup"><span data-stu-id="d7358-150">Routing</span></span>](xref:fundamentals/routing)
* [<span data-ttu-id="d7358-151">認証</span><span class="sxs-lookup"><span data-stu-id="d7358-151">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="d7358-152">応答圧縮ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="d7358-152">Response Compression Middleware</span></span>](xref:performance/response-compression)
* [<span data-ttu-id="d7358-153">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="d7358-153">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)

<span data-ttu-id="d7358-154">ASP.NET Core アプリで [OWIN](http://owin.org) ベースのミドルウェアを使用することができます。また、独自のカスタム ミドルウェアを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d7358-154">[OWIN](http://owin.org)-based middleware is available for ASP.NET Core apps, and you can write your own custom middleware.</span></span>

<span data-ttu-id="d7358-155">詳細については、[ミドルウェア](xref:fundamentals/middleware/index)と [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-155">For more information, see [Middleware](xref:fundamentals/middleware/index) and [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="environments"></a><span data-ttu-id="d7358-156">環境</span><span class="sxs-lookup"><span data-stu-id="d7358-156">Environments</span></span>

<span data-ttu-id="d7358-157">"開発" や "実稼働" などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数を使用して設定できます。</span><span class="sxs-lookup"><span data-stu-id="d7358-157">Environments, such as "Development" and "Production", are a first-class notion in ASP.NET Core and can be set using environment variables.</span></span>

<span data-ttu-id="d7358-158">詳細については、「[Working with multiple environments](xref:fundamentals/environments)」 (複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-158">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

## <a name="configuration"></a><span data-ttu-id="d7358-159">構成</span><span class="sxs-lookup"><span data-stu-id="d7358-159">Configuration</span></span>

<span data-ttu-id="d7358-160">ASP.NET Core は、名前と値のペアに基づく構成モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="d7358-160">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="d7358-161">この構成モデルは `System.Configuration` または *web.config* には基づきません。構成は、構成プロバイダーの順序付けされたセットから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="d7358-161">The configuration model isn't based on `System.Configuration` or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="d7358-162">組み込みの構成プロバイダーではさまざまなファイル形式 (XML、JSON、INI) と環境変数がサポートされ、これにより環境ベースの構成が可能になります。</span><span class="sxs-lookup"><span data-stu-id="d7358-162">The built-in configuration providers support a variety of file formats (XML, JSON, INI) and environment variables to enable environment-based configuration.</span></span> <span data-ttu-id="d7358-163">独自のカスタム構成プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="d7358-163">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="d7358-164">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-164">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="logging"></a><span data-ttu-id="d7358-165">ログの記録</span><span class="sxs-lookup"><span data-stu-id="d7358-165">Logging</span></span>

<span data-ttu-id="d7358-166">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d7358-166">ASP.NET Core supports a logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="d7358-167">組み込みプロバイダーは、1 つまたは複数の送信先へのログの送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d7358-167">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="d7358-168">サード パーティ製のログ記録フレームワークを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d7358-168">Third-party logging frameworks can be used.</span></span>

[<span data-ttu-id="d7358-169">ログ</span><span class="sxs-lookup"><span data-stu-id="d7358-169">Logging</span></span>](xref:fundamentals/logging/index)

## <a name="error-handling"></a><span data-ttu-id="d7358-170">エラー処理</span><span class="sxs-lookup"><span data-stu-id="d7358-170">Error handling</span></span>

<span data-ttu-id="d7358-171">ASP.NET Core には、開発者の例外ページ、カスタム エラー ページ、静的ステータス コード ページ、起動時の例外処理を含む、アプリのエラー処理の機能が組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="d7358-171">ASP.NET Core has built-in features for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="d7358-172">詳細については、[エラー処理](xref:fundamentals/error-handling)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-172">For more information, see [Error Handling](xref:fundamentals/error-handling).</span></span>

## <a name="routing"></a><span data-ttu-id="d7358-173">ルーティング</span><span class="sxs-lookup"><span data-stu-id="d7358-173">Routing</span></span>

<span data-ttu-id="d7358-174">ASP.NET Core は、ルート ハンドラーにアプリ要求をルーティングする機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="d7358-174">ASP.NET Core offers features for routing of app requests to route handlers.</span></span>

<span data-ttu-id="d7358-175">詳細については、[ルーティング](xref:fundamentals/routing)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-175">For more information, see [Routing](xref:fundamentals/routing).</span></span>

## <a name="file-providers"></a><span data-ttu-id="d7358-176">ファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="d7358-176">File providers</span></span>

<span data-ttu-id="d7358-177">ASP.NET Core は、プラットフォーム間でファイルを操作するための共通のインターフェイスを提供するファイル プロバイダーの使用により、ファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="d7358-177">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="d7358-178">詳細については、[ファイル プロバイダー](xref:fundamentals/file-providers)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-178">For more information, see [File Providers](xref:fundamentals/file-providers).</span></span>

## <a name="static-files"></a><span data-ttu-id="d7358-179">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="d7358-179">Static files</span></span>

<span data-ttu-id="d7358-180">静的ファイルのミドルウェアは、HTML、CSS、画像、JavaScript などの静的ファイルを処理します。</span><span class="sxs-lookup"><span data-stu-id="d7358-180">Static files middleware serves static files, such as HTML, CSS, image, and JavaScript.</span></span>

<span data-ttu-id="d7358-181">詳細については、[静的ファイルの使用](xref:fundamentals/static-files)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-181">For more information, see [Working with static files](xref:fundamentals/static-files).</span></span>

## <a name="hosting"></a><span data-ttu-id="d7358-182">ホスト</span><span class="sxs-lookup"><span data-stu-id="d7358-182">Hosting</span></span>

<span data-ttu-id="d7358-183">ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="d7358-183">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="d7358-184">詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-184">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

## <a name="session-and-application-state"></a><span data-ttu-id="d7358-185">セッションとアプリケーションの状態</span><span class="sxs-lookup"><span data-stu-id="d7358-185">Session and application state</span></span>

<span data-ttu-id="d7358-186">セッション状態は ASP.NET Core の機能で、これを使用することでユーザーが Web アプリを参照中にユーザー データを保存して格納することができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-186">Session state is a feature in ASP.NET Core that you can use to save and store user data while the user browses your web app.</span></span>

<span data-ttu-id="d7358-187">詳細については、[セッションとアプリケーション状態](xref:fundamentals/app-state)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-187">For more information, see [Session and application state](xref:fundamentals/app-state).</span></span>

## <a name="servers"></a><span data-ttu-id="d7358-188">サーバー</span><span class="sxs-lookup"><span data-stu-id="d7358-188">Servers</span></span>

<span data-ttu-id="d7358-189">ASP.NET Core のホスティング モデルは、直接要求をリッスンしません。</span><span class="sxs-lookup"><span data-stu-id="d7358-189">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="d7358-190">ホスティング モデルは HTTP サーバー実装に依存して要求をアプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="d7358-190">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="d7358-191">転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。</span><span class="sxs-lookup"><span data-stu-id="d7358-191">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="d7358-192">ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d7358-192">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="d7358-193">Kestrel は多くの場合、[IIS](https://www.iis.net/) や [Nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-193">Kestrel is often run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org).</span></span> <span data-ttu-id="d7358-194">Kestrel は、エッジ サーバーとして実行することができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-194">Kestrel can be run as an edge server.</span></span>

<span data-ttu-id="d7358-195">詳細については、[サーバー](xref:fundamentals/servers/index)に関するページと、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-195">For more information, see [Servers](xref:fundamentals/servers/index) and the following topics:</span></span>

* [<span data-ttu-id="d7358-196">Kestrel</span><span class="sxs-lookup"><span data-stu-id="d7358-196">Kestrel</span></span>](xref:fundamentals/servers/kestrel)
* [<span data-ttu-id="d7358-197">ASP.NET Core モジュール</span><span class="sxs-lookup"><span data-stu-id="d7358-197">ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* <span data-ttu-id="d7358-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (旧称 [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="d7358-198">[HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="d7358-199">グローバリゼーションとローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="d7358-199">Globalization and localization</span></span>

<span data-ttu-id="d7358-200">ASP.NET Core で多言語の Web サイトを作成すると、より幅広い対象者がサイトにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="d7358-200">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="d7358-201">ASP.NET Core は、さまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="d7358-201">ASP.NET Core provides services and middleware for localizing into different languages and cultures.</span></span>

<span data-ttu-id="d7358-202">詳細については、[グローバリゼーションとローカリゼーション](xref:fundamentals/localization)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-202">For more information, see [Globalization and localization](xref:fundamentals/localization).</span></span>

## <a name="request-features"></a><span data-ttu-id="d7358-203">要求機能</span><span class="sxs-lookup"><span data-stu-id="d7358-203">Request features</span></span>

<span data-ttu-id="d7358-204">HTTP の要求と応答に関連する Web サーバーの実装の詳細が、インターフェイスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="d7358-204">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="d7358-205">これらのインターフェイスは、サーバーの実装とミドルウェアがアプリのホスティングのパイプラインを作成および変更するために使用します。</span><span class="sxs-lookup"><span data-stu-id="d7358-205">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="d7358-206">詳細については、 [要求機能](xref:fundamentals/request-features)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-206">For more information, see [Request Features](xref:fundamentals/request-features).</span></span>

## <a name="background-tasks"></a><span data-ttu-id="d7358-207">バックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="d7358-207">Background tasks</span></span>

<span data-ttu-id="d7358-208">バックグラウンド タスクは*ホストされるサービス*として実装されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-208">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="d7358-209">ホストされるサービスは、[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="d7358-209">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span>

<span data-ttu-id="d7358-210">詳細については、「[Background tasks with hosted services](xref:fundamentals/hosted-services)」(ホストされるタスクを使用するバックグラウンド タスク) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-210">For more information, see [Background tasks with hosted services](xref:fundamentals/hosted-services).</span></span>

## <a name="open-web-interface-for-net-owin"></a><span data-ttu-id="d7358-211">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="d7358-211">Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="d7358-212">ASP.NET Core は、Open Web Interface for .NET (OWIN) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d7358-212">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="d7358-213">OWIN により、Web アプリを Web サーバーから切り離すことが可能になります。</span><span class="sxs-lookup"><span data-stu-id="d7358-213">OWIN allows web apps to be decoupled from web servers.</span></span>

<span data-ttu-id="d7358-214">詳細については、[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-214">For more information, see [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin).</span></span>

## <a name="websockets"></a><span data-ttu-id="d7358-215">WebSocket</span><span class="sxs-lookup"><span data-stu-id="d7358-215">WebSockets</span></span>

<span data-ttu-id="d7358-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="d7358-216">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="d7358-217">チャット、株価情報、ゲームなどのアプリ、さらに Web アプリでのリアルタイムな機能が必要とされるあらゆる場所で使用されます。</span><span class="sxs-lookup"><span data-stu-id="d7358-217">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="d7358-218">ASP.NET Core は、Web ソケットの機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d7358-218">ASP.NET Core supports web socket features.</span></span>

<span data-ttu-id="d7358-219">詳細については、[WebSockets](xref:fundamentals/websockets) に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-219">For more information, see [WebSockets](xref:fundamentals/websockets).</span></span>

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="d7358-220">Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="d7358-220">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="d7358-221">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7358-221">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="d7358-222">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="d7358-222">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="d7358-223">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="d7358-223">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="d7358-224">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="d7358-224">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="d7358-225">詳細については、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-225">For more information, see [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="d7358-226">.NET Core と .NET Framework ランタイム</span><span class="sxs-lookup"><span data-stu-id="d7358-226">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="d7358-227">ASP.NET Core アプリは、.NET Core または .NET Framework ランタイムを対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d7358-227">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="d7358-228">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-228">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="d7358-229">ASP.NET Core と ASP.NET の選択</span><span class="sxs-lookup"><span data-stu-id="d7358-229">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="d7358-230">ASP.NET Core と ASP.NET の選択の詳細については、「[ ASP.NET と ASP.NET Core の選択](xref:fundamentals/choose-between-aspnet-and-aspnetcore)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7358-230">For more information on choosing between ASP.NET Core and ASP.NET, see [Choose between ASP.NET Core and ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore).</span></span>
