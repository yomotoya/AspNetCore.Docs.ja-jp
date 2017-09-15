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
ms.openlocfilehash: db277ee6b079eab973a565983d6661bf95fce2e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="f5027-104">ASP.NET Core 1.x から ASP.NET Core 2.0 への移行</span><span class="sxs-lookup"><span data-stu-id="f5027-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="f5027-105">作成者: [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f5027-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f5027-106">この記事では、既存の ASP.NET Core 1.x プロジェクトを ASP.NET Core 2.0 に更新する手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="f5027-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="f5027-107">アプリケーションを ASP.NET Core 2.0 に移行すると、[多数の新機能を利用したり、パフォーマンスを向上させたりすることができます](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="f5027-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="f5027-108">既存の ASP.NET Core 1.x アプリケーションは、バージョン固有のプロジェクト テンプレートには基づいていません。</span><span class="sxs-lookup"><span data-stu-id="f5027-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="f5027-109">ASP.NET Core のフレームワークの進化に伴い、プロジェクト テンプレートと、それに含まれるスタート コードも進化しています。</span><span class="sxs-lookup"><span data-stu-id="f5027-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="f5027-110">ASP.NET Core のフレームワークを更新するだけでなく、アプリケーションのコードも更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f5027-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f5027-111">Prerequisites</span></span>
<span data-ttu-id="f5027-112">「[ASP.NET Core の概要](xref:getting-started)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f5027-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="f5027-113">ターゲット フレームワーク モニカー (TFM) の更新</span><span class="sxs-lookup"><span data-stu-id="f5027-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="f5027-114">.NET Core をターゲットとするプロジェクトでは、.NET Core 2.0 以上のバージョンの [TFM](/dotnet/standard/frameworks#referring-to-frameworks) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="f5027-115">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `netcoreapp2.0` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f5027-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="f5027-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="f5027-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="f5027-117">.NET Framework をターゲットとするプロジェクトでは、.NET Framework 4.6.1 以上の TFM バージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="f5027-118">*.csproj* ファイルで `<TargetFramework>` ノードを探し、その内側のテキストを `net461` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f5027-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="f5027-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="f5027-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="f5027-120">.NET Core 1.x よりセキュリティ上外部からアクセスできるところの多い .NET core 2.0</span><span class="sxs-lookup"><span data-stu-id="f5027-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="f5027-121">NET Core 1.x に API がないためだけに .NET Framework をターゲットにする場合、.NET Core 2.0 をターゲットにすると機能します。</span><span class="sxs-lookup"><span data-stu-id="f5027-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="f5027-122">global.json での .NET Core SDK バージョンの更新</span><span class="sxs-lookup"><span data-stu-id="f5027-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="f5027-123">特定の .NET Core SDK バージョンをターゲットとするよう、ソリューションが [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) ファイルに依存する場合、コンピューターにインストールされている 2.0 バージョンを使用するよう、その `version` プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="f5027-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="f5027-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="f5027-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="f5027-125">パッケージ参照の更新</span><span class="sxs-lookup"><span data-stu-id="f5027-125">Update package references</span></span>
<span data-ttu-id="f5027-126">1.x プロジェクト内の *.csproj* ファイルには、プロジェクトによって使用される各 NuGet パッケージの一覧があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="f5027-127">.NET Core 2.0 をターゲットとする ASP.NET Core 2.0 プロジェクトでは、*.csproj* ファイル内の 1 つの[メタパッケージ](xref:fundamentals/metapackage)への参照によってパッケージのコレクションが置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="f5027-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="f5027-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="f5027-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="f5027-129">メタパッケージには、ASP.NET Core 2.0 および Entity Framework Core 2.0 のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f5027-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="f5027-130">.NET Framework をターゲットとする ASP.NET Core 2.0 プロジェクトは、個々の NuGet パッケージを参照し続ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="f5027-131">各 `<PackageReference />` ノードの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="f5027-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="f5027-132">たとえば、.NET Framework をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される `<PackageReference />` ノードの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f5027-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="f5027-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="f5027-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="f5027-134">.NET Core CLI ツールの更新</span><span class="sxs-lookup"><span data-stu-id="f5027-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="f5027-135">*.csproj* ファイルで、`<DotNetCliToolReference />` ノードのそれぞれの `Version` 属性を 2.0.0 に更新します。</span><span class="sxs-lookup"><span data-stu-id="f5027-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="f5027-136">たとえば、.NET Core 2.0 をターゲットとする一般的な ASP.NET Core 2.0 プロジェクトで使用される CLI ツールの一覧を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f5027-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="f5027-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="f5027-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="f5027-138">Package Target Fallback プロパティの名前変更</span><span class="sxs-lookup"><span data-stu-id="f5027-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="f5027-139">1.x プロジェクトの *.csproj* ファイルでは、`PackageTargetFallback` ノードと変数を使用していました。</span><span class="sxs-lookup"><span data-stu-id="f5027-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="f5027-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="f5027-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="f5027-141">ノードと変数の両方を `AssetTargetFallback` に名前変更します。</span><span class="sxs-lookup"><span data-stu-id="f5027-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="f5027-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="f5027-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="f5027-143">Program.cs の Main メソッドの更新</span><span class="sxs-lookup"><span data-stu-id="f5027-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="f5027-144">1.x プロジェクトでは、*Program.cs* の `Main` メソッドは次のようでした。</span><span class="sxs-lookup"><span data-stu-id="f5027-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="f5027-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="f5027-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="f5027-146">2.0 プロジェクトでは、*Program.cs* の `Main` メソッドは次のように簡素化されました。</span><span class="sxs-lookup"><span data-stu-id="f5027-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="f5027-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="f5027-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="f5027-148">この新しい 2.0 のパターンの導入は強く推奨され、これらの動作には、[Entity Framework Core Migrations](xref:data/ef-mvc/migrations) などのような製品機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="f5027-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="f5027-149">たとえば、パッケージ マネージャーのコンソール ウィンドウから `Update-Database` を実行したり、(ASP.NET Core 2.0 に変換されたプロジェクトで) コマンドラインから `dotnet ef database update` を実行したりすると、次のエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="f5027-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="f5027-150">Razor ビュー コンパイル設定の確認</span><span class="sxs-lookup"><span data-stu-id="f5027-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="f5027-151">ユーザーに最も重要なことは、アプリケーションを高速に起動することとパブリッシュされたバンドル数を少なくすることです。</span><span class="sxs-lookup"><span data-stu-id="f5027-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="f5027-152">これらの理由から、ASP.NET Core 2.0 では [Razor ビュー コンパイル](xref:mvc/views/view-compilation)が既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="f5027-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="f5027-153">`MvcRazorCompileOnPublish` プロパティを true に設定する必要がなくなりました。</span><span class="sxs-lookup"><span data-stu-id="f5027-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="f5027-154">ビューのコンパイルを無効にしている場合を除き、*.csproj* ファイルからこのプロパティが削除されている場合があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="f5027-155">.NET Framework をターゲットとする場合、継続して *.csproj* ファイルの [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet パッケージを明示的に参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="f5027-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="f5027-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="f5027-157">Application Insights の "Light-Up" 機能の利用</span><span class="sxs-lookup"><span data-stu-id="f5027-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="f5027-158">アプリケーション パフォーマンスのインストルメンテーションは、楽に設定できることが重要です。</span><span class="sxs-lookup"><span data-stu-id="f5027-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="f5027-159">Visual Studio 2017 のツールの新しい [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" 機能が利用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f5027-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="f5027-160">Visual Studio 2017 で作成された ASP.NET Core 1.1 プロジェクトには、既定で Application Insights が追加されています。</span><span class="sxs-lookup"><span data-stu-id="f5027-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="f5027-161">*Program.cs* と *Startup.cs* 外で、Application Insights SDK を直接使用していない場合、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="f5027-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="f5027-162">*.csproj* ファイルから次の `<PackageReference />` ノードを削除します。</span><span class="sxs-lookup"><span data-stu-id="f5027-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="f5027-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="f5027-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="f5027-164">*Program.cs* から `UseApplicationInsights` 拡張メソッド呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="f5027-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="f5027-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="f5027-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="f5027-166">*_Layout.cshtml* から Application Insights のクライアント側 API 呼び出しを削除します。</span><span class="sxs-lookup"><span data-stu-id="f5027-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="f5027-167">これによって、次の 2 つのコード行が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f5027-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="f5027-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="f5027-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="f5027-169">Application Insights SDK を直接使用している場合は、それを継続してください。</span><span class="sxs-lookup"><span data-stu-id="f5027-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="f5027-170">2.0 の[メタパッケージ](xref:fundamentals/metapackage)には、Application Insights の最新バージョンが含まれているので、以前のバージョンを参照している場合、パッケージのダウングレード エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f5027-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="f5027-171">認証/ID の機能強化の採用</span><span class="sxs-lookup"><span data-stu-id="f5027-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="f5027-172">ASP.NET Core 2.0 には、新しい認証モデルと ASP.NET Core ID への大幅な変更があります。</span><span class="sxs-lookup"><span data-stu-id="f5027-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="f5027-173">個々のユーザー アカウントを有効にしてプロジェクトを作成した場合や認証または ID を手動で追加した場合、「[ASP.NET Core 2.0 への認証と ID の移行](xref:migration/1x-to-2x/identity-2x)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f5027-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f5027-174">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="f5027-174">Additional Resources</span></span>
- [<span data-ttu-id="f5027-175">ASP.NET Core 2.0 での互換性に影響する変更点</span><span class="sxs-lookup"><span data-stu-id="f5027-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
