---
title: ASP.NET Core でのアプリケーションのスタートアップ
author: ardalis
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
ms.author: tdykstra
ms.custom: mvc
ms.date: 4/13/2018
uid: fundamentals/startup
ms.openlocfilehash: 285d74c0d12e3aca4d8c33d39467dfda02712993
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063261"
---
# <a name="application-startup-in-aspnet-core"></a><span data-ttu-id="950c9-103">ASP.NET Core でのアプリケーションのスタートアップ</span><span class="sxs-lookup"><span data-stu-id="950c9-103">Application startup in ASP.NET Core</span></span>

<span data-ttu-id="950c9-104">作成者: [Steve Smith](https://ardalis.com)、[Tom Dykstra](https://github.com/tdykstra)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="950c9-104">By [Steve Smith](https://ardalis.com), [Tom Dykstra](https://github.com/tdykstra), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="950c9-105">`Startup` クラスはサービスとアプリケーションの要求パイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="950c9-105">The `Startup` class configures services and the app's request pipeline.</span></span>

## <a name="the-startup-class"></a><span data-ttu-id="950c9-106">Startup クラス</span><span class="sxs-lookup"><span data-stu-id="950c9-106">The Startup class</span></span>

<span data-ttu-id="950c9-107">ASP.NET Core アプリケーションでは `Startup` クラスが使用されています。このクラスは規約に従って `Startup` と名前が付けられています。</span><span class="sxs-lookup"><span data-stu-id="950c9-107">ASP.NET Core apps use a `Startup` class, which is named `Startup` by convention.</span></span> <span data-ttu-id="950c9-108">`Startup` クラスには次の特徴があります。</span><span class="sxs-lookup"><span data-stu-id="950c9-108">The `Startup` class:</span></span>

* <span data-ttu-id="950c9-109">必要に応じて [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドを含め、アプリケーションのサービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="950c9-109">Can optionally include a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method to configure the app's services.</span></span>
* <span data-ttu-id="950c9-110">アプリケーションの要求処理パイプラインを作成するために [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="950c9-110">Must include a [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method to create the app's request processing pipeline.</span></span>

<span data-ttu-id="950c9-111">`ConfigureServices` と `Configure` はアプリケーションの起動時にランタイムによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-111">`ConfigureServices` and `Configure` are called by the runtime when the app starts:</span></span>

[!code-csharp[](startup/snapshot_sample/Startup1.cs)]

<span data-ttu-id="950c9-112">[WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) メソッドで `Startup` クラスを指定します。</span><span class="sxs-lookup"><span data-stu-id="950c9-112">Specify the `Startup` class with the [WebHostBuilderExtensions](/dotnet/api/Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions) [UseStartup&lt;TStartup&gt;](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) method:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=10)]

<span data-ttu-id="950c9-113">`Startup` クラス コンストラクターは、ホストに定義されている依存関係を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="950c9-113">The `Startup` class constructor accepts dependencies defined by the host.</span></span> <span data-ttu-id="950c9-114">`Startup` クラスへの[依存関係挿入](xref:fundamentals/dependency-injection)の一般的な用途は、以下を挿入する場合です。</span><span class="sxs-lookup"><span data-stu-id="950c9-114">A common use of [dependency injection](xref:fundamentals/dependency-injection) into the `Startup` class is to inject:</span></span>

* <span data-ttu-id="950c9-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) (環境別にサービスを構成するため)。</span><span class="sxs-lookup"><span data-stu-id="950c9-115">[IHostingEnvironment](/dotnet/api/Microsoft.AspNetCore.Hosting.IHostingEnvironment) to configure services by environment.</span></span>
* <span data-ttu-id="950c9-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) (スタートアップ時にアプリケーションを構成するため)。</span><span class="sxs-lookup"><span data-stu-id="950c9-116">[IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) to configure the app during startup.</span></span>

[!code-csharp[](startup/snapshot_sample/Startup2.cs)]

<span data-ttu-id="950c9-117">`IHostingEnvironment` を挿入する代わりに、規約ベースのアプローチがあります。</span><span class="sxs-lookup"><span data-stu-id="950c9-117">An alternative to injecting `IHostingEnvironment` is to use a conventions-based approach.</span></span> <span data-ttu-id="950c9-118">アプリケーションでは、環境 (たとえば `StartupDevelopment`) ごとに個別の `Startup` クラスを定義することができます。実行時に適切な `Startup` クラスが選択されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-118">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="950c9-119">名前のサフィックスが現在の環境と一致するクラスが優先されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-119">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="950c9-120">アプリケーションが Development 環境で実行され、`Startup` クラスと `StartupDevelopment` クラスの両方が含まれている場合は、`StartupDevelopment` クラスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-120">If the app is run in the Development environment and includes both a `Startup` class and a `StartupDevelopment` class, the `StartupDevelopment` class is used.</span></span> <span data-ttu-id="950c9-121">詳細については、「[Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods)」(複数の環境の使用) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-121">For more information, see [Use multiple environments](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span></span>

<span data-ttu-id="950c9-122">`WebHostBuilder` の詳細については、[ホスティング](xref:fundamentals/host/index)に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-122">To learn more about `WebHostBuilder`, see the [Hosting](xref:fundamentals/host/index) topic.</span></span> <span data-ttu-id="950c9-123">スタートアップ時のエラー処理については、「[Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling)」(スタートアップ例外処理) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-123">For information on handling errors during startup, see [Startup exception handling](xref:fundamentals/error-handling#startup-exception-handling).</span></span>

## <a name="the-configureservices-method"></a><span data-ttu-id="950c9-124">ConfigureServices メソッド</span><span class="sxs-lookup"><span data-stu-id="950c9-124">The ConfigureServices method</span></span>

<span data-ttu-id="950c9-125">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) メソッドの特徴:</span><span class="sxs-lookup"><span data-stu-id="950c9-125">The [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) method is:</span></span>

* <span data-ttu-id="950c9-126">Optional</span><span class="sxs-lookup"><span data-stu-id="950c9-126">Optional</span></span>
* <span data-ttu-id="950c9-127">アプリケーションのサービスを構成する `Configure` メソッドの前に、Web ホストによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-127">Called by the web host before the `Configure` method to configure the app's services.</span></span>
* <span data-ttu-id="950c9-128">規約によって[構成オプション](xref:fundamentals/configuration/index)が設定されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-128">Where [configuration options](xref:fundamentals/configuration/index) are set by convention.</span></span>

<span data-ttu-id="950c9-129">サービス コンテナーにサービスを追加すると、アプリケーション内と `Configure` メソッド内でサービスを利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="950c9-129">Adding services to the service container makes them available within the app and in the `Configure` method.</span></span> <span data-ttu-id="950c9-130">サービスは、[依存関係挿入](xref:fundamentals/dependency-injection)を介して、または [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) から解決されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-130">The services are resolved via [dependency injection](xref:fundamentals/dependency-injection) or from [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices).</span></span>

<span data-ttu-id="950c9-131">Web ホストでは、`Startup` メソッドが呼び出される前にいくつかのサービスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="950c9-131">The web host may configure some services before `Startup` methods are called.</span></span> <span data-ttu-id="950c9-132">詳細については、[ASP.NET Core](xref:fundamentals/host/index) に関するトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-132">Details are available in the [Host in ASP.NET Core](xref:fundamentals/host/index) topic.</span></span>

<span data-ttu-id="950c9-133">多くの設定が必要な機能には、[IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection) 上の `Add[Service]` 拡張メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="950c9-133">For features that require substantial setup, there are `Add[Service]` extension methods on [IServiceCollection](/dotnet/api/Microsoft.Extensions.DependencyInjection.IServiceCollection).</span></span> <span data-ttu-id="950c9-134">一般的な Web アプリケーションは、Entity Framework、Identity、および MVC のサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="950c9-134">A typical web app registers services for Entity Framework, Identity, and MVC:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=4,7,11&start=40&end=55)]

::: moniker range=">= aspnetcore-2.1"

<a name="setcompatibilityversion"></a>

### <a name="setcompatibilityversion-for-aspnet-core-mvc"></a><span data-ttu-id="950c9-135">ASP.NET Core MVC の SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="950c9-135">SetCompatibilityVersion for ASP.NET Core MVC</span></span> 

<span data-ttu-id="950c9-136">`SetCompatibilityVersion` メソッドを使用すると、ASP.NET MVC Core 2.1 以降に導入されている、互換性に影響する重大な変更をオプトインまたはオプトアウトすることができます。</span><span class="sxs-lookup"><span data-stu-id="950c9-136">The `SetCompatibilityVersion` method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET MVC Core 2.1+.</span></span> <span data-ttu-id="950c9-137">互換性に影響する可能性のあるこれらの重大な変更は、ほとんどの場合、MVC サブシステムの動作方法と、ランタイムで**ユーザーのコード**が呼び出される方法についてです。</span><span class="sxs-lookup"><span data-stu-id="950c9-137">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="950c9-138">オプトインした場合、最新の動作と ASP.NET Core の最新の動作を得ることができます。</span><span class="sxs-lookup"><span data-stu-id="950c9-138">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="950c9-139">次のコードにより、互換性モードは ASP.NET Core 2.1 に設定されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-139">The following code sets the compatibility mode to ASP.NET Core 2.1:</span></span>

[!code-csharp[Main](startup/sampleCompatibility/Startup.cs?name=snippet1)]

<span data-ttu-id="950c9-140">最新のバージョンを使用して (`CompatibilityVersion.Version_2_1`) アプリケーションをテストすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="950c9-140">We recommend you test your application using the latest version (`CompatibilityVersion.Version_2_1`).</span></span> <span data-ttu-id="950c9-141">ほとんどのアプリケーションには、最新のバージョンを使用しても、互換性に影響する重大な変更はないと見込まれます。</span><span class="sxs-lookup"><span data-stu-id="950c9-141">We anticipate that most applications will not have breaking behavior changes using the latest version.</span></span> 

<span data-ttu-id="950c9-142">`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すアプリケーションは、ASP.NET Core 2.1 MVC およびそれ以降の 2. バージョンで実装されている、動作が中断する可能性がある変更から保護されています。</span><span class="sxs-lookup"><span data-stu-id="950c9-142">Applications that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="950c9-143">この保護とは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="950c9-143">This protection:</span></span>

* <span data-ttu-id="950c9-144">2.1 以降のすべての変更には該当しません。これは、MVC サブシステムの ASP.NET Core ランタイムの互換性に影響する可能性のある重大な変更のみを対象としています。</span><span class="sxs-lookup"><span data-stu-id="950c9-144">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="950c9-145">次のメジャー バージョンには適用されません。</span><span class="sxs-lookup"><span data-stu-id="950c9-145">Does not extend to the next major version.</span></span>

<span data-ttu-id="950c9-146">`SetCompatibilityVersion` を呼び出さ**ない** ASP.NET Core 2.1 およびそれ以降の 2.x アプリケーションの既定の互換性は、2.0 の互換性です。</span><span class="sxs-lookup"><span data-stu-id="950c9-146">The default compatibility for ASP.NET Core 2.1 and later 2.x applications that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="950c9-147">つまり、`SetCompatibilityVersion` を呼び出さないことは、`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すことと同じです。</span><span class="sxs-lookup"><span data-stu-id="950c9-147">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="950c9-148">次のコードは、以下の動作を除き、互換性モードを ASP.NET Core 2.1 に設定します。</span><span class="sxs-lookup"><span data-stu-id="950c9-148">The following code sets the compatibility mode to ASP.NET Core 2.1, except for the following behaviors:</span></span>

* [<span data-ttu-id="950c9-149">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="950c9-149">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="950c9-150">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="950c9-150">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](startup/sampleCompatibility/Startup2.cs?name=snippet1)]

