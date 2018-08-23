---
title: ASP.NET Core の基礎
author: rick-anderson
description: ASP.NET Core アプリの構築に関する基本概念について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/index
ms.openlocfilehash: 68760f179c4d6e806510b727e2284f8c2c4a4ff6
ms.sourcegitcommit: d27317c16f113e7c111583042ec7e4c5a26adf6f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2018
ms.locfileid: "41751246"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="cf4b4-103">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="cf4b4-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="cf4b4-104">ASP.NET Core アプリは、以下の `Main` メソッドで Web サーバーを作成するコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-104">An ASP.NET Core app is a console app that creates a web server in its `Main` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="cf4b4-105">`Main` メソッドは、Web ホストを作成する[ビルダー パターン](https://wikipedia.org/wiki/Builder_pattern)に従う、[WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-105">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="cf4b4-106">ビルダーには、Web サーバー (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> など) とスタートアップ クラス (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-106">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="cf4b4-107">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-107">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="cf4b4-108">ASP.NET Core の Web ホストは、IIS (使用可能な場合) で実行を試みます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-108">ASP.NET Core's web host attempts to run on IIS, if available.</span></span> <span data-ttu-id="cf4b4-109">[HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-109">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="cf4b4-110">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-110">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="cf4b4-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-111"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="cf4b4-112">これらのメソッドの一部には、HTTP.sys でアプリをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-112">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="cf4b4-113"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> および <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する <xref:Microsoft.AspNetCore.Hosting.IWebHost> オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-113">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="cf4b4-114">`Main` メソッドは、Web アプリ ホストを作成する[ビルダー パターン](https://wikipedia.org/wiki/Builder_pattern)に従う、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を使用します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-114">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="cf4b4-115">ビルダーには、Web サーバー (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> など) とスタートアップ クラス (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-115">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="cf4b4-116">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-116">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="cf4b4-117">[WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-117">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="cf4b4-118">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-118">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="cf4b4-119">`WebHostBuilder` では、IIS および IIS Express でホストするための <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> と、ルート コンテンツ ディレクトリを指定するための <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> を含む、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-119">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="cf4b4-120"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> および <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する <xref:Microsoft.AspNetCore.Hosting.IWebHost> オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-120">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="cf4b4-121">スタートアップ</span><span class="sxs-lookup"><span data-stu-id="cf4b4-121">Startup</span></span>

<span data-ttu-id="cf4b4-122">`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-122">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="cf4b4-123">`Startup` クラスでは、要求処理パイプラインを定義し、アプリで必要なサービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-123">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="cf4b4-124">`Startup` クラスはパブリックであり、次のメソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-124">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="cf4b4-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> は、アプリで使用される[サービス](#dependency-injection-services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-125"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="cf4b4-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> は、要求パイプラインで呼び出される[ミドルウェア](xref:fundamentals/middleware/index)を定義します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-126"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="cf4b4-127">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-127">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="cf4b4-128">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="cf4b4-128">Content root</span></span>

<span data-ttu-id="cf4b4-129">コンテンツ ルートは、[Razor ページ](xref:razor-pages/index)、MVC ビュー、静的資産など、アプリで使用されるコンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-129">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="cf4b4-130">既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルのアプリ基本パスと同じ場所です。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-130">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root"></a><span data-ttu-id="cf4b4-131">Web ルート</span><span class="sxs-lookup"><span data-stu-id="cf4b4-131">Web root</span></span>

<span data-ttu-id="cf4b4-132">アプリの Web ルートは、CSS、JavaScript、イメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-132">The web root of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="cf4b4-133">依存性の注入 (サービス)</span><span class="sxs-lookup"><span data-stu-id="cf4b4-133">Dependency injection (services)</span></span>

<span data-ttu-id="cf4b4-134">*サービス*は、アプリでの一般的な使用を想定されているコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-134">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="cf4b4-135">サービスは[依存性の注入 (DI)](xref:fundamentals/dependency-injection) を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-135">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cf4b4-136">ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (Inversion of Control、IoC) コンテナーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-136">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="cf4b4-137">必要に応じて、既定のコンテナーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-137">You can replace the default container if you wish.</span></span> <span data-ttu-id="cf4b4-138">その[疎結合の利点](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)に加え、DI はアプリ全体でサービス ([ログ](xref:fundamentals/logging/index)など) を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-138">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="cf4b4-139">詳細については、「<xref:fundamentals/dependency-injection>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-139">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="cf4b4-140">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="cf4b4-140">Middleware</span></span>

<span data-ttu-id="cf4b4-141">ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware/index)を使用して要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-141">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="cf4b4-142">ASP.NET Core ミドルウェアは `HttpContext` で非同期操作を実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-142">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="cf4b4-143">通常、"XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出してパイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-143">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="cf4b4-144">ASP.NET Core には組み込みのミドルウェアの豊富なセットが含まれており、独自のカスタム ミドルウェアを作成できます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-144">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="cf4b4-145">ASP.NET Core アプリでは、[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) がサポートされています。これにより、Web アプリケーションを Web サーバーから切り離すことができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-145">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="cf4b4-146">詳細については、<xref:fundamentals/middleware/index> および <xref:fundamentals/owin> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-146">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="cf4b4-147">HTTP 要求の開始</span><span class="sxs-lookup"><span data-stu-id="cf4b4-147">Initiate HTTP requests</span></span>

<span data-ttu-id="cf4b4-148"><xref:System.Net.Http.IHttpClientFactory> を使用して <xref:System.Net.Http.HttpClient> インスタンスアクセスにアクセスし、HTTP 要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-148"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="cf4b4-149">詳細については、「<xref:fundamentals/http-requests>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-149">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="cf4b4-150">環境</span><span class="sxs-lookup"><span data-stu-id="cf4b4-150">Environments</span></span>

<span data-ttu-id="cf4b4-151">*開発*や*実稼働*などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数、設定ファイル、およびコマンド ライン引数を使用して設定できます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-151">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="cf4b4-152">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-152">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="cf4b4-153">ホスト</span><span class="sxs-lookup"><span data-stu-id="cf4b4-153">Hosting</span></span>

<span data-ttu-id="cf4b4-154">ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-154">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="cf4b4-155">詳細については、「<xref:fundamentals/host/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-155">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="cf4b4-156">サーバー</span><span class="sxs-lookup"><span data-stu-id="cf4b4-156">Servers</span></span>

<span data-ttu-id="cf4b4-157">ASP.NET Core のホスティング モデルは、直接要求をリッスンしません。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-157">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="cf4b4-158">ホスティング モデルは HTTP サーバー実装に依存して要求をアプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-158">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span> <span data-ttu-id="cf4b4-159">転送された要求は、インターフェイスを介してアクセス可能な一連の機能オブジェクトとしてラップされます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-159">The forwarded request is wrapped as a set of feature objects that can be accessed through interfaces.</span></span> <span data-ttu-id="cf4b4-160">ASP.NET Core には、[Kestrel](xref:fundamentals/servers/kestrel) と呼ばれる、マネージド クロスプラットフォーム Web サーバーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-160">ASP.NET Core includes a managed, cross-platform web server, called [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="cf4b4-161">Kestrel は通常、リバース プロキシ構成で、[IIS](https://www.iis.net/) や [Nginx](http://nginx.org) などの実稼働 Web サーバーの背後で実行されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-161">Kestrel is commonly run behind a production web server, such as [IIS](https://www.iis.net/) or [Nginx](http://nginx.org) in a reverse proxy configuration.</span></span> <span data-ttu-id="cf4b4-162">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開されるエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-162">Kestrel can also be run as an edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

<span data-ttu-id="cf4b4-163">詳細については、「<xref:fundamentals/servers/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-163">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="cf4b4-164">構成</span><span class="sxs-lookup"><span data-stu-id="cf4b4-164">Configuration</span></span>

<span data-ttu-id="cf4b4-165">ASP.NET Core は、名前と値のペアに基づく構成モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-165">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="cf4b4-166">この構成モデルは <xref:System.Configuration> または *web.config* には基づきません。構成は、構成プロバイダーの順序付けされたセットから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-166">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="cf4b4-167">組み込みの構成プロバイダーは、さまざまなファイル形式 (XML、JSON、INI)、環境変数、およびコマンド ライン引数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-167">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="cf4b4-168">独自のカスタム構成プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-168">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="cf4b4-169">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-169">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="cf4b4-170">ログの記録</span><span class="sxs-lookup"><span data-stu-id="cf4b4-170">Logging</span></span>

<span data-ttu-id="cf4b4-171">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-171">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="cf4b4-172">組み込みプロバイダーは、1 つまたは複数の送信先へのログの送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-172">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="cf4b4-173">サード パーティ製のログ記録フレームワークを使用できます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-173">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="cf4b4-174">詳細については、「<xref:fundamentals/logging/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-174">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="cf4b4-175">エラー処理</span><span class="sxs-lookup"><span data-stu-id="cf4b4-175">Error handling</span></span>

<span data-ttu-id="cf4b4-176">ASP.NET Core には、開発者の例外ページ、カスタム エラー ページ、静的ステータス コード ページ、起動時の例外処理を含む、アプリのエラー処理のシナリオが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-176">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="cf4b4-177">詳細については、「<xref:fundamentals/error-handling>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-177">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="cf4b4-178">ルーティング</span><span class="sxs-lookup"><span data-stu-id="cf4b4-178">Routing</span></span>

<span data-ttu-id="cf4b4-179">ASP.NET Core は、ルート ハンドラーにアプリ要求をルーティングするシナリオを提供します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-179">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="cf4b4-180">詳細については、「<xref:fundamentals/routing>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="file-providers"></a><span data-ttu-id="cf4b4-181">ファイル プロバイダー</span><span class="sxs-lookup"><span data-stu-id="cf4b4-181">File Providers</span></span>

<span data-ttu-id="cf4b4-182">ASP.NET Core は、プラットフォーム間でファイルを操作するための共通のインターフェイスを提供するファイル プロバイダーの使用により、ファイル システムへのアクセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-182">ASP.NET Core abstracts file system access through the use of File Providers, which offers a common interface for working with files across platforms.</span></span>

<span data-ttu-id="cf4b4-183">詳細については、「<xref:fundamentals/file-providers>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-183">For more information, see <xref:fundamentals/file-providers>.</span></span>

## <a name="static-files"></a><span data-ttu-id="cf4b4-184">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="cf4b4-184">Static files</span></span>

<span data-ttu-id="cf4b4-185">静的ファイルのミドルウェアは、HTML、CSS、画像、JavaScript ファイルなどの静的ファイルを処理します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-185">Static Files Middleware serves static files, such as HTML, CSS, image, and JavaScript files.</span></span>

<span data-ttu-id="cf4b4-186">詳細については、「<xref:fundamentals/static-files>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-186">For more information, see <xref:fundamentals/static-files>.</span></span>

## <a name="session-and-app-state"></a><span data-ttu-id="cf4b4-187">セッションとアプリの状態</span><span class="sxs-lookup"><span data-stu-id="cf4b4-187">Session and app state</span></span>

<span data-ttu-id="cf4b4-188">ASP.NET Core では、ユーザーによる Web アプリの参照中にセッションとアプリの状態を保持するためのいくつかの方法を提供しています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-188">ASP.NET Core offers several approaches to preserve session and app state while a user browses a web app.</span></span>

<span data-ttu-id="cf4b4-189">詳細については、「<xref:fundamentals/app-state>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-189">For more information, see <xref:fundamentals/app-state>.</span></span>

## <a name="globalization-and-localization"></a><span data-ttu-id="cf4b4-190">グローバリゼーションとローカリゼーション</span><span class="sxs-lookup"><span data-stu-id="cf4b4-190">Globalization and localization</span></span>

<span data-ttu-id="cf4b4-191">ASP.NET Core で多言語の Web サイトを作成すると、より幅広い対象者がサイトにアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-191">Creating a multilingual website with ASP.NET Core allows your site to reach a wider audience.</span></span> <span data-ttu-id="cf4b4-192">ASP.NET Core は、コンテンツをさまざまな言語と文化にローカライズするためのサービスとミドルウェアを提供します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-192">ASP.NET Core provides services and middleware for localizing content into different languages and cultures.</span></span>

<span data-ttu-id="cf4b4-193">詳細については、「<xref:fundamentals/localization>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-193">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="request-features"></a><span data-ttu-id="cf4b4-194">要求機能</span><span class="sxs-lookup"><span data-stu-id="cf4b4-194">Request features</span></span>

<span data-ttu-id="cf4b4-195">HTTP の要求と応答に関連する Web サーバーの実装の詳細が、インターフェイスで定義されています。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-195">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="cf4b4-196">これらのインターフェイスは、サーバーの実装とミドルウェアがアプリのホスティングのパイプラインを作成および変更するために使用します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-196">These interfaces are used by server implementations and middleware to create and modify the app's hosting pipeline.</span></span>

<span data-ttu-id="cf4b4-197">詳細については、「<xref:fundamentals/request-features>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-197">For more information, see <xref:fundamentals/request-features>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="cf4b4-198">バックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="cf4b4-198">Background tasks</span></span>

<span data-ttu-id="cf4b4-199">バックグラウンド タスクは*ホストされるサービス*として実装されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-199">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="cf4b4-200">ホストされるサービスは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-200">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="cf4b4-201">詳細については、「<xref:fundamentals/host/hosted-services>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-201">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="cf4b4-202">HttpContext へのアクセス</span><span class="sxs-lookup"><span data-stu-id="cf4b4-202">Access HttpContext</span></span>

<span data-ttu-id="cf4b4-203">`HttpContext` は、Razor ページおよび MVC で要求を処理するときに自動的に使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-203">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="cf4b4-204">`HttpContext` をすぐに使用できない状況では、<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> インターフェイスとその既定の実装 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> を介して `HttpContext` にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-204">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="cf4b4-205">詳細については、「<xref:fundamentals/httpcontext>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-205">For more information, see <xref:fundamentals/httpcontext>.</span></span>

## <a name="websockets"></a><span data-ttu-id="cf4b4-206">WebSocket</span><span class="sxs-lookup"><span data-stu-id="cf4b4-206">WebSockets</span></span>

<span data-ttu-id="cf4b4-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) は TCP 接続を使用した双方向の永続的通信チャネルを有効にするプロトコルです。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-207">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="cf4b4-208">チャット、株価情報、ゲームなどのアプリ、さらに Web アプリでのリアルタイムな機能が必要とされるあらゆる場所で使用されます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-208">It's used for apps such as chat, stock tickers, games, and anywhere you desire real-time functionality in a web app.</span></span> <span data-ttu-id="cf4b4-209">ASP.NET Core は、Web ソケットのシナリオをサポートします。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-209">ASP.NET Core supports web socket scenarios.</span></span>

<span data-ttu-id="cf4b4-210">詳細については、「<xref:fundamentals/websockets>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-210">For more information, see <xref:fundamentals/websockets>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="microsoftaspnetcoreapp-metapackage"></a><span data-ttu-id="cf4b4-211">Microsoft.AspNetCore.App メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="cf4b4-211">Microsoft.AspNetCore.App metapackage</span></span>

<span data-ttu-id="cf4b4-212">[Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) メタパッケージは、パッケージ管理を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-212">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App/) metapackage simplifies package management.</span></span>

<span data-ttu-id="cf4b4-213">詳細については、「<xref:fundamentals/metapackage-app>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-213">For more information, see <xref:fundamentals/metapackage-app>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="microsoftaspnetcoreall-metapackage"></a><span data-ttu-id="cf4b4-214">Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="cf4b4-214">Microsoft.AspNetCore.All metapackage</span></span>

<span data-ttu-id="cf4b4-215">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-215">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="cf4b4-216">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-216">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="cf4b4-217">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-217">All supported packages by Entity Framework Core.</span></span>
* <span data-ttu-id="cf4b4-218">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-218">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="cf4b4-219">詳細については、「<xref:fundamentals/metapackage>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-219">For more information, see <xref:fundamentals/metapackage>.</span></span>

::: moniker-end

## <a name="net-core-vs-net-framework-runtime"></a><span data-ttu-id="cf4b4-220">.NET Core と .NET Framework ランタイム</span><span class="sxs-lookup"><span data-stu-id="cf4b4-220">.NET Core vs. .NET Framework runtime</span></span>

<span data-ttu-id="cf4b4-221">ASP.NET Core アプリは、.NET Core または .NET Framework ランタイムを対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-221">An ASP.NET Core app can target the .NET Core or .NET Framework runtime.</span></span>

<span data-ttu-id="cf4b4-222">詳細については、「[サーバー アプリ用 .NET Core と .NET Framework の選択](/dotnet/articles/standard/choosing-core-framework-server)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-222">For more information, see [Choosing between .NET Core and .NET Framework](/dotnet/articles/standard/choosing-core-framework-server).</span></span>

## <a name="choose-between-aspnet-core-and-aspnet"></a><span data-ttu-id="cf4b4-223">ASP.NET Core と ASP.NET の選択</span><span class="sxs-lookup"><span data-stu-id="cf4b4-223">Choose between ASP.NET Core and ASP.NET</span></span>

<span data-ttu-id="cf4b4-224">ASP.NET Core と ASP.NET の選択の詳細については、「<xref:fundamentals/choose-between-aspnet-and-aspnetcore>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cf4b4-224">For more information on choosing between ASP.NET Core and ASP.NET, see <xref:fundamentals/choose-between-aspnet-and-aspnetcore>.</span></span>
