---
title: ASP.NET Core でのアプリケーションのスタートアップ
author: tdykstra
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/startup
ms.openlocfilehash: d629c2f02f9021454d8f8539018f3e2ca4703a00
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346399"
---
# <a name="app-startup-in-aspnet-core"></a><span data-ttu-id="017dc-103">ASP.NET Core でのアプリケーションのスタートアップ</span><span class="sxs-lookup"><span data-stu-id="017dc-103">App startup in ASP.NET Core</span></span>

<span data-ttu-id="017dc-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex)、[Steve Smith](https://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="017dc-104">By [Tom Dykstra](https://github.com/tdykstra), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com)</span></span>

<span data-ttu-id="017dc-105">`Startup` クラスはサービスとアプリケーションの要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="017dc-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="017dc-106">Startup クラス</span><span class="sxs-lookup"><span data-stu-id="017dc-106">The Startup class</span></span>

<span data-ttu-id="017dc-107">ASP.NET Core アプリケーションでは `Startup` クラスが使用されています。このクラスは規約に従って `Startup` と名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="017dc-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="017dc-108">`Startup` クラス:</span><span class="sxs-lookup"><span data-stu-id="017dc-108">The `Startup` class:</span></span>

* <span data-ttu-id="017dc-109">必要に応じて <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> メソッドを含め、アプリの*サービス*を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="017dc-109">Optionally includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method to configure the app's *services*.</span></span> <span data-ttu-id="017dc-110">サービスとは、アプリ機能を提供する再利用可能なコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="017dc-110">A service is a reusable component that provides app functionality.</span></span> <span data-ttu-id="017dc-111">サービスは `ConfigureServices` で構成され (&mdash;*登録*と表現されることもあります&mdash;)、[依存関係の挿入 (DI)](xref:fundamentals/dependency-injection) または <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> を介してアプリ全体で利用されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-111">Services are configured&mdash;also described as *registered*&mdash;in `ConfigureServices` and consumed across the app via [dependency injection (DI)](xref:fundamentals/dependency-injection) or <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>
* <span data-ttu-id="017dc-112">アプリの要求処理パイプラインを作成するために <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> メソッドを含めます。</span><span class="sxs-lookup"><span data-stu-id="017dc-112">Includes a <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="017dc-113">`ConfigureServices` と `Configure` はアプリケーションの起動時にランタイムによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-113">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup1.cs?highlight=4,10)]

<span data-ttu-id="017dc-114">アプリの`Startup`ホスト[がビルドされるとき、](xref:fundamentals/index#host) クラスがアプリに指定されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-114">The `Startup` class is specified to the app when the app's [host](xref:fundamentals/index#host) is built.</span></span> <span data-ttu-id="017dc-115">`Program` クラスのホスト ビルダーで `Build` が呼び出されるとき、アプリのホストがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="017dc-115">The app's host is built when `Build` is called on the host builder in the `Program` class.</span></span> <span data-ttu-id="017dc-116">`Startup` クラスは通常、ホスト ビルダーで [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) メソッドを呼び出すことで指定されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-116">The `Startup` class is usually specified by calling the [WebHostBuilderExtensions.UseStartup\<TStartup>](xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*) method on the host builder:</span></span>

[!code-csharp[](startup/sample_snapshot/Program3.cs?name=snippet_Program&highlight=10)]

<span data-ttu-id="017dc-117">ホストには、`Startup` クラス コンストラクターで使用できるサービスが用意されています。</span><span class="sxs-lookup"><span data-stu-id="017dc-117">The host provides services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="017dc-118">アプリケーションから `ConfigureServices` 経由でサービスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-118">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="017dc-119">これで、ホスト サービスとアプリケーション サービスの両方が `Configure` 内とアプリ全体で使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="017dc-119">Both the host and app services are then available in `Configure` and throughout the app.</span></span>

<span data-ttu-id="017dc-120">`Startup` クラスへの[依存関係挿入](xref:fundamentals/dependency-injection)の一般的な用途は、以下を挿入する場合です。</span><span class="sxs-lookup"><span data-stu-id="017dc-120">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="017dc-121"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> (環境別にサービスを構成するため)。</span><span class="sxs-lookup"><span data-stu-id="017dc-121"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> to configure services by environment.</span></span>
* <span data-ttu-id="017dc-122"><xref:Microsoft.Extensions.Configuration.IConfiguration> (構成を読み取るため)。</span><span class="sxs-lookup"><span data-stu-id="017dc-122"><xref:Microsoft.Extensions.Configuration.IConfiguration> to read configuration.</span></span>
* <span data-ttu-id="017dc-123"><xref:Microsoft.Extensions.Logging.ILoggerFactory> (`Startup.ConfigureServices` でロガーを作成するため)。</span><span class="sxs-lookup"><span data-stu-id="017dc-123"><xref:Microsoft.Extensions.Logging.ILoggerFactory> to create a logger in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](startup/sample_snapshot/Startup2.cs?highlight=7-8)]

