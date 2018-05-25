---
title: IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する
author: guardrex
description: IHostingStartup 実装を使用して、外部アセンブリから ASP.NET Core アプリを拡張する方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="c99e2-103">IHostingStartup を使用して ASP.NET Core の外部アセンブリからアプリを拡張する</span><span class="sxs-lookup"><span data-stu-id="c99e2-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="c99e2-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c99e2-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c99e2-105">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) の実装により、アプリの `Startup` クラスの外部にある外部アセンブリからの起動時に拡張機能をアプリに追加できるようになります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="c99e2-106">たとえば、外部ツール ライブラリでは `IHostingStartup` 実装を使用して、アプリに追加の構成プロバイダーやサービスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="c99e2-107">`IHostingStartup`  *は、ASP.NET Core 2.0 以降でのみ使用できます。*</span><span class="sxs-lookup"><span data-stu-id="c99e2-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="c99e2-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c99e2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="c99e2-109">読み込まれたホスティング スタートアップ アセンブリを見つける</span><span class="sxs-lookup"><span data-stu-id="c99e2-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="c99e2-110">アプリまたはライブラリによって読み込まれたホスティング スタートアップ アセンブリを見つけるには、ログ記録を有効にして、アプリケーション ログを確認します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="c99e2-111">アセンブリの読み込み時に発生したエラーがログ記録されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="c99e2-112">読み込まれたホスティング スタートアップ アセンブリは、デバッグ レベルでログ記録され、すべてのエラーが記録されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="c99e2-113">サンプル アプリでは、[HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) を `string` 配列に読み込み、アプリのインデックス ページに結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="c99e2-114">ホスティング スタートアップ アセンブリの自動読み込みを無効にする</span><span class="sxs-lookup"><span data-stu-id="c99e2-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="c99e2-115">ホスティング スタートアップ アセンブリの自動読み込みを無効にするには、2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="c99e2-116">「[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup)」(ホスティング スタートアップの禁止) のホスト構成設定を設定する</span><span class="sxs-lookup"><span data-stu-id="c99e2-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="c99e2-117">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="c99e2-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="c99e2-118">ホストの設定または環境変数が `true` または `1` に設定されている場合、ホスティング スタートアップ アセンブリは自動的に読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="c99e2-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="c99e2-119">どちらも設定されている場合は、ホスト設定で動作が制御されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="c99e2-120">ホスト設定または環境変数を使用してホスティング スタートアップ アセンブリを無効にすると、グローバルに無効化され、アプリのいくつかの属性も無効にされる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="c99e2-121">ライブラリで独自の構成オプションが提供されない限り、現在、追加されたホスティング スタートアップ アセンブリを部分的に無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="c99e2-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="c99e2-122">将来のリリースで、ホスティング スタートアップ アセンブリを部分的に無効にする機能が提供されます ([GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243) を参照)。</span><span class="sxs-lookup"><span data-stu-id="c99e2-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="c99e2-123">IHostingStartup の実装</span><span class="sxs-lookup"><span data-stu-id="c99e2-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="c99e2-124">アセンブリの作成</span><span class="sxs-lookup"><span data-stu-id="c99e2-124">Create the assembly</span></span>

<span data-ttu-id="c99e2-125">`IHostingStartup` 拡張機能は、エントリ ポイントのない、コンソール アプリに基づいてアセンブリとして配置されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="c99e2-126">アセンブリでは、[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) パッケージが参照されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="c99e2-127">[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 属性では、[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) のビルド時に、読み込みと実行のために `IHostingStartup` の実装としてクラスを識別します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="c99e2-128">次の例では、名前空間は `StartupEnhancement`、クラスは `StartupEnhancementHostingStartup` です。</span><span class="sxs-lookup"><span data-stu-id="c99e2-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="c99e2-129">クラスは `IHostingStartup` を実装しています。</span><span class="sxs-lookup"><span data-stu-id="c99e2-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="c99e2-130">クラスの [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) メソッドでは、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) を使用してアプリに拡張機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="c99e2-131">`IHostingStartup` プロジェクトのビルド時に、依存関係ファイル (*\*.deps.json*) によってアセンブリの `runtime` の場所が *bin* フォルダーに設定されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="c99e2-132">ファイルの一部のみが示されています。</span><span class="sxs-lookup"><span data-stu-id="c99e2-132">Only part of the file is shown.</span></span> <span data-ttu-id="c99e2-133">例のアセンブリ名は `StartupEnhancement` です。</span><span class="sxs-lookup"><span data-stu-id="c99e2-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="c99e2-134">依存関係ファイルの更新</span><span class="sxs-lookup"><span data-stu-id="c99e2-134">Update the dependencies file</span></span>

