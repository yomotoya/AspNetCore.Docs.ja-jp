---
title: "Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。"
author: rick-anderson
description: "作成する方法を説明します。 Visual Studio での ASP.NET Core アプリケーションのプロファイルを発行します。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="0cfda-103">Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="0cfda-104">作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0cfda-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0cfda-105">この記事では、Visual Studio 2017 を使用して発行プロファイルを作成する方法に焦点を当てます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="0cfda-106">Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="0cfda-107">この記事では、発行プロセスについて詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="0cfda-108">Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="0cfda-109">次の *.csproj* ファイルは、コマンド `dotnet new mvc` で作成されました。</span><span class="sxs-lookup"><span data-stu-id="0cfda-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cfda-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cfda-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cfda-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cfda-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="0cfda-112">上記のマークアップで (1 行目にある) `<Project>` 要素の `Sdk` 属性は、以下の処理を実行しています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="0cfda-113">プロパティ ファイルからインポート*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*先頭にします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="0cfda-114">最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="0cfda-115">(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="0cfda-116">`Microsoft.NET.Sdk.Web` は以下に依存しています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="0cfda-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="0cfda-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="0cfda-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="0cfda-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="0cfda-119">これは、次のプロパティとをインポートするターゲットによってが発生します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="0cfda-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="0cfda-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="0cfda-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="0cfda-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="0cfda-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="0cfda-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="0cfda-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="0cfda-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="0cfda-124">ターゲットのインポートの右側を使用する発行方法に基づいてターゲットの設定を発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="0cfda-125">MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="0cfda-126">プロジェクトをビルドする</span><span class="sxs-lookup"><span data-stu-id="0cfda-126">Build project</span></span>
* <span data-ttu-id="0cfda-127">発行するファイルを計算する</span><span class="sxs-lookup"><span data-stu-id="0cfda-127">Compute files to publish</span></span>
* <span data-ttu-id="0cfda-128">ターゲットにファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="0cfda-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="0cfda-129">プロジェクト項目を比較する</span><span class="sxs-lookup"><span data-stu-id="0cfda-129">Compute project items</span></span>

<span data-ttu-id="0cfda-130">プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="0cfda-131">`item type` 属性によって、ファイルの処理方法が決まります。</span><span class="sxs-lookup"><span data-stu-id="0cfda-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="0cfda-132">既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="0cfda-133">`Compile` 項目一覧のファイルがコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="0cfda-134">`Content`項目リストには、ビルドの出力だけでなく公開されているファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="0cfda-135">既定では、ファイル、パターンに一致する`wwwroot/**`に含まれる、`Content`項目。</span><span class="sxs-lookup"><span data-stu-id="0cfda-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="0cfda-136">[wwwroot/\* \*グロブ パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)内のすべてのファイルを指定する、 *wwwroot*フォルダー**と**サブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="0cfda-137">発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#including-files)」で説明されているように、*.csproj* ファイルに直接ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="0cfda-138">選択すると、**発行**Visual Studio またはコマンドラインから発行するときにボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="0cfda-139">プロパティ/項目が計算されます (ビルドに必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="0cfda-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="0cfda-140">Visual Studio のみ: NuGet パッケージが復元されます</span><span class="sxs-lookup"><span data-stu-id="0cfda-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="0cfda-141">(CLI では、ユーザーが明示的に復元する必要があります)。</span><span class="sxs-lookup"><span data-stu-id="0cfda-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="0cfda-142">プロジェクトがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-142">The project builds.</span></span>
* <span data-ttu-id="0cfda-143">発行項目が計算されます (発行に必要なファイル)。</span><span class="sxs-lookup"><span data-stu-id="0cfda-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="0cfda-144">プロジェクトが発行されます</span><span class="sxs-lookup"><span data-stu-id="0cfda-144">The project is published.</span></span> <span data-ttu-id="0cfda-145">(計算されたファイルは、発行先にコピーされます)。</span><span class="sxs-lookup"><span data-stu-id="0cfda-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="0cfda-146">ASP.NET Core プロジェクトの参照と`Microsoft.NET.Sdk.Web`プロジェクト ファイルで、 *app_offline.htm*ファイルは、web アプリのディレクトリのルートに配置します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="0cfda-147">ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="0cfda-148">詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#appofflinehtm)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="0cfda-149">基本的なコマンド ライン発行</span><span class="sxs-lookup"><span data-stu-id="0cfda-149">Basic command-line publishing</span></span>

