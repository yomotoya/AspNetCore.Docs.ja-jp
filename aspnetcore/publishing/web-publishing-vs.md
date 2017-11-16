---
title: "Visual Studio および MSBuild 用の発行プロファイルを作成する"
author: rick-anderson
description: "Visual Studio の Web 発行について説明します。"
keywords: "ASP.NET Core, Web 発行, 発行, msbuild, Web 配置, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="8a545-104">ASP.NET Core アプリを展開するために、Visual Studio および MSBuild 用の発行プロファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="8a545-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="8a545-105">作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8a545-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8a545-106">この記事では、Visual Studio 2017 を使用して発行プロファイルを作成する方法に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="8a545-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="8a545-107">Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="8a545-108">この記事では、発行プロセスについて詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="8a545-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="8a545-109">Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a545-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="8a545-110">次の *.csproj* ファイルは、コマンド `dotnet new mvc` で作成されました。</span><span class="sxs-lookup"><span data-stu-id="8a545-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a545-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a545-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a545-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a545-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="8a545-113">上記のマークアップで (1 行目にある) `<Project>` 要素の `Sdk` 属性は、以下の処理を実行しています。</span><span class="sxs-lookup"><span data-stu-id="8a545-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="8a545-114">最初に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* から `props` ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="8a545-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="8a545-115">最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="8a545-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="8a545-116">(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="8a545-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="8a545-117">`Microsoft.NET.Sdk.Web` は以下に依存しています。</span><span class="sxs-lookup"><span data-stu-id="8a545-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="8a545-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="8a545-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="8a545-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="8a545-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="8a545-120">そのため、以下の `props` と `targets` がインポートされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="8a545-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="8a545-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="8a545-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="8a545-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="8a545-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="8a545-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="8a545-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="8a545-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="8a545-125">使用される発行方法に基づいて、発行ターゲットは適切なターゲット セットをもう一度インポートします。</span><span class="sxs-lookup"><span data-stu-id="8a545-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="8a545-126">MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="8a545-127">プロジェクトをビルドする</span><span class="sxs-lookup"><span data-stu-id="8a545-127">Build project</span></span>
* <span data-ttu-id="8a545-128">発行するファイルを計算する</span><span class="sxs-lookup"><span data-stu-id="8a545-128">Compute files to publish</span></span>
* <span data-ttu-id="8a545-129">ターゲットにファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="8a545-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="8a545-130">プロジェクト項目を比較する</span><span class="sxs-lookup"><span data-stu-id="8a545-130">Compute project items</span></span>

