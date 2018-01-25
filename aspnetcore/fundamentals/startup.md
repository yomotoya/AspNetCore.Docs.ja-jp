---
title: "ASP.NET Core でのアプリケーションの起動"
author: ardalis
description: "ASP.NET Core でスタートアップ クラスがサービスとアプリケーションの要求パイプラインを構成する方法を検出します。"
ms.author: tdykstra
manager: wpickett
ms.custom: mvc
ms.date: 12/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/startup
ms.openlocfilehash: 81d76c39b7890e2d4ab86252cb0a343e3bb7359a
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2018
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="ec517-103">ASP.NET Core でのアプリケーションの起動</span><span class="sxs-lookup"><span data-stu-id="ec517-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="ec517-104">によって[Steve Smith](https://ardalis.com)、 [Tom Dykstra](https://github.com/tdykstra)、および[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ec517-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ec517-105">`Startup`クラスは、サービスとアプリケーションの要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="ec517-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="ec517-106">スタートアップ クラス</span><span class="sxs-lookup"><span data-stu-id="ec517-106">The Startup class</span></span>

<span data-ttu-id="ec517-107">ASP.NET Core アプリで使用する、`Startup`というクラス`Startup`慣例です。</span><span class="sxs-lookup"><span data-stu-id="ec517-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="ec517-108">`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="ec517-108">The `Startup` class:</span></span>

* <span data-ttu-id="ec517-109">必要に応じて含めることができます、 [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)アプリのサービスを構成する方法です。</span><span class="sxs-lookup"><span data-stu-id="ec517-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="ec517-110">含める必要があります、[構成](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)メソッドをアプリの要求処理パイプラインを作成します。</span><span class="sxs-lookup"><span data-stu-id="ec517-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="ec517-111">`ConfigureServices`および`Configure`アプリの起動時にランタイムによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ec517-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="ec517-112">指定して、`Startup`クラス、 [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt; ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)メソッド。</span><span class="sxs-lookup"><span data-stu-id="ec517-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="ec517-113">`Startup`クラス コンス トラクターは、ホストによって定義された依存関係を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ec517-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="ec517-114">一般的な用途[依存性の注入](xref:fundamentals/dependency-injection)に、`Startup`クラスが挿入するには。</span><span class="sxs-lookup"><span data-stu-id="ec517-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="ec517-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment)環境によってサービスを構成します。</span><span class="sxs-lookup"><span data-stu-id="ec517-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment.</span></span>
* <span data-ttu-id="ec517-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration)の起動中にアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="ec517-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) to configure the app during startup.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="ec517-117">挿入する代わりに`IHostingEnvironment`規則ベースのアプローチを使用することです。</span><span class="sxs-lookup"><span data-stu-id="ec517-117">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="ec517-118">アプリを個別に定義できます`Startup`さまざまな環境のためのクラス (たとえば、 `StartupDevelopment`)、実行時に適切なスタートアップ クラスが選択されているとします。</span><span class="sxs-lookup"><span data-stu-id="ec517-118">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate startup class is selected at runtime.</span></span> <span data-ttu-id="ec517-119">ある名前サフィックスが、現在の環境と一致するクラスが優先順位します。</span><span class="sxs-lookup"><span data-stu-id="ec517-119">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="ec517-120">アプリ開発環境で実行され、両方が含まれる場合、`Startup`クラスおよび`StartupDevelopment`クラス、`StartupDevelopment`クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ec517-120">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="ec517-121">詳細については、[「Working with multiple environments」](xref:fundamentals/environments#startup-conventions) (複数の環境での使用) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="ec517-121">For more information, see [Working with multiple environments](xref:fundamentals/environments#startup-conventions).</span></span>