<span data-ttu-id="0cfda-150">コマンド ライン パブリッシングでは、すべての .NET Core のサポートされているプラットフォームで動作し、Visual Studio は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0cfda-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="0cfda-151">以下の例では、プロジェクト ディレクトリ (*.csproj* ファイルが含まれているディレクトリ) から `dotnet publish` コマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="0cfda-152">指定しない場合、プロジェクト フォルダー内に明示的に渡すプロジェクト ファイルのパスにします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="0cfda-153">例:</span><span class="sxs-lookup"><span data-stu-id="0cfda-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="0cfda-154">Web アプリを作成して発行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="0cfda-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="0cfda-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="0cfda-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="0cfda-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="0cfda-157">`dotnet publish` では次のような出力が生成されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="0cfda-158">既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="0cfda-159">`$(Configuration)` の既定値は Debug です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="0cfda-160">上の例で `<TargetFramework>` は `netcoreapp2.0` です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="0cfda-161">`dotnet publish -h` では、発行のヘルプ情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="0cfda-162">次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="0cfda-163">`dotnet publish`コマンドによって実行される MSBuild を呼び出し、`Publish`ターゲットです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="0cfda-164">渡されるパラメーター `dotnet publish` MSBuild に渡されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="0cfda-165">`-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="0cfda-166">`-o` パラメーターは `OutputPath` にマップされます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="0cfda-167">MSBuild プロパティは、次の形式のいずれかを使用して渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="0cfda-168">次のコマンドは、`Release` ビルドをネットワーク共有に発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="0cfda-169">ネットワーク共有は、スラッシュを使用して指定します (*//r8/*).NET Core がサポートされるすべてのプラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="0cfda-170">配置用に発行したアプリが実行されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="0cfda-171">アプリが実行中は、*publish* フォルダー内のファイルがロックされます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="0cfda-172">ロックされているファイルはコピーできないため、配置は行われません。</span><span class="sxs-lookup"><span data-stu-id="0cfda-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="0cfda-173">プロファイルを発行する</span><span class="sxs-lookup"><span data-stu-id="0cfda-173">Publish profiles</span></span>