<span data-ttu-id="8a545-131">プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="8a545-132">`item type` 属性によって、ファイルの処理方法が決まります。</span><span class="sxs-lookup"><span data-stu-id="8a545-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="8a545-133">既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。</span><span class="sxs-lookup"><span data-stu-id="8a545-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="8a545-134">`Compile` 項目一覧のファイルがコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="8a545-135">`Content` 項目一覧には、ビルド出力に加え、発行されるファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8a545-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="8a545-136">既定で、wwwroot/** というパターンに一致するファイルが `Content` 項目に含まれます。</span><span class="sxs-lookup"><span data-stu-id="8a545-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="8a545-137">[wwwroot/** は glob パターンであり](https://gruntjs.com/configuring-tasks#globbing-patterns)、*wwwroot* フォルダー**および**サブフォルダーのすべてのファイルが指定されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="8a545-138">発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#including-files)」で説明されているように、*.csproj* ファイルに直接ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="8a545-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="8a545-139">Visual Studio で **[発行]** ボタンを選択するか、コマンド ラインから発行した場合:</span><span class="sxs-lookup"><span data-stu-id="8a545-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="8a545-140">プロパティ/項目が計算されます (ビルドに必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="8a545-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="8a545-141">Visual Studio のみ: NuGet パッケージが復元されます</span><span class="sxs-lookup"><span data-stu-id="8a545-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="8a545-142">(CLI では、ユーザーが明示的に復元する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="8a545-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="8a545-143">プロジェクトがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-143">The project builds.</span></span>
- <span data-ttu-id="8a545-144">発行項目が計算されます (発行に必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="8a545-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="8a545-145">プロジェクトが発行されます</span><span class="sxs-lookup"><span data-stu-id="8a545-145">The project is published.</span></span> <span data-ttu-id="8a545-146">(計算されたファイルは、発行先にコピーされます)。</span><span class="sxs-lookup"><span data-stu-id="8a545-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="8a545-147">基本的なコマンドラインによる発行</span><span class="sxs-lookup"><span data-stu-id="8a545-147">Basic command line publishing</span></span>

<span data-ttu-id="8a545-148">このセクションの方法は、.NET Core がサポートされるすべてのプラットフォームに使用できます。Visual Studio は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8a545-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="8a545-149">以下の例では、プロジェクト ディレクトリ (*.csproj* ファイルが含まれているディレクトリ) から `dotnet publish` コマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="8a545-150">現在のフォルダーがプロジェクト フォルダーではない場合は、プロジェクト ファイル パスで明示的に渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="8a545-151">例:</span><span class="sxs-lookup"><span data-stu-id="8a545-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="8a545-152">Web アプリを作成して発行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="8a545-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8a545-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8a545-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8a545-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8a545-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="8a545-155">`dotnet publish` では次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="8a545-156">既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。</span><span class="sxs-lookup"><span data-stu-id="8a545-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="8a545-157">`$(Configuration)` の既定値は Debug です。</span><span class="sxs-lookup"><span data-stu-id="8a545-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="8a545-158">上の例で `<TargetFramework>` は `netcoreapp2.0` です。</span><span class="sxs-lookup"><span data-stu-id="8a545-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="8a545-159">`dotnet publish -h` では、発行のヘルプ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="8a545-160">次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="8a545-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="8a545-161">`dotnet publish` コマンドは、`Publish` ターゲットを呼び出す `MSBuild` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8a545-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="8a545-162">`dotnet publish` に渡されたすべてのパラメーターは、`MSBuild` に渡されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="8a545-163">`-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="8a545-164">`-o` パラメーターは `OutputPath` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="8a545-165">次のいずれかの形式を使用して MSBuild プロパティを渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="8a545-166">次のコマンドは、`Release` ビルドをネットワーク共有に発行します。</span><span class="sxs-lookup"><span data-stu-id="8a545-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="8a545-167">ネットワーク共有は、スラッシュを使用して指定します (*//r8/*)。 .NET Core がサポートされるすべてのプラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="8a545-168">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8a545-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="8a545-169">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="8a545-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="8a545-170">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="8a545-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="8a545-171">プロファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="8a545-171">Publish profiles</span></span>

<span data-ttu-id="8a545-172">このセクションでは、Visual Studio 2017 以降を使用して発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8a545-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="8a545-173">作成したら、Visual Studio またはコマンド ラインから発行できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="8a545-174">発行プロファイルによって、発行プロセスが簡単になります。</span><span class="sxs-lookup"><span data-stu-id="8a545-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="8a545-175">複数の発行プロファイルを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="8a545-176">Visual Studio で発行プロファイルを作成するには、ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8a545-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="8a545-177">また、ビルド メニューから **[\<プロジェクト名> の発行]** を選択する方法もあります。</span><span class="sxs-lookup"><span data-stu-id="8a545-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="8a545-178">アプリケーション容量ページの **[発行]** タブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="8a545-179">プロジェクトに発行プロファイルが含まれていない場合、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![アプリケーション容量ページの **[発行]** タブに、Azure、IIS、FTB、フォルダーが表示され、Azure が選択されている図。](web-publishing-vs/_static/az.png)

<span data-ttu-id="8a545-182">**[フォルダー]** が選択されている場合、**[発行]** ボタンをクリックすると、フォルダーの発行プロファイルが作成され、発行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Azure、IIS、FTB、フォルダーが表示されているアプリケーション容量ページの **[発行]** タブ](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="8a545-184">発行プロファイルが作成されると、**[発行]** タブが変わり、**[新しいプロファイルの作成]** を選択して新しいプロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="8a545-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![最後の手順で作成した FolderProfile と、[新しいプロファイルの作成] リンクが表示されたアプリケーション容量ページの **[発行]** タブ](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="8a545-186">発行ウィザードは、次の発行ターゲットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8a545-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="8a545-187">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="8a545-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="8a545-188">IIS、FTP など (任意の Web サーバーの場合)</span><span class="sxs-lookup"><span data-stu-id="8a545-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="8a545-189">フォルダー</span><span class="sxs-lookup"><span data-stu-id="8a545-189">Folder</span></span>
- <span data-ttu-id="8a545-190">プロファイルのインポート (プロファイルをインポートできます)。</span><span class="sxs-lookup"><span data-stu-id="8a545-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="8a545-191">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="8a545-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="8a545-192">詳細については、「[状況に適した発行オプション](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a545-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="8a545-193">Visual Studio で発行プロファイルを作成すると、*Properties/PublishProfiles/\<発行名>.pubxml* MSBuild ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="8a545-194">この *.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8a545-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="8a545-195">このファイルを変更して、ビルドと発行プロセスをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="8a545-196">このファイルは、発行プロファイルで読み取られます。</span><span class="sxs-lookup"><span data-stu-id="8a545-196">This file is read by the publishing process.</span></span> <span data-ttu-id="8a545-197">`<LastUsedBuildConfiguration>` は、グローバル プロパティであり、ビルドにインポートされるファイル内には含めないので、特殊です。</span><span class="sxs-lookup"><span data-stu-id="8a545-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="8a545-198">詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a545-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="8a545-199">*.pubxml* ファイルはソース コード管理にチェックインしないことをお勧めします。このファイルは *.user* ファイルに依存しています。</span><span class="sxs-lookup"><span data-stu-id="8a545-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="8a545-200">また、*.user* ファイルは、機密情報が含まれている可能性があり、1 ユーザーおよびコンピューターに対してのみ有効なので、ソース コード管理にチェックインしないでください。</span><span class="sxs-lookup"><span data-stu-id="8a545-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="8a545-201">機密情報 (発行のパスワードなど) はユーザー/コンピューターごとのレベルで暗号化され、*Properties/PublishProfiles/\<発行名>.pubxml.user* ファイルに保存されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="8a545-202">このファイルには機密情報が含まれている可能性があるため、ソース コード管理に**チェックインしないでください**。</span><span class="sxs-lookup"><span data-stu-id="8a545-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="8a545-203">ASP.NET Core で Web アプリを発行する方法の概要については、「[Publishing and Deployment](index.md)」(発行と配置) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a545-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="8a545-204">[Publishing and Deployment](index.md) は、https://github.com/aspnet/websdk のオープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="8a545-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="8a545-205">`dotnet publish` は Msdeploy のフォルダーを使用することができ、[KUDU](https://github.com/projectkudu/kudu/wiki) は次のプロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="8a545-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="8a545-206">フォルダー (クロスプラット フォームで機能) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="8a545-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="8a545-207">Msdeploy (msdeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="8a545-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="8a545-208">Msdeploy パッケージ (msdeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="8a545-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="8a545-209">上記の例で、`deployonbuild` を `dotnet publish` に**渡さない**ようにしてください。</span><span class="sxs-lookup"><span data-stu-id="8a545-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="8a545-210">詳細については、「[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)」を参照してください</span><span class="sxs-lookup"><span data-stu-id="8a545-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="8a545-211">`dotnet publish` は、任意のプラットフォームから Azure に発行する KUDU API をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8a545-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="8a545-212">Visual Studio の発行は、KUDU API をサポートしていますが、Azure へのクロスプラットフォームの発行は websdk がサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8a545-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="8a545-213">次の内容の発行プロファイルを *[Properties/PublishProfiles]* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="8a545-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="8a545-214">次のコマンドを実行すると発行するコンテンツが圧縮され、KUDU API を使用して Azure に発行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="8a545-215">発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="8a545-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="8a545-216">たとえば、*FolderProfile* というプロファイルを使用して発行する場合、以下のいずれかのコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="8a545-217">`dotnet build` を呼び出すと、`msbuild` が呼び出され、ビルドと発行プロセスが実行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="8a545-218">`dotnet build` または `msbuild` の呼び出しは、フォルダー プロファイルで渡す場合と基本的に同じです。</span><span class="sxs-lookup"><span data-stu-id="8a545-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="8a545-219">Windows で MSBuild を直接呼び出すときに、MSBuild の .NET Framework バージョンを取得します。</span><span class="sxs-lookup"><span data-stu-id="8a545-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="8a545-220">MSDeploy の発行は、現在 Windows コンピューターに制限されています。</span><span class="sxs-lookup"><span data-stu-id="8a545-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="8a545-221">フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="8a545-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="8a545-222">フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。</span><span class="sxs-lookup"><span data-stu-id="8a545-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="8a545-223">フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="8a545-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="8a545-224">次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="8a545-225">`<LastUsedBuildConfiguration>` は `Release` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="8a545-226">Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="8a545-227">`<LastUsedBuildConfiguration>` 構成プロパティは特殊なので、インポートされる MSBuild ファイルで上書きされないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="8a545-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="8a545-228">コマンド ラインからこのプロパティを上書きできます。</span><span class="sxs-lookup"><span data-stu-id="8a545-228">You can override this property from the command line.</span></span> <span data-ttu-id="8a545-229">例:</span><span class="sxs-lookup"><span data-stu-id="8a545-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="8a545-230">MSBuild の使用:</span><span class="sxs-lookup"><span data-stu-id="8a545-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="8a545-231">コマンド ラインから MSDeploy エンドポイントに発行する</span><span class="sxs-lookup"><span data-stu-id="8a545-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="8a545-232">前述のように、`dotnet publish` または `msbuild` コマンドを使用して発行できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="8a545-233">`dotnet publish` は .NET Core のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="8a545-234">`msbuild` には .NET Framework が必要なので、Windows 環境に制限されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="8a545-235">MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="8a545-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="8a545-236">次の例では、(`dotnet new mvc` を使用して) ASP.NET Core Web アプリが作成され、Visual Studio を使用して Azure 発行プロファイルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="8a545-237">**[開発者コマンド プロンプト for VS 2017]** から `msbuild` を実行します。</span><span class="sxs-lookup"><span data-stu-id="8a545-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="8a545-238">開発者コマンド プロンプトのパスには、正しい *msbuild.exe* が表示され、いくつかの MSBuild 編集が設定されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="8a545-239">MSBuild は次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="8a545-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="8a545-240">*\<発行名>.PublishSettings* ファイルから `Password` を取得できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="8a545-241">*.PublishSettings* ファイルは次の機能でダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="8a545-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="8a545-242">ソリューション エクスプローラー: Web アプリを右クリックし、**[発行プロファイルのダウンロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8a545-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="8a545-243">Microsoft Azure の管理ポータル: Web アプリ ブレードから **[発行プロファイルの取得]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8a545-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="8a545-244">`Username` は発行プロファイルで確認できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="8a545-245">次の例では、"Web11112 - Web 配置" 発行プロファイルを使用しています。</span><span class="sxs-lookup"><span data-stu-id="8a545-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="8a545-246">ファイルの除外</span><span class="sxs-lookup"><span data-stu-id="8a545-246">Excluding files</span></span>

<span data-ttu-id="8a545-247">ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。</span><span class="sxs-lookup"><span data-stu-id="8a545-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="8a545-248">`msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8a545-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="8a545-249">たとえば、次の `<Content>` 要素マークアップでは、*wwwroot/content* フォルダーとそのすべてのサブフォルダーからすべてのテキスト (*.txt*) ファイルが除外されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="8a545-250">上記のマークアップは、発行プロファイルまたは *.csproj* ファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="8a545-251">*.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="8a545-252">次の `<MsDeploySkipRules>` 要素マークアップは、*wwwroot/content* フォルダーからすべてのファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="8a545-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="8a545-253">`<MsDeploySkipRules>` は、配置サイトから *skip* ターゲットは削除されません。</span><span class="sxs-lookup"><span data-stu-id="8a545-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="8a545-254">`<Content>` のターゲットとなるファイルとフォルダーは、配置サイトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="8a545-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="8a545-255">たとえば、次のファイルを含む Web アプリを配置したとします。</span><span class="sxs-lookup"><span data-stu-id="8a545-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="8a545-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8a545-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="8a545-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8a545-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="8a545-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="8a545-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="8a545-259">次の `<MsDeploySkipRules>` マークアップを追加した場合、配置サイトのそれらのファイルは削除されません。</span><span class="sxs-lookup"><span data-stu-id="8a545-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="8a545-260">上記の `<MsDeploySkipRules>` マークアップは*スキップ対象の*ファイルの配置を防ぎますが、配置されているそれらのファイルは削除しません。</span><span class="sxs-lookup"><span data-stu-id="8a545-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="8a545-261">次の `<Content>` マークアップは、配置サイトのターゲット ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="8a545-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="8a545-262">上記の `<Content>` マークアップを指定してコマンド ライン配置を使用すると、次のような出力になります。</span><span class="sxs-lookup"><span data-stu-id="8a545-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="8a545-263">ファイルを含める</span><span class="sxs-lookup"><span data-stu-id="8a545-263">Including files</span></span>

<span data-ttu-id="8a545-264">次のマークアップを使用して、プロジェクト ディレクトリにない *images* フォルダーを発行サイトの *wwwroot/images* フォルダーに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="8a545-265">このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="8a545-266">*.csproj* ファイルに追加した場合、プロジェクトの各発行プロファイルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="8a545-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="8a545-267">次の強調表示されているマークアップは、この方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="8a545-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="8a545-268">プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="8a545-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="8a545-269">*wwwroot\Content* フォルダーを除外します。</span><span class="sxs-lookup"><span data-stu-id="8a545-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="8a545-270">*Views\Home\About2.cshtml* を除外します。</span><span class="sxs-lookup"><span data-stu-id="8a545-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="8a545-271">他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8a545-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="8a545-272">発行の前または後にターゲットを実行する</span><span class="sxs-lookup"><span data-stu-id="8a545-272">Run a target before or after publishing</span></span>

<span data-ttu-id="8a545-273">組み込みの `BeforePublish` および `AfterPublish` ターゲットを使用して、発行ターゲットの前または後にターゲットを実行できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="8a545-274">次のマークアップを発行プロファイルに追加して、発行の前と後にメッセージのログをコンソールの出力に記録することができます。</span><span class="sxs-lookup"><span data-stu-id="8a545-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="8a545-275">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="8a545-275">The Kudu service</span></span>

<span data-ttu-id="8a545-276">Azure Web アプリでファイルを表示するには、[Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)を使用します。</span><span class="sxs-lookup"><span data-stu-id="8a545-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="8a545-277">`scm` トークンを名前または Web アプリに追加します。</span><span class="sxs-lookup"><span data-stu-id="8a545-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="8a545-278">例:</span><span class="sxs-lookup"><span data-stu-id="8a545-278">For example:</span></span>

| <span data-ttu-id="8a545-279">URL</span><span class="sxs-lookup"><span data-stu-id="8a545-279">URL</span></span>               | <span data-ttu-id="8a545-280">結果</span><span class="sxs-lookup"><span data-stu-id="8a545-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="8a545-281">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="8a545-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="8a545-282">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="8a545-282">Kudu sevice</span></span> |

<span data-ttu-id="8a545-283">[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択して、ファイルの表示、編集、削除、追加を行います。</span><span class="sxs-lookup"><span data-stu-id="8a545-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8a545-284">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="8a545-284">Additional resources</span></span>

- <span data-ttu-id="8a545-285">[Web 配置](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) を使用すると、Web アプリケーションと Web サイトを IIS サーバーに配置する処理を簡略化できます。</span><span class="sxs-lookup"><span data-stu-id="8a545-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="8a545-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置のファイルの問題と要求機能。</span><span class="sxs-lookup"><span data-stu-id="8a545-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
