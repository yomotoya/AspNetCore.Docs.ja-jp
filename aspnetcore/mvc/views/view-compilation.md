---
title: "Razor ビューのコンパイルとプリコンパイル"
author: rick-anderson
description: "MVC Razor ビューのコンパイル、および ASP.NET Core アプリケーションでのプリコンパイルを有効にする方法を説明するリファレンス ドキュメント。"
keywords: "ASP.NET Core、Razor ビューのコンパイル、Razor 事前コンパイル、Razor プリコンパイル"
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="5453f-104">Razor ビューのコンパイル、および ASP.NET Core でのプリコンパイル</span><span class="sxs-lookup"><span data-stu-id="5453f-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="5453f-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5453f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5453f-106">Razor ビューは、ビューが呼び出されたときに、実行時にコンパイルされます。</span><span class="sxs-lookup"><span data-stu-id="5453f-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="5453f-107">ASP.NET 1.1.0 のコアし以降が必要に応じて Razor ビューをコンパイルし、アプリの配置&mdash;プリコンパイルと呼ばれるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="5453f-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="5453f-108">ASP.NET Core 2.x プロジェクト テンプレートは、既定では、プリコンパイルを有効にします。</span><span class="sxs-lookup"><span data-stu-id="5453f-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="5453f-109">Razor ビュー プリコンパイルを実行するときに現在使用できません、[自己完結型の配置 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="5453f-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="5453f-110">2.1 を離したときに、機能は Scd の使用可能になります。</span><span class="sxs-lookup"><span data-stu-id="5453f-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="5453f-111">詳細については、次を参照してください。 [Windows 上の Linux でのクロス コンパイルするときに、ビューのコンパイルが失敗した](https://github.com/aspnet/MvcPrecompilation/issues/102)です。</span><span class="sxs-lookup"><span data-stu-id="5453f-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="5453f-112">プリコンパイルの考慮事項:</span><span class="sxs-lookup"><span data-stu-id="5453f-112">Precompilation considerations:</span></span>

* <span data-ttu-id="5453f-113">ビューをプリコンパイルするは、パブリッシュされたバンドル小さいと起動時間が高速になります。</span><span class="sxs-lookup"><span data-stu-id="5453f-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="5453f-114">ビューをプリコンパイルした後、Razor ファイルを編集することはできません。</span><span class="sxs-lookup"><span data-stu-id="5453f-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="5453f-115">編集済みのビューは、パブリッシュされたバンドル内に存在することはできません。</span><span class="sxs-lookup"><span data-stu-id="5453f-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="5453f-116">プリコンパイル済みのビューを展開します。</span><span class="sxs-lookup"><span data-stu-id="5453f-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="5453f-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="5453f-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="5453f-118">参照をパッケージを含める場合は、プロジェクトは、.NET Framework を対象、 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="5453f-118">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="5453f-119">プロジェクトは .NET Core をターゲットの変更の必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5453f-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="5453f-120">ASP.NET Core 2.x プロジェクト テンプレートが暗黙的に設定`MvcRazorCompileOnPublish`に`true`既定では、つまりこのノードはから安全に削除できる、 *.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5453f-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="5453f-121">明示的に指定する場合は、設定で害はありません、`MvcRazorCompileOnPublish`プロパティを`true`です。</span><span class="sxs-lookup"><span data-stu-id="5453f-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="5453f-122">次*.csproj*サンプルには、この設定が強調表示します。</span><span class="sxs-lookup"><span data-stu-id="5453f-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="5453f-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="5453f-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="5453f-124">設定`MvcRazorCompileOnPublish`に`true`、パッケージの参照を含めて、`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`です。</span><span class="sxs-lookup"><span data-stu-id="5453f-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="5453f-125">次*.csproj*サンプルにはこれらの設定が強調表示されます。</span><span class="sxs-lookup"><span data-stu-id="5453f-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="5453f-126">A *< project_name >。PrecompiledViews.dll*プリコンパイルが成功した場合、コンパイル済みの Razor ビューを含むファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="5453f-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="5453f-127">たとえば、次のスクリーン ショットの内容を示しています*Index.cshtml*内の*WebApplication1.PrecompiledViews.dll*:。</span><span class="sxs-lookup"><span data-stu-id="5453f-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 内の razor ビュー](view-compilation/_static/razor-views-in-dll.png)
