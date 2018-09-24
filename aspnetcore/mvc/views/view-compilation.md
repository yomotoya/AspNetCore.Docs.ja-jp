---
title: ASP.NET Core での Razor ファイルのコンパイルとプリコンパイル
author: rick-anderson
description: Razor ファイルをプリコンパイルする利点と、ASP.NET Core アプリケーションで Razor ファイルのプリコンパイルを実行する方法について説明します。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: f5888cf43d8d8192acedaa33b3fa0f313737fc9b
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011288"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="e9d7a-103">ASP.NET Core での Razor ファイルのコンパイル</span><span class="sxs-lookup"><span data-stu-id="e9d7a-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="e9d7a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9d7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e9d7a-105">関連する MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="e9d7a-106">ビルド時の Razor ファイルの公開はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e9d7a-107">Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e9d7a-108">関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e9d7a-109">ビルド時の Razor ファイルの公開はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="e9d7a-110">Razor ファイルはオプションで発行時にコンパイルして、アプリ &mdash; でプリコンパイル ツールを使用して展開できます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e9d7a-111">関連する Razor ページまたは MVC ビューが呼び出されたときに、Razor ファイルが実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="e9d7a-112">Razor ファイルは、[Razor SDK](xref:razor-pages/sdk) を使用してビルド時と公開時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="e9d7a-113">プリコンパイルに関する考慮事項</span><span class="sxs-lookup"><span data-stu-id="e9d7a-113">Precompilation considerations</span></span>

<span data-ttu-id="e9d7a-114">Razor ファイルのプリコンパイルの副作用を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="e9d7a-115">公開されるバンドルが小さくなります</span><span class="sxs-lookup"><span data-stu-id="e9d7a-115">A smaller published bundle</span></span>
* <span data-ttu-id="e9d7a-116">起動時間が短縮されます</span><span class="sxs-lookup"><span data-stu-id="e9d7a-116">A faster startup time</span></span>
* <span data-ttu-id="e9d7a-117">Razor ファイルを編集することはできません。公開されるバンドルには関連付けられたコンテンツがありません。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="e9d7a-118">プリコンパイル済みファイルの展開</span><span class="sxs-lookup"><span data-stu-id="e9d7a-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e9d7a-119">Razor ファイルのビルドおよび発行時のコンパイルは、既定で Razor SDK によって有効になっています。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="e9d7a-120">更新後の Razor ファイルの編集は、ビルド時にサポートされています。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="e9d7a-121">既定では、*.cshtml* ファイルなしのコンパイルされた *Views.dll* のみがアプリケーションと展開されます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9d7a-122">このプリコンパイル ツールは、ASP.NET Core 3.0 で削除されます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e9d7a-123">[Razor Sdk](xref:razor-pages/sdk) に移行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e9d7a-124">Razor SDK は、プロジェクト ファイルにプリコンパイル固有のプロパティが設定されていない場合のみ有効です。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="e9d7a-125">たとえば、*.csproj* ファイルの `MvcRazorCompileOnPublish` プロパティを `true` に設定して、Razor SDK を無効にします。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="e9d7a-126">プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="e9d7a-127">プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="e9d7a-128">ASP.NET Core 2.x プロジェクト テンプレートは既定で、暗黙的に `MvcRazorCompileOnPublish` プロパティを `true` に設定します。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="e9d7a-129">そのため、この要素は *.csproj* ファイルから安全に削除できます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e9d7a-130">このプリコンパイル ツールは、ASP.NET Core 3.0 で削除されます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="e9d7a-131">[Razor Sdk](xref:razor-pages/sdk) に移行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="e9d7a-132">ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、Razor ファイルのプリコンパイルを使用できません。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="e9d7a-133">`MvcRazorCompileOnPublish` プロパティを `true` に設定して、[Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="e9d7a-134">次の *.csproj* サンプルでは、これらの設定が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="e9d7a-135">[.NET Core CLI publish コマンド](/dotnet/core/tools/dotnet-publish)を使用して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="e9d7a-136">たとえば、プロジェクト ルートで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="e9d7a-137">コンパイル済みの Razor ファイルを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="e9d7a-138">たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。</span><span class="sxs-lookup"><span data-stu-id="e9d7a-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="e9d7a-140">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e9d7a-140">Additional resources</span></span>

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
