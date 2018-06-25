---
title: ASP.NET Core での Razor ファイルのコンパイルとプリコンパイル
author: rick-anderson
description: Razor ファイルをプリコンパイルする利点と、ASP.NET Core アプリケーションで Razor ファイルのプリコンパイルを実行する方法について説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 6ef450a24f57c721021f77f6df5088574caa2645
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274041"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="913bd-103">ASP.NET Core での Razor ファイルのコンパイル</span><span class="sxs-lookup"><span data-stu-id="913bd-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="913bd-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="913bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="913bd-105">関連する MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="913bd-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="913bd-106">ビルド時の Razor ファイルの公開はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="913bd-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="913bd-107">Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。</span><span class="sxs-lookup"><span data-stu-id="913bd-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="913bd-108">関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="913bd-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="913bd-109">ビルド時の Razor ファイルの公開はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="913bd-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="913bd-110">Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。</span><span class="sxs-lookup"><span data-stu-id="913bd-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="913bd-111">関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="913bd-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="913bd-112">Razor ファイルは、[Razor SDK](xref:razor-pages/sdk) を使用してビルド時と公開時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="913bd-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="913bd-113">プリコンパイルに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="913bd-113">Precompilation considerations</span></span>

<span data-ttu-id="913bd-114">Razor ファイルのプリコンパイルの副作用を次に示します。</span><span class="sxs-lookup"><span data-stu-id="913bd-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="913bd-115">公開されるバンドルが小さくなります</span><span class="sxs-lookup"><span data-stu-id="913bd-115">A smaller published bundle</span></span>
* <span data-ttu-id="913bd-116">起動時間が短縮されます</span><span class="sxs-lookup"><span data-stu-id="913bd-116">A faster startup time</span></span>
* <span data-ttu-id="913bd-117">Razor ファイルを編集することはできません。公開されるバンドルには関連付けられたコンテンツがありません。</span><span class="sxs-lookup"><span data-stu-id="913bd-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="913bd-118">プリコンパイル済みファイルの展開</span><span class="sxs-lookup"><span data-stu-id="913bd-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="913bd-119">Razor ファイルのビルドおよび発行時のコンパイルは、既定で Razor SDK によって有効になっています。</span><span class="sxs-lookup"><span data-stu-id="913bd-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="913bd-120">更新後の Razor ファイルの編集は、ビルド時にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="913bd-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="913bd-121">既定では、*.cshtml* ファイルなしのコンパイルされた *Views.dll* のみがアプリケーションと展開されます。</span><span class="sxs-lookup"><span data-stu-id="913bd-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="913bd-122">Razor SDK は、プロジェクト ファイルにプリコンパイル固有のプロパティが設定されていない場合のみ有効です。</span><span class="sxs-lookup"><span data-stu-id="913bd-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="913bd-123">たとえば、*.csproj* ファイルの `MvcRazorCompileOnPublish` プロパティを `true` に設定して、Razor SDK を無効にします。</span><span class="sxs-lookup"><span data-stu-id="913bd-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="913bd-124">プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="913bd-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

<span data-ttu-id="913bd-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span><span class="sxs-lookup"><span data-stu-id="913bd-125">[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]</span></span>

<span data-ttu-id="913bd-126">プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="913bd-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="913bd-127">ASP.NET Core 2.x プロジェクト テンプレートは既定で、暗黙的に `MvcRazorCompileOnPublish` プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="913bd-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="913bd-128">そのため、この要素は *.csproj* ファイルから安全に削除できます。</span><span class="sxs-lookup"><span data-stu-id="913bd-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="913bd-129">ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、Razor ファイルのプリコンパイルを使用できません。</span><span class="sxs-lookup"><span data-stu-id="913bd-129">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="913bd-130">`MvcRazorCompileOnPublish` プロパティを `true` に設定して、[Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="913bd-130">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="913bd-131">次の *.csproj* サンプルでは、これらの設定が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="913bd-131">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="913bd-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span><span class="sxs-lookup"><span data-stu-id="913bd-132">[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="913bd-133">[.NET Core CLI publish コマンド](/dotnet/core/tools/dotnet-publish)を使用して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。</span><span class="sxs-lookup"><span data-stu-id="913bd-133">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="913bd-134">たとえば、プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="913bd-134">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="913bd-135">コンパイル済みの Razor ファイルを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。</span><span class="sxs-lookup"><span data-stu-id="913bd-135">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="913bd-136">たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。</span><span class="sxs-lookup"><span data-stu-id="913bd-136">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="913bd-138">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="913bd-138">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
::: moniker-end