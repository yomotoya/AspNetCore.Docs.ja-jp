---
title: "ASP.NET Core のプラットフォーム固有の構成を使用してアプリ機能を追加します。"
author: guardrex
description: "IHostingStartup 実装を使用して外部のアセンブリから ASP.NET Core アプリに機能を追加する方法を検出します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/platform-specific-configuration
ms.openlocfilehash: 2663cd1e05be9e8695966df959082e6e574d0b4a
ms.sourcegitcommit: 809ee4baf8bf7b4cae9e366ecae29de1037d2bbb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/15/2018
---
# <a name="add-app-features-using-a-platform-specific-configuration-in-aspnet-core"></a><span data-ttu-id="d170d-103">ASP.NET Core のプラットフォーム固有の構成を使用してアプリ機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="d170d-103">Add app features using a platform-specific configuration in ASP.NET Core</span></span>

<span data-ttu-id="d170d-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d170d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d170d-105">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)実装により、アプリの外部で外部アセンブリからの起動時にアプリに機能を追加する`Startup`クラスです。</span><span class="sxs-lookup"><span data-stu-id="d170d-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="d170d-106">たとえば、外部ツール ライブラリを使用できます、`IHostingStartup`追加の構成プロバイダーやアプリへのサービスを提供する実装。</span><span class="sxs-lookup"><span data-stu-id="d170d-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="d170d-107">`IHostingStartup` *ASP.NET Core 2.0 で使用できる以降。*</span><span class="sxs-lookup"><span data-stu-id="d170d-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="d170d-108">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d170d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="d170d-109">読み込まれたホスティング スタートアップ アセンブリを検出します。</span><span class="sxs-lookup"><span data-stu-id="d170d-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="d170d-110">ライブラリや、アプリによって読み込まスタートアップ アセンブリのホストを探索するには、ログ記録を有効にし、アプリケーションのログを確認します。</span><span class="sxs-lookup"><span data-stu-id="d170d-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="d170d-111">アセンブリを読み込むときに発生するエラーが記録されます。</span><span class="sxs-lookup"><span data-stu-id="d170d-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="d170d-112">読み込まれたホスティング スタートアップ アセンブリがデバッグ レベルで記録され、すべてのエラーが記録されます。</span><span class="sxs-lookup"><span data-stu-id="d170d-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="d170d-113">サンプル アプリの読み取り、 [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey)に、`string`配列し、アプリのインデックス ページに結果を表示します。</span><span class="sxs-lookup"><span data-stu-id="d170d-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="d170d-114">ホストしているスタートアップ アセンブリの自動読み込みを無効にします。</span><span class="sxs-lookup"><span data-stu-id="d170d-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="d170d-115">ホストしているスタートアップ アセンブリの自動読み込みを無効にする 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="d170d-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="d170d-116">設定、[をホストしているスタートアップを防ぐ](xref:fundamentals/hosting#prevent-hosting-startup)構成設定をホストします。</span><span class="sxs-lookup"><span data-stu-id="d170d-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="d170d-117">設定、`ASPNETCORE_preventHostingStartup`環境変数。</span><span class="sxs-lookup"><span data-stu-id="d170d-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="d170d-118">ときに、ホストの設定または環境変数のいずれかに設定されている`true`または`1`ホスティング、スタートアップ アセンブリは自動的に読み込まれていません。</span><span class="sxs-lookup"><span data-stu-id="d170d-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="d170d-119">どちらも設定されている場合、ホストの設定は、動作を制御します。</span><span class="sxs-lookup"><span data-stu-id="d170d-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="d170d-120">ホスト設定または環境変数を使用してホストのスタートアップ アセンブリの無効化、グローバルに無効にし、アプリの複数の機能を無効にすることがあります。</span><span class="sxs-lookup"><span data-stu-id="d170d-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="d170d-121">現在、ライブラリが独自の構成オプションを提供しない限り、ライブラリによって追加されたホスト スタートアップ アセンブリを選択的に無効にすることはできません。</span><span class="sxs-lookup"><span data-stu-id="d170d-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="d170d-122">将来のリリースはホスティング スタートアップ アセンブリを選択的に無効にする機能を提供 (を参照してください[GitHub 発行 aspnet/ホスティング #1243](https://github.com/aspnet/Hosting/pull/1243))。</span><span class="sxs-lookup"><span data-stu-id="d170d-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="d170d-123">IHostingStartup 機能を実装します。</span><span class="sxs-lookup"><span data-stu-id="d170d-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="d170d-124">アセンブリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d170d-124">Create the assembly</span></span>

<span data-ttu-id="d170d-125">`IHostingStartup`機能は、エントリ ポイントがないコンソール アプリに基づいて、アセンブリとして展開します。</span><span class="sxs-lookup"><span data-stu-id="d170d-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="d170d-126">アセンブリ参照、 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="d170d-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](platform-specific-configuration/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="d170d-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)属性の実装としてクラスを識別する`IHostingStartup`を読み込み、構築するときに実行、 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost)です。</span><span class="sxs-lookup"><span data-stu-id="d170d-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="d170d-128">名前空間は、次の例では、 `StartupFeature`、クラスと`StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="d170d-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="d170d-129">クラスを実装する`IHostingStartup`です。</span><span class="sxs-lookup"><span data-stu-id="d170d-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="d170d-130">クラスの[構成](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)メソッドを使用、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)アプリに機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="d170d-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](platform-specific-configuration/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="d170d-131">作成するときに、`IHostingStartup`プロジェクト、依存関係ファイル (*\*. deps.json*) 設定、`runtime`するアセンブリの場所、 *bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d170d-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="d170d-132">ファイルの一部のみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d170d-132">Only part of the file is shown.</span></span> <span data-ttu-id="d170d-133">例では、アセンブリ名が`StartupFeature`です。</span><span class="sxs-lookup"><span data-stu-id="d170d-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="d170d-134">更新プログラムの依存関係ファイル</span><span class="sxs-lookup"><span data-stu-id="d170d-134">Update the dependencies file</span></span>

<span data-ttu-id="d170d-135">実行時の位置、  *\*. deps.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="d170d-136">アクティブな機能を`runtime`要素は、機能のランタイム アセンブリの場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d170d-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="d170d-137">プレフィックス、`runtime`場所`lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="d170d-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](platform-specific-configuration/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="d170d-138">サンプル アプリでの変更、  *\*. deps.json*はファイルの実行によって、 [PowerShell](/powershell/scripting/powershell-scripting)スクリプト。</span><span class="sxs-lookup"><span data-stu-id="d170d-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="d170d-139">PowerShell スクリプトはプロジェクト ファイルでビルド ターゲットによって自動的にトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="d170d-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="d170d-140">機能のアクティブ化</span><span class="sxs-lookup"><span data-stu-id="d170d-140">Feature activation</span></span>

<span data-ttu-id="d170d-141">**アセンブリ ファイルを配置します。**</span><span class="sxs-lookup"><span data-stu-id="d170d-141">**Place the assembly file**</span></span>

<span data-ttu-id="d170d-142">`IHostingStartup`実装のアセンブリ ファイルである必要があります*bin*-アプリの展開またはに配置されて、[ランタイム ストア](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="d170d-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="d170d-143">ユーザーごとの使用時にユーザー プロファイルのランタイム ストアにアセンブリを配置します。</span><span class="sxs-lookup"><span data-stu-id="d170d-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="d170d-144">グローバルに使用できる .NET Core のインストールの実行時のストアにアセンブリを配置します。</span><span class="sxs-lookup"><span data-stu-id="d170d-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="d170d-145">アセンブリをランタイム ストアを展開する場合、シンボル ファイルはも展開することがありますが、機能を使用するために必要ではありません。</span><span class="sxs-lookup"><span data-stu-id="d170d-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="d170d-146">**依存関係ファイルを配置します。**</span><span class="sxs-lookup"><span data-stu-id="d170d-146">**Place the dependencies file**</span></span>

<span data-ttu-id="d170d-147">実装の *\*. deps.json*ファイルがアクセスできる場所である必要があります。</span><span class="sxs-lookup"><span data-stu-id="d170d-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="d170d-148">ユーザーごとの使用するためにファイルを置きます、`additonalDeps`ユーザー プロファイルのフォルダー`.dotnet`設定。</span><span class="sxs-lookup"><span data-stu-id="d170d-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="d170d-149">グローバルを使用して、配置内のファイル、 `additonalDeps` .NET Core のインストール フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d170d-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="d170d-150">バージョンをメモ`2.0.0`対象とするアプリケーションで使用される共有のランタイムのバージョンを反映します。</span><span class="sxs-lookup"><span data-stu-id="d170d-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="d170d-151">共有のランタイムが示すように、  *\*. runtimeconfig.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="d170d-152">指定された共有のランタイムのサンプル アプリで、 *HostingStartupSample.runtimeconfig.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="d170d-153">**環境変数な設定**</span><span class="sxs-lookup"><span data-stu-id="d170d-153">**Set environment variables**</span></span>

<span data-ttu-id="d170d-154">機能を使用するアプリのコンテキストでは、次の環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="d170d-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="d170d-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="d170d-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="d170d-156">ホスティングのスタートアップ アセンブリだけがスキャン、`HostingStartupAttribute`です。</span><span class="sxs-lookup"><span data-stu-id="d170d-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="d170d-157">実装のアセンブリ名は、この環境変数で提供されます。</span><span class="sxs-lookup"><span data-stu-id="d170d-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="d170d-158">サンプル アプリでは、この値を設定`StartupDiagnostics`です。</span><span class="sxs-lookup"><span data-stu-id="d170d-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="d170d-159">使用して、値を設定することも、[をホストしているスタートアップ アセンブリ](xref:fundamentals/hosting#hosting-startup-assemblies)構成設定をホストします。</span><span class="sxs-lookup"><span data-stu-id="d170d-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="d170d-160">DOTNET\_追加\_DEPS</span><span class="sxs-lookup"><span data-stu-id="d170d-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="d170d-161">実装の場所 *\*. deps.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="d170d-162">ユーザー プロファイルのかどうか、ファイルを配置*.dotnet*ユーザーごとの使用フォルダー。</span><span class="sxs-lookup"><span data-stu-id="d170d-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="d170d-163">ファイルがグローバルに使用できる .NET Core のインストールに配置されている場合は、ファイルへの完全パスを提供します。</span><span class="sxs-lookup"><span data-stu-id="d170d-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="d170d-164">サンプル アプリにこの値を設定します。</span><span class="sxs-lookup"><span data-stu-id="d170d-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="d170d-165">さまざまなオペレーティング システムの環境変数を設定する方法の例については、次を参照してください。[複数の環境で作業](xref:fundamentals/environments)です。</span><span class="sxs-lookup"><span data-stu-id="d170d-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="d170d-166">サンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="d170d-166">Sample app</span></span>

<span data-ttu-id="d170d-167">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample)) を使用して`IHostingStartup`診断ツールを作成します。</span><span class="sxs-lookup"><span data-stu-id="d170d-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="d170d-168">ツールでは、2 つの middlewares を診断情報を提供するための起動時に、アプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="d170d-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="d170d-169">登録されているサービス</span><span class="sxs-lookup"><span data-stu-id="d170d-169">Registered services</span></span>
* <span data-ttu-id="d170d-170">アドレス: スキーム、ホスト、パスのベース パス、クエリ文字列</span><span class="sxs-lookup"><span data-stu-id="d170d-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="d170d-171">接続ですリモート IP、ローカル IP は、リモート ポートは、ローカル ポート、クライアント証明書。</span><span class="sxs-lookup"><span data-stu-id="d170d-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="d170d-172">要求ヘッダー</span><span class="sxs-lookup"><span data-stu-id="d170d-172">Request headers</span></span>
* <span data-ttu-id="d170d-173">環境変数</span><span class="sxs-lookup"><span data-stu-id="d170d-173">Environment variables</span></span>

<span data-ttu-id="d170d-174">このサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="d170d-174">To run the sample:</span></span>

1. <span data-ttu-id="d170d-175">診断スタートアップ プロジェクトを使用して[PowerShell](/powershell/scripting/powershell-scripting)を変更するその*StartupDiagnostics.deps.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="d170d-176">Windows 7 SP1 および Windows Server 2008 R2 SP1 以降の Windows OS 上の既定では、PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="d170d-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="d170d-177">他のプラットフォームで PowerShell を入手する[Windows PowerShell をインストールする](/powershell/scripting/setup/installing-windows-powershell)です。</span><span class="sxs-lookup"><span data-stu-id="d170d-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="d170d-178">診断スタートアップ プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d170d-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="d170d-179">プロジェクト ファイルでビルド ターゲット:</span><span class="sxs-lookup"><span data-stu-id="d170d-179">A build target in the project file:</span></span>
   * <span data-ttu-id="d170d-180">アセンブリに移動し、シンボル ファイルをユーザー プロファイルのランタイム ストア。</span><span class="sxs-lookup"><span data-stu-id="d170d-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="d170d-181">トリガーを変更する PowerShell スクリプト、 *StartupDiagnostics.deps.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="d170d-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="d170d-182">移動、 *StartupDiagnostics.deps.json*をユーザー プロファイルのファイル`additionalDeps`フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d170d-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="d170d-183">環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="d170d-183">Set the environment variables:</span></span>
    * <span data-ttu-id="d170d-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="d170d-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="d170d-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="d170d-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="d170d-186">サンプル アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="d170d-186">Run the sample app.</span></span>
5. <span data-ttu-id="d170d-187">要求、`/services`して、アプリのエンドポイントがサービスを登録します。</span><span class="sxs-lookup"><span data-stu-id="d170d-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="d170d-188">要求、`/diag`診断情報を表示するエンドポイント。</span><span class="sxs-lookup"><span data-stu-id="d170d-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
