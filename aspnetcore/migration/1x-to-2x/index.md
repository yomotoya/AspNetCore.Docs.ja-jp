---
title: "ASP.NET Core 1.x から 2.0 への移行"
author: scottaddie
description: "この記事では、ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に移行する前提条件と最も一般的な手順について説明します。"
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: ebc4efdedd60c38280a0d37b20313e09671f7f9d
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="a47e4-103">ASP.NET Core 1.x から ASP.NET Core 2.0 への移行</span><span class="sxs-lookup"><span data-stu-id="a47e4-103">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="a47e4-104">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a47e4-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a47e4-105">この記事では、既存の ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に更新する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-105">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="a47e4-106">アプリケーションを ASP.NET Core 2.0 に移行すると、[多数の新機能を利用したり、パフォーマンスを向上させたりすることができます](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="a47e4-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="a47e4-107">既存の ASP.NET Core 1.x アプリケーションは、バージョン固有のプロジェクト テンプレートには基づいていません。</span><span class="sxs-lookup"><span data-stu-id="a47e4-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="a47e4-108">ASP.NET Core のフレームワークの進化に伴い、プロジェクト テンプレートと、それに含まれるスタート コードも進化しています。</span><span class="sxs-lookup"><span data-stu-id="a47e4-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="a47e4-109">ASP.NET Core のフレームワークを更新するだけでなく、アプリケーションのコードも更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="a47e4-110">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="a47e4-110">Prerequisites</span></span>
<span data-ttu-id="a47e4-111">「[ASP.NET Core の概要](xref:getting-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a47e4-111">Please see [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="a47e4-112">ターゲット フレームワーク モニカー (TFM) の更新</span><span class="sxs-lookup"><span data-stu-id="a47e4-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="a47e4-113">.NET Core をターゲットとするプロジェクトでは、.NET Core 2.0 以上のバージョンの [TFM](/dotnet/standard/frameworks#referring-to-frameworks) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="a47e4-114">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `netcoreapp2.0` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="a47e4-115">.NET Framework をターゲットとするプロジェクトでは、.NET Framework 4.6.1 以上の TFM バージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="a47e4-116">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `net461` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="a47e4-117">.NET Core 1.x よりセキュリティ上外部からアクセスできるところの多い .NET core 2.0</span><span class="sxs-lookup"><span data-stu-id="a47e4-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="a47e4-118">NET Core 1.x に API がないためだけに .NET Framework をターゲットにする場合、.NET Core 2.0 をターゲットにすると機能します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="a47e4-119">global.json での .NET Core SDK バージョンの更新</span><span class="sxs-lookup"><span data-stu-id="a47e4-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="a47e4-120">特定の .NET Core SDK バージョンをターゲットとするよう、ソリューションが [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) ファイルに依存する場合、コンピューターにインストールされている 2.0 バージョンを使用するよう、その `version` プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="a47e4-121">パッケージ参照の更新</span><span class="sxs-lookup"><span data-stu-id="a47e4-121">Update package references</span></span>
<span data-ttu-id="a47e4-122">1.x プロジェクト内の *.csproj* ファイルには、プロジェクトによって使用される各 NuGet パッケージの一覧があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="a47e4-123">.NET Core 2.0 をターゲットとする ASP.NET Core 2.0 プロジェクトでは、*.csproj* ファイル内の 1 つの[メタパッケージ](xref:fundamentals/metapackage)への参照によってパッケージのコレクションが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="a47e4-124">メタパッケージには、ASP.NET Core 2.0 および Entity Framework Core 2.0 のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="a47e4-125">.NET Framework をターゲットとする ASP.NET Core 2.0 プロジェクトは、個々の NuGet パッケージを参照し続ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="a47e4-126">各 `<PackageReference />` ノードの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="a47e4-127">たとえば、.NET Framework をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される `<PackageReference />` ノードの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="a47e4-128">.NET Core CLI ツールの更新</span><span class="sxs-lookup"><span data-stu-id="a47e4-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="a47e4-129">*.csproj* ファイルで、`<DotNetCliToolReference />` ノードのそれぞれの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="a47e4-130">たとえば、.NET Core 2.0 をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される CLI ツールの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="a47e4-131">Package Target Fallback プロパティの名前変更</span><span class="sxs-lookup"><span data-stu-id="a47e4-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="a47e4-132">1.x プロジェクトの *.csproj* ファイルでは、`PackageTargetFallback` ノードと変数を使用していました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="a47e4-133">ノードと変数の両方を `AssetTargetFallback` に名前変更します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="a47e4-134">Program.cs の Main メソッドの更新</span><span class="sxs-lookup"><span data-stu-id="a47e4-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="a47e4-135">1.x プロジェクトでは、*Program.cs* の `Main` メソッドは次のようでした。</span><span class="sxs-lookup"><span data-stu-id="a47e4-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="a47e4-136">2.0 プロジェクトでは、*Program.cs* の `Main` メソッドは次のように簡素化されました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="a47e4-137">この新しい 2.0 のパターンの導入は強く推奨され、これらの動作には、[Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) のような製品機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="a47e4-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="a47e4-138">たとえば、パッケージ マネージャーのコンソール ウィンドウから `Update-Database` を実行したり、(ASP.NET Core 2.0 に変換されたプロジェクトで) コマンドラインから `dotnet ef database update` を実行したりすると、次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="a47e4-139">構成プロバイダーの追加</span><span class="sxs-lookup"><span data-stu-id="a47e4-139">Add configuration providers</span></span>
<span data-ttu-id="a47e4-140">1.x プロジェクトでは、アプリへの構成プロバイダーの追加は `Startup` コンストラクターを使用して実行しました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="a47e4-141">この手順には `ConfigurationBuilder` のインスタンスの作成、適用可能なプロバイダー (環境変数、アプリの設定など) の読み込み、`IConfigurationRoot` のメンバーの初期化などが伴いました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="a47e4-142">前の例では、`IHostingEnvironment.EnvironmentName` プロパティに一致する、*appsettings.json* とすべての *appsettings.\<EnvironmentName\>.json* ファイルの構成設定の `Configuration` メンバーを読み込んでいます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="a47e4-143">これらのファイルの場所は *Startup.cs* と同じパスです。</span><span class="sxs-lookup"><span data-stu-id="a47e4-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="a47e4-144">2.0 プロジェクトでは、1.x プロジェクトに固有の定型句による構成コードがバックグラウンドで実行されていました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="a47e4-145">たとえば、環境変数とアプリの設定は起動時に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="a47e4-146">同等の *Startup.cs* コードは、挿入されたインスタンスによって `IConfiguration` の初期化に削減されます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="a47e4-147">`WebHostBuilder.CreateDefaultBuilder` によって追加された既定のプロバイダーを削除するには、`ConfigureAppConfiguration` の内部の `IConfigurationBuilder.Sources` プロパティで `Clear` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="a47e4-148">プロバイダーを戻すには、*Program.cs* の `ConfigureAppConfiguration` メソッドを利用します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="a47e4-149">前のコード スニペットの `CreateDefaultBuilder` メソッドで使用される構成は、[こちら](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152)で確認できます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="a47e4-150">詳細については、「[ASP.NET Core の構成](xref:fundamentals/configuration/index)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a47e4-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="a47e4-151">データベース初期化コードの移動</span><span class="sxs-lookup"><span data-stu-id="a47e4-151">Move database initialization code</span></span>
<span data-ttu-id="a47e4-152">EF Core 1.x を利用する 1.x プロジェクトで、`dotnet ef migrations add` のようなコマンドは以下を行います。</span><span class="sxs-lookup"><span data-stu-id="a47e4-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="a47e4-153">`Startup` インスタンスをインスタンス化する</span><span class="sxs-lookup"><span data-stu-id="a47e4-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="a47e4-154">`ConfigureServices` メソッドを呼び出し、依存性の注入にすべてのサービスを登録する (`DbContext` タイプを含める)</span><span class="sxs-lookup"><span data-stu-id="a47e4-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="a47e4-155">その必要なタスクを実行する</span><span class="sxs-lookup"><span data-stu-id="a47e4-155">Performs its requisite tasks</span></span>

<span data-ttu-id="a47e4-156">EF Core 2.0 を使用する 2.0 プロジェクトでは、`Program.BuildWebHost` が呼び出され、アプリケーション サービスを取得します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="a47e4-157">1.x とは異なり、`Startup.Configure` を呼び出すという副作用が加わります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="a47e4-158">1.x アプリがその `Configure` メソッドでデータベース初期化コードを呼び出すと、予想外の問題が発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="a47e4-159">たとえば、データベースがまだ存在しない場合、EF Core 移行コマンド実行前にシード処理コードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="a47e4-160">この問題が原因で、データベースがまだ存在しない場合に `dotnet ef migrations list` コマンドが失敗します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="a47e4-161">*Startup.cs* の `Configure` メソッドで次の 1.x シード処理初期化コードを検討してください。</span><span class="sxs-lookup"><span data-stu-id="a47e4-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="a47e4-162">2.0 プロジェクトでは、*Program.cs* の `Main` メソッドに `SeedData.Initialize` 呼び出しを移動します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="a47e4-163">2.0 以降、`BuildWebHost` で Web ホストのビルドと構成以外を行うことは正しくない使用例となります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="a47e4-164">アプリケーションの実行に関するあらゆることは `BuildWebHost` の外側で、一般的には *Program.cs* の `Main` メソッドで処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="a47e4-165">Razor ビュー コンパイル設定の確認</span><span class="sxs-lookup"><span data-stu-id="a47e4-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="a47e4-166">ユーザーに最も重要なことは、アプリケーションを高速に起動することとパブリッシュされたバンドル数を少なくすることです。</span><span class="sxs-lookup"><span data-stu-id="a47e4-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="a47e4-167">これらの理由から、ASP.NET Core 2.0 では [Razor ビュー コンパイル](xref:mvc/views/view-compilation)が既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="a47e4-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="a47e4-168">`MvcRazorCompileOnPublish` プロパティを true に設定する必要がなくなりました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="a47e4-169">ビューのコンパイルを無効にしている場合を除き、*.csproj* ファイルからこのプロパティが削除されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="a47e4-170">.NET Framework をターゲットとする場合、継続して *.csproj* ファイルの [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet パッケージを明示的に参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="a47e4-171">Application Insights の "Light-Up" 機能の利用</span><span class="sxs-lookup"><span data-stu-id="a47e4-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="a47e4-172">アプリケーション パフォーマンスのインストルメンテーションは、楽に設定できることが重要です。</span><span class="sxs-lookup"><span data-stu-id="a47e4-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="a47e4-173">Visual Studio 2017 のツールの新しい [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" 機能が利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="a47e4-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="a47e4-174">Visual Studio 2017 で作成された ASP.NET Core 1.1 プロジェクトには、既定で Application Insights が追加されています。</span><span class="sxs-lookup"><span data-stu-id="a47e4-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="a47e4-175">*Program.cs* と *Startup.cs* 外で、Application Insights SDK を直接使用していない場合、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="a47e4-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="a47e4-176">.NET Core をターゲットにする場合、*.csproj* ファイルから次の `<PackageReference />` ノードを削除します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="a47e4-177">.NET Core をターゲットにする場合、*Program.cs* から `UseApplicationInsights` 拡張メソッド呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="a47e4-178">*_Layout.cshtml* から Application Insights のクライアント側 API 呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="a47e4-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="a47e4-179">これによって、次の 2 つのコード行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="a47e4-180">Application Insights SDK を直接使用している場合は、それを継続してください。</span><span class="sxs-lookup"><span data-stu-id="a47e4-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="a47e4-181">2.0 の[メタパッケージ](xref:fundamentals/metapackage)には、Application Insights の最新バージョンが含まれているので、以前のバージョンを参照している場合、パッケージのダウングレード エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a47e4-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="a47e4-182">認証/ID の機能強化の採用</span><span class="sxs-lookup"><span data-stu-id="a47e4-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="a47e4-183">ASP.NET Core 2.0 には、新しい認証モデルと ASP.NET Core ID への大幅な変更があります。</span><span class="sxs-lookup"><span data-stu-id="a47e4-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="a47e4-184">個々のユーザー アカウントを有効にしてプロジェクトを作成した場合や認証または ID を手動で追加した場合、「[ASP.NET Core 2.0 への認証と ID の移行](xref:migration/1x-to-2x/identity-2x)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a47e4-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a47e4-185">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="a47e4-185">Additional resources</span></span>

* [<span data-ttu-id="a47e4-186">ASP.NET Core 2.0 での互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="a47e4-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