<span data-ttu-id="0cfda-174">このセクションでは、Visual Studio 2017 以降を使用して発行プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="0cfda-175">作成した後は、Visual Studio またはコマンドラインからの発行を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="0cfda-176">発行プロファイルによって、発行プロセスが簡単になります。</span><span class="sxs-lookup"><span data-stu-id="0cfda-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="0cfda-177">複数の発行プロファイルが存在します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="0cfda-178">Visual Studio での発行プロファイルを作成するには、ソリューション エクスプ ローラーでプロジェクトを右クリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="0cfda-179">また、選択**発行\<プロジェクト名 >**ビルド メニューからです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="0cfda-180">アプリケーション容量ページの **[発行]** タブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="0cfda-181">プロジェクトに発行プロファイルが含まれていない場合、次のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Azure、IIS、FTB、Azure とフォルダーを示すアプリケーション容量ページの [発行] タブを選択します。](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="0cfda-184">**[フォルダー]** が選択されている場合、**[発行]** ボタンをクリックすると、フォルダーの発行プロファイルが作成され、発行されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Azure、IIS、FTB、フォルダーが表示されているアプリケーション容量ページの **[発行]** タブ](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="0cfda-186">発行プロファイルを作成した後、**発行** タブの変更、および選択**新しいプロファイルを作成する**新しいプロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![最後の手順で作成した FolderProfile と、[新しいプロファイルの作成] リンクが表示されたアプリケーション容量ページの **[発行]** タブ](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="0cfda-188">発行ウィザードは、次の発行ターゲットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="0cfda-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0cfda-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="0cfda-190">IIS、FTP など (任意の Web サーバーの場合)</span><span class="sxs-lookup"><span data-stu-id="0cfda-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="0cfda-191">フォルダー</span><span class="sxs-lookup"><span data-stu-id="0cfda-191">Folder</span></span>
* <span data-ttu-id="0cfda-192">プロファイル (プロファイルのインポートを許可する) をインポートします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="0cfda-193">Microsoft Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="0cfda-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="0cfda-194">詳細については、「[状況に適した発行オプション](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="0cfda-195">Visual Studio が、発行プロファイルを作成するときに、*プロパティ/PublishProfiles/\<発行名 > .pubxml* MSBuild ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="0cfda-196">この *.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="0cfda-197">このファイルは、ビルドのカスタマイズし、発行プロセスに変更することができます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="0cfda-198">このファイルは、発行プロファイルで読み取られます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-198">This file is read by the publishing process.</span></span> <span data-ttu-id="0cfda-199">`<LastUsedBuildConfiguration>`特殊なため、グローバル プロパティであり、ビルドでインポートするすべてのファイルではないはずです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="0cfda-200">詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="0cfda-201">*.Pubxml*に依存しているために、ソース管理にファイルをチェックインしないでください、 *.user*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0cfda-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="0cfda-202">また、*.user* ファイルは、機密情報が含まれている可能性があり、1 ユーザーおよびコンピューターに対してのみ有効なので、ソース コード管理にチェックインしないでください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="0cfda-203">機密情報 (発行のパスワードなど) はユーザー/コンピューターごとのレベルで暗号化され、*Properties/PublishProfiles/\<発行名>.pubxml.user* ファイルに保存されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="0cfda-204">このファイルには機密情報が含まれている可能性があるため、ソース コード管理に**チェックインしないでください**。</span><span class="sxs-lookup"><span data-stu-id="0cfda-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="0cfda-205">ASP.NET Core 上の web アプリを公開する方法の概要については、次を参照してください。[ホストを展開および](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="0cfda-206">[ホストし、展開](index.md)https://github.com/aspnet/websdk でオープン ソース プロジェクトです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="0cfda-207">`dotnet publish`MSDeploy のフォルダーを使用し、 [KUDU](https://github.com/projectkudu/kudu/wiki)プロファイルの発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="0cfda-208">フォルダー (クロスプラット フォームで機能):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="0cfda-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="0cfda-209">MSDeploy (現在こののみで機能します windows MSDeploy クロスプラット フォームのないため)。`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="0cfda-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="0cfda-210">MSDeploy パッケージ (現在こののみで機能します windows MSDeploy クロスプラット フォームのないため)。`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="0cfda-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="0cfda-211">上記のサンプルで**しない**渡す`deployonbuild`に`dotnet publish`です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="0cfda-212">詳細については、次を参照してください。 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="0cfda-213">`dotnet publish` は、任意のプラットフォームから Azure に発行する KUDU API をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="0cfda-214">Visual Studio では、サポートが KUDU Api でサポートされて websdk クロス plat が Azure に発行を公開します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="0cfda-215">次の内容の発行プロファイルを *[Properties/PublishProfiles]* フォルダーに追加します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="0cfda-216">次のコマンドを実行している公開内容を圧縮し、KUDU Api を使用して Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="0cfda-217">発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="0cfda-218">という名前のプロファイルで発行するときに*FolderProfile*、次のコマンドのいずれかを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="0cfda-219">呼び出すときに`dotnet build`、呼び出す`msbuild`ビルドを実行してプロセスを発行します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="0cfda-220">呼び出す`dotnet build`または`msbuild`はフォルダー プロファイルに渡す場合、本質的に同等です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="0cfda-221">Windows 上で直接には、MSBuild を呼び出し、MSBuild の .NET Framework のバージョンが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="0cfda-222">MSDeploy の発行は、現在 Windows コンピューターに制限されています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="0cfda-223">フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="0cfda-224">フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。</span><span class="sxs-lookup"><span data-stu-id="0cfda-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="0cfda-225">フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="0cfda-226">次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="0cfda-227">`<LastUsedBuildConfiguration>` は `Release` に設定されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="0cfda-228">Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="0cfda-229">`<LastUsedBuildConfiguration>`構成プロパティは特別であり、インポートされた MSBuild ファイル内でオーバーライドすることはできません。</span><span class="sxs-lookup"><span data-stu-id="0cfda-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="0cfda-230">コマンドラインからこのプロパティをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="0cfda-231">例:</span><span class="sxs-lookup"><span data-stu-id="0cfda-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="0cfda-232">MSBuild の使用:</span><span class="sxs-lookup"><span data-stu-id="0cfda-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="0cfda-233">コマンド ラインから MSDeploy エンドポイントに発行する</span><span class="sxs-lookup"><span data-stu-id="0cfda-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="0cfda-234">既に触れましたが、発行を使用して実現できる`dotnet publish`または`msbuild`コマンド。</span><span class="sxs-lookup"><span data-stu-id="0cfda-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="0cfda-235">`dotnet publish` は .NET Core のコンテキストで実行されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="0cfda-236">`msbuild`コマンドは、.NET framework が必要であるため、Windows 環境に制限されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="0cfda-237">MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="0cfda-238">次の例では、ASP.NET Core web アプリを作成 (を使用して`dotnet new mvc`)、Visual Studio で Azure の発行プロファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="0cfda-239">実行`msbuild`から、 **VS 2017 用開発者コマンド プロンプト**です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="0cfda-240">開発者コマンド プロンプトが、正しい*msbuild.exe*一部 MSBuild 変数セットをそのパスにします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="0cfda-241">MSBuild は次の構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="0cfda-242">取得、`Password`から、 *\<発行名 >。PublishSettings*ファイル。</span><span class="sxs-lookup"><span data-stu-id="0cfda-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="0cfda-243">ダウンロード、*です。PublishSettings*からいずれかのファイル。</span><span class="sxs-lookup"><span data-stu-id="0cfda-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="0cfda-244">ソリューション エクスプ ローラー: は、この Web アプリでを右クリックし、選択**発行プロファイルのダウンロード**です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="0cfda-245">Azure Management Portal: 選択**Get 発行プロファイル**Web アプリのブレードからです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="0cfda-246">`Username` は発行プロファイルで確認できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="0cfda-247">次の例では、"Web11112 - Web 配置" 発行プロファイルを使用しています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="0cfda-248">ファイルの除外</span><span class="sxs-lookup"><span data-stu-id="0cfda-248">Excluding files</span></span>