<span data-ttu-id="ec517-122">について詳しく学習する`WebHostBuilder`を参照してください、[ホスティング](xref:fundamentals/hosting)トピックです。</span><span class="sxs-lookup"><span data-stu-id="ec517-122">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/hosting) topic.</span></span> <span data-ttu-id="ec517-123">起動中にエラーを処理する方法については、次を参照してください。[スタートアップ例外処理](xref:fundamentals/error-handling#startup-exception-handling)です。</span><span class="sxs-lookup"><span data-stu-id="ec517-123">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="ec517-124">ConfigureServices メソッド</span><span class="sxs-lookup"><span data-stu-id="ec517-124">The ConfigureServices method</span></span>

<span data-ttu-id="ec517-125">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices)メソッドは。</span><span class="sxs-lookup"><span data-stu-id="ec517-125">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="ec517-126">任意。</span><span class="sxs-lookup"><span data-stu-id="ec517-126">Optional.</span></span>
* <span data-ttu-id="ec517-127">前に、web ホストによって呼び出される、`Configure`アプリのサービスを構成する方法です。</span><span class="sxs-lookup"><span data-stu-id="ec517-127">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="ec517-128">ここで[構成オプション](xref:fundamentals/configuration/index)規約によって設定されます。</span><span class="sxs-lookup"><span data-stu-id="ec517-128">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="ec517-129">サービスをサービス コンテナーに追加することが可能になります内およびアプリ内で、`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ec517-129">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="ec517-130">サービスは、によって解決[依存性の注入](xref:fundamentals/dependency-injection)またはから[IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices)です。</span><span class="sxs-lookup"><span data-stu-id="ec517-130">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="ec517-131">Web ホストが前にいくつかのサービスを構成することがあります`Startup`メソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ec517-131">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="ec517-132">詳細については、[ホスティング](xref:fundamentals/hosting)トピックです。</span><span class="sxs-lookup"><span data-stu-id="ec517-132">Details are available in the [Hosting](xref:fundamentals/hosting) topic.</span></span> 

<span data-ttu-id="ec517-133">大幅なセットアップを必要とする機能がある`Add[Service]`拡張メソッド[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection)です。</span><span class="sxs-lookup"><span data-stu-id="ec517-133">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="ec517-134">一般的な web アプリは、Entity Framework、Id、および MVC 用のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="ec517-134">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

## <a name="services-available-in-startup"></a><span data-ttu-id="ec517-135">スタートアップで使用できるサービス</span><span class="sxs-lookup"><span data-stu-id="ec517-135">Services available in Startup</span></span>

<span data-ttu-id="ec517-136">Web ホストに使用できるいくつかのサービスを提供する、`Startup`クラスのコンス トラクターです。</span><span class="sxs-lookup"><span data-stu-id="ec517-136">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="ec517-137">アプリを使用して追加のサービスを追加する`ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="ec517-137">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="ec517-138">ホストとアプリの両方のサービスを利用しで`Configure`およびアプリケーション全体でします。</span><span class="sxs-lookup"><span data-stu-id="ec517-138">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="ec517-139">Configure メソッド</span><span class="sxs-lookup"><span data-stu-id="ec517-139">The Configure method</span></span>