<span data-ttu-id="c99e2-135">ランタイムの場所は、*\*.deps.json* ファイルで指定されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="c99e2-136">拡張機能をアクティブ化するには、`runtime` 要素で拡張機能のランタイム アセンブリの場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="c99e2-137">`runtime` の場所には `lib/netcoreapp2.0/` のプレフィックスを付けます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="c99e2-138">サンプル アプリでは、*\*.deps.json* ファイルの変更は [PowerShell](/powershell/scripting/powershell-scripting) スクリプトによって実行されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="c99e2-139">PowerShell スクリプトは、プロジェクト ファイル内のビルド ターゲットによって自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="c99e2-140">拡張機能のアクティブ化</span><span class="sxs-lookup"><span data-stu-id="c99e2-140">Enhancement activation</span></span>

<span data-ttu-id="c99e2-141">**アセンブリ ファイルの配置**</span><span class="sxs-lookup"><span data-stu-id="c99e2-141">**Place the assembly file**</span></span>

<span data-ttu-id="c99e2-142">`IHostingStartup` 実装のアセンブリ ファイルは、アプリの *bin* に配置されるか、[ランタイム ストア](/dotnet/core/deploying/runtime-store)に配置される必要があります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="c99e2-143">ユーザーごとに使用する場合は、ユーザー プロファイルのプロファイルのランタイム ストアにアセンブリを配置します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="c99e2-144">グローバルに使用する場合は、.NET Core インストールのランタイム ストアにアセンブリを配置します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="c99e2-145">アセンブリをランタイム ストアに配置するときに、シンボル ファイルも配置されますが、拡張機能を機能させるためには必要ありません。</span><span class="sxs-lookup"><span data-stu-id="c99e2-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="c99e2-146">**依存関係ファイルの配置**</span><span class="sxs-lookup"><span data-stu-id="c99e2-146">**Place the dependencies file**</span></span>

<span data-ttu-id="c99e2-147">実装の *\*.deps.json* ファイルは、アセンブリの場所に配置されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="c99e2-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="c99e2-148">ユーザーごとに使用する場合は、ユーザー プロファイルの `.dotnet` 設定の `additonalDeps` フォルダーにファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="c99e2-149">グローバルに使用する場合は、.NET Core インストールの `additonalDeps` フォルダーにファイルを配置します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="c99e2-150">バージョン (`2.0.0`) には、ターゲット アプリが使用する共有ランタイムのバージョンを反映することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c99e2-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="c99e2-151">共有ランタイムは、*\*.runtimeconfig.json* ファイルに示されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="c99e2-152">サンプル アプリでは、共有ランタイムは、*HostingStartupSample.runtimeconfig.json* ファイルで指定されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="c99e2-153">**環境変数の設定**</span><span class="sxs-lookup"><span data-stu-id="c99e2-153">**Set environment variables**</span></span>