<span data-ttu-id="0cfda-249">ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="0cfda-250">`msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="0cfda-251">たとえば、次`<Content>`要素マークアップには、すべてのテキストが含まれません (*.txt*) ファイルから、 *wwwroot/コンテンツ*フォルダーとそのすべてのサブフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="0cfda-252">上記のマークアップは、発行プロファイルまたは *.csproj* ファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="0cfda-253">*.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="0cfda-254">次の `<MsDeploySkipRules>` 要素マークアップは、*wwwroot/content* フォルダーからすべてのファイルを除外します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="0cfda-255">`<MsDeploySkipRules>`削除されません、*スキップ*配置サイトからターゲットです。</span><span class="sxs-lookup"><span data-stu-id="0cfda-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="0cfda-256">`<Content>`対象となるファイルとフォルダーは、配置サイトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="0cfda-257">たとえば、展開された web アプリケーションが、次のファイルとします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="0cfda-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cfda-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="0cfda-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cfda-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="0cfda-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0cfda-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="0cfda-261">場合、次`<MsDeploySkipRules>`マークアップが追加される、展開のサイトでそれらのファイルは削除はありません。</span><span class="sxs-lookup"><span data-stu-id="0cfda-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="0cfda-262">`<MsDeploySkipRules>`マークアップの前に示したように、*スキップ*depoyed されないファイルは、配置されると、それらのファイルを削除しません。</span><span class="sxs-lookup"><span data-stu-id="0cfda-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="0cfda-263">次`<Content>`マークアップは、展開のサイトで対象となるファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="0cfda-264">コマンド ラインの展開を使用して、`<Content>`マークアップ上、次のような出力結果。</span><span class="sxs-lookup"><span data-stu-id="0cfda-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="0cfda-265">ファイルを含める</span><span class="sxs-lookup"><span data-stu-id="0cfda-265">Including files</span></span>

<span data-ttu-id="0cfda-266">次のマークアップが含まれています、*イメージ*フォルダーをプロジェクト ディレクトリの外部、 *wwwroot/イメージ*発行サイトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="0cfda-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="0cfda-267">このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="0cfda-268">追加された場合、 *.csproj*ファイルが含まれているプロジェクト内の発行プロファイルごとにします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="0cfda-269">次の強調表示されているマークアップは、この方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="0cfda-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="0cfda-270">プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="0cfda-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="0cfda-271">*wwwroot\Content* フォルダーを除外します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="0cfda-272">*Views\Home\About2.cshtml* を除外します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="0cfda-273">他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0cfda-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="0cfda-274">発行の前または後にターゲットを実行する</span><span class="sxs-lookup"><span data-stu-id="0cfda-274">Run a target before or after publishing</span></span>

<span data-ttu-id="0cfda-275">組み込み`BeforePublish`と`AfterPublish`発行先の前後にターゲットを実行するターゲットを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="0cfda-276">次のマークアップを発行プロファイルに追加して、発行の前と後にメッセージのログをコンソールの出力に記録することができます。</span><span class="sxs-lookup"><span data-stu-id="0cfda-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="0cfda-277">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="0cfda-277">The Kudu service</span></span>

<span data-ttu-id="0cfda-278">内のファイルを表示する、アプリのサービスを Azure web アプリの展開、使用、 [Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)です。</span><span class="sxs-lookup"><span data-stu-id="0cfda-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="0cfda-279">追加、`scm`トークンを web アプリの名前。</span><span class="sxs-lookup"><span data-stu-id="0cfda-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="0cfda-280">例:</span><span class="sxs-lookup"><span data-stu-id="0cfda-280">For example:</span></span>

| <span data-ttu-id="0cfda-281">URL</span><span class="sxs-lookup"><span data-stu-id="0cfda-281">URL</span></span>                                    | <span data-ttu-id="0cfda-282">結果</span><span class="sxs-lookup"><span data-stu-id="0cfda-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="0cfda-283">Web アプリ</span><span class="sxs-lookup"><span data-stu-id="0cfda-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="0cfda-284">Kudu サービス</span><span class="sxs-lookup"><span data-stu-id="0cfda-284">Kudu sevice</span></span> |

<span data-ttu-id="0cfda-285">[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択して、ファイルの表示、編集、削除、追加を行います。</span><span class="sxs-lookup"><span data-stu-id="0cfda-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0cfda-286">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0cfda-286">Additional resources</span></span>

* <span data-ttu-id="0cfda-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) が IIS サーバーへの web サイトと web アプリの展開を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="0cfda-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="0cfda-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置のファイルの問題と要求機能。</span><span class="sxs-lookup"><span data-stu-id="0cfda-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
