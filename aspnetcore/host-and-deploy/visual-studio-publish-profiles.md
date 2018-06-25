---
title: ASP.NET Core アプリを配置するための Visual Studio 発行プロファイル
author: rick-anderson
description: Visual Studio で発行プロファイルを作成し、それらを使用してさまざまなターゲットへの ASP.NET Core アプリの配置を管理する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 280599ab4b4f0a70d154cc4408e7232aaf766d8e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279559"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="d58c4-103">ASP.NET Core アプリを配置するための Visual Studio 発行プロファイル</span><span class="sxs-lookup"><span data-stu-id="d58c4-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="d58c4-104">作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d58c4-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d58c4-105">このドキュメントでは、Visual Studio 2017 を使用して、発行プロファイルを作成して使用する方法に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="d58c4-106">Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="d58c4-107">Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="d58c4-108">次のプロジェクト ファイルは、`dotnet new mvc` コマンドを使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="d58c4-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d58c4-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d58c4-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d58c4-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d58c4-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="d58c4-111">`<Project>` 要素の `Sdk` 属性は、次のタスクを実行します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="d58c4-112">最初に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* からプロパティ ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="d58c4-113">最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="d58c4-114">(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="d58c4-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="d58c4-115">`Microsoft.NET.Sdk.Web` SDK は、以下に依存しています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="d58c4-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="d58c4-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="d58c4-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="d58c4-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="d58c4-118">それにより、次のプロパティとターゲットのインポートが発生します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="d58c4-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="d58c4-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="d58c4-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="d58c4-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="d58c4-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="d58c4-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="d58c4-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="d58c4-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="d58c4-123">使用される発行方法に基づいて、発行ターゲットは適切なターゲット セットをインポートします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="d58c4-124">MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが発生します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="d58c4-125">プロジェクトをビルドする</span><span class="sxs-lookup"><span data-stu-id="d58c4-125">Build project</span></span>
* <span data-ttu-id="d58c4-126">発行するファイルを計算する</span><span class="sxs-lookup"><span data-stu-id="d58c4-126">Compute files to publish</span></span>
* <span data-ttu-id="d58c4-127">ターゲットにファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="d58c4-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="d58c4-128">プロジェクト項目を比較する</span><span class="sxs-lookup"><span data-stu-id="d58c4-128">Compute project items</span></span>

<span data-ttu-id="d58c4-129">プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="d58c4-130">`item type` 属性によって、ファイルの処理方法が決まります。</span><span class="sxs-lookup"><span data-stu-id="d58c4-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="d58c4-131">既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="d58c4-132">`Compile` 項目一覧のファイルがコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="d58c4-133">`Content` 項目一覧には、ビルドの出力に加え、発行されるファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="d58c4-134">既定で、`wwwroot/**` というパターンと一致するファイルが `Content` 項目に含まれます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="d58c4-135">`wwwroot/\*\*` [グロビング パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)は、*wwwroot* フォルダー**と**サブフォルダー内のすべてのファイルと一致します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="d58c4-136">発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#include-files)」で説明されているように *.csproj* ファイルに直接ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="d58c4-137">Visual Studio で **[発行]** ボタンを選択するか、コマンド ラインから発行すると、以下が実行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="d58c4-138">プロパティ/項目が計算されます (ビルドに必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="d58c4-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="d58c4-139">**Visual Studio のみ**: NuGet パッケージが復元されます </span><span class="sxs-lookup"><span data-stu-id="d58c4-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="d58c4-140">(CLI では、ユーザーが明示的に復元する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="d58c4-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="d58c4-141">プロジェクトがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-141">The project builds.</span></span>
* <span data-ttu-id="d58c4-142">発行項目が計算されます (発行に必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="d58c4-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="d58c4-143">プロジェクトが発行されます (計算されたファイルが発行先にコピーされます)。</span><span class="sxs-lookup"><span data-stu-id="d58c4-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="d58c4-144">ASP.NET Core プロジェクトは、プロジェクト ファイルの `Microsoft.NET.Sdk.Web` を参照して、*app_offline.htm* ファイルを Web アプリのディレクトリのルートに配置します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="d58c4-145">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="d58c4-146">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="d58c4-147">基本的なコマンド ラインからの発行</span><span class="sxs-lookup"><span data-stu-id="d58c4-147">Basic command-line publishing</span></span>

<span data-ttu-id="d58c4-148">コマンド ラインからの発行は、.NET Core をサポートするすべてのプラットフォームで機能し、Visual Studio は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="d58c4-149">次の例では、プロジェクト ディレクトリ (*.csproj* ファイルが含まれているディレクトリ) から [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="d58c4-150">現在のフォルダーがプロジェクト フォルダーではない場合は、プロジェクト ファイル パスにパスを明示的に渡します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="d58c4-151">例:</span><span class="sxs-lookup"><span data-stu-id="d58c4-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="d58c4-152">Web アプリを作成して発行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d58c4-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d58c4-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d58c4-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d58c4-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="d58c4-155">[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドは、次のような出力を生成します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="d58c4-156">既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="d58c4-157">`$(Configuration)` の既定値は *Debug* です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="d58c4-158">上記のサンプルでは、`<TargetFramework>` は `netcoreapp2.0`です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="d58c4-159">`dotnet publish -h` では、発行のヘルプ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="d58c4-160">次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="d58c4-161">[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドは MSBuild を呼び出し、MSBuild は `Publish` ターゲットを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="d58c4-162">`dotnet publish` に渡されたすべてのパラメーターが MSBuild に渡されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="d58c4-163">`-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="d58c4-164">`-o` パラメーターは `OutputPath` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="d58c4-165">MSBuild のプロパティは、次のいずれかの形式を使用して渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="d58c4-166">次のコマンドは、`Release` ビルドをネットワーク共有に発行します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="d58c4-167">ネットワーク共有は、スラッシュを使用して指定します (*//r8/*)。 .NET Core がサポートされるすべてのプラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="d58c4-168">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="d58c4-169">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="d58c4-170">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="d58c4-171">プロファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="d58c4-171">Publish profiles</span></span>

<span data-ttu-id="d58c4-172">このセクションでは、Visual Studio 2017 を使用して発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="d58c4-173">作成したら、Visual Studio またはコマンド ラインから発行できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="d58c4-174">発行プロファイルは発行プロセスを簡略化でき、任意の数のプロファイルが存在できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="d58c4-175">次のいずれかの方法で、Visual Studio で発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="d58c4-176">ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="d58c4-177">**[ビルド]** メニューの **[&lt;プロジェクト名&gt; の発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="d58c4-178">アプリ容量ページの **[発行]** タブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="d58c4-179">プロジェクトに発行プロファイルが含まれていない場合は、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![アプリ容量ページの [発行] タブ](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="d58c4-181">**[フォルダー]** を選択した場合は、発行された資産を保存するフォルダーのパスを指定します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="d58c4-182">既定のフォルダーは *bin\Release\PublishOutput* です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="d58c4-183">**[プロファイルの作成]** ボタンをクリックして完了します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="d58c4-184">発行プロファイルが作成されると、**[発行]** タブが変化します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="d58c4-185">新しく作成したプロファイルがドロップダウン リストに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="d58c4-186">別の新しいプロファイルを作成するには、**[新しいプロファイルの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-186">Click **Create new profile** to create another new profile.</span></span>

![FolderProfile を示しているアプリ容量ページの [発行] タブ](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="d58c4-188">発行ウィザードは、次の発行ターゲットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="d58c4-189">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d58c4-189">Azure App Service</span></span>
* <span data-ttu-id="d58c4-190">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="d58c4-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="d58c4-191">IIS、FTP など (すべての Web サーバー)</span><span class="sxs-lookup"><span data-stu-id="d58c4-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="d58c4-192">フォルダー</span><span class="sxs-lookup"><span data-stu-id="d58c4-192">Folder</span></span>
* <span data-ttu-id="d58c4-193">インポート プロファイル</span><span class="sxs-lookup"><span data-stu-id="d58c4-193">Import Profile</span></span>

<span data-ttu-id="d58c4-194">詳細については、「[状況に適した発行オプション](/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="d58c4-195">Visual Studio で発行プロファイルを作成すると、*Properties/PublishProfiles/&lt;発行名&gt;.pubxml* MSBuild ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="d58c4-196">*.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="d58c4-197">このファイルを変更して、ビルドと発行プロセスをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="d58c4-198">このファイルは、発行プロファイルで読み取られます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-198">This file is read by the publishing process.</span></span> <span data-ttu-id="d58c4-199">`<LastUsedBuildConfiguration>` はグローバル プロパティであり、ビルドにインポートされるファイルには含めるべきではない特別なプロパティです。</span><span class="sxs-lookup"><span data-stu-id="d58c4-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="d58c4-200">詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="d58c4-201">Azure ターゲットに発行する場合、*.pubxml* ファイルには、Azure サブスクリプション識別子が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="d58c4-202">そのターゲットの種類では、このファイルをソース管理に追加することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="d58c4-203">Azure 以外のターゲットに発行する場合は、*.pubxml* ファイルをチェックインするほうが安全です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="d58c4-204">機微な情報 (発行パスワードなど) は、個々のユーザー/コンピューター レベルで暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="d58c4-205">それは、*Properties/PublishProfiles/&lt;プロファイル名&gt;.pubxml.user* ファイルに格納されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="d58c4-206">このファイルには機微な情報が格納される可能性があるため、ソース コード管理にチェックインしないでください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="d58c4-207">ASP.NET Core で Web アプリを発行する方法の概要については、「[ホストと展開](xref:host-and-deploy/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="d58c4-208">ASP.NET Core アプリを発行するために必要な MSBuild タスクとターゲットは、オープン ソースです (https://github.com/aspnet/websdk)。</span><span class="sxs-lookup"><span data-stu-id="d58c4-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="d58c4-209">`dotnet publish` は、フォルダー、MSDeploy、および [Kudu](https://github.com/projectkudu/kudu/wiki) 発行プロファイルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="d58c4-210">フォルダー (クロス プラットフォームで動作します):</span><span class="sxs-lookup"><span data-stu-id="d58c4-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="d58c4-211">MSDeploy (MSDeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します):</span><span class="sxs-lookup"><span data-stu-id="d58c4-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="d58c4-212">MSDeploy パッケージ (MSDeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します):</span><span class="sxs-lookup"><span data-stu-id="d58c4-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="d58c4-213">上記の例で、`deployonbuild` を `dotnet publish` に**渡さない**ようにしてください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="d58c4-214">詳細については、「[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="d58c4-215">`dotnet publish` は、任意のプラットフォームから Azure に発行する Kudu API をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="d58c4-216">Visual Studio の発行は、Kudu API をサポートしていますが、Azure へのクロスプラットフォームの発行は WebSDK がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="d58c4-217">次の内容の発行プロファイルを *Properties/PublishProfiles* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="d58c4-218">次のコマンドを実行すると、発行するコンテンツが圧縮され、Kudu API を使用して Azure に発行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="d58c4-219">発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="d58c4-220">*FolderProfile* という名前のプロファイルを使用して発行する場合は、以下のいずれかのコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="d58c4-221">[dotnet build](/dotnet/core/tools/dotnet-build) を呼び出すと、`msbuild` が呼び出されて、ビルドと発行プロセスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="d58c4-222">`dotnet build` または `msbuild` の呼び出しは、フォルダー プロファイルで渡す場合は同等です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="d58c4-223">Windows で MSBuild を直接呼び出すと、MSBuild の .NET Framework バージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="d58c4-224">MSDeploy の発行は、現在 Windows コンピューターに制限されています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="d58c4-225">フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="d58c4-226">フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。</span><span class="sxs-lookup"><span data-stu-id="d58c4-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="d58c4-227">フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="d58c4-228">次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="d58c4-229">`<LastUsedBuildConfiguration>` は `Release` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="d58c4-230">Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="d58c4-231">`<LastUsedBuildConfiguration>` 構成プロパティは特別なプロパティなので、インポートされる MSBuild ファイルで上書きされないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="d58c4-232">このプロパティは、コマンド ラインからオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="d58c4-233">.NET Core CLI の使用:</span><span class="sxs-lookup"><span data-stu-id="d58c4-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="d58c4-234">MSBuild の使用:</span><span class="sxs-lookup"><span data-stu-id="d58c4-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="d58c4-235">コマンド ラインから MSDeploy エンドポイントに発行する</span><span class="sxs-lookup"><span data-stu-id="d58c4-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="d58c4-236">発行は、.NET Core CLI または MSBuild を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="d58c4-237">`dotnet publish` は .NET Core のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="d58c4-238">`msbuild` コマンドは .NET Framework を必要とするため、Windows 環境に制限されています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="d58c4-239">MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="d58c4-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="d58c4-240">次の例では (`dotnet new mvc` を使用して) ASP.NET Core Web アプリが作成され、Visual Studio を使用して Azure 発行プロファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="d58c4-241">**VS 2017 用開発者コマンド プロンプト**から `msbuild` を実行します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="d58c4-242">この開発者コマンド プロンプトのパスには、いくつかの MSBuild 変数が設定された正しい *msbuild.exe* が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="d58c4-243">MSBuild は次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="d58c4-244">*\<発行名>.PublishSettings* ファイルから `Password` を取得します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="d58c4-245">次のいずれかの方法で、*.PublishSettings* ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="d58c4-246">ソリューション エクスプローラー: Web アプリを右クリックし、**[発行プロファイルのダウンロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="d58c4-247">Azure Portal: Web アプリの **[概要]** ウィンドウで **[発行プロファイルの取得]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="d58c4-248">`Username` は発行プロファイルで確認できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="d58c4-249">次の例では、*Web11112 - Web Deploy* プロファイルを使用しています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="d58c4-250">ファイルを除外する</span><span class="sxs-lookup"><span data-stu-id="d58c4-250">Exclude files</span></span>

<span data-ttu-id="d58c4-251">ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="d58c4-252">`msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="d58c4-253">たとえば、次の `<Content>` 要素は、*wwwroot/content* フォルダーとそのすべてのサブフォルダーのすべてのテキスト (*.txt*) ファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="d58c4-254">上記のマークアップは、発行プロファイルまたは *.csproj* ファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="d58c4-255">*.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="d58c4-256">次の `<MsDeploySkipRules>` 要素は、*wwwroot/content* フォルダーのすべてのファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="d58c4-257">`<MsDeploySkipRules>` は、"*スキップされる*" ターゲットを配置サイトから削除しません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="d58c4-258">`<Content>` のターゲットであるファイルとフォルダーは、配置サイトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="d58c4-259">たとえば、配置される Web アプリに次のファイルが含まれているとします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="d58c4-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d58c4-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="d58c4-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d58c4-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="d58c4-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d58c4-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="d58c4-263">次の `<MsDeploySkipRules>` 要素を追加した場合、これらのファイルは配置サイトでは削除されません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="d58c4-264">前述の `<MsDeploySkipRules>` 要素は、"*スキップされる*" ファイルが配置されないようにします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="d58c4-265">それは、いったん配置されたファイルは削除しません。</span><span class="sxs-lookup"><span data-stu-id="d58c4-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="d58c4-266">次の `<Content>` 要素は、配置サイトのターゲット ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="d58c4-267">前述の`<Content>` 要素を使用してコマンド ライン配置を行うと、次の出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="d58c4-268">インクルード ファイル</span><span class="sxs-lookup"><span data-stu-id="d58c4-268">Include files</span></span>

<span data-ttu-id="d58c4-269">次のマークアップは、プロジェクト ディレクトリの外部にある *images* フォルダーを、発行サイトの *wwwroot/images* フォルダーに含めます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="d58c4-270">このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="d58c4-271">*.csproj* ファイルに追加した場合は、プロジェクトの各発行プロファイルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="d58c4-272">次の強調表示されているマークアップは、この方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="d58c4-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="d58c4-273">プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="d58c4-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="d58c4-274">*wwwroot\Content* フォルダーを除外します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="d58c4-275">*Views\Home\About2.cshtml* を除外します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-275">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="d58c4-276">他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d58c4-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="d58c4-277">発行の前または後にターゲットを実行する</span><span class="sxs-lookup"><span data-stu-id="d58c4-277">Run a target before or after publishing</span></span>

<span data-ttu-id="d58c4-278">組み込みの `BeforePublish` と `AfterPublish` ターゲットは、ターゲットの発行前または後にターゲットを実行できます。</span><span class="sxs-lookup"><span data-stu-id="d58c4-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="d58c4-279">発行の前と後の両方でコンソール メッセージをログに記録するには、発行プロファイルに次の要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="d58c4-280">信頼されていない証明書を使用してサーバーに発行する</span><span class="sxs-lookup"><span data-stu-id="d58c4-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="d58c4-281">値が `True` の `<AllowUntrustedCertificate>` プロパティを発行プロファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="d58c4-282">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="d58c4-282">The Kudu service</span></span>

<span data-ttu-id="d58c4-283">Azure App Service での Web アプリのデプロイに含まれるファイルを表示するには、[Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service) を使用します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="d58c4-284">`scm` トークンを Web アプリ名に追加します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="d58c4-285">例:</span><span class="sxs-lookup"><span data-stu-id="d58c4-285">For example:</span></span>

| <span data-ttu-id="d58c4-286">URL</span><span class="sxs-lookup"><span data-stu-id="d58c4-286">URL</span></span>                                    | <span data-ttu-id="d58c4-287">結果</span><span class="sxs-lookup"><span data-stu-id="d58c4-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="d58c4-288">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="d58c4-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="d58c4-289">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="d58c4-289">Kudu service</span></span> |

<span data-ttu-id="d58c4-290">ファイルの表示、編集、削除、追加を行うには、[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d58c4-291">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="d58c4-291">Additional resources</span></span>

* <span data-ttu-id="d58c4-292">[Web 配置](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) は、IIS サーバーへの Web アプリと Web サイトの配置を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="d58c4-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="d58c4-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置でのファイルの問題と要求機能。</span><span class="sxs-lookup"><span data-stu-id="d58c4-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
