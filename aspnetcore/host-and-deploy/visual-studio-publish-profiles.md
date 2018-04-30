---
title: Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。
author: rick-anderson
description: 作成する方法、発行プロファイルを Visual Studio で、さまざまな対象の ASP.NET Core アプリ展開を管理するために使用します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="eff8f-103">Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="eff8f-104">作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eff8f-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eff8f-105">このドキュメントに焦点を当てます Visual Studio 2017 作成および使用すると、発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="eff8f-106">Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="eff8f-107">Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="eff8f-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="eff8f-108">次のプロジェクト ファイルは、コマンドを使用して作成された`dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="eff8f-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eff8f-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eff8f-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eff8f-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eff8f-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="eff8f-111">`<Project>`要素の`Sdk`属性は、次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="eff8f-112">プロパティ ファイルからインポート *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*先頭にします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="eff8f-113">最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="eff8f-114">(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="eff8f-115">`Microsoft.NET.Sdk.Web` SDK によって異なります。</span><span class="sxs-lookup"><span data-stu-id="eff8f-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="eff8f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="eff8f-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="eff8f-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="eff8f-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="eff8f-118">これは、次のプロパティとをインポートするターゲットによってが発生します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="eff8f-119">*$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="eff8f-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="eff8f-120">*$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="eff8f-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="eff8f-121">*$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="eff8f-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="eff8f-122">*$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="eff8f-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="eff8f-123">ターゲットのインポートの右側を使用する発行方法に基づいてターゲットの設定を発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="eff8f-124">MSBuild または Visual Studio プロジェクトが読み込まれると、次の高度な操作が行われます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="eff8f-125">プロジェクトをビルドする</span><span class="sxs-lookup"><span data-stu-id="eff8f-125">Build project</span></span>
* <span data-ttu-id="eff8f-126">発行するファイルを計算する</span><span class="sxs-lookup"><span data-stu-id="eff8f-126">Compute files to publish</span></span>
* <span data-ttu-id="eff8f-127">ターゲットにファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="eff8f-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="eff8f-128">プロジェクト項目を比較する</span><span class="sxs-lookup"><span data-stu-id="eff8f-128">Compute project items</span></span>

<span data-ttu-id="eff8f-129">プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="eff8f-130">`item type` 属性によって、ファイルの処理方法が決まります。</span><span class="sxs-lookup"><span data-stu-id="eff8f-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="eff8f-131">既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="eff8f-132">`Compile` 項目一覧のファイルがコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="eff8f-133">`Content`項目リストには、ビルドの出力だけでなく公開されているファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="eff8f-134">既定では、ファイル、パターンに一致する`wwwroot/**`に含まれる、`Content`項目。</span><span class="sxs-lookup"><span data-stu-id="eff8f-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="eff8f-135">`wwwroot/\*\*` [グロブ パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)内のすべてのファイルと一致する、 *wwwroot*フォルダー**と**サブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="eff8f-136">発行の一覧にファイルを明示的に追加するで直接ファイルを追加、 *.csproj*に示すように、ファイル[ファイルを含める](#include-files)です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="eff8f-137">選択すると、**発行**Visual Studio またはコマンドラインから発行するときにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="eff8f-138">プロパティ/項目が計算されます (ビルドに必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="eff8f-139">**Visual Studio のみ**: NuGet パッケージを復元します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="eff8f-140">(CLI では、ユーザーが明示的に復元する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="eff8f-141">プロジェクトがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-141">The project builds.</span></span>
* <span data-ttu-id="eff8f-142">発行項目が計算されます (発行に必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="eff8f-143">プロジェクトを発行 (計算されたファイルは、発行先にコピーされます)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="eff8f-144">ASP.NET Core プロジェクトの参照と`Microsoft.NET.Sdk.Web`プロジェクト ファイルで、 *app_offline.htm*ファイルは、web アプリのディレクトリのルートに配置します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="eff8f-145">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="eff8f-146">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="eff8f-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="eff8f-147">基本的なコマンド ライン発行</span><span class="sxs-lookup"><span data-stu-id="eff8f-147">Basic command-line publishing</span></span>

<span data-ttu-id="eff8f-148">コマンド ライン パブリッシングでは、.NET Core でサポートされているすべてのプラットフォームでは機能し、Visual Studio は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="eff8f-149">以下のサンプルで、 [dotnet 発行](/dotnet/core/tools/dotnet-publish)プロジェクト ディレクトリからコマンドを実行 (が含まれています、 *.csproj*ファイル)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="eff8f-150">指定しない場合、プロジェクト フォルダー内に明示的に渡すプロジェクト ファイルのパスにします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="eff8f-151">例えば:</span><span class="sxs-lookup"><span data-stu-id="eff8f-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="eff8f-152">Web アプリを作成して発行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eff8f-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="eff8f-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eff8f-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="eff8f-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="eff8f-155">[Dotnet 発行](/dotnet/core/tools/dotnet-publish)コマンドには、次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="eff8f-156">既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="eff8f-157">既定の`$(Configuration)`は*デバッグ*です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="eff8f-158">上記のサンプル、`<TargetFramework>`は`netcoreapp2.0`します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="eff8f-159">`dotnet publish -h` では、発行のヘルプ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="eff8f-160">次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="eff8f-161">[Dotnet 発行](/dotnet/core/tools/dotnet-publish)コマンドを呼び出し、MSBuild を呼び出す、`Publish`ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="eff8f-162">渡されるパラメーター `dotnet publish` MSBuild に渡されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="eff8f-163">`-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="eff8f-164">`-o` パラメーターは `OutputPath` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="eff8f-165">MSBuild プロパティは、次の形式のいずれかを使用して渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="eff8f-166">次のコマンドは、`Release` ビルドをネットワーク共有に発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="eff8f-167">ネットワーク共有は、スラッシュを使用して指定します (*//r8/*).NET Core がサポートされるすべてのプラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="eff8f-168">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="eff8f-169">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="eff8f-170">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="eff8f-171">プロファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="eff8f-171">Publish profiles</span></span>

<span data-ttu-id="eff8f-172">このセクションでは、Visual Studio 2017 を使用して、発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="eff8f-173">作成した後は、Visual Studio またはコマンドラインからの発行を使用できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="eff8f-174">発行プロファイルには、発行プロセスが簡略化され、任意の数のプロファイルが存在できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="eff8f-175">次のパスのいずれかを選択して、Visual Studio で発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="eff8f-176">ソリューション エクスプ ローラーでプロジェクトを右クリックし **発行**です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="eff8f-177">選択**発行&lt;project_name&gt;** から、**ビルド**メニュー。</span><span class="sxs-lookup"><span data-stu-id="eff8f-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="eff8f-178">**発行**アプリ容量ページのタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="eff8f-179">プロジェクトでは、発行プロファイルがない、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![アプリの容量がページの [発行] タブ](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="eff8f-181">ときに**フォルダー**は選択すると、パブリッシュされたメディアを保存するフォルダー パスを指定します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="eff8f-182">既定のフォルダーは*bin\Release\PublishOutput*です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="eff8f-183">クリックして、**プロファイルの作成**[完了] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="eff8f-184">発行プロファイルを作成した後、**発行** タブの変更。</span><span class="sxs-lookup"><span data-stu-id="eff8f-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="eff8f-185">新しく作成したプロファイルは、ドロップダウン リストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="eff8f-186">をクリックして**新しいプロファイルを作成**を別の新しいプロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-186">Click **Create new profile** to create another new profile.</span></span>

![FolderProfile を表示するアプリの容量がページの [発行] タブ](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="eff8f-188">発行ウィザードは、次の発行ターゲットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="eff8f-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="eff8f-189">Azure App Service</span></span>
* <span data-ttu-id="eff8f-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="eff8f-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="eff8f-191">IIS、FTP など (の任意の web サーバー)</span><span class="sxs-lookup"><span data-stu-id="eff8f-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="eff8f-192">フォルダー</span><span class="sxs-lookup"><span data-stu-id="eff8f-192">Folder</span></span>
* <span data-ttu-id="eff8f-193">プロファイルのインポート</span><span class="sxs-lookup"><span data-stu-id="eff8f-193">Import Profile</span></span>

<span data-ttu-id="eff8f-194">詳細については、次を参照してください。[適切な発行オプション](/visualstudio/ide/not-in-toc/web-publish-options)です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="eff8f-195">Visual Studio が、発行プロファイルを作成するときに、*プロパティ/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="eff8f-196">*.Pubxml*ファイルは、MSBuild とファイルが含まれています構成設定を発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="eff8f-197">このファイルは、ビルドのカスタマイズし、発行プロセスに変更することができます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="eff8f-198">このファイルは、発行プロファイルで読み取られます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-198">This file is read by the publishing process.</span></span> <span data-ttu-id="eff8f-199">`<LastUsedBuildConfiguration>` 特殊なため、グローバル プロパティであり、ビルドでインポートするすべてのファイルではないはずです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="eff8f-200">参照してください[MSBuild: 構成プロパティを設定する方法](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="eff8f-201">ターゲットの Azure にパブリッシュする場合、 *.pubxml*ファイルには、Azure サブスクリプション識別子が含まれています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="eff8f-202">そのターゲット型とソース管理にこのファイルを追加することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="eff8f-203">Azure 以外のターゲットに発行するときは、チェックインする安全、 *.pubxml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="eff8f-204">上 (パブリッシュ用のパスワード) などの機密情報は暗号化されて、個々 のユーザー/コンピューター レベル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="eff8f-205">格納されている、*プロパティ/PublishProfiles/&lt;profile_name&gt;. pubxml.user*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="eff8f-206">このファイルでは、機密情報を格納できるため、ソース管理にチェックインするべきではありません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="eff8f-207">ASP.NET Core 上の web アプリを公開する方法の詳細については、次を参照してください。[ホストを展開および](xref:host-and-deploy/index)です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="eff8f-208">MSBuild タスクおよび ASP.NET Core アプリケーションを発行するために必要なターゲットは、オープン ソースでhttps://github.com/aspnet/websdkです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="eff8f-209">`dotnet publish` MSDeploy のフォルダーを使用し、 [Kudu](https://github.com/projectkudu/kudu/wiki)プロファイルの発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="eff8f-210">フォルダー (クロスプラット フォームで機能):</span><span class="sxs-lookup"><span data-stu-id="eff8f-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="eff8f-211">MSDeploy (現在こののみで機能します Windows MSDeploy クロスプラット フォームのないため)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="eff8f-212">MSDeploy パッケージ (現在こののみで機能します Windows MSDeploy クロスプラット フォームのないため)。</span><span class="sxs-lookup"><span data-stu-id="eff8f-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="eff8f-213">上記のサンプルで**しない**渡す`deployonbuild`に`dotnet publish`です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="eff8f-214">詳細については、次を参照してください。 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="eff8f-215">`dotnet publish` 任意のプラットフォームから Azure に発行する Kudu Api をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="eff8f-216">Visual Studio では、サポートが Kudu Api でサポートされて WebSDK クロスプラット フォームが Azure に発行を公開します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="eff8f-217">発行プロファイルを追加、*プロパティ/PublishProfiles*次のコンテンツ フォルダー。</span><span class="sxs-lookup"><span data-stu-id="eff8f-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="eff8f-218">発行の内容を zip 圧縮し、Kudu Api を使用して Azure に発行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="eff8f-219">発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="eff8f-220">という名前のプロファイルで発行するときに*FolderProfile*、次のコマンドのいずれかを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="eff8f-221">呼び出すときに[dotnet ビルド](/dotnet/core/tools/dotnet-build)、呼び出す`msbuild`ビルドを実行してプロセスを発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="eff8f-222">いずれかを呼び出す`dotnet build`または`msbuild`フォルダー プロファイルに渡す場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="eff8f-223">Windows 上で直接には、MSBuild を呼び出し、MSBuild の .NET Framework のバージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="eff8f-224">MSDeploy の発行は、現在 Windows コンピューターに制限されています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="eff8f-225">フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="eff8f-226">フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。</span><span class="sxs-lookup"><span data-stu-id="eff8f-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="eff8f-227">フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="eff8f-228">次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="eff8f-229">`<LastUsedBuildConfiguration>` は `Release` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="eff8f-230">Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="eff8f-231">`<LastUsedBuildConfiguration>`構成プロパティは特別であり、インポートされた MSBuild ファイル内でオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="eff8f-232">コマンドラインからこのプロパティをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="eff8f-233">.NET Core CLI を使用します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="eff8f-234">MSBuild の使用:</span><span class="sxs-lookup"><span data-stu-id="eff8f-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="eff8f-235">コマンド ラインから MSDeploy エンドポイントに発行する</span><span class="sxs-lookup"><span data-stu-id="eff8f-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="eff8f-236">発行は、.NET Core CLI または MSBuild を使用して実現できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="eff8f-237">`dotnet publish` は .NET Core のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="eff8f-238">`msbuild`コマンドには、.NET Framework は、Windows 環境に制限が必要です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="eff8f-239">MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="eff8f-240">次の例では、ASP.NET Core web アプリを作成 (を使用して`dotnet new mvc`)、Visual Studio で Azure の発行プロファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="eff8f-241">実行`msbuild`から、 **VS 2017 用開発者コマンド プロンプト**です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="eff8f-242">開発者コマンド プロンプトが、正しい*msbuild.exe*一部 MSBuild 変数セットをそのパスにします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="eff8f-243">MSBuild は次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="eff8f-244">取得、`Password`から、 *\<発行名 >。PublishSettings*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="eff8f-245">ダウンロード、*です。PublishSettings*からいずれかのファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="eff8f-246">ソリューション エクスプ ローラー: は、この Web アプリでを右クリックし、選択**発行プロファイルのダウンロード**です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="eff8f-247">Azure ポータル: をクリックして**Get 発行プロファイル**Web app の**概要**パネルです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="eff8f-248">`Username` は発行プロファイルで確認できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="eff8f-249">次のサンプルは、 *Web11112 - Web Deploy*発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="eff8f-250">ファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-250">Exclude files</span></span>

<span data-ttu-id="eff8f-251">ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="eff8f-252">`msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="eff8f-253">たとえば、次`<Content>`要素には、すべてのテキストが含まれません (*.txt*) ファイルから、 *wwwroot/コンテンツ*フォルダーとそのすべてのサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="eff8f-254">上記のマークアップは、発行プロファイルに追加することができます、または *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="eff8f-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="eff8f-255">*.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="eff8f-256">次`<MsDeploySkipRules>`要素からすべてのファイルを除外する、 *wwwroot/コンテンツ*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="eff8f-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="eff8f-257">`<MsDeploySkipRules>` 削除されません、*スキップ*配置サイトからターゲットです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="eff8f-258">`<Content>` 対象となるファイルとフォルダーは、配置サイトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="eff8f-259">たとえば、展開された web アプリケーションが、次のファイルとします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="eff8f-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eff8f-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="eff8f-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eff8f-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="eff8f-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eff8f-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="eff8f-263">場合、次`<MsDeploySkipRules>`要素が追加される、展開のサイトでそれらのファイルは削除はありません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="eff8f-264">前述の`<MsDeploySkipRules>`要素を防ぐ、*スキップ*ファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="eff8f-265">それらのファイルは、配置されると、削除されません。</span><span class="sxs-lookup"><span data-stu-id="eff8f-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="eff8f-266">次`<Content>`要素は、展開のサイトで対象となるファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="eff8f-267">コマンドライン配置を使用して、前述の`<Content>`要素には、次の出力が得られます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="eff8f-268">インクルード ファイル</span><span class="sxs-lookup"><span data-stu-id="eff8f-268">Include files</span></span>

<span data-ttu-id="eff8f-269">次のマークアップが含まれています、*イメージ*フォルダーをプロジェクト ディレクトリの外部、 *wwwroot/イメージ*発行サイトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="eff8f-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="eff8f-270">このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="eff8f-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="eff8f-271">追加された場合、 *.csproj*ファイルが含まれているプロジェクト内の発行プロファイルごとにします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="eff8f-272">次の強調表示されているマークアップは、この方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="eff8f-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="eff8f-273">プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="eff8f-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="eff8f-274">*wwwroot\Content* フォルダーを除外します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="eff8f-275">*Views\Home\About2.cshtml* を除外します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="eff8f-276">他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="eff8f-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="eff8f-277">発行の前または後にターゲットを実行する</span><span class="sxs-lookup"><span data-stu-id="eff8f-277">Run a target before or after publishing</span></span>

<span data-ttu-id="eff8f-278">組み込み`BeforePublish`と`AfterPublish`発行先の前後に、ターゲットがターゲットを実行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="eff8f-279">前に、と発行後に、コンソール メッセージをログに発行プロファイルを次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="eff8f-280">信頼されていない証明書を使用してサーバーに発行します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="eff8f-281">追加、`<AllowUntrustedCertificate>`の値を持つプロパティ`True`発行プロファイルに。</span><span class="sxs-lookup"><span data-stu-id="eff8f-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="eff8f-282">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="eff8f-282">The Kudu service</span></span>

<span data-ttu-id="eff8f-283">Azure App Service web アプリの展開で、ファイルを表示する、 [Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)です。</span><span class="sxs-lookup"><span data-stu-id="eff8f-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="eff8f-284">追加、 `scm` web アプリ名をトークンです。</span><span class="sxs-lookup"><span data-stu-id="eff8f-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="eff8f-285">例えば:</span><span class="sxs-lookup"><span data-stu-id="eff8f-285">For example:</span></span>

| <span data-ttu-id="eff8f-286">URL</span><span class="sxs-lookup"><span data-stu-id="eff8f-286">URL</span></span>                                    | <span data-ttu-id="eff8f-287">結果</span><span class="sxs-lookup"><span data-stu-id="eff8f-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="eff8f-288">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="eff8f-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="eff8f-289">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="eff8f-289">Kudu service</span></span> |

<span data-ttu-id="eff8f-290">選択、[デバッグ コンソール](https://github.com/projectkudu/kudu/wiki/Kudu-console)メニュー項目を表示、編集、削除、またはファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eff8f-291">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="eff8f-291">Additional resources</span></span>

* <span data-ttu-id="eff8f-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) が IIS サーバーへの web サイトと web アプリの展開を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="eff8f-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="eff8f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): ファイルに問題、機能の展開を要求してください。</span><span class="sxs-lookup"><span data-stu-id="eff8f-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
