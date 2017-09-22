---
title: "ASP.NET Core 1.x から 2.0 への移行"
author: scottaddie
description: "この記事では、ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に移行する前提条件と最も一般的な手順について説明します。"
keywords: "ASP.NET Core,移行"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: 541774d46bbf570ee860c72fdff5cece364935df
ms.sourcegitcommit: 55759ae80e7039036a7c6da8e3806f7c88ade325
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="882b9-104">ASP.NET Core 1.x から ASP.NET Core 2.0 への移行</span><span class="sxs-lookup"><span data-stu-id="882b9-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="882b9-105">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="882b9-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="882b9-106">この記事では、既存の ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に更新する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="882b9-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="882b9-107">アプリケーションを ASP.NET Core 2.0 に移行すると、[多数の新機能を利用したり、パフォーマンスを向上させたりすることができます](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="882b9-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="882b9-108">既存の ASP.NET Core 1.x アプリケーションは、バージョン固有のプロジェクト テンプレートには基づいていません。</span><span class="sxs-lookup"><span data-stu-id="882b9-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="882b9-109">ASP.NET Core のフレームワークの進化に伴い、プロジェクト テンプレートと、それに含まれるスタート コードも進化しています。</span><span class="sxs-lookup"><span data-stu-id="882b9-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="882b9-110">ASP.NET Core のフレームワークを更新するだけでなく、アプリケーションのコードも更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="882b9-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="882b9-111">Prerequisites</span></span>
<span data-ttu-id="882b9-112">「[ASP.NET Core の概要](xref:getting-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="882b9-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="882b9-113">ターゲット フレームワーク モニカー (TFM) の更新</span><span class="sxs-lookup"><span data-stu-id="882b9-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="882b9-114">.NET Core をターゲットとするプロジェクトでは、.NET Core 2.0 以上のバージョンの [TFM](/dotnet/standard/frameworks#referring-to-frameworks) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="882b9-115">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `netcoreapp2.0` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="882b9-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="882b9-116">.NET Framework をターゲットとするプロジェクトでは、.NET Framework 4.6.1 以上の TFM バージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-116">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="882b9-117">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `net461` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="882b9-117">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="882b9-118">.NET Core 1.x よりセキュリティ上外部からアクセスできるところの多い .NET core 2.0</span><span class="sxs-lookup"><span data-stu-id="882b9-118">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="882b9-119">NET Core 1.x に API がないためだけに .NET Framework をターゲットにする場合、.NET Core 2.0 をターゲットにすると機能します。</span><span class="sxs-lookup"><span data-stu-id="882b9-119">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="882b9-120">global.json での .NET Core SDK バージョンの更新</span><span class="sxs-lookup"><span data-stu-id="882b9-120">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="882b9-121">特定の .NET Core SDK バージョンをターゲットとするよう、ソリューションが [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) ファイルに依存する場合、コンピューターにインストールされている 2.0 バージョンを使用するよう、その `version` プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="882b9-121">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="882b9-122">パッケージ参照の更新</span><span class="sxs-lookup"><span data-stu-id="882b9-122">Update package references</span></span>
<span data-ttu-id="882b9-123">1.x プロジェクト内の *.csproj* ファイルには、プロジェクトによって使用される各 NuGet パッケージの一覧があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-123">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="882b9-124">.NET Core 2.0 をターゲットとする ASP.NET Core 2.0 プロジェクトでは、*.csproj* ファイル内の 1 つの[メタパッケージ](xref:fundamentals/metapackage)への参照によってパッケージのコレクションが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="882b9-124">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="882b9-125">メタパッケージには、ASP.NET Core 2.0 および Entity Framework Core 2.0 のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="882b9-125">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="882b9-126">.NET Framework をターゲットとする ASP.NET Core 2.0 プロジェクトは、個々の NuGet パッケージを参照し続ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-126">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="882b9-127">各 `<PackageReference />` ノードの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="882b9-127">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="882b9-128">たとえば、.NET Framework をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される `<PackageReference />` ノードの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="882b9-128">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="882b9-129">.NET Core CLI ツールの更新</span><span class="sxs-lookup"><span data-stu-id="882b9-129">Update .NET Core CLI tools</span></span>
<span data-ttu-id="882b9-130">*.csproj* ファイルで、`<DotNetCliToolReference />` ノードのそれぞれの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="882b9-130">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="882b9-131">たとえば、.NET Core 2.0 をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される CLI ツールの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="882b9-131">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="882b9-132">Package Target Fallback プロパティの名前変更</span><span class="sxs-lookup"><span data-stu-id="882b9-132">Rename Package Target Fallback property</span></span>
<span data-ttu-id="882b9-133">1.x プロジェクトの *.csproj* ファイルでは、`PackageTargetFallback` ノードと変数を使用していました。</span><span class="sxs-lookup"><span data-stu-id="882b9-133">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="882b9-134">ノードと変数の両方を `AssetTargetFallback` に名前変更します。</span><span class="sxs-lookup"><span data-stu-id="882b9-134">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="882b9-135">Program.cs の Main メソッドの更新</span><span class="sxs-lookup"><span data-stu-id="882b9-135">Update Main method in Program.cs</span></span>
<span data-ttu-id="882b9-136">1.x プロジェクトでは、*Program.cs* の `Main` メソッドは次のようでした。</span><span class="sxs-lookup"><span data-stu-id="882b9-136">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="882b9-137">2.0 プロジェクトでは、*Program.cs* の `Main` メソッドは次のように簡素化されました。</span><span class="sxs-lookup"><span data-stu-id="882b9-137">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="882b9-138">この新しい 2.0 のパターンの導入は強く推奨され、これらの動作には、[Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) のような製品機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="882b9-138">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="882b9-139">たとえば、パッケージ マネージャーのコンソール ウィンドウから `Update-Database` を実行したり、(ASP.NET Core 2.0 に変換されたプロジェクトで) コマンドラインから `dotnet ef database update` を実行したりすると、次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="882b9-139">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="882b9-140">データベース初期化コードの移動</span><span class="sxs-lookup"><span data-stu-id="882b9-140">Move database initialization code</span></span>
<span data-ttu-id="882b9-141">EF Core 1.x を利用する 1.x プロジェクトで、`dotnet ef migrations add` のようなコマンドは以下を行います。</span><span class="sxs-lookup"><span data-stu-id="882b9-141">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="882b9-142">`Startup` インスタンスをインスタンス化する</span><span class="sxs-lookup"><span data-stu-id="882b9-142">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="882b9-143">`ConfigureServices` メソッドを呼び出し、依存性の注入にすべてのサービスを登録する (`DbContext` タイプを含める)</span><span class="sxs-lookup"><span data-stu-id="882b9-143">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="882b9-144">その必要なタスクを実行する</span><span class="sxs-lookup"><span data-stu-id="882b9-144">Performs its requisite tasks</span></span>

<span data-ttu-id="882b9-145">EF Core 2.0 を使用する 2.0 プロジェクトでは、`Program.BuildWebHost` が呼び出され、アプリケーション サービスを取得します。</span><span class="sxs-lookup"><span data-stu-id="882b9-145">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="882b9-146">1.x とは異なり、`Startup.Configure` を呼び出すという副作用が加わります。</span><span class="sxs-lookup"><span data-stu-id="882b9-146">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="882b9-147">1.x アプリがその `Configure` メソッドでデータベース初期化コードを呼び出すと、予想外の問題が発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="882b9-147">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="882b9-148">たとえば、データベースがまだ存在しない場合、EF Core 移行コマンド実行前にシード処理コードが実行されます。</span><span class="sxs-lookup"><span data-stu-id="882b9-148">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="882b9-149">この問題が原因で、データベースがまだ存在しない場合に `dotnet ef migrations list` コマンドが失敗します。</span><span class="sxs-lookup"><span data-stu-id="882b9-149">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="882b9-150">*Startup.cs* の `Configure` メソッドで次の 1.x シード処理初期化コードを検討してください。</span><span class="sxs-lookup"><span data-stu-id="882b9-150">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="882b9-151">2.0 プロジェクトでは、*Program.cs* の `Main` メソッドに `SeedData.Initialize` 呼び出しを移動します。</span><span class="sxs-lookup"><span data-stu-id="882b9-151">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="882b9-152">2.0 以降、`BuildWebHost` で Web ホストのビルドと構成以外を行うことは正しくない使用例となります。</span><span class="sxs-lookup"><span data-stu-id="882b9-152">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="882b9-153">アプリケーションの実行に関するあらゆることは `BuildWebHost` の外側で、一般的には *Program.cs* の `Main` メソッドで処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-153">Anything that is about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="882b9-154">Razor ビュー コンパイル設定の確認</span><span class="sxs-lookup"><span data-stu-id="882b9-154">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="882b9-155">ユーザーに最も重要なことは、アプリケーションを高速に起動することとパブリッシュされたバンドル数を少なくすることです。</span><span class="sxs-lookup"><span data-stu-id="882b9-155">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="882b9-156">これらの理由から、ASP.NET Core 2.0 では [Razor ビュー コンパイル](xref:mvc/views/view-compilation)が既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="882b9-156">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="882b9-157">`MvcRazorCompileOnPublish` プロパティを true に設定する必要がなくなりました。</span><span class="sxs-lookup"><span data-stu-id="882b9-157">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="882b9-158">ビューのコンパイルを無効にしている場合を除き、*.csproj* ファイルからこのプロパティが削除されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-158">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="882b9-159">.NET Framework をターゲットとする場合、継続して *.csproj* ファイルの [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet パッケージを明示的に参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-159">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="882b9-160">Application Insights の "Light-Up" 機能の利用</span><span class="sxs-lookup"><span data-stu-id="882b9-160">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="882b9-161">アプリケーション パフォーマンスのインストルメンテーションは、楽に設定できることが重要です。</span><span class="sxs-lookup"><span data-stu-id="882b9-161">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="882b9-162">Visual Studio 2017 のツールの新しい [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" 機能が利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="882b9-162">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="882b9-163">Visual Studio 2017 で作成された ASP.NET Core 1.1 プロジェクトには、既定で Application Insights が追加されています。</span><span class="sxs-lookup"><span data-stu-id="882b9-163">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="882b9-164">*Program.cs* と *Startup.cs* 外で、Application Insights SDK を直接使用していない場合、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="882b9-164">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="882b9-165">*.csproj* ファイルから次の `<PackageReference />` ノードを削除します。</span><span class="sxs-lookup"><span data-stu-id="882b9-165">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="882b9-166">*Program.cs* から `UseApplicationInsights` 拡張メソッド呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="882b9-166">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="882b9-167">*_Layout.cshtml* から Application Insights のクライアント側 API 呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="882b9-167">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="882b9-168">これによって、次の 2 つのコード行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="882b9-168">It comprises the following two lines of code:</span></span>

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="882b9-169">Application Insights SDK を直接使用している場合は、それを継続してください。</span><span class="sxs-lookup"><span data-stu-id="882b9-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="882b9-170">2.0 の[メタパッケージ](xref:fundamentals/metapackage)には、Application Insights の最新バージョンが含まれているので、以前のバージョンを参照している場合、パッケージのダウングレード エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="882b9-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="882b9-171">認証/ID の機能強化の採用</span><span class="sxs-lookup"><span data-stu-id="882b9-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="882b9-172">ASP.NET Core 2.0 には、新しい認証モデルと ASP.NET Core ID への大幅な変更があります。</span><span class="sxs-lookup"><span data-stu-id="882b9-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="882b9-173">個々のユーザー アカウントを有効にしてプロジェクトを作成した場合や認証または ID を手動で追加した場合、「[ASP.NET Core 2.0 への認証と ID の移行](xref:migration/1x-to-2x/identity-2x)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="882b9-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="882b9-174">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="882b9-174">Additional Resources</span></span>
- [<span data-ttu-id="882b9-175">ASP.NET Core 2.0 での互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="882b9-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