<span data-ttu-id="950c9-151">互換性に影響する重大な変更があったアプリでは、適切な互換性スイッチを使用することにより、次が得られます。</span><span class="sxs-lookup"><span data-stu-id="950c9-151">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="950c9-152">最新のリリースを使用し、互換性に影響する特定の重大な変更をオプトアウトできます。</span><span class="sxs-lookup"><span data-stu-id="950c9-152">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="950c9-153">アプリが最新の変更に対応するよう、更新を行う時間を得られます。</span><span class="sxs-lookup"><span data-stu-id="950c9-153">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="950c9-154">[MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) クラス ソースのコメントには、変更があった個所とそれらの改善が多くのユーザーに与えるメリットについて説明しています。</span><span class="sxs-lookup"><span data-stu-id="950c9-154">The [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="950c9-155">将来的に、[ASP.NET Core 3.0 バージョン](https://github.com/aspnet/Home/wiki/Roadmap)がリリースされます。</span><span class="sxs-lookup"><span data-stu-id="950c9-155">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="950c9-156">3.0 バージョンでは、互換性スイッチによってサポートされている古い動作は削除されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-156">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="950c9-157">これらの正の変更は、ほぼすべてのユーザーにとってメリットとなると感じています。</span><span class="sxs-lookup"><span data-stu-id="950c9-157">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="950c9-158">これらの変更を現在導入することにより、ほとんどのアプリでは今メリット享受でき、他ではアプリケーションをアップデートする時間を得られます。</span><span class="sxs-lookup"><span data-stu-id="950c9-158">By introducing these changes now, most apps can benefit now, and the others will have time to update their applications.</span></span>

::: moniker-end

## <a name="services-available-in-startup"></a><span data-ttu-id="950c9-159">Startup で使用できるサービス</span><span class="sxs-lookup"><span data-stu-id="950c9-159">Services available in Startup</span></span>

<span data-ttu-id="950c9-160">Web ホストには、`Startup` クラス コンストラクターに使用できるサービスがいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="950c9-160">The web host provides some services that are available to the `Startup` class constructor.</span></span> <span data-ttu-id="950c9-161">アプリケーションから `ConfigureServices` 経由でサービスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-161">The app adds additional services via `ConfigureServices`.</span></span> <span data-ttu-id="950c9-162">これで、ホスト サービスとアプリケーション サービスの両方が `Configure` 内とアプリケーション全体で使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="950c9-162">Both the host and app services are then available in `Configure` and throughout the application.</span></span>

## <a name="the-configure-method"></a><span data-ttu-id="950c9-163">Configure メソッド</span><span class="sxs-lookup"><span data-stu-id="950c9-163">The Configure method</span></span>

<span data-ttu-id="950c9-164">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) メソッドは、アプリケーションが HTTP 要求にどのように応答するかを指定するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-164">The [Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) method is used to specify how the app responds to HTTP requests.</span></span> <span data-ttu-id="950c9-165">要求パイプラインは、[ミドルウェア](xref:fundamentals/middleware/index) コンポーネントを [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) インスタンスに追加することで構成されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-165">The request pipeline is configured by adding [middleware](xref:fundamentals/middleware/index) components to an [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) instance.</span></span> <span data-ttu-id="950c9-166">`IApplicationBuilder` は `Configure` メソッドから使用できますが、サービス コンテナーに登録されていません。</span><span class="sxs-lookup"><span data-stu-id="950c9-166">`IApplicationBuilder` is available to the `Configure` method, but it isn't registered in the service container.</span></span> <span data-ttu-id="950c9-167">ホスティングによって `IApplicationBuilder` が作成され、`Configure` ([参照元](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)) に直接渡されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-167">Hosting creates an `IApplicationBuilder` and passes it directly to `Configure` ([reference source](https://github.com/aspnet/Hosting/blob/release/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/WebHost.cs#L179-L192)).</span></span>

<span data-ttu-id="950c9-168">[ASP.NET Core テンプレート](/dotnet/core/tools/dotnet-new)では、開発者の例外ページ、[BrowserLink](http://vswebessentials.com/features/browserlink)、エラー ページ、静的ファイル、および ASP.NET MVC をサポートするパイプラインを構成します。</span><span class="sxs-lookup"><span data-stu-id="950c9-168">The [ASP.NET Core templates](/dotnet/core/tools/dotnet-new) configure the pipeline with support for a developer exception page, [BrowserLink](http://vswebessentials.com/features/browserlink), error pages, static files, and ASP.NET MVC:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Startup.cs?range=28-48&highlight=5,6,10,13,15)]

<span data-ttu-id="950c9-169">各 `Use` 拡張メソッドで、要求パイプラインにミドルウェア コンポーネントを追加します。</span><span class="sxs-lookup"><span data-stu-id="950c9-169">Each `Use` extension method adds a middleware component to the request pipeline.</span></span> <span data-ttu-id="950c9-170">たとえば、`UseMvc` 拡張メソッドでは、[ルーティング ミドルウェア](xref:fundamentals/routing)を要求パイプラインに追加し、既定のハンドラーとして [MVC](xref:mvc/overview) を構成します。</span><span class="sxs-lookup"><span data-stu-id="950c9-170">For instance, the `UseMvc` extension method adds the [Routing Middleware](xref:fundamentals/routing) to the request pipeline and configures [MVC](xref:mvc/overview) as the default handler.</span></span>

<span data-ttu-id="950c9-171">要求パイプライン内の各ミドルウェア コンポーネントは、パイプラインの次のコンポーネントを呼び出すか、妥当な場合はチェーンをショートさせます。</span><span class="sxs-lookup"><span data-stu-id="950c9-171">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the chain, if appropriate.</span></span> <span data-ttu-id="950c9-172">ショート サーキットがミドルウェアのチェーンで発生しない場合、各ミドルウェアには、クライアントに送信される前に要求を処理する 2 度目の機会があります。</span><span class="sxs-lookup"><span data-stu-id="950c9-172">If short-circuiting doesn't occur along the middleware chain, each middleware has a second chance to process the request before it's sent to the client.</span></span>

<span data-ttu-id="950c9-173">また、`IHostingEnvironment` や `ILoggerFactory` などの追加サービスをメソッド シグネチャで指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="950c9-173">Additional services, such as `IHostingEnvironment` and `ILoggerFactory`, may also be specified in the method signature.</span></span> <span data-ttu-id="950c9-174">指定すると、(使用可能なサービスの場合) 追加サービスが挿入されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-174">When specified, additional services are injected if they're available.</span></span>

<span data-ttu-id="950c9-175">`IApplicationBuilder` の使用方法およびミドルウェアの処理の順序については、「[ミドルウェア](xref:fundamentals/middleware/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-175">For more information on how to use `IApplicationBuilder` and the order of middleware processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="convenience-methods"></a><span data-ttu-id="950c9-176">簡易メソッド</span><span class="sxs-lookup"><span data-stu-id="950c9-176">Convenience methods</span></span>

<span data-ttu-id="950c9-177">`Startup` クラスを指定する代わりに、[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) および [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) 簡易メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="950c9-177">[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder.configureservices) and [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) convenience methods can be used instead of specifying a `Startup` class.</span></span> <span data-ttu-id="950c9-178">`ConfigureServices` の複数回の呼び出しでは、互いに追加されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-178">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="950c9-179">`Configure` の複数回の呼び出しでは、最後のメソッド呼び出しが使用されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-179">Multiple calls to `Configure` use the last method call.</span></span>

[!code-csharp[](startup/snapshot_sample/Program.cs?highlight=18,22)]

## <a name="startup-filters"></a><span data-ttu-id="950c9-180">Startup フィルター</span><span class="sxs-lookup"><span data-stu-id="950c9-180">Startup filters</span></span>

<span data-ttu-id="950c9-181">[IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) を使用して、アプリケーションのミドルウェア [Configure](#the-configure-method) パイプラインの先頭または末尾でミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="950c9-181">Use [IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) to configure middleware at the beginning or end of an app's [Configure](#the-configure-method) middleware pipeline.</span></span> <span data-ttu-id="950c9-182">`IStartupFilter` は、アプリケーションの要求処理パイプラインの先頭または末尾で、ライブラリによってミドルウェアが追加される前または後にミドルウェアを確実に実行する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="950c9-182">`IStartupFilter` is useful to ensure that a middleware runs before or after middleware added by libraries at the start or end of the app's request processing pipeline.</span></span>

<span data-ttu-id="950c9-183">`IStartupFilter` は、`Action<IApplicationBuilder>` を受け取って返す 1 つのメソッド [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure) を実装しています。</span><span class="sxs-lookup"><span data-stu-id="950c9-183">`IStartupFilter` implements a single method, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter.configure), which receives and returns an `Action<IApplicationBuilder>`.</span></span> <span data-ttu-id="950c9-184">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) で、アプリケーションの要求パイプラインを構成するクラスを定義します。</span><span class="sxs-lookup"><span data-stu-id="950c9-184">An [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) defines a class to configure an app's request pipeline.</span></span> <span data-ttu-id="950c9-185">詳細については、「[Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)」(IApplicationBuilder を使用したミドルウェア パイプラインの作成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-185">For more information, see [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder).</span></span>

<span data-ttu-id="950c9-186">各 `IStartupFilter` は、要求パイプラインで 1 つまたは複数のミドルウェアを実装します。</span><span class="sxs-lookup"><span data-stu-id="950c9-186">Each `IStartupFilter` implements one or more middlewares in the request pipeline.</span></span> <span data-ttu-id="950c9-187">フィルターは、サービス コンテナーに追加された順に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-187">The filters are invoked in the order they were added to the service container.</span></span> <span data-ttu-id="950c9-188">フィルターは、コントロールを次のフィルターに渡す前または後にミドルウェアを追加できるため、アプリケーション パイプラインの先頭または末尾に追加されます。</span><span class="sxs-lookup"><span data-stu-id="950c9-188">Filters may add middleware before or after passing control to the next filter, thus they append to the beginning or end of the app pipeline.</span></span>

<span data-ttu-id="950c9-189">この[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample)) は、ミドルウェアを `IStartupFilter` に登録する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="950c9-189">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/startup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) demonstrates how to register a middleware with `IStartupFilter`.</span></span> <span data-ttu-id="950c9-190">このサンプル アプリケーションには、クエリ文字列パラメーターからオプション値を設定するミドルウェアが含まれています。</span><span class="sxs-lookup"><span data-stu-id="950c9-190">The sample app includes a middleware that sets an options value from a query string parameter:</span></span>

