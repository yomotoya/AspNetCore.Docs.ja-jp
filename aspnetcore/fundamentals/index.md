---
title: ASP.NET Core の基礎
author: rick-anderson
description: ASP.NET Core アプリの構築に関する基本概念について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/index
ms.openlocfilehash: 8bd447632f915cadcc5199ec50b292ad27f6c3ba
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861590"
---
# <a name="aspnet-core-fundamentals"></a><span data-ttu-id="3b6de-103">ASP.NET Core の基礎</span><span class="sxs-lookup"><span data-stu-id="3b6de-103">ASP.NET Core fundamentals</span></span>

<span data-ttu-id="3b6de-104">ASP.NET Core アプリは、その `Program.Main` メソッドで Web サーバーを作成するコンソール アプリです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-104">An ASP.NET Core app is a console app that creates a web server in its `Program.Main` method.</span></span> <span data-ttu-id="3b6de-105">`Main` メソッドは、アプリの*マネージド エントリ ポイント*です。</span><span class="sxs-lookup"><span data-stu-id="3b6de-105">The `Main` method is the app's *managed entry point*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

<span data-ttu-id="3b6de-106">.NET Core ホスト:</span><span class="sxs-lookup"><span data-stu-id="3b6de-106">The .NET Core Host:</span></span>

* <span data-ttu-id="3b6de-107">[.NET Core ランタイム](https://github.com/dotnet/coreclr)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-107">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="3b6de-108">エントリ ポイント (`Main`) を含むマネージド バイナリへのパスとして最初のコマンドライン引数を使用し、コードの実行を開始します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-108">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="3b6de-109">`Main` メソッドは、Web ホストを作成する[ビルダー パターン](https://wikipedia.org/wiki/Builder_pattern)に従う、[WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*) を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-109">The `Main` method invokes [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*), which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web host.</span></span> <span data-ttu-id="3b6de-110">ビルダーには、Web サーバー (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> など) とスタートアップ クラス (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-110">The builder has methods that define a web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="3b6de-111">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-111">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is automatically allocated.</span></span> <span data-ttu-id="3b6de-112">ASP.NET Core の Web ホストは、[インターネット インフォメーション サービス (IIS)](https://www.iis.net/) (使用可能な場合) で実行を試みます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-112">ASP.NET Core's web host attempts to run on [Internet Information Services (IIS)](https://www.iis.net/), if available.</span></span> <span data-ttu-id="3b6de-113">[HTTP.sys](xref:fundamentals/servers/httpsys) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-113">Other web servers, such as [HTTP.sys](xref:fundamentals/servers/httpsys), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3b6de-114">`UseStartup` については、「[スタートアップ](#startup)」のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-114">`UseStartup` is explained further in the [Startup](#startup) section.</span></span>

<span data-ttu-id="3b6de-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> (`WebHost.CreateDefaultBuilder` 呼び出しの戻り値の型) では、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-115"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>, the return type of the `WebHost.CreateDefaultBuilder` invocation, provides many optional methods.</span></span> <span data-ttu-id="3b6de-116">これらのメソッドの一部には、HTTP.sys でアプリをホストするための `UseHttpSys` と、ルート コンテンツ ディレクトリを指定するための <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3b6de-116">Some of these methods include `UseHttpSys` for hosting the app in HTTP.sys and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="3b6de-117"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> および <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する <xref:Microsoft.AspNetCore.Hosting.IWebHost> オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-117">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

<span data-ttu-id="3b6de-118">.NET Core ホスト:</span><span class="sxs-lookup"><span data-stu-id="3b6de-118">The .NET Core Host:</span></span>

* <span data-ttu-id="3b6de-119">[.NET Core ランタイム](https://github.com/dotnet/coreclr)を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-119">Loads the [.NET Core runtime](https://github.com/dotnet/coreclr).</span></span>
* <span data-ttu-id="3b6de-120">エントリ ポイント (`Main`) を含むマネージド バイナリへのパスとして最初のコマンドライン引数を使用し、コードの実行を開始します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-120">Uses the first command-line argument as the path to the managed binary that contains the entry point (`Main`) and begins code execution.</span></span>

<span data-ttu-id="3b6de-121">`Main` メソッドは、Web アプリ ホストを作成する[ビルダー パターン](https://wikipedia.org/wiki/Builder_pattern)に従う、<xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-121">The `Main` method uses <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, which follows the [builder pattern](https://wikipedia.org/wiki/Builder_pattern) to create a web app host.</span></span> <span data-ttu-id="3b6de-122">ビルダーには、Web サーバー (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> など) とスタートアップ クラス (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) を定義するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-122">The builder has methods that define the web server (for example, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>) and the startup class (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>).</span></span> <span data-ttu-id="3b6de-123">前の例では、[Kestrel](xref:fundamentals/servers/kestrel) Web サーバーが使用されています。</span><span class="sxs-lookup"><span data-stu-id="3b6de-123">In the preceding example, the [Kestrel](xref:fundamentals/servers/kestrel) web server is used.</span></span> <span data-ttu-id="3b6de-124">[WebListener](xref:fundamentals/servers/weblistener) などの他の Web サーバーは、適切な拡張メソッドを呼び出して使用することができます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-124">Other web servers, such as [WebListener](xref:fundamentals/servers/weblistener), can be used by invoking the appropriate extension method.</span></span> <span data-ttu-id="3b6de-125">`UseStartup` については、次のセクションで詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-125">`UseStartup` is explained further in the next section.</span></span>

<span data-ttu-id="3b6de-126">`WebHostBuilder` では、IIS および IIS Express でホストするための <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> と、ルート コンテンツ ディレクトリを指定するための <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> を含む、省略可能な多くのメソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-126">`WebHostBuilder` provides many optional methods, including <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> for hosting in IIS and IIS Express and <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*> for specifying the root content directory.</span></span> <span data-ttu-id="3b6de-127"><xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> および <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> メソッドは、アプリをホストし、HTTP 要求のリッスンを開始する <xref:Microsoft.AspNetCore.Hosting.IWebHost> オブジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-127">The <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> and <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> methods build the <xref:Microsoft.AspNetCore.Hosting.IWebHost> object that hosts the app and begins listening for HTTP requests.</span></span>

::: moniker-end

## <a name="startup"></a><span data-ttu-id="3b6de-128">スタートアップ</span><span class="sxs-lookup"><span data-stu-id="3b6de-128">Startup</span></span>

<span data-ttu-id="3b6de-129">`WebHostBuilder` の `UseStartup` メソッドは、アプリの `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-129">The `UseStartup` method on `WebHostBuilder` specifies the `Startup` class for your app:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

<span data-ttu-id="3b6de-130">`Startup` クラスでは、要求処理パイプラインを定義し、アプリで必要なサービスが構成されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-130">The `Startup` class is where you define the request handling pipeline and where any services needed by the app are configured.</span></span> <span data-ttu-id="3b6de-131">`Startup` クラスはパブリックであり、次のメソッドを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-131">The `Startup` class must be public and contain the following methods:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<span data-ttu-id="3b6de-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> は、アプリで使用される[サービス](#dependency-injection-services) (ASP.NET Core MVC、Entity Framework Core、Identity など) を定義します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-132"><xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> defines the [Services](#dependency-injection-services) used by your app (for example, ASP.NET Core MVC, Entity Framework Core, Identity).</span></span> <span data-ttu-id="3b6de-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> は、要求パイプラインで呼び出される[ミドルウェア](xref:fundamentals/middleware/index)を定義します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-133"><xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> defines the [middleware](xref:fundamentals/middleware/index) called in the request pipeline.</span></span>

<span data-ttu-id="3b6de-134">詳細については、「<xref:fundamentals/startup>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-134">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="content-root"></a><span data-ttu-id="3b6de-135">コンテンツ ルート</span><span class="sxs-lookup"><span data-stu-id="3b6de-135">Content root</span></span>

<span data-ttu-id="3b6de-136">コンテンツ ルートは、[Razor ページ](xref:razor-pages/index)、MVC ビュー、静的資産など、アプリで使用されるコンテンツへの基本パスです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-136">The content root is the base path to any content used by the app, such as [Razor Pages](xref:razor-pages/index), MVC views, and static assets.</span></span> <span data-ttu-id="3b6de-137">既定では、コンテンツ ルートは、アプリをホストする実行可能ファイルのアプリ基本パスと同じ場所です。</span><span class="sxs-lookup"><span data-stu-id="3b6de-137">By default, the content root is the same location as the app base path for the executable hosting the app.</span></span>

## <a name="web-root-webroot"></a><span data-ttu-id="3b6de-138">Web ルート (webroot)</span><span class="sxs-lookup"><span data-stu-id="3b6de-138">Web root (webroot)</span></span>

<span data-ttu-id="3b6de-139">アプリの webroot は、CSS、JavaScript、イメージ ファイルなどのパブリックな静的なリソースを含むプロジェクトのディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-139">The webroot of an app is the directory in the project containing public, static resources, such as CSS, JavaScript, and image files.</span></span> <span data-ttu-id="3b6de-140">既定では、*wwwroot* が webroot です。</span><span class="sxs-lookup"><span data-stu-id="3b6de-140">By default, *wwwroot* is the webroot.</span></span>

<span data-ttu-id="3b6de-141">Razor (*.cshtml*) のファイルの場合、チルダとスラッシュ `~/` が webroot を指します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-141">For Razor (*.cshtml*) files, the tilde-slash  `~/` points to the webroot.</span></span> <span data-ttu-id="3b6de-142">`~/` で始まるパスは仮想パスと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-142">Paths beginning with `~/` are referred to as virtual paths.</span></span>

## <a name="dependency-injection-services"></a><span data-ttu-id="3b6de-143">依存性の注入 (サービス)</span><span class="sxs-lookup"><span data-stu-id="3b6de-143">Dependency injection (services)</span></span>

<span data-ttu-id="3b6de-144">*サービス*は、アプリでの一般的な使用を想定されているコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-144">A *service* is a component that's intended for common consumption in an app.</span></span> <span data-ttu-id="3b6de-145">サービスは[依存性の注入 (DI)](xref:fundamentals/dependency-injection) を介して使用できます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-145">Services are made available through [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3b6de-146">ASP.NET Core には、既定で[コンストラクターの挿入](xref:mvc/controllers/dependency-injection#constructor-injection)をサポートする、ネイティブの制御の反転 (Inversion of Control、IoC) コンテナーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-146">ASP.NET Core includes a native Inversion of Control (IoC) container that supports [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) by default.</span></span> <span data-ttu-id="3b6de-147">必要に応じて、既定のコンテナーを置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-147">You can replace the default container if you wish.</span></span> <span data-ttu-id="3b6de-148">その[疎結合の利点](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)に加え、DI はアプリ全体でサービス ([ログ](xref:fundamentals/logging/index)など) を使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-148">In addition to its [loose coupling benefit](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation), DI makes services, such as [logging](xref:fundamentals/logging/index), available throughout your app.</span></span>

<span data-ttu-id="3b6de-149">詳細については、「<xref:fundamentals/dependency-injection>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-149">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="middleware"></a><span data-ttu-id="3b6de-150">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="3b6de-150">Middleware</span></span>

<span data-ttu-id="3b6de-151">ASP.NET Core では、[ミドルウェア](xref:fundamentals/middleware/index)を使用して要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-151">In ASP.NET Core, you compose your request pipeline using [middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="3b6de-152">ASP.NET Core ミドルウェアは `HttpContext` で非同期操作を実行してから、シーケンス内の次のミドルウェアを呼び出すか、直接要求を終了します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-152">ASP.NET Core middleware performs asynchronous operations on an `HttpContext` and then either invokes the next middleware in the pipeline or terminates the request.</span></span>

<span data-ttu-id="3b6de-153">通常、"XYZ" と呼ばれるミドルウェア コンポーネントは、`Configure` メソッドで `UseXYZ` 拡張メソッドを呼び出してパイプラインに追加されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-153">By convention, a middleware component called "XYZ" is added to the pipeline by invoking a `UseXYZ` extension method in the `Configure` method.</span></span>

<span data-ttu-id="3b6de-154">ASP.NET Core には組み込みのミドルウェアの豊富なセットが含まれており、独自のカスタム ミドルウェアを作成できます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-154">ASP.NET Core includes a rich set of built-in middleware, and you can write your own custom middleware.</span></span> <span data-ttu-id="3b6de-155">ASP.NET Core アプリでは、[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin) がサポートされています。これにより、Web アプリケーションを Web サーバーから切り離すことができます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-155">[Open Web Interface for .NET (OWIN)](xref:fundamentals/owin), which allows web apps to be decoupled from web servers, is supported in ASP.NET Core apps.</span></span>

<span data-ttu-id="3b6de-156">詳細については、次のトピックを参照してください。 <xref:fundamentals/middleware/index> および <xref:fundamentals/owin>.</span><span class="sxs-lookup"><span data-stu-id="3b6de-156">For more information, see <xref:fundamentals/middleware/index> and <xref:fundamentals/owin>.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a><span data-ttu-id="3b6de-157">HTTP 要求の開始</span><span class="sxs-lookup"><span data-stu-id="3b6de-157">Initiate HTTP requests</span></span>

<span data-ttu-id="3b6de-158"><xref:System.Net.Http.IHttpClientFactory> を使用して <xref:System.Net.Http.HttpClient> インスタンスアクセスにアクセスし、HTTP 要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-158"><xref:System.Net.Http.IHttpClientFactory> is available to access <xref:System.Net.Http.HttpClient> instances to make HTTP requests.</span></span>

<span data-ttu-id="3b6de-159">詳細については、「<xref:fundamentals/http-requests>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-159">For more information, see <xref:fundamentals/http-requests>.</span></span>

::: moniker-end

## <a name="environments"></a><span data-ttu-id="3b6de-160">環境</span><span class="sxs-lookup"><span data-stu-id="3b6de-160">Environments</span></span>

<span data-ttu-id="3b6de-161">*開発*や*実稼働*などの環境は ASP.NET Core におけるファースト クラスの概念であり、環境変数、設定ファイル、およびコマンド ライン引数を使用して設定できます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-161">Environments, such as *Development* and *Production*, are a first-class notion in ASP.NET Core and can be set using an environment variable, settings file, and command-line argument.</span></span>

<span data-ttu-id="3b6de-162">詳細については、「<xref:fundamentals/environments>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-162">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="hosting"></a><span data-ttu-id="3b6de-163">ホスト</span><span class="sxs-lookup"><span data-stu-id="3b6de-163">Hosting</span></span>

<span data-ttu-id="3b6de-164">ASP.NET Core アプリは、アプリの起動と有効期間の管理を担当する*ホスト*を構成して起動します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-164">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="3b6de-165">詳細については、「<xref:fundamentals/host/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-165">For more information, see <xref:fundamentals/host/index>.</span></span>

## <a name="servers"></a><span data-ttu-id="3b6de-166">サーバー</span><span class="sxs-lookup"><span data-stu-id="3b6de-166">Servers</span></span>

<span data-ttu-id="3b6de-167">ASP.NET Core のホスティング モデルは、直接要求をリッスンしません。</span><span class="sxs-lookup"><span data-stu-id="3b6de-167">The ASP.NET Core hosting model doesn't directly listen for requests.</span></span> <span data-ttu-id="3b6de-168">ホスティング モデルは HTTP サーバー実装に依存して要求をアプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-168">The hosting model relies on an HTTP server implementation to forward the request to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="3b6de-169">Windows</span><span class="sxs-lookup"><span data-stu-id="3b6de-169">Windows</span></span>](#tab/windows)

<span data-ttu-id="3b6de-170">ASP.NET Core では、次のサーバー実装が提供されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-170">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="3b6de-171">[Kestrel](xref:fundamentals/servers/kestrel) サーバーは、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-171">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-172">Kestrel は [IIS](https://www.iis.net/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-172">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="3b6de-173">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-173">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="3b6de-174">IIS HTTP サーバー (`IISHttpServer`) は、[インプロセス IIS サーバー](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model)です。</span><span class="sxs-lookup"><span data-stu-id="3b6de-174">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model).</span></span>
* <span data-ttu-id="3b6de-175">[HTTP.sys](xref:fundamentals/servers/httpsys) サーバーは Windows 上の ASP.NET Core 用 Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-175">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="3b6de-176">macOS</span><span class="sxs-lookup"><span data-stu-id="3b6de-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="3b6de-177">ASP.NET Core は [Kestrel](xref:fundamentals/servers/kestrel) サーバーの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-177">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="3b6de-178">Kestrel は、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-178">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-179">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-179">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="3b6de-180">Linux</span><span class="sxs-lookup"><span data-stu-id="3b6de-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="3b6de-181">ASP.NET Core は [Kestrel](xref:fundamentals/servers/kestrel) サーバーの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-181">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="3b6de-182">Kestrel は、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-182">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-183">Kestrel は [Nginx](http://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-183">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3b6de-184">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-184">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="3b6de-185">Windows</span><span class="sxs-lookup"><span data-stu-id="3b6de-185">Windows</span></span>](#tab/windows)

<span data-ttu-id="3b6de-186">ASP.NET Core では、次のサーバー実装が提供されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-186">ASP.NET Core provides the following server implementations:</span></span>

* <span data-ttu-id="3b6de-187">[Kestrel](xref:fundamentals/servers/kestrel) サーバーは、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-187">[Kestrel](xref:fundamentals/servers/kestrel) server is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-188">Kestrel は [IIS](https://www.iis.net/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-188">Kestrel is often run in a reverse proxy configuration using [IIS](https://www.iis.net/).</span></span> <span data-ttu-id="3b6de-189">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-189">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>
* <span data-ttu-id="3b6de-190">[HTTP.sys](xref:fundamentals/servers/httpsys) サーバーは Windows 上の ASP.NET Core 用 Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-190">[HTTP.sys](xref:fundamentals/servers/httpsys) server is a web server for ASP.NET Core on Windows.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="3b6de-191">macOS</span><span class="sxs-lookup"><span data-stu-id="3b6de-191">macOS</span></span>](#tab/macos)

<span data-ttu-id="3b6de-192">ASP.NET Core は [Kestrel](xref:fundamentals/servers/kestrel) サーバーの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-192">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="3b6de-193">Kestrel は、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-193">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-194">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-194">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="3b6de-195">Linux</span><span class="sxs-lookup"><span data-stu-id="3b6de-195">Linux</span></span>](#tab/linux)

<span data-ttu-id="3b6de-196">ASP.NET Core は [Kestrel](xref:fundamentals/servers/kestrel) サーバーの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-196">ASP.NET Core uses the [Kestrel](xref:fundamentals/servers/kestrel) server implementation.</span></span> <span data-ttu-id="3b6de-197">Kestrel は、管理された、クロスプラットフォームの Web サーバーです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-197">Kestrel is a managed, cross-platform web server.</span></span> <span data-ttu-id="3b6de-198">Kestrel は [Nginx](http://nginx.org) または [Apache](https://httpd.apache.org/) を使用してリバース プロキシ構成で実行されることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-198">Kestrel is often run in a reverse proxy configuration with [Nginx](http://nginx.org) or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3b6de-199">ASP.NET Core 2.0 以降では、Kestrel は、インターネットに直接公開される一般向けエッジ サーバーとして実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-199">Kestrel can also be run as a public-facing edge server exposed directly to the Internet in ASP.NET Core 2.0 or later.</span></span>

---

::: moniker-end

<span data-ttu-id="3b6de-200">詳細については、「<xref:fundamentals/servers/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-200">For more information, see <xref:fundamentals/servers/index>.</span></span>

## <a name="configuration"></a><span data-ttu-id="3b6de-201">構成</span><span class="sxs-lookup"><span data-stu-id="3b6de-201">Configuration</span></span>

<span data-ttu-id="3b6de-202">ASP.NET Core は、名前と値のペアに基づく構成モデルを使用します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-202">ASP.NET Core uses a configuration model based on name-value pairs.</span></span> <span data-ttu-id="3b6de-203">この構成モデルは <xref:System.Configuration> または *web.config* には基づきません。構成は、構成プロバイダーの順序付けされたセットから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-203">The configuration model isn't based on <xref:System.Configuration> or *web.config*. Configuration obtains settings from an ordered set of configuration providers.</span></span> <span data-ttu-id="3b6de-204">組み込みの構成プロバイダーは、さまざまなファイル形式 (XML、JSON、INI)、環境変数、およびコマンド ライン引数をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-204">The built-in configuration providers support a variety of file formats (XML, JSON, INI), environment variables, and command-line arguments.</span></span> <span data-ttu-id="3b6de-205">独自のカスタム構成プロバイダーを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-205">You can also write your own custom configuration providers.</span></span>

<span data-ttu-id="3b6de-206">詳細については、「<xref:fundamentals/configuration/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-206">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="logging"></a><span data-ttu-id="3b6de-207">ログの記録</span><span class="sxs-lookup"><span data-stu-id="3b6de-207">Logging</span></span>

<span data-ttu-id="3b6de-208">ASP.NET Core は、さまざまなログ プロバイダーと連携するログ API をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-208">ASP.NET Core supports a Logging API that works with a variety of logging providers.</span></span> <span data-ttu-id="3b6de-209">組み込みプロバイダーは、1 つまたは複数の送信先へのログの送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="3b6de-209">Built-in providers support sending logs to one or more destinations.</span></span> <span data-ttu-id="3b6de-210">サード パーティ製のログ記録フレームワークを使用できます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-210">Third-party logging frameworks can be used.</span></span>

<span data-ttu-id="3b6de-211">詳細については、「<xref:fundamentals/logging/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-211">For more information, see <xref:fundamentals/logging/index>.</span></span>

## <a name="error-handling"></a><span data-ttu-id="3b6de-212">エラー処理</span><span class="sxs-lookup"><span data-stu-id="3b6de-212">Error handling</span></span>

<span data-ttu-id="3b6de-213">ASP.NET Core には、開発者の例外ページ、カスタム エラー ページ、静的ステータス コード ページ、起動時の例外処理を含む、アプリのエラー処理のシナリオが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="3b6de-213">ASP.NET Core has built-in scenarios for handling errors in apps, including a developer exception page, custom error pages, static status code pages, and startup exception handling.</span></span>

<span data-ttu-id="3b6de-214">詳細については、「<xref:fundamentals/error-handling>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-214">For more information, see <xref:fundamentals/error-handling>.</span></span>

## <a name="routing"></a><span data-ttu-id="3b6de-215">ルーティング</span><span class="sxs-lookup"><span data-stu-id="3b6de-215">Routing</span></span>

<span data-ttu-id="3b6de-216">ASP.NET Core は、ルート ハンドラーにアプリ要求をルーティングするシナリオを提供します。</span><span class="sxs-lookup"><span data-stu-id="3b6de-216">ASP.NET Core offers scenarios for routing of app requests to route handlers.</span></span>

<span data-ttu-id="3b6de-217">詳細については、「<xref:fundamentals/routing>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-217">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="background-tasks"></a><span data-ttu-id="3b6de-218">バックグラウンド タスク</span><span class="sxs-lookup"><span data-stu-id="3b6de-218">Background tasks</span></span>

<span data-ttu-id="3b6de-219">バックグラウンド タスクは*ホストされるサービス*として実装されます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-219">Background tasks are implemented as *hosted services*.</span></span> <span data-ttu-id="3b6de-220">ホストされるサービスは、<xref:Microsoft.Extensions.Hosting.IHostedService> インターフェイスを実装するバックグラウンド タスク ロジックを持つクラスです。</span><span class="sxs-lookup"><span data-stu-id="3b6de-220">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span>

<span data-ttu-id="3b6de-221">詳細については、「<xref:fundamentals/host/hosted-services>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-221">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

## <a name="access-httpcontext"></a><span data-ttu-id="3b6de-222">HttpContext へのアクセス</span><span class="sxs-lookup"><span data-stu-id="3b6de-222">Access HttpContext</span></span>

<span data-ttu-id="3b6de-223">`HttpContext` は、Razor ページおよび MVC で要求を処理するときに自動的に使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="3b6de-223">`HttpContext` is automatically available when processing requests with Razor Pages and MVC.</span></span> <span data-ttu-id="3b6de-224">`HttpContext` をすぐに使用できない状況では、<xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> インターフェイスとその既定の実装 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> を介して `HttpContext` にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b6de-224">In circumstances where `HttpContext` isn't readily available, you can access the `HttpContext` through the <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> interface and its default implementation, <xref:Microsoft.AspNetCore.Http.HttpContextAccessor>.</span></span>

<span data-ttu-id="3b6de-225">詳細については、「<xref:fundamentals/httpcontext>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b6de-225">For more information, see <xref:fundamentals/httpcontext>.</span></span>
