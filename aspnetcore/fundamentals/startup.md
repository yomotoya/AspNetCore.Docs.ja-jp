---
title: "ASP.NET Core でのアプリケーションのスタートアップ"
author: ardalis
description: "ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/startup
ms.openlocfilehash: c324918b33af82b619bb2251f32308e4a57c27e5
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2018
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="f8ee8-103">ASP.NET Core でのアプリケーションのスタートアップ</span><span class="sxs-lookup"><span data-stu-id="f8ee8-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="f8ee8-104">作成者: [Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f8ee8-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f8ee8-105">`Startup` クラスはサービスとアプリケーションの要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="f8ee8-106">Startup クラス</span><span class="sxs-lookup"><span data-stu-id="f8ee8-106">The Startup class</span></span>

<span data-ttu-id="f8ee8-107">ASP.NET Core アプリケーションでは `Startup` クラスが使用されています。このクラスは規約に従って `Startup` と名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="f8ee8-108">`Startup` クラスには次の特徴があります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-108">The `Startup` class:</span></span>

* <span data-ttu-id="f8ee8-109">必要に応じて [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドを含め、アプリケーションのサービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="f8ee8-110">アプリケーションの要求処理パイプラインを作成するために [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="f8ee8-111">`ConfigureServices` と `Configure` はアプリケーションの起動時にランタイムによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="f8ee8-112">[WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) メソッドで `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="f8ee8-113">`Startup` クラス コンストラクターは、ホストに定義されている依存関係を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="f8ee8-114">`Startup` クラスへの[依存関係挿入](xref:fundamentals/dependency-injection)の一般的な用途は、以下を挿入する場合です。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="f8ee8-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) (環境別にサービスを構成するため)。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment.</span></span>
* <span data-ttu-id="f8ee8-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) (スタートアップ時にアプリケーションを構成するため)。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) to configure the app during startup.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="f8ee8-117">`IHostingEnvironment` を挿入する代わりに、規約ベースのアプローチがあります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-117">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="f8ee8-118">アプリケーションでは、環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスを定義することができます。実行時に適切な startup クラスが選択されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-118">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="f8ee8-119">名前のサフィックスが現在の環境と一致するクラスが優先されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-119">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="f8ee8-120">アプリケーションが Development 環境で実行され、`Startup` クラスと `StartupDevelopment` クラスの両方が含まれている場合は、`StartupDevelopment` クラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-120">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="f8ee8-121">詳細については、[「Working with multiple environments」](xref:fundamentals/environments#startup-conventions) (複数の環境での使用) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-121">For more information, see [Working with multiple environments](xref:fundamentals/environments#startup-conventions).</span></span>

<span data-ttu-id="f8ee8-122">`WebHostBuilder` の詳細については、[ホスティング](xref:fundamentals/hosting)に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-122">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/hosting) topic.</span></span> <span data-ttu-id="f8ee8-123">スタートアップ時のエラー処理については、「[Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling)」(スタートアップ例外処理) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-123">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="f8ee8-124">ConfigureServices メソッド</span><span class="sxs-lookup"><span data-stu-id="f8ee8-124">The ConfigureServices method</span></span>

<span data-ttu-id="f8ee8-125">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドの特徴:</span><span class="sxs-lookup"><span data-stu-id="f8ee8-125">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="f8ee8-126">任意。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-126">Optional.</span></span>
* <span data-ttu-id="f8ee8-127">アプリケーションのサービスを構成する `Configure` メソッドの前に、Web ホストによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-127">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="f8ee8-128">規約によって[構成オプション](xref:fundamentals/configuration/index)が設定されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-128">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="f8ee8-129">サービス コンテナーにサービスを追加すると、アプリケーション内と `Configure` メソッド内でサービスを利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-129">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="f8ee8-130">サービスは、[依存関係挿入](xref:fundamentals/dependency-injection)を介して、または [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) から解決されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-130">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="f8ee8-131">Web ホストでは、`Startup` メソッドが呼び出される前にいくつかのサービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-131">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="f8ee8-132">詳細については、[ホスティング](xref:fundamentals/hosting)に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-132">Details are available in the [Hosting](xref:fundamentals/hosting) topic.</span></span> 

<span data-ttu-id="f8ee8-133">多くの設定が必要な機能には、[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上の `Add[Service]` 拡張メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-133">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="f8ee8-134">一般的な Web アプリケーションは、Entity Framework、Identity、および MVC のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-134">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a><span data-ttu-id="f8ee8-135">Startup で使用できるサービス</span><span class="sxs-lookup"><span data-stu-id="f8ee8-135">Services available in Startup</span></span>

<span data-ttu-id="f8ee8-136">Web ホストには、`Startup` クラス コンストラクターに使用できるサービスがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-136">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="f8ee8-137">アプリケーションから `ConfigureServices` 経由でサービスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-137">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="f8ee8-138">これで、ホスト サービスとアプリケーション サービスの両方が `Configure` 内とアプリケーション全体で使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-138">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="f8ee8-139">Configure メソッド</span><span class="sxs-lookup"><span data-stu-id="f8ee8-139">The Configure method</span></span>

<span data-ttu-id="f8ee8-140">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドは、アプリケーションが HTTP 要求にどのように応答するかを指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-140">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="f8ee8-141">要求パイプラインは、[ミドルウェア](xref:fundamentals/middleware/index) コンポーネントを [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) インスタンスに追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-141">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="f8ee8-142">`IApplicationBuilder` は `Configure` メソッドから使用できますが、サービス コンテナーに登録されていません。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-142">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="f8ee8-143">ホスティングによって `IApplicationBuilder` が作成され、`Configure` ([参照元](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)) に直接渡されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-143">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="f8ee8-144">[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)では、開発者の例外ページ、[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的ファイル、および ASP.NET MVC をサポートするパイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-144">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="f8ee8-145">各 `Use` 拡張メソッドで、要求パイプラインにミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-145">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="f8ee8-146">たとえば、`UseMvc` 拡張メソッドでは、[ルーティング ミドルウェア](xref:fundamentals/routing)を要求パイプラインに追加し、既定のハンドラーとして [MVC](xref:mvc/overview) を構成します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-146">For instance, the `UseMvc` extension method adds the [routing middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="f8ee8-147">また、`IHostingEnvironment` や `ILoggerFactory` などの追加サービスをメソッド シグネチャで指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-147">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="f8ee8-148">指定すると、(使用可能なサービスの場合) 追加サービスが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-148">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="f8ee8-149">[ の使用方法の詳細については、`IApplicationBuilder`ミドルウェア](xref:fundamentals/middleware/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-149">For more information on how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="f8ee8-150">簡易メソッド</span><span class="sxs-lookup"><span data-stu-id="f8ee8-150">Convenience methods</span></span>

<span data-ttu-id="f8ee8-151">`Startup` クラスを指定する代わりに、[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) および [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 簡易メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-151">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="f8ee8-152">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-152">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="f8ee8-153">`Configure` の複数回の呼び出しでは、最後のメソッド呼び出しが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-153">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a><span data-ttu-id="f8ee8-154">Startup フィルター</span><span class="sxs-lookup"><span data-stu-id="f8ee8-154">Startup filters</span></span>

<span data-ttu-id="f8ee8-155">[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) を使用して、アプリケーションのミドルウェア [Configure](#the-configure-method) パイプラインの先頭または末尾でミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-155">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="f8ee8-156">`IStartupFilter` は、アプリケーションの要求処理パイプラインの先頭または末尾で、ライブラリによってミドルウェアが追加される前または後にミドルウェアを確実に実行する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-156">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="f8ee8-157">`IStartupFilter` は、`Action<IApplicationBuilder>` を受け取って返す 1 つのメソッド [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure) を実装しています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-157">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="f8ee8-158">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) で、アプリケーションの要求パイプラインを構成するクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-158">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="f8ee8-159">詳細については、「[Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)」(IApplicationBuilder を使用したミドルウェア パイプラインの作成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-159">For more information, see [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="f8ee8-160">各 `IStartupFilter` は、要求パイプラインで 1 つまたは複数のミドルウェアを実装します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-160">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="f8ee8-161">フィルターは、サービス コンテナーに追加された順に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-161">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="f8ee8-162">フィルターは、コントロールを次のフィルターに渡す前または後にミドルウェアを追加できるため、アプリケーション パイプラインの先頭または末尾に追加されます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-162">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="f8ee8-163">この[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) は、ミドルウェアを `IStartupFilter` に登録する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-163">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="f8ee8-164">このサンプル アプリケーションには、クエリ文字列パラメーターからオプション値を設定するミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-164">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="f8ee8-165">`RequestSetOptionsMiddleware` は `RequestSetOptionsStartupFilter` クラスで構成されています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-165">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="f8ee8-166">`IStartupFilter` は `ConfigureServices` のサービス コンテナーに登録されています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-166">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="f8ee8-167">`option` のクエリ文字列パラメーターを指定すると、ミドルウェアは MVC ミドルウェアが応答をレンダリングする前に値の割り当てを処理します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-167">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![レンダリングされたインデックス ページを表示するブラウザー ウィンドウ。](startup/_static/index.png)

<span data-ttu-id="f8ee8-170">ミドルウェアの実行順序は、`IStartupFilter` の登録順に設定されています。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-170">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="f8ee8-171">複数の `IStartupFilter` の実装が、同じオブジェクトとやり取りする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-171">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="f8ee8-172">順序が重要な場合は、ミドルウェアの実行順序に合わせて `IStartupFilter` サービスの登録順序を指定してください。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-172">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="f8ee8-173">`IStartupFilter` に登録された他のアプリケーション ミドルウェアの前または後に実行される `IStartupFilter` の実装が 1 つまたは複数あるミドルウェアを、ライブラリで追加することができます。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-173">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="f8ee8-174">ライブラリの `IStartupFilter` によって追加されたミドルウェアの前に `IStartupFilter` ミドルウェアを呼び出すには、ライブラリがサービス コンテナーに追加される前にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-174">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="f8ee8-175">後で呼び出すには、ライブラリが追加される後にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="f8ee8-175">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8ee8-176">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="f8ee8-176">Additional resources</span></span>

* [<span data-ttu-id="f8ee8-177">ホスティング</span><span class="sxs-lookup"><span data-stu-id="f8ee8-177">Hosting</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="f8ee8-178">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="f8ee8-178">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="f8ee8-179">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="f8ee8-179">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="f8ee8-180">ログ</span><span class="sxs-lookup"><span data-stu-id="f8ee8-180">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="f8ee8-181">構成</span><span class="sxs-lookup"><span data-stu-id="f8ee8-181">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="f8ee8-182">StartupLoader クラス: FindStartupType メソッド (参照元)</span><span class="sxs-lookup"><span data-stu-id="f8ee8-182">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