[!code-csharp[](startup/sample/RequestSetOptionsMiddleware.cs?name=snippet1)]

<span data-ttu-id="950c9-191">`RequestSetOptionsMiddleware` は `RequestSetOptionsStartupFilter` クラスで構成されています。</span><span class="sxs-lookup"><span data-stu-id="950c9-191">The `RequestSetOptionsMiddleware` is configured in the `RequestSetOptionsStartupFilter` class:</span></span>

[!code-csharp[](startup/sample/RequestSetOptionsStartupFilter.cs?name=snippet1&highlight=7)]

<span data-ttu-id="950c9-192">`IStartupFilter` は `ConfigureServices` のサービス コンテナーに登録されています。</span><span class="sxs-lookup"><span data-stu-id="950c9-192">The `IStartupFilter` is registered in the service container in `ConfigureServices`:</span></span>

[!code-csharp[](startup/sample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="950c9-193">`option` のクエリ文字列パラメーターを指定すると、ミドルウェアは MVC ミドルウェアが応答をレンダリングする前に値の割り当てを処理します。</span><span class="sxs-lookup"><span data-stu-id="950c9-193">When a query string parameter for `option` is provided, the middleware processes the value assignment before the MVC middleware renders the response:</span></span>

![レンダリングされたインデックス ページを表示するブラウザー ウィンドウ。](startup/_static/index.png)

<span data-ttu-id="950c9-196">ミドルウェアの実行順序は、`IStartupFilter` の登録順に設定されています。</span><span class="sxs-lookup"><span data-stu-id="950c9-196">Middleware execution order is set by the order of `IStartupFilter` registrations:</span></span>

* <span data-ttu-id="950c9-197">複数の `IStartupFilter` の実装が、同じオブジェクトとやり取りする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="950c9-197">Multiple `IStartupFilter` implementations may interact with the same objects.</span></span> <span data-ttu-id="950c9-198">順序が重要な場合は、ミドルウェアの実行順序に合わせて `IStartupFilter` サービスの登録順序を指定してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-198">If ordering is important, order their `IStartupFilter` service registrations to match the order that their middlewares should run.</span></span>
* <span data-ttu-id="950c9-199">`IStartupFilter` に登録された他のアプリケーション ミドルウェアの前または後に実行される `IStartupFilter` の実装が 1 つまたは複数あるミドルウェアを、ライブラリで追加することができます。</span><span class="sxs-lookup"><span data-stu-id="950c9-199">Libraries may add middleware with one or more `IStartupFilter` implementations that run before or after other app middleware registered with `IStartupFilter`.</span></span> <span data-ttu-id="950c9-200">ライブラリの `IStartupFilter` によって追加されたミドルウェアの前に `IStartupFilter` ミドルウェアを呼び出すには、ライブラリがサービス コンテナーに追加される前にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="950c9-200">To invoke an `IStartupFilter` middleware before a middleware added by a library's `IStartupFilter`, position the service registration before the library is added to the service container.</span></span> <span data-ttu-id="950c9-201">後で呼び出すには、ライブラリが追加される後にサービスの登録を配置します。</span><span class="sxs-lookup"><span data-stu-id="950c9-201">To invoke it afterward, position the service registration after the library is added.</span></span>

## <a name="adding-configuration-at-startup-from-an-external-assembly"></a><span data-ttu-id="950c9-202">外部アセンブリからの起動時に構成を追加する</span><span class="sxs-lookup"><span data-stu-id="950c9-202">Adding configuration at startup from an external assembly</span></span>

<span data-ttu-id="950c9-203">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) の実装により、アプリの `Startup` クラスの外部にある外部アセンブリからの起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="950c9-203">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="950c9-204">詳細については、「[Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration)」(外部アセンブリからアプリを拡張する) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="950c9-204">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="950c9-205">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="950c9-205">Additional resources</span></span>

* [<span data-ttu-id="950c9-206">ホスティング</span><span class="sxs-lookup"><span data-stu-id="950c9-206">Hosting</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="950c9-207">複数の環境の使用</span><span class="sxs-lookup"><span data-stu-id="950c9-207">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="950c9-208">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="950c9-208">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="950c9-209">ログ</span><span class="sxs-lookup"><span data-stu-id="950c9-209">Logging</span></span>](xref:fundamentals/logging/index)
* [<span data-ttu-id="950c9-210">構成</span><span class="sxs-lookup"><span data-stu-id="950c9-210">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="950c9-211">StartupLoader クラス: FindStartupType メソッド (参照元)</span><span class="sxs-lookup"><span data-stu-id="950c9-211">StartupLoader class: FindStartupType method (reference source)</span></span>](https://github.com/aspnet/Hosting/blob/rel/2.0.0/src/Microsoft.AspNetCore.Hosting/Internal/StartupLoader.cs#L66-L116)
