---
title: "Razor ビューのコンパイルとプリコンパイル"
author: rick-anderson
description: "ASP.NET Core アプリケーションで MVC Razor ビューのコンパイルとプリコンパイルを有効にする方法を説明するリファレンス ドキュメント。"
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: bd3f4470035b0375fc79aa7caa73b60ba6fc4f53
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="c4c6f-103">ASP.NET Core での Razor ビューのコンパイルとプリコンパイル</span><span class="sxs-lookup"><span data-stu-id="c4c6f-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="c4c6f-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4c6f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4c6f-105">Razor ビューは、ビューが呼び出されるときに、実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="c4c6f-106">ASP.NET Core 1.1.0 以降では、必要に応じて Razor ビューをコンパイルし、それらをアプリを使用して展開します (プリコンパイルというプロセス)。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="c4c6f-107">ASP.NET Core 2.x プロジェクトのテンプレートでは、既定でプリコンパイルが有効になります。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c4c6f-108">ASP.NET Core 2.0 で[自己完結型の展開 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) を実行する場合、現時点では Razor ビューのプリコンパイルを使用できません。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="c4c6f-109">この機能は 2.1 のリリース時に SCD で使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="c4c6f-110">詳細については、「[View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102)」 (Windows の Linux のクロス コンパイル時にビューをコンパイルできない) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="c4c6f-111">プリコンパイルに関する考慮事項:</span><span class="sxs-lookup"><span data-stu-id="c4c6f-111">Precompilation considerations:</span></span>

* <span data-ttu-id="c4c6f-112">ビューをプリコンパイルすると、公開されるバンドルが小さくなり、起動時間が短縮されます。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="c4c6f-113">ビューをプリコンパイルした後に Razor ファイルを編集することはできません。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="c4c6f-114">公開されたバンドルには編集されたビューは存在しません。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="c4c6f-115">プリコンパイル済みのビューを展開するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c4c6f-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c4c6f-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="c4c6f-117">プロジェクトのターゲットが .NET Framework である場合は、次のように [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) へのパッケージ参照を含めます。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="c4c6f-118">プロジェクトのターゲットが .NET Core である場合、変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="c4c6f-119">ASP.NET Core 2.x プロジェクトのテンプレートでは既定で暗黙的に `MvcRazorCompileOnPublish` が `true` に設定されます。これは、このノードを *.csproj* ファイルから安全に削除できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="c4c6f-120">明示的に `MvcRazorCompileOnPublish` プロパティを `true` に設定しても問題はありません。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="c4c6f-121">次の *.csproj* サンプルでは、この設定が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c4c6f-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c4c6f-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="c4c6f-123">`MvcRazorCompileOnPublish` を `true` に設定し、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` へのパッケージ参照を含めます。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="c4c6f-124">次の *.csproj* サンプルでは、これらの設定が強調表示されています。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="c4c6f-125">プロジェクト ルートで次のようなコマンドを実行して、[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを準備します。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="c4c6f-126">コンパイル済みの Razor ビューを含む、*<project_name>.PrecompiledViews.dll* ファイルは、プリコンパイルが正常に行われたときに生成されます。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="c4c6f-127">たとえば、次のスクリーンショットは、*WebApplication1.PrecompiledViews.dll* 内の *Index.cshtml* のコンテンツを示しています。</span><span class="sxs-lookup"><span data-stu-id="c4c6f-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 内の Razor ビュー](view-compilation/_static/razor-views-in-dll.png)