<span data-ttu-id="017dc-124">`IHostingEnvironment` を挿入する代わりに、規約ベースのアプローチがあります。</span><span class="sxs-lookup"><span data-stu-id="017dc-124">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="017dc-125">アプリケーションの環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスが定義されると、実行時に適切な `Startup` クラスが選択されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-125">When the app defines separate `Startup` classes for different environments (for example, `StartupDevelopment`), the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="017dc-126">名前のサフィックスが現在の環境と一致するクラスが優先されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-126">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="017dc-127">アプリケーションが Development 環境で実行され、`Startup` クラスと `StartupDevelopment` クラスの両方が含まれている場合は、`StartupDevelopment` クラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-127">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="017dc-128">詳細については、「[Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-128">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="017dc-129">ホストの詳細については、「[ホスト](xref:fundamentals/index#host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-129">To learn more about the host, see [The host](xref:fundamentals/index#host).</span></span> <span data-ttu-id="017dc-130">スタートアップ時のエラー処理については、「[Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling)」(スタートアップ例外処理) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-130">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="017dc-131">ConfigureServices メソッド</span><span class="sxs-lookup"><span data-stu-id="017dc-131">The ConfigureServices method</span></span>

<span data-ttu-id="017dc-132"><xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> メソッド:</span><span class="sxs-lookup"><span data-stu-id="017dc-132">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> method is:</span></span>

* <span data-ttu-id="017dc-133">任意。</span><span class="sxs-lookup"><span data-stu-id="017dc-133">Optional.</span></span>
* <span data-ttu-id="017dc-134">アプリケーションのサービスを構成する `Configure` メソッドの前にホストによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-134">Called by the host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="017dc-135">規約によって[構成オプション](xref:fundamentals/configuration/index)が設定されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-135">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="017dc-136">一般的なパターンとしては、すべての `Add{Service}` メソッドを呼び出した後にすべての `services.Configure{Service}` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="017dc-136">The typical pattern is to call all the `Add{Service}` methods and then call all of the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="017dc-137">たとえば、[Identity サービスの構成](xref:security/authentication/identity#pw)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="017dc-137">For example, see [Configure Identity services](xref:security/authentication/identity#pw).</span></span>

<span data-ttu-id="017dc-138">ホストでは、`Startup` メソッドが呼び出される前にいくつかのサービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="017dc-138">The host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="017dc-139">詳細については、「[ホスト](xref:fundamentals/index#host)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-139">For more information, see [The host](xref:fundamentals/index#host).</span></span>

<span data-ttu-id="017dc-140">多くの設定が必要な機能には、<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 上の `Add{Service}` 拡張メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="017dc-140">For features that require substantial setup, there are `Add{Service}` extension methods on <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>.</span></span> <span data-ttu-id="017dc-141">一般的な ASP.NET Core アプリは、Entity Framework、Identity、および MVC のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="017dc-141">A typical ASP.NET Core app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[](startup/sample_snapshot/Startup3.cs?highlight=4,7,11)]

<span data-ttu-id="017dc-142">サービス コンテナーにサービスを追加すると、アプリケーション内と `Configure` メソッド内でサービスを利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="017dc-142">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="017dc-143">サービスは[依存関係の挿入](xref:fundamentals/dependency-injection)または <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*> によって解決されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-143">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices*>.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="017dc-144">Configure メソッド</span><span class="sxs-lookup"><span data-stu-id="017dc-144">The Configure method</span></span>

<span data-ttu-id="017dc-145"><xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> メソッドは、アプリケーションが HTTP 要求にどのように応答するかを指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-145">The <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="017dc-146">要求パイプラインは、[ミドルウェア](xref:fundamentals/middleware/index) コンポーネントを <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> インスタンスに追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-146">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> instance.</span></span> <span data-ttu-id="017dc-147">`IApplicationBuilder` は `Configure` メソッドから使用できますが、サービス コンテナーに登録されていません。</span><span class="sxs-lookup"><span data-stu-id="017dc-147">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="017dc-148">ホスティングによって `IApplicationBuilder` が作成され、`Configure` に直接渡されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-148">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure`.</span></span>

<span data-ttu-id="017dc-149">[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)では、次をサポートするパイプラインが構成されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-149">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for:</span></span>

* [<span data-ttu-id="017dc-150">開発者例外ページ</span><span class="sxs-lookup"><span data-stu-id="017dc-150">Developer Exception Page</span></span>](xref:fundamentals/error-handling#developer-exception-page)
* [<span data-ttu-id="017dc-151">例外ハンドラー</span><span class="sxs-lookup"><span data-stu-id="017dc-151">Exception handler</span></span>](xref:fundamentals/error-handling#configure-a-custom-exception-handling-page)
* [<span data-ttu-id="017dc-152">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="017dc-152">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts)
* [<span data-ttu-id="017dc-153">HTTPS リダイレクト</span><span class="sxs-lookup"><span data-stu-id="017dc-153">HTTPS redirection</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="017dc-154">静的ファイル</span><span class="sxs-lookup"><span data-stu-id="017dc-154">Static files</span></span>](xref:fundamentals/static-files)
* [<span data-ttu-id="017dc-155">一般データ保護規制 (GDPR)</span><span class="sxs-lookup"><span data-stu-id="017dc-155">General Data Protection Regulation (GDPR)</span></span>](xref:security/gdpr)
* <span data-ttu-id="017dc-156">ASP.NET Core [MVC](xref:mvc/overview) と [Razor ページ](xref:razor-pages/index)</span><span class="sxs-lookup"><span data-stu-id="017dc-156">ASP.NET Core [MVC](xref:mvc/overview) and [Razor Pages](xref:razor-pages/index)</span></span>

[!code-csharp[](startup/sample_snapshot/Startup4.cs)]

<span data-ttu-id="017dc-157">各 `Use` 拡張メソッドによって、要求パイプラインに 1 つまたは複数のミドルウェア コンポーネントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-157">Each `Use` extension method adds one or more middleware components to the request pipeline.</span></span> <span data-ttu-id="017dc-158">たとえば、`UseMvc` 拡張メソッドでは、[ルーティング ミドルウェア](xref:fundamentals/routing)が要求パイプラインに追加され、既定のハンドラーとして [MVC](xref:mvc/overview) が構成されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-158">For instance, the `UseMvc` extension method adds [Routing Middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="017dc-159">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。</span><span class="sxs-lookup"><span data-stu-id="017dc-159">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span> <span data-ttu-id="017dc-160">ショート サーキットがミドルウェアのチェーンで発生しない場合、各ミドルウェアには、クライアントに送信される前に要求を処理する 2 度目の機会があります。</span><span class="sxs-lookup"><span data-stu-id="017dc-160">If short-circuiting doesn't occur along the middleware chain, each middleware has a second chance to process the request before it's sent to the client.</span></span>

<span data-ttu-id="017dc-161">また、`IHostingEnvironment` や `ILoggerFactory` などの追加サービスを `Configure` メソッド シグネチャで指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="017dc-161">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the `Configure` method signature.</span></span> <span data-ttu-id="017dc-162">指定すると、(使用可能なサービスの場合) 追加サービスが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-162">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="017dc-163">`IApplicationBuilder` の使用方法およびミドルウェアの処理の順序については、「<xref:fundamentals/middleware/index>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-163">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see <xref:fundamentals/middleware/index>.</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="017dc-164">簡易メソッド</span><span class="sxs-lookup"><span data-stu-id="017dc-164">Convenience methods</span></span>

<span data-ttu-id="017dc-165">`Startup` クラスを使用せず、サービスと要求処理パイプラインを構成するには、ホスト ビルダーで便利なメソッド、`ConfigureServices` と `Configure` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="017dc-165">To configure services and the request processing pipeline without using a `Startup` class, call `ConfigureServices` and `Configure` convenience methods on the host builder.</span></span> <span data-ttu-id="017dc-166">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-166">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="017dc-167">`Configure` メソッドが複数回呼び出された場合、最後の `Configure` 呼び出しが使用されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-167">If multiple `Configure` method calls exist, the last `Configure` call is used.</span></span>

[!code-csharp[](startup/sample_snapshot/Program1.cs?highlight=18,22)]

## <a name="extend-startup-with-startup-filters"></a><span data-ttu-id="017dc-168">スタートアップ フィルターを使用した Startup の拡張</span><span class="sxs-lookup"><span data-stu-id="017dc-168">Extend Startup with startup filters</span></span>

<span data-ttu-id="017dc-169"><xref:Microsoft.AspNetCore.Hosting.IStartupFilter> を使用して、アプリケーションの [Configure](#the-configure-method) ミドルウェア パイプラインの先頭または末尾でミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="017dc-169">Use <xref:Microsoft.AspNetCore.Hosting.IStartupFilter> to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="017dc-170">`IStartupFilter` は、アプリケーションの要求処理パイプラインの先頭または末尾で、ライブラリによってミドルウェアが追加される前または後にミドルウェアを確実に実行する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="017dc-170">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="017dc-171">`IStartupFilter` は、`Action<IApplicationBuilder>` を受け取って返す 1 つのメソッド <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*> を実装しています。</span><span class="sxs-lookup"><span data-stu-id="017dc-171">`IStartupFilter` implements a single method, <xref:Microsoft.AspNetCore.Hosting.StartupBase.Configure*>, which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="017dc-172"><xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> で、アプリケーションの要求パイプラインを構成するクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="017dc-172">An <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="017dc-173">詳細については、「[Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)」(IApplicationBuilder を使用したミドルウェア パイプラインの作成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-173">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="017dc-174">各 `IStartupFilter` は、要求パイプラインで 1 つまたは複数のミドルウェアを実装します。</span><span class="sxs-lookup"><span data-stu-id="017dc-174">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="017dc-175">フィルターは、サービス コンテナーに追加された順に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-175">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="017dc-176">フィルターは、コントロールを次のフィルターに渡す前または後にミドルウェアを追加できるため、アプリケーション パイプラインの先頭または末尾に追加されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-176">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="017dc-177">`IStartupFilter` でミドルウェアを登録する方法を次の例に示します。</span><span class="sxs-lookup"><span data-stu-id="017dc-177">The following example demonstrates how to register a middleware with `IStartupFilter`.</span></span>

<span data-ttu-id="017dc-178">`RequestSetOptionsMiddleware` ミドルウェアによって、クエリ文字列パラメーターからオプション値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="017dc-178">The `RequestSetOptionsMiddleware` middleware sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsMiddleware.cs?name=snippet1&highlight=21)]

<span data-ttu-id="017dc-179">`RequestSetOptionsMiddleware` は `RequestSetOptionsStartupFilter` クラスで構成されています。</span><span class="sxs-lookup"><span data-stu-id="017dc-179">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample_snapshot/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="017dc-180">`IStartupFilter` は <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> のサービス コンテナーに登録され、`Startup` クラスの外側から `Startup` を補強します。</span><span class="sxs-lookup"><span data-stu-id="017dc-180">The `IStartupFilter` is registered in the service container in <xref:Microsoft.AspNetCore.Hosting.StartupBase.ConfigureServices*> and augments `Startup` from outside of the `Startup` class:</span></span>

[!code-csharp[](startup/sample_snapshot/Program2.cs?name=snippet1&highlight=4-5)]

<span data-ttu-id="017dc-181">`option` のクエリ文字列パラメーターを指定すると、ミドルウェアは MVC ミドルウェアが応答をレンダリングする前に値の割り当てを処理します。</span><span class="sxs-lookup"><span data-stu-id="017dc-181">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![レンダリングされたインデックス ページを表示するブラウザー ウィンドウ。](startup/_static/index.png)

<span data-ttu-id="017dc-184">ミドルウェアの実行順序は、`IStartupFilter` の登録順に設定されています。</span><span class="sxs-lookup"><span data-stu-id="017dc-184">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="017dc-185">複数の `IStartupFilter` の実装が、同じオブジェクトとやり取りする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="017dc-185">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="017dc-186">順序が重要な場合は、ミドルウェアの実行順序に合わせて `IStartupFilter` サービスの登録順序を指定してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-186">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="017dc-187">`IStartupFilter` に登録された他のアプリケーション ミドルウェアの前または後に実行される `IStartupFilter` の実装が 1 つまたは複数あるミドルウェアを、ライブラリで追加することができます。</span><span class="sxs-lookup"><span data-stu-id="017dc-187">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="017dc-188">ライブラリの `IStartupFilter` によって追加されたミドルウェアの前に `IStartupFilter` ミドルウェアを呼び出すには、ライブラリがサービス コンテナーに追加される前にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="017dc-188">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="017dc-189">後で呼び出すには、ライブラリが追加される後にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="017dc-189">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="add-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="017dc-190">外部アセンブリからの起動時に構成を追加する</span><span class="sxs-lookup"><span data-stu-id="017dc-190">Add configuration at startup from an external assembly</span></span>

<span data-ttu-id="017dc-191"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup> の実装により、アプリの `Startup` クラスの外部にある外部アセンブリから、起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="017dc-191">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="017dc-192">詳細については、「<xref:fundamentals/configuration/platform-specific-configuration>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="017dc-192">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="017dc-193">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="017dc-193">Additional resources</span></span>

* [<span data-ttu-id="017dc-194">ホスト</span><span class="sxs-lookup"><span data-stu-id="017dc-194">The host</span></span>](xref:fundamentals/index#host)
* <xref:fundamentals/environments>
* <xref:fundamentals/middleware/index>
* <xref:fundamentals/logging/index>
* <xref:fundamentals/configuration/index>