<span data-ttu-id="ec517-140">[構成](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure)メソッドは、アプリが HTTP 要求に応答する方法を指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="ec517-140">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="ec517-141">追加することで、要求パイプラインが構成されている[ミドルウェア](xref:fundamentals/middleware)コンポーネントを[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ec517-141">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="ec517-142">`IApplicationBuilder`使用できる、`Configure`メソッドは、サービス コンテナーに登録されていません。</span><span class="sxs-lookup"><span data-stu-id="ec517-142">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="ec517-143">ホストを作成、`IApplicationBuilder`に直接渡されますと`Configure`([参照ソース](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192))。</span><span class="sxs-lookup"><span data-stu-id="ec517-143">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="ec517-144">[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)開発者例外ページでは、サポート、パイプラインを構成する[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的ファイル、および ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="ec517-144">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="ec517-145">各`Use`拡張メソッドは、要求パイプラインにミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="ec517-145">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="ec517-146">インスタンス、`UseMvc`拡張メソッドを追加、[ルーティング ミドルウェア](xref:fundamentals/routing)要求パイプラインを構成および[MVC](xref:mvc/overview)既定のハンドラーとして。</span><span class="sxs-lookup"><span data-stu-id="ec517-146">For instance, the `UseMvc` extension method adds the [routing middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="ec517-147">追加サービスなど、`IHostingEnvironment`と`ILoggerFactory`、メソッド シグネチャ内でも指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="ec517-147">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="ec517-148">指定した場合、使用可能な場合にその他のサービスが挿入されたします。</span><span class="sxs-lookup"><span data-stu-id="ec517-148">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="ec517-149">使用する方法の詳細についての`IApplicationBuilder`を参照してください[ミドルウェア](xref:fundamentals/middleware)です。</span><span class="sxs-lookup"><span data-stu-id="ec517-149">For more information on how to use `IApplicationBuilder`, see [Middleware](xref:fundamentals/middleware).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="ec517-150">便利なメソッド</span><span class="sxs-lookup"><span data-stu-id="ec517-150">Convenience methods</span></span>

<span data-ttu-id="ec517-151">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices)と[構成](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure)便利なメソッドを指定する代わりに使用できます、`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ec517-151">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="ec517-152">複数回呼び出す`ConfigureServices`互いに追加します。</span><span class="sxs-lookup"><span data-stu-id="ec517-152">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="ec517-153">複数回呼び出す`Configure`最後のメソッド呼び出しを使用します。</span><span class="sxs-lookup"><span data-stu-id="ec517-153">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[Main](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a><span data-ttu-id="ec517-154">スタートアップ フィルター</span><span class="sxs-lookup"><span data-stu-id="ec517-154">Startup filters</span></span>

<span data-ttu-id="ec517-155">使用して[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter)ミドルウェアを構成するには、先頭または末尾のアプリの[構成](#the-configure-method)ミドルウェア パイプライン。</span><span class="sxs-lookup"><span data-stu-id="ec517-155">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="ec517-156">`IStartupFilter`ミドルウェアが前に、または後に開始またはアプリの要求処理パイプラインの最後のライブラリによって追加されたミドルウェアが実行されるようにするのに便利です。</span><span class="sxs-lookup"><span data-stu-id="ec517-156">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="ec517-157">`IStartupFilter`1 つのメソッドを実装する[構成](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure)、受信を返す、`Action<IApplicationBuilder>`です。</span><span class="sxs-lookup"><span data-stu-id="ec517-157">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="ec517-158">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder)アプリの要求パイプラインを構成するクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="ec517-158">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="ec517-159">詳細については、次を参照してください。 [IApplicationBuilder のミドルウェア パイプラインを作成する](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder)です。</span><span class="sxs-lookup"><span data-stu-id="ec517-159">For more information, see [Creating a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="ec517-160">各`IStartupFilter`要求パイプライン内の 1 つまたは複数の middlewares を実装します。</span><span class="sxs-lookup"><span data-stu-id="ec517-160">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="ec517-161">フィルターは、サービス コンテナーに追加された順序で呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="ec517-161">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="ec517-162">フィルターを追加する前にミドルウェアまたは後、次のフィルターに制御を渡すこと、したがって、追加の先頭または末尾にあるアプリケーション パイプラインにします。</span><span class="sxs-lookup"><span data-stu-id="ec517-162">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="ec517-163">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) とミドルウェアを登録する方法を示します`IStartupFilter`です。</span><span class="sxs-lookup"><span data-stu-id="ec517-163">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="ec517-164">サンプル アプリには、クエリ文字列パラメーターから、オプションの値を設定するミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ec517-164">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="ec517-165">`RequestSetOptionsMiddleware`で構成された、`RequestSetOptionsStartupFilter`クラス。</span><span class="sxs-lookup"><span data-stu-id="ec517-165">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[Main](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="ec517-166">`IStartupFilter`でサービス コンテナーに登録されて`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec517-166">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[Main](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="ec517-167">ときのクエリ文字列パラメーター`option`は提供された場合、ミドルウェアが MVC ミドルウェアが応答を表示する前に、値の割り当てを処理します。</span><span class="sxs-lookup"><span data-stu-id="ec517-167">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![ブラウザー ウィンドウが表示するインデックス ページを表示します。](startup/_static/index.png)

<span data-ttu-id="ec517-170">ミドルウェアの実行順序がの順序で設定されている`IStartupFilter`登録。</span><span class="sxs-lookup"><span data-stu-id="ec517-170">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="ec517-171">複数`IStartupFilter`実装が同じのオブジェクトと対話することがあります。</span><span class="sxs-lookup"><span data-stu-id="ec517-171">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="ec517-172">順序付けが重要な場合は、注文、 `IStartupFilter` middlewares を実行する順序と一致する登録のサービスを提供します。</span><span class="sxs-lookup"><span data-stu-id="ec517-172">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="ec517-173">ライブラリは追加する 1 つまたは複数のミドルウェア`IStartupFilter`前に、または後に登録されている他のアプリのミドルウェアを実行している実装`IStartupFilter`です。</span><span class="sxs-lookup"><span data-stu-id="ec517-173">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="ec517-174">呼び出す、`IStartupFilter`ライブラリのによって追加されたミドルウェアの前にミドルウェア`IStartupFilter`ライブラリは、サービス コンテナーに追加される前に、サービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="ec517-174">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="ec517-175">後でこれを呼び出すには、ライブラリを追加した後、サービスの登録を置きます。</span><span class="sxs-lookup"><span data-stu-id="ec517-175">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec517-176">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ec517-176">Additional resources</span></span>

* [<span data-ttu-id="ec517-177">ホスティング</span><span class="sxs-lookup"><span data-stu-id="ec517-177">Hosting</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="ec517-178">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="ec517-178">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="ec517-179">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="ec517-179">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="ec517-180">ログ</span><span class="sxs-lookup"><span data-stu-id="ec517-180">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="ec517-181">構成</span><span class="sxs-lookup"><span data-stu-id="ec517-181">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="ec517-182">StartupLoader クラス: FindStartupType メソッド (参照元)</span><span class="sxs-lookup"><span data-stu-id="ec517-182">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
