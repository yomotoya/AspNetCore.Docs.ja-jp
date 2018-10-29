---
title: IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する
author: guardrex
description: IHostingStartup 実装を使用して、外部アセンブリから ASP.NET Core アプリを拡張する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207564"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="283cb-103">IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する</span><span class="sxs-lookup"><span data-stu-id="283cb-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="283cb-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="283cb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="283cb-105">外部アセンブリからの起動時には、[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (ホスティング スタートアップ) 実装によって拡張機能がアプリに追加されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="283cb-106">たとえば、外部ライブラリではホスティング スタートアップ実装を使用して、追加の構成プロバイダーまたはサービスをアプリに提供できます。</span><span class="sxs-lookup"><span data-stu-id="283cb-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="283cb-107">`IHostingStartup` *は、ASP.NET Core 2.0 以降で使用できます。*</span><span class="sxs-lookup"><span data-stu-id="283cb-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="283cb-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="283cb-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="283cb-109">HostingStartup 属性</span><span class="sxs-lookup"><span data-stu-id="283cb-109">HostingStartup attribute</span></span>

<span data-ttu-id="283cb-110">[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性は、実行時にアクティブ化されるホスティング スタートアップ アセンブリが存在することを示しています。</span><span class="sxs-lookup"><span data-stu-id="283cb-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="283cb-111">入力アセンブリまたは `Startup` クラスを含むアセンブリは、`HostingStartup` 属性について自動的にスキャンされます。</span><span class="sxs-lookup"><span data-stu-id="283cb-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="283cb-112">`HostingStartup` 属性を検索するアセンブリの一覧は、[WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 内の構成からランタイム時に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="283cb-113">検出から除外されるアセンブリの一覧は、[WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) から読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="283cb-114">詳細については、Web ホストに関するページの「[ホスティング スタートアップ アセンブリ](xref:fundamentals/host/web-host#hosting-startup-assemblies)」および[除外するホスティング スタートアップ アセンブリ](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)に関するセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="283cb-115">次に例では、ホスティング スタートアップ アセンブリの名前空間は `StartupEnhancement` となります。</span><span class="sxs-lookup"><span data-stu-id="283cb-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="283cb-116">ホスティング スタートアップ コードを含むクラスは `StartupEnhancementHostingStartup` です。</span><span class="sxs-lookup"><span data-stu-id="283cb-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="283cb-117">`HostingStartup` 属性は、通常、ホスティング スタートアップ アセンブリの `IHostingStartup` 実装クラス ファイル内に置かれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="283cb-118">読み込まれたホスティング スタートアップ アセンブリを見つける</span><span class="sxs-lookup"><span data-stu-id="283cb-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="283cb-119">読み込まれたホスティング スタートアップ アセンブリを検出するには、ログ記録を有効にしてアプリのログを確認します。</span><span class="sxs-lookup"><span data-stu-id="283cb-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="283cb-120">アセンブリの読み込み時に発生したエラーがログ記録されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="283cb-121">読み込まれたホスティング スタートアップ アセンブリは、デバッグ レベルでログ記録され、すべてのエラーが記録されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="283cb-122">ホスティング スタートアップ アセンブリの自動読み込みを無効にする</span><span class="sxs-lookup"><span data-stu-id="283cb-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="283cb-123">ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、次の方法のいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="283cb-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="283cb-124">すべてのホスティング スタートアップ アセンブリの読み込みを回避するには、次のいずれかを `true` または `1` に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="283cb-125">[[ホスティング スタートアップを回避する]](xref:fundamentals/host/web-host#prevent-hosting-startup) ホスト構成設定。</span><span class="sxs-lookup"><span data-stu-id="283cb-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="283cb-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="283cb-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="283cb-127">特定のホスティング スタートアップ アセンブリの読み込みを回避するには、次に示すいずれかを、起動時に除外するホスティング スタートアップ アセンブリを示すセミコロンで区切られた文字列に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="283cb-128">[[Hosting Startup Exclude Assemblies]\(除外するホスティング スタートアップ アセンブリ\)](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ホスト構成設定。</span><span class="sxs-lookup"><span data-stu-id="283cb-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="283cb-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="283cb-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="283cb-130">ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、次に示すいずれかを `true` または `1` に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="283cb-131">[[ホスティング スタートアップを回避する]](xref:fundamentals/host/web-host#prevent-hosting-startup) ホスト構成設定。</span><span class="sxs-lookup"><span data-stu-id="283cb-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="283cb-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数。</span><span class="sxs-lookup"><span data-stu-id="283cb-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="283cb-133">ホスト構成設定と環境変数が両方とも設定されている場合は、ホスト設定によって動作が制御されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="283cb-134">ホスト設定または環境変数を使用してホスティング スタートアップ アセンブリを無効にすると、アセンブリがグローバルに無効になり、場合によってはアプリのいくつかの属性も無効になります。</span><span class="sxs-lookup"><span data-stu-id="283cb-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="283cb-135">プロジェクト</span><span class="sxs-lookup"><span data-stu-id="283cb-135">Project</span></span>

<span data-ttu-id="283cb-136">次のいずれかの種類のプロジェクトを使用して、ホスティング スタートアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="283cb-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="283cb-137">クラス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="283cb-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="283cb-138">エントリ ポイントのないコンソール アプリ</span><span class="sxs-lookup"><span data-stu-id="283cb-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="283cb-139">クラス ライブラリ</span><span class="sxs-lookup"><span data-stu-id="283cb-139">Class library</span></span>

<span data-ttu-id="283cb-140">クラス ライブラリでは、ホスティング スタートアップの拡張機能を提供できます。</span><span class="sxs-lookup"><span data-stu-id="283cb-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="283cb-141">このライブラリには `HostingStartup` 属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="283cb-142">[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)には、Razor Pages アプリ、*HostingStartupApp*、クラス ライブラリ、および *HostingStartupLibrary* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="283cb-143">クラス ライブラリには次のものが含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-143">The class library:</span></span>

* <span data-ttu-id="283cb-144">`IHostingStartup` を実装するホスティング スタートアップ クラス `ServiceKeyInjection`。</span><span class="sxs-lookup"><span data-stu-id="283cb-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="283cb-145">`ServiceKeyInjection` では、メモリ内の構成プロバイダー ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)) を使用して、サービスの文字列のペアがアプリの構成に追加されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="283cb-146">ホスティング スタートアップの名前空間とクラスを識別する `HostingStartup` 属性。</span><span class="sxs-lookup"><span data-stu-id="283cb-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="283cb-147">`ServiceKeyInjection` クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能が追加されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="283cb-148">ホスティング スタートアップ アセンブリ内の `IHostingStartup.Configure` は、ユーザー コード内の `Startup.Configure` よりも前にランタイムによって呼び出されます。このため、ホスティング スタートアップ アセンブリによって提供されるすべての構成をユーザー コードで上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="283cb-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="283cb-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="283cb-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="283cb-150">アプリのインデックス ページでは、クラス ライブラリのホスティング スタートアップ アセンブリによって設定された 2 つのキーの構成値が読み取られて表示されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="283cb-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="283cb-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="283cb-152">[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) には、別のホスティング スタートアップ *HostingStartupPackage* を提供する NuGet パッケージ プロジェクトも含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="283cb-153">パッケージには、前に説明したクラス ライブラリと同じ特性があります。</span><span class="sxs-lookup"><span data-stu-id="283cb-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="283cb-154">パッケージには次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-154">The package:</span></span>

* <span data-ttu-id="283cb-155">`IHostingStartup` を実装するホスティング スタートアップ クラス `ServiceKeyInjection`。</span><span class="sxs-lookup"><span data-stu-id="283cb-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="283cb-156">`ServiceKeyInjection` では、アプリの構成にサービス文字列のペアが追加されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="283cb-157">`HostingStartup`属性。</span><span class="sxs-lookup"><span data-stu-id="283cb-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="283cb-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="283cb-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="283cb-159">アプリのインデックス ページでは、パッケージのホスティング スタートアップ アセンブリによって設定された 2 つのキーの構成値が読み取られて表示されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="283cb-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="283cb-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="283cb-161">エントリ ポイントのないコンソール アプリ</span><span class="sxs-lookup"><span data-stu-id="283cb-161">Console app without an entry point</span></span>

<span data-ttu-id="283cb-162">*このアプローチは、.NET Core アプリでのみ利用でき、.NET Framework では利用できません。*</span><span class="sxs-lookup"><span data-stu-id="283cb-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="283cb-163">エントリ ポイントのないコンソール アプリ内では、アクティブ化に関するコンパイル時参照を必要としない動的ホスティング スタートアップ拡張機能を指定できます。</span><span class="sxs-lookup"><span data-stu-id="283cb-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="283cb-164">このアプリには `HostingStartup` 属性が含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="283cb-165">動的ホスティング スタートアップを作成するには:</span><span class="sxs-lookup"><span data-stu-id="283cb-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="283cb-166">実装ライブラリは、`IHostingStartup` 実装を含むクラスから作成されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="283cb-167">実装ライブラリは、通常のパッケージとして扱われます。</span><span class="sxs-lookup"><span data-stu-id="283cb-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="283cb-168">エントリ ポイントのないコンソール アプリでは、実装ライブラリ パッケージが参照されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="283cb-169">コンソール アプリを使用する理由:</span><span class="sxs-lookup"><span data-stu-id="283cb-169">A console app is used because:</span></span>
   * <span data-ttu-id="283cb-170">依存関係ファイルは実行可能なアプリ資産です。そのため、依存関係ファイルをライブラリで提供することはできません。</span><span class="sxs-lookup"><span data-stu-id="283cb-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="283cb-171">[ランタイム パッケージ ストア](/dotnet/core/deploying/runtime-store) にライブラリを直接追加することはできません。それには、共有ランタイムを対象とする実行可能なプロジェクトが必要です。</span><span class="sxs-lookup"><span data-stu-id="283cb-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="283cb-172">ホスティング スタートアップの依存関係を取得するには、コンソール アプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="283cb-173">コンソール アプリを発行すると、その結果として、依存関係ファイルから未使用の依存関係が切り捨てられます。</span><span class="sxs-lookup"><span data-stu-id="283cb-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="283cb-174">アプリとその依存関係ファイルは、ランタイム パッケージ ストアに格納されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="283cb-175">ホスティング スタートアップ アセンブリとその依存関係ファイルは、環境変数のペアで参照され、検出できるようになっています。</span><span class="sxs-lookup"><span data-stu-id="283cb-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="283cb-176">コンソール アセンブリでは、[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) パッケージが参照されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="283cb-177">[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性では、[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) のビルド時に、読み込みと実行のために `IHostingStartup` の実装としてクラスを識別します。</span><span class="sxs-lookup"><span data-stu-id="283cb-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="283cb-178">次の例では、名前空間は `StartupEnhancement`、クラスは `StartupEnhancementHostingStartup` です。</span><span class="sxs-lookup"><span data-stu-id="283cb-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="283cb-179">クラスは `IHostingStartup` を実装しています。</span><span class="sxs-lookup"><span data-stu-id="283cb-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="283cb-180">クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="283cb-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="283cb-181">ホスティング スタートアップ アセンブリ内の `IHostingStartup.Configure` は、ユーザー コード内の `Startup.Configure` よりも前にランタイムによって呼び出されます。このため、ホスティング スタートアップ アセンブリによって提供されるすべての構成をユーザー コードで上書きすることができます。</span><span class="sxs-lookup"><span data-stu-id="283cb-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="283cb-182">`IHostingStartup` プロジェクトのビルド時に、依存関係ファイル (*\*.deps.json*) によってアセンブリの `runtime` の場所が *bin* フォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="283cb-183">ファイルの一部のみが示されています。</span><span class="sxs-lookup"><span data-stu-id="283cb-183">Only part of the file is shown.</span></span> <span data-ttu-id="283cb-184">例のアセンブリ名は `StartupEnhancement` です。</span><span class="sxs-lookup"><span data-stu-id="283cb-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="283cb-185">ホスティング スタートアップ アセンブリを指定する</span><span class="sxs-lookup"><span data-stu-id="283cb-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="283cb-186">クラス ライブラリまたはコンソール アプリのいずれかで提供されるホスティング スタートアップの場合は、`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数内にホスティング スタートアップ アセンブリの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="283cb-187">環境変数はアセンブリのセミコロン区切りのリストです。</span><span class="sxs-lookup"><span data-stu-id="283cb-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="283cb-188">ホスティング スタートアップ アセンブリのみが、`HostingStartup` 属性に対してスキャンされます。</span><span class="sxs-lookup"><span data-stu-id="283cb-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="283cb-189">サンプル アプリ *HostingStartupApp* では、前述したホスティング スタートアップを検出するために、環境変数が次の値に設定されています。</span><span class="sxs-lookup"><span data-stu-id="283cb-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="283cb-190">ホスティング スタートアップ アセンブリはまた、[[ホスティング スタートアップ アセンブリ]](xref:fundamentals/host/web-host#hosting-startup-assemblies) ホスト構成設定を使用して設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="283cb-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="283cb-191">複数のホスティング スタートアップ アセンブリがある場合、それらの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドは、アセンブリが一覧表示されている順序で実行されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="283cb-192">アクティベーション</span><span class="sxs-lookup"><span data-stu-id="283cb-192">Activation</span></span>

<span data-ttu-id="283cb-193">ホスティング スタートアップのアクティブ化のオプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="283cb-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="283cb-194">[ランタイム ストア](#runtime-store) &ndash; アクティブ化では、アクティブ化に関するコンパイル時参照を必要としません。</span><span class="sxs-lookup"><span data-stu-id="283cb-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="283cb-195">サンプル アプリでは、ホスティング スタートアップ アセンブリおよび依存関係のファイルが *deployment* フォルダーに配置されています。これにより、複数のコンピューターから成る環境でのホスティング スタートアップの配置が容易になります。</span><span class="sxs-lookup"><span data-stu-id="283cb-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="283cb-196">*deployment* フォルダーには、ホスティングスタートアップが有効になるように配置システム上で環境変数を作成および変更する PowerShell スクリプトも含まれています。</span><span class="sxs-lookup"><span data-stu-id="283cb-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="283cb-197">アクティブ化で必須のコンパイル時参照</span><span class="sxs-lookup"><span data-stu-id="283cb-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="283cb-198">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="283cb-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="283cb-199">プロジェクトの bin フォルダー</span><span class="sxs-lookup"><span data-stu-id="283cb-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="283cb-200">ランタイム ストア</span><span class="sxs-lookup"><span data-stu-id="283cb-200">Runtime store</span></span>

<span data-ttu-id="283cb-201">ホスティング スタートアップ実装は、[ランタイム ストア](/dotnet/core/deploying/runtime-store)に置かれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="283cb-202">アセンブリへのコンパイル時参照は、機能強化されたアプリで必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="283cb-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="283cb-203">ホスティング スタートアップがビルドされると、ホスティング スタートアップのプロジェクト ファイルが [dotnet store](/dotnet/core/tools/dotnet-store) コマンド用のマニフェスト ファイルとして機能します。</span><span class="sxs-lookup"><span data-stu-id="283cb-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="283cb-204">共有フレームワークに属していないホスティング スタートアップ アセンブリとその他の依存関係は、このコマンドによって次の場所にある、ユーザー プロファイルのランタイム ストアに置かれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-205">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-206">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-207">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="283cb-208">グローバルに使用できるようにアセンブリと依存関係を配置したい場合は、次のパスを使用して `dotnet store` コマンドに `-o|--output` オプションを追加します。</span><span class="sxs-lookup"><span data-stu-id="283cb-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-209">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-210">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-211">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="283cb-212">**ホスティング スタートアップの依存関係ファイルを変更して配置する**</span><span class="sxs-lookup"><span data-stu-id="283cb-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="283cb-213">ランタイムの場所は、*\*.deps.json* ファイルで指定されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="283cb-214">拡張機能をアクティブ化するには、`runtime` 要素で拡張機能のランタイム アセンブリの場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="283cb-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="283cb-215">`runtime` の場所には `lib/<TARGET_FRAMEWORK_MONIKER>/` のプレフィックスを付けます。</span><span class="sxs-lookup"><span data-stu-id="283cb-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="283cb-216">サンプル コード (*StartupDiagnostics* プロジェクト) では、*\*.deps.json* ファイルの変更は [PowerShell](/powershell/scripting/powershell-scripting) スクリプトによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="283cb-217">PowerShell スクリプトは、プロジェクト ファイル内のビルド ターゲットによって自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="283cb-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="283cb-218">実装の *\*.deps.json* ファイルは、アセンブリの場所に配置されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="283cb-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="283cb-219">ユーザーごとに使用する場合は、ユーザー プロファイルの `.dotnet` 設定の *additonalDeps* フォルダーにファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="283cb-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-220">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-221">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-222">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="283cb-223">グローバルに使用する場合は、.NET Core インストールの *additonalDeps* フォルダーにファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="283cb-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-224">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-225">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-226">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="283cb-227">共有フレームワークのバージョンには、ターゲット アプリが使用する共有ランタイムのバージョンが反映されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="283cb-228">共有ランタイムは、*\*.runtimeconfig.json* ファイルに示されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="283cb-229">サンプル アプリ (*HostingStartupApp*) では、共有ランタイムは、*HostingStartupApp.runtimeconfig.json* ファイル内に指定されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="283cb-230">**ホスティング スタートアップの依存関係ファイルを一覧表示する**</span><span class="sxs-lookup"><span data-stu-id="283cb-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="283cb-231">実装の *\*.deps.json* ファイルの場所は、`DOTNET_ADDITIONAL_DEPS` 環境変数内に一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="283cb-232">そのファイルがユーザー プロファイルの *.dotnet* フォルダー内に置かれている場合は、環境変数の値を次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-233">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-234">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-235">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="283cb-236">ファイルがグローバルに使用するために .NET Core インストールに配置される場合は、ファイルに完全なパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-237">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-238">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-239">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="283cb-240">依存関係ファイル (*HostingStartupApp.runtimeconfig.json*) を検索するサンプル アプリ (*HostingStartupApp*) の場合、依存関係ファイルはユーザーのプロファイルに置かれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="283cb-241">Windows</span><span class="sxs-lookup"><span data-stu-id="283cb-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="283cb-242">`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="283cb-243">macOS</span><span class="sxs-lookup"><span data-stu-id="283cb-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="283cb-244">`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="283cb-245">Linux</span><span class="sxs-lookup"><span data-stu-id="283cb-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="283cb-246">`DOTNET_ADDITIONAL_DEPS` 環境変数を次の値に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="283cb-247">さまざまなオペレーティング システムの環境変数を設定する方法の例については、[複数の環境の使用](xref:fundamentals/environments)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="283cb-248">**配置**</span><span class="sxs-lookup"><span data-stu-id="283cb-248">**Deployment**</span></span>

<span data-ttu-id="283cb-249">サンプル アプリでは、複数のコンピューターから成る環境へのホスティング スタートアップの配置を容易にするために、発行された出力内に、次を含む *deployment* フォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="283cb-250">ホスティング スタートアップ アセンブリ。</span><span class="sxs-lookup"><span data-stu-id="283cb-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="283cb-251">ホスティング スタートアップの依存関係ファイル。</span><span class="sxs-lookup"><span data-stu-id="283cb-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="283cb-252">ホスティング スタートアップのアクティブ化をサポートできるように `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` と `DOTNET_ADDITIONAL_DEPS` を作成および変更する PowerShell スクリプト。</span><span class="sxs-lookup"><span data-stu-id="283cb-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="283cb-253">このスクリプトは、配置システム上の PowerShell 管理用コマンド プロンプトから実行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="283cb-254">NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="283cb-254">NuGet package</span></span>

<span data-ttu-id="283cb-255">NuGet パッケージでは、ホスティング スタートアップの拡張機能を提供できます。</span><span class="sxs-lookup"><span data-stu-id="283cb-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="283cb-256">このパッケージには `HostingStartup` 属性があります。</span><span class="sxs-lookup"><span data-stu-id="283cb-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="283cb-257">このパッケージで提供される種類のホスティング スタートアップは、次のアプローチのいずれかを使用してアプリで利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="283cb-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="283cb-258">機能強化されたアプリのプロジェクト ファイルでは、アプリのプロジェクト ファイル内のホスティング スタートアップに対するパッケージ参照 (コンパイル時参照) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="283cb-259">コンパイル時参照を適用すると、ホスティング スタートアップ アセンブリとその依存関係のすべてが、アプリの依存関係ファイル (*\*.deps.json*) に組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="283cb-260">このアプローチは、[nuget.org](https://www.nuget.org/) に発行されるホスティング スタートアップ アセンブリ パッケージに適用されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="283cb-261">ホスティング スタートアップの依存関係ファイルは、「[ランタイム ストア](#runtime-store)」のセクションで説明にしたように (コンパイル参照がない場合)、機能強化されたアプリで利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="283cb-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="283cb-262">NuGet パッケージとランタイム ストアの詳細については、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="283cb-263">クロスプラットフォーム ツールを使用して NuGet パッケージを作成する方法</span><span class="sxs-lookup"><span data-stu-id="283cb-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="283cb-264">パッケージを公開する</span><span class="sxs-lookup"><span data-stu-id="283cb-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="283cb-265">ランタイム パッケージ ストア</span><span class="sxs-lookup"><span data-stu-id="283cb-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="283cb-266">プロジェクトの bin フォルダー</span><span class="sxs-lookup"><span data-stu-id="283cb-266">Project bin folder</span></span>

<span data-ttu-id="283cb-267">ホスティング スタートアップの拡張機能は、機能強化されたアプリ内の *bin* 配置アセンブリによって提供できます。</span><span class="sxs-lookup"><span data-stu-id="283cb-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="283cb-268">このアセンブリで提供される種類のホスティング スタートアップは、次のアプローチのいずれかを使用してアプリで利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="283cb-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="283cb-269">機能強化されたアプリのプロジェクト ファイルでは、ホスティング スタートアップへのアセンブリ参照 (コンパイル時参照) が作成されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="283cb-270">コンパイル時参照を適用すると、ホスティング スタートアップ アセンブリとその依存関係のすべてが、アプリの依存関係ファイル (*\*.deps.json*) に組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="283cb-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="283cb-271">このアプローチが適用されるのは、コンパイルされたホスティング スタートアップ ライブラリのアセンブリ (DLL ファイル) を、使用するプロジェクトに移動するか、または使用するプロジェクトからアクセス可能な場所に移動することが配置シナリオで必要であり、かつホスティング スタートアップのアセンブリへの参照が作成される場合です。</span><span class="sxs-lookup"><span data-stu-id="283cb-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="283cb-272">ホスティング スタートアップの依存関係ファイルは、「[ランタイム ストア](#runtime-store)」のセクションで説明にしたように (コンパイル参照がない場合)、機能強化されたアプリで利用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="283cb-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="283cb-273">サンプル コード</span><span class="sxs-lookup"><span data-stu-id="283cb-273">Sample code</span></span>

<span data-ttu-id="283cb-274">[サンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([ダウンロードする方法](xref:index#how-to-download-a-sample)) では、ホスティング スタートアップ実装のシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="283cb-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="283cb-275">2 つのホスティング スタートアップ アセンブリ (クラス ライブラリ) ではそれぞれ、メモリ内の構成のキーと値のペアの組が設定されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="283cb-276">NuGet パッケージ (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="283cb-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="283cb-277">クラス ライブラリ (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="283cb-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="283cb-278">ホスティング スタートアップは、ランタイム ストア配置アセンブリ (*StartupDiagnostics*) からアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="283cb-279">このアセンブリによってスタートアップ時に 2 つのミドルウェアがアプリに追加され、これにより診断情報が提供されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="283cb-280">登録サービス</span><span class="sxs-lookup"><span data-stu-id="283cb-280">Registered services</span></span>
  * <span data-ttu-id="283cb-281">アドレス (スキーム、ホスト、パス ベース、パス、クエリ文字列)</span><span class="sxs-lookup"><span data-stu-id="283cb-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="283cb-282">接続 (リモート IP、リモート ポート、ローカル IP、ローカル ポート、クライアント証明書)</span><span class="sxs-lookup"><span data-stu-id="283cb-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="283cb-283">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="283cb-283">Request headers</span></span>
  * <span data-ttu-id="283cb-284">環境変数</span><span class="sxs-lookup"><span data-stu-id="283cb-284">Environment variables</span></span>

<span data-ttu-id="283cb-285">サンプルを実行するには</span><span class="sxs-lookup"><span data-stu-id="283cb-285">To run the sample:</span></span>

<span data-ttu-id="283cb-286">**NuGet パッケージからのアクティブ化**</span><span class="sxs-lookup"><span data-stu-id="283cb-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="283cb-287">[dotnet pack](/dotnet/core/tools/dotnet-pack) コマンドを使用して *HostingStartupPackage* パッケージをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="283cb-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="283cb-288">*HostingStartupPackage* のパッケージのアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。</span><span class="sxs-lookup"><span data-stu-id="283cb-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="283cb-289">アプリをコンパイルして実行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-289">Compile and run the app.</span></span> <span data-ttu-id="283cb-290">機能拡張されたアプリにはパッケージ参照 (コンパイル時参照) が存在します。</span><span class="sxs-lookup"><span data-stu-id="283cb-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="283cb-291">アプリのプロジェクト ファイル内の `<PropertyGroup>` では、パッケージ プロジェクトの出力 (*../HostingStartupPackage/Bin/debug*) がパッケージ ソースとして指定されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="283cb-292">これにより、[nuget.org](https://www.nuget.org/) にパッケージをアップロードすることなく、アプリによりパッケージが使用されるようになります。詳細については、HostingStartupApp のプロジェクト ファイル内の注を参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="283cb-293">インデックス ページで表示されるサービス構成キー値が、パッケージの `ServiceKeyInjection.Configure` メソッドによって設定された値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="283cb-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="283cb-294">*HostingStartupPackage* プロジェクトに変更を加えてそれをコンパイルする場合は、ローカル キャッシュから、古いパッケージではなく更新されたパッケージが、*HostingStartupApp* によって確実に受信されるように、ローカルの NuGet パッケージ キャッシュをクリアします。</span><span class="sxs-lookup"><span data-stu-id="283cb-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="283cb-295">ローカルの NuGet キャッシュをクリアするには、次の [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="283cb-296">**クラス ライブラリからのアクティブ化**</span><span class="sxs-lookup"><span data-stu-id="283cb-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="283cb-297">[dotnet build](/dotnet/core/tools/dotnet-build) コマンドを使用して *HostingStartupLibrary* クラス ライブラリをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="283cb-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="283cb-298">*HostingStartupLibrary* のクラス ライブラリのアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。</span><span class="sxs-lookup"><span data-stu-id="283cb-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="283cb-299">クラス ライブラリのアセンブリをアプリに *bin* 配置するには、クラス ライブラリのコンパイルされた出力からアプリの *bin/Debug* フォルダーに *HostingStartupLibrary.dll* ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="283cb-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="283cb-300">アプリをコンパイルして実行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-300">Compile and run the app.</span></span> <span data-ttu-id="283cb-301">アプリのプロジェクト ファイル内の `<ItemGroup>` は、クラス ライブラリのアセンブリ (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) を参照します (コンパイル時参照)。</span><span class="sxs-lookup"><span data-stu-id="283cb-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="283cb-302">詳細については、HostingStartupApp のプロジェクト ファイル内の注を参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="283cb-303">インデックス ページで表示されるサービス構成キー値が、クラス ライブラリの `ServiceKeyInjection.Configure` メソッドによって設定された値と一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="283cb-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="283cb-304">**ランタイム ストア配置アセンブリからのアクティブ化**</span><span class="sxs-lookup"><span data-stu-id="283cb-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="283cb-305">*StartupDiagnostics* プロジェクトでは、[PowerShell](/powershell/scripting/powershell-scripting) を使用して、その *StartupDiagnostics.deps.json* ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="283cb-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="283cb-306">既定では、PowerShell は Windows 7 SP1 と Windows Server 2008 R2 SP1 以降の Windows 上にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="283cb-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="283cb-307">他のプラットフォームで PowerShell を取得するには、「[Windows PowerShell のインストール](/powershell/scripting/setup/installing-powershell#powershell-core)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="283cb-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="283cb-308">*StartupDiagnostics* プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="283cb-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="283cb-309">プロジェクトをビルドすると、プロジェクト ファイル内のビルド ターゲットによって自動的に次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="283cb-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="283cb-310">PowerShell スクリプトをトリガーして、*StartupDiagnostics.deps.json* ファイルを変更する。</span><span class="sxs-lookup"><span data-stu-id="283cb-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="283cb-311">*StartupDiagnostics.deps.json* ファイルをユーザー プロファイルの *additionalDeps* フォルダーに移動する。</span><span class="sxs-lookup"><span data-stu-id="283cb-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="283cb-312">ホスティング スタートアップのディレクトリ内のコマンド プロンプトで `dotnet store` を実行して、アセンブリとその依存関係をユーザー プロファイルのランタイム ストアに格納します。</span><span class="sxs-lookup"><span data-stu-id="283cb-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="283cb-313">Windows の場合、そのコマンドでは `win7-x64` [ランタイム識別子 (RID)](/dotnet/core/rid-catalog) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="283cb-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="283cb-314">別のランタイムに対してホスティング スタートアップを指定する場合は、適切な RID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="283cb-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="283cb-315">環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-315">Set the environment variables:</span></span>
   * <span data-ttu-id="283cb-316">*StartupDiagnostics* のアセンブリ名を `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境変数に追加します。</span><span class="sxs-lookup"><span data-stu-id="283cb-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="283cb-317">Windows の場合は、`DOTNET_ADDITIONAL_DEPS` 環境変数を `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\` に設定します。</span><span class="sxs-lookup"><span data-stu-id="283cb-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="283cb-318">macOS または Linux の場合は、`DOTNET_ADDITIONAL_DEPS` 環境変数を `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/` に設定します。ここで、`<USER>` は、ホスティング スタートアップを含むユーザー プロファイルです。</span><span class="sxs-lookup"><span data-stu-id="283cb-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="283cb-319">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="283cb-319">Run the sample app.</span></span>
1. <span data-ttu-id="283cb-320">`/services` エンドポイントを要求して、アプリの登録サービスを参照します。</span><span class="sxs-lookup"><span data-stu-id="283cb-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="283cb-321">`/diag` エンドポイントを要求して、診断情報を参照します。</span><span class="sxs-lookup"><span data-stu-id="283cb-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
