---
title: ASP.NET Core の Razor SDK
author: Rick-Anderson
description: ASP.NET Core の Razor ページを使用して、ページのコーディングに重点を置いたシナリオをより簡略化して、MVC を使用する場合よりも生産性を高める方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751032"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="6389d-103">ASP.NET Core の Razor SDK</span><span class="sxs-lookup"><span data-stu-id="6389d-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="6389d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6389d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6389d-105">[!INCLUDE[](~/includes/2.1-SDK.md)]が含まれています、 `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK)。</span><span class="sxs-lookup"><span data-stu-id="6389d-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="6389d-106">Razor SDK とは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6389d-106">The Razor SDK:</span></span>

* <span data-ttu-id="6389d-107">ASP.NET Core MVC ベース プロジェクト用の [Razor](xref:mvc/views/razor) ファイルを含むプロジェクトのビルド、パッケージ、および発行に関わる表示と操作を標準化できます。</span><span class="sxs-lookup"><span data-stu-id="6389d-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="6389d-108">Razor ファイルのコンパイルのカスタマイズを可能にする事前定義されたターゲット、プロパティ、項目のセットを含みます。</span><span class="sxs-lookup"><span data-stu-id="6389d-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6389d-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6389d-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="6389d-110">Razor SDK を使用する</span><span class="sxs-lookup"><span data-stu-id="6389d-110">Using the Razor SDK</span></span>

<span data-ttu-id="6389d-111">ほとんどの web アプリは、Razor の SDK を明示的に参照する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6389d-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="6389d-112">Razor SDK を使用して Razor ビューまたは Razor ページを含むクラス ライブラリを構築する方法は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="6389d-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="6389d-113">`Microsoft.NET.Sdk.Razor` の代わりに `Microsoft.NET.Sdk` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="6389d-114">通常、パッケージ参照を`Microsoft.AspNetCore.Mvc`ビルドし、Razor ページと Razor ビュー コンパイルに必要な追加の依存関係を受信するが必要です。</span><span class="sxs-lookup"><span data-stu-id="6389d-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="6389d-115">少なくとも、プロジェクトへのパッケージ参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6389d-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="6389d-116">`Microsoft.AspNetCore.Razor.Design`パッケージがプロジェクトに、Razor コンパイル タスクとターゲットを提供します。</span><span class="sxs-lookup"><span data-stu-id="6389d-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="6389d-117">前述のパッケージは、`Microsoft.AspNetCore.Mvc` に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="6389d-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="6389d-118">次のマークアップは、Razor ファイルの ASP.NET Core Razor ページ アプリをビルドする Razor SDK を使用するプロジェクト ファイルを示します。</span><span class="sxs-lookup"><span data-stu-id="6389d-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="6389d-119">`Microsoft.AspNetCore.Razor.Design`と`Microsoft.AspNetCore.Mvc.Razor.Extensions`でパッケージが含まれている、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。</span><span class="sxs-lookup"><span data-stu-id="6389d-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="6389d-120">ただし、バージョンのない`Microsoft.AspNetCore.App`パッケージ参照は、の最新バージョンが含まれていないアプリへのメタパッケージ`Microsoft.AspNetCore.Razor.Design`します。</span><span class="sxs-lookup"><span data-stu-id="6389d-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="6389d-121">プロジェクトは、一貫したバージョンを参照する必要があります`Microsoft.AspNetCore.Razor.Design`(または`Microsoft.AspNetCore.Mvc`) Razor の最新のビルド時の修正プログラムが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="6389d-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="6389d-122">詳細については、次を参照してください。[この GitHub の問題](https://github.com/aspnet/Razor/issues/2553)します。</span><span class="sxs-lookup"><span data-stu-id="6389d-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="6389d-123">プロパティ</span><span class="sxs-lookup"><span data-stu-id="6389d-123">Properties</span></span>

<span data-ttu-id="6389d-124">Razor の SDK の動作は、プロジェクトをビルドする際に次のプロパティにより制御されます。</span><span class="sxs-lookup"><span data-stu-id="6389d-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="6389d-125">`RazorCompileOnBuild` &ndash; ときに`true`、コンパイルし、プロジェクトのビルドの一部として Razor アセンブリを生成します。</span><span class="sxs-lookup"><span data-stu-id="6389d-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="6389d-126">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-126">Defaults to `true`.</span></span>
* <span data-ttu-id="6389d-127">`RazorCompileOnPublish` &ndash; ときに`true`、コンパイルし、プロジェクトの発行の一部として Razor アセンブリを生成します。</span><span class="sxs-lookup"><span data-stu-id="6389d-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="6389d-128">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-128">Defaults to `true`.</span></span>

<span data-ttu-id="6389d-129">プロパティと、次の表の項目は、入力と剃刀 SDK への出力の構成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="6389d-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="6389d-130">項目</span><span class="sxs-lookup"><span data-stu-id="6389d-130">Items</span></span> | <span data-ttu-id="6389d-131">説明</span><span class="sxs-lookup"><span data-stu-id="6389d-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="6389d-132">コードの生成対象に入力する項目要素 (*.cshtml* ファイル) です。</span><span class="sxs-lookup"><span data-stu-id="6389d-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="6389d-133">項目要素 (*.cs*ファイル)、Razor コンパイルのターゲットに入力します。</span><span class="sxs-lookup"><span data-stu-id="6389d-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="6389d-134">Razor アセンブリに追加でコンパイルするファイルを指定するには、この ItemGroup を使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="6389d-135">Razor アセンブリ用の属性をコード生成するために使用する項目要素です。</span><span class="sxs-lookup"><span data-stu-id="6389d-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="6389d-136">例:</span><span class="sxs-lookup"><span data-stu-id="6389d-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="6389d-137">項目の要素が生成された Razor アセンブリに埋め込みリソースとして追加します。</span><span class="sxs-lookup"><span data-stu-id="6389d-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="6389d-138">プロパティ</span><span class="sxs-lookup"><span data-stu-id="6389d-138">Property</span></span> | <span data-ttu-id="6389d-139">説明</span><span class="sxs-lookup"><span data-stu-id="6389d-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="6389d-140">Razor によって生成されたアセンブリの (拡張子なしの) ファイル名です。</span><span class="sxs-lookup"><span data-stu-id="6389d-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="6389d-141">Razor の出力ディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="6389d-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="6389d-142">Razor アセンブリをビルドするために使用するツールセットを決定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="6389d-143">有効な値は `Implicit`、`RazorSDK`、`PrecompilationTool` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="6389d-144">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="6389d-144">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="6389d-145">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-145">Default is `true`.</span></span> <span data-ttu-id="6389d-146">ときに`true`が含まれています*web.config*、 *.json*、および *.cshtml*プロジェクト内のコンテンツとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="6389d-146">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="6389d-147">使用して参照されている場合`Microsoft.NET.Sdk.Web`、ファイル*wwwroot*し、構成ファイルも含まれています。</span><span class="sxs-lookup"><span data-stu-id="6389d-147">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="6389d-148">`true` の場合、`RazorGenerate` 項目の `Content` 項目の *.cshtml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6389d-148">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="6389d-149">ときに`true`、生成、 *.cs*ファイルで指定された属性を格納している`RazorAssemblyAttribute`コンパイル出力ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="6389d-149">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="6389d-150">`true` の場合、`RazorAssemblyAttribute` にアセンブリ属性の既定のセットが追加されます。</span><span class="sxs-lookup"><span data-stu-id="6389d-150">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="6389d-151">ときに`true`、コピー`RazorGenerate`項目 (*.cshtml*) ファイルを発行ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="6389d-151">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="6389d-152">通常、Razor ファイルは必要ありません発行されたアプリの場合、それらのビルド時または発行時のコンパイルに参加します。</span><span class="sxs-lookup"><span data-stu-id="6389d-152">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="6389d-153">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-153">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="6389d-154">`true` の場合、発行ディレクトリに参照アセンブリ項目がコピーされます。</span><span class="sxs-lookup"><span data-stu-id="6389d-154">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="6389d-155">通常、参照アセンブリは必要ありません発行されたアプリのビルド時または発行時に Razor コンパイルが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="6389d-155">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="6389d-156">設定`true`発行されたアプリケーションがランタイムのコンパイルを必要とする場合。</span><span class="sxs-lookup"><span data-stu-id="6389d-156">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="6389d-157">などの値を設定`true`アプリを変更する場合 *.cshtml*実行時にファイルや埋め込みのビューを使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-157">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="6389d-158">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-158">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="6389d-159">ときに`true`、すべての Razor コンテンツ アイテム (*.cshtml*ファイル)、生成された NuGet パッケージに含める対象としてマークされました。</span><span class="sxs-lookup"><span data-stu-id="6389d-159">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="6389d-160">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-160">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="6389d-161">`true` の場合、生成された Razor アセンブリに、埋め込みファイルとして RazorGenerate (*.cshtml*) 項目が追加されます。</span><span class="sxs-lookup"><span data-stu-id="6389d-161">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="6389d-162">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="6389d-162">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="6389d-163">`true` の場合、コードの生成作業をオフロードするために、永続的なビルド サーバーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="6389d-163">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="6389d-164">既定値は、`UseSharedCompilation` の値です。</span><span class="sxs-lookup"><span data-stu-id="6389d-164">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="6389d-165">プロパティの詳細については、「[MSBuild プロパティ](/visualstudio/msbuild/msbuild-properties)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6389d-165">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="6389d-166">ターゲット</span><span class="sxs-lookup"><span data-stu-id="6389d-166">Targets</span></span>

<span data-ttu-id="6389d-167">Razor SDK では、次の 2 つの主要なターゲットが定義されています。</span><span class="sxs-lookup"><span data-stu-id="6389d-167">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="6389d-168">`RazorGenerate` &ndash; コード生成 *.cs*ファイルから`RazorGenerate`項目要素。</span><span class="sxs-lookup"><span data-stu-id="6389d-168">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="6389d-169">このターゲットの前後に実行する追加のターゲットを指定するには、`RazorGenerateDependsOn` プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-169">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="6389d-170">`RazorCompile` &ndash; 生成されたコンパイル *.cs* Razor アセンブリへのファイルします。</span><span class="sxs-lookup"><span data-stu-id="6389d-170">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="6389d-171">このターゲットの前後に実行する追加のターゲットを指定するには、`RazorCompileDependsOn` を使用します。</span><span class="sxs-lookup"><span data-stu-id="6389d-171">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="6389d-172">Razor ビューの実行時のコンパイル</span><span class="sxs-lookup"><span data-stu-id="6389d-172">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="6389d-173">既定では、Razor SDK は、実行時のコンパイルを実行するために必要な参照アセンブリを公開しません。</span><span class="sxs-lookup"><span data-stu-id="6389d-173">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="6389d-174">この結果、アプリケーション モデルが実行時のコンパイルに依存している場合には、コンパイルが失敗します。たとえば、アプリが公開後に埋め込まれたビューを使用したり、ビューを変更したりする場合などです。</span><span class="sxs-lookup"><span data-stu-id="6389d-174">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="6389d-175">`CopyRefAssembliesToPublishDirectory` を `true` に設定して、参照アセンブリの公開を続行します。</span><span class="sxs-lookup"><span data-stu-id="6389d-175">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="6389d-176">Web アプリの場合、アプリが対象とすることを確認して、 `Microsoft.NET.Sdk.Web` SDK。</span><span class="sxs-lookup"><span data-stu-id="6389d-176">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>