<span data-ttu-id="c99e2-154">拡張機能を使用するアプリのコンテキストで次の環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="c99e2-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="c99e2-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="c99e2-156">ホスティング スタートアップ アセンブリのみが、`HostingStartupAttribute` に対してスキャンされます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="c99e2-157">実装のアセンブリ名は、この環境変数で提供されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="c99e2-158">サンプル アプリでは、この値を `StartupDiagnostics` に設定します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="c99e2-159">また、この値は「[Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies)」(ホスティング スタートアップ アセンブリ) のホスト構成設定を使用して設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="c99e2-160">DOTNET\_ADDITIONAL\_DEPS</span><span class="sxs-lookup"><span data-stu-id="c99e2-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="c99e2-161">実装の *\*.deps.json* ファイルの場所。</span><span class="sxs-lookup"><span data-stu-id="c99e2-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="c99e2-162">ファイルがユーザーごとに使用するためにユーザー プロファイルの *.dotnet* フォルダーに配置される場合は、次を使用します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="c99e2-163">ファイルがグローバルに使用するために .NET Core インストールに配置される場合は、ファイルに完全なパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="c99e2-164">サンプル アプリではこの値を次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="c99e2-165">さまざまなオペレーティング システムの環境変数を設定する方法の例については、[複数の環境の使用](xref:fundamentals/environments)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c99e2-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="c99e2-166">サンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="c99e2-166">Sample app</span></span>

<span data-ttu-id="c99e2-167">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([ダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) では、`IHostingStartup` を使用して診断ツールを作成します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="c99e2-168">ツールはスタートアップ時に 2 つのミドルウェアをアプリに追加し、これにより診断情報が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="c99e2-169">登録サービス</span><span class="sxs-lookup"><span data-stu-id="c99e2-169">Registered services</span></span>
* <span data-ttu-id="c99e2-170">アドレス: スキーム、ホスト、パス ベース、パス、クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="c99e2-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="c99e2-171">接続: リモート IP、リオート ポート、ローカル IP、ローカル ポート、クライアント証明書</span><span class="sxs-lookup"><span data-stu-id="c99e2-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="c99e2-172">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="c99e2-172">Request headers</span></span>
* <span data-ttu-id="c99e2-173">環境変数</span><span class="sxs-lookup"><span data-stu-id="c99e2-173">Environment variables</span></span>

<span data-ttu-id="c99e2-174">サンプルを実行するには</span><span class="sxs-lookup"><span data-stu-id="c99e2-174">To run the sample:</span></span>

1. <span data-ttu-id="c99e2-175">スタートアップ診断プロジェクトでは、[PowerShell](/powershell/scripting/powershell-scripting) を使用して、その *StartupDiagnostics.deps.json* ファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="c99e2-176">既定では、PowerShell は Windows 7 SP1 と Windows Server 2008 R2 SP1 以降の Windows OS 上にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="c99e2-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="c99e2-177">他のプラットフォームで PowerShell を取得するには、「[Windows PowerShell のインストール](/powershell/scripting/setup/installing-windows-powershell)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c99e2-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="c99e2-178">スタートアップ診断プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="c99e2-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="c99e2-179">プロジェクト ファイル内のビルド ターゲット:</span><span class="sxs-lookup"><span data-stu-id="c99e2-179">A build target in the project file:</span></span>
   * <span data-ttu-id="c99e2-180">アセンブリ ファイルとシンボル ファイルをユーザー プロファイルのランタイム ストアに移動する。</span><span class="sxs-lookup"><span data-stu-id="c99e2-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="c99e2-181">PowerShell スクリプトをトリガーして、*StartupDiagnostics.deps.json* ファイルを変更する。</span><span class="sxs-lookup"><span data-stu-id="c99e2-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="c99e2-182">*StartupDiagnostics.deps.json* ファイルをユーザー プロファイルの `additionalDeps` フォルダーに移動する。</span><span class="sxs-lookup"><span data-stu-id="c99e2-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="c99e2-183">環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-183">Set the environment variables:</span></span>
    * <span data-ttu-id="c99e2-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="c99e2-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="c99e2-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="c99e2-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="c99e2-186">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-186">Run the sample app.</span></span>
5. <span data-ttu-id="c99e2-187">`/services` エンドポイントを要求して、アプリの登録サービスを参照します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="c99e2-188">`/diag` エンドポイントを要求して、診断情報を参照します。</span><span class="sxs-lookup"><span data-stu-id="c99e2-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
