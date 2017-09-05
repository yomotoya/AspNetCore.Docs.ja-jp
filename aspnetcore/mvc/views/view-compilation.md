---
title: "Razor ビューのコンパイルとプリコンパイル"
author: rick-anderson
description: "MVC Razor ビューのコンパイル、および ASP.NET Core アプリケーションでのプリコンパイルを有効にする方法を説明するリファレンス ドキュメント。"
keywords: "ASP.NET Core、Razor ビューのコンパイル、Razor 事前コンパイル、Razor プリコンパイル"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 1395717341bfcf5441b78633ca3957630ae5d899
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="6c60b-104">Razor ビューのコンパイル、および ASP.NET Core でのプリコンパイル</span><span class="sxs-lookup"><span data-stu-id="6c60b-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="6c60b-105">によって[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6c60b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6c60b-106">Razor ビューは、ビューが呼び出されたときに、実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="6c60b-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="6c60b-107">ASP.NET 1.1.0 のコアし以降が必要に応じて Razor ビューをコンパイルし、アプリの配置&mdash;プリコンパイルと呼ばれるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="6c60b-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="6c60b-108">ASP.NET Core 2.x プロジェクト テンプレートは、既定では、プリコンパイルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="6c60b-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="6c60b-109">Razor ビュー プリコンパイル実施する際に使用できません、 [Self-Contained 展開](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd)ASP.NET Core バージョン 2.0.0 で以前のバージョン。</span><span class="sxs-lookup"><span data-stu-id="6c60b-109">Razor view precompilation is unavailable when doing a [Self-Contained Deployment](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core versions 2.0.0 and earlier.</span></span>

<span data-ttu-id="6c60b-110">プリコンパイルの考慮事項:</span><span class="sxs-lookup"><span data-stu-id="6c60b-110">Precompilation considerations:</span></span>

* <span data-ttu-id="6c60b-111">ビューをプリコンパイルするは、パブリッシュされたバンドル小さいと起動時間が高速になります。</span><span class="sxs-lookup"><span data-stu-id="6c60b-111">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="6c60b-112">ビューをプリコンパイルした後、Razor ファイルを編集することはできません。</span><span class="sxs-lookup"><span data-stu-id="6c60b-112">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="6c60b-113">編集済みのビューは、パブリッシュされたバンドル内に存在することはできません。</span><span class="sxs-lookup"><span data-stu-id="6c60b-113">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="6c60b-114">プリコンパイル済みのビューを展開します。</span><span class="sxs-lookup"><span data-stu-id="6c60b-114">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6c60b-115">ASP.NET 2.x のコア</span><span class="sxs-lookup"><span data-stu-id="6c60b-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6c60b-116">参照をパッケージを含める場合は、プロジェクトは、.NET Framework を対象、 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="6c60b-116">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="6c60b-117">プロジェクトは .NET Core をターゲットの変更の必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6c60b-117">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="6c60b-118">ASP.NET Core 2.x プロジェクト テンプレートが暗黙的に設定`MvcRazorCompileOnPublish`に`true`既定では、つまりこのノードはから安全に削除できる、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c60b-118">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="6c60b-119">明示的に指定する場合は、設定で害はありません、`MvcRazorCompileOnPublish`プロパティを`true`です。</span><span class="sxs-lookup"><span data-stu-id="6c60b-119">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="6c60b-120">次*.csproj*サンプルには、この設定が強調表示します。</span><span class="sxs-lookup"><span data-stu-id="6c60b-120">The following *.csproj* sample highlights this setting:</span></span>

<span data-ttu-id="6c60b-121">[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="6c60b-121">[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6c60b-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6c60b-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6c60b-123">設定`MvcRazorCompileOnPublish`に`true`、パッケージの参照を含めて、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`です。</span><span class="sxs-lookup"><span data-stu-id="6c60b-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="6c60b-124">次*.csproj*サンプルにはこれらの設定が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c60b-124">The following *.csproj* sample highlights these settings:</span></span>

<span data-ttu-id="6c60b-125">[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]</span><span class="sxs-lookup"><span data-stu-id="6c60b-125">[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]</span></span>

---