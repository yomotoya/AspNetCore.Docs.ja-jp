---
title: "ASP.NET core Microsoft.AspNetCore.All metapackage 2.x 以降"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage には、サポートされているすべてのパッケージが含まれています。"
keywords: "ASP.NET Core では、NuGet でパッケージ化、Microsoft.AspNetCore.All、metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="371b0-104">ASP.NET core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="371b0-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="371b0-105">この機能では ASP.NET Core 2.x です。</span><span class="sxs-lookup"><span data-stu-id="371b0-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="371b0-106">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET core metapackage が含まれています。</span><span class="sxs-lookup"><span data-stu-id="371b0-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="371b0-107">パッケージは、ASP.NET Core チームがすべてサポートされます。</span><span class="sxs-lookup"><span data-stu-id="371b0-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="371b0-108">Entity Framework Core パッケージはすべてサポートされます。</span><span class="sxs-lookup"><span data-stu-id="371b0-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="371b0-109">ASP.NET Core および Entity Framework のコアで使用される内部およびサードパーティの依存します。</span><span class="sxs-lookup"><span data-stu-id="371b0-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="371b0-110">ASP.NET Core のすべての機能 2.x および Entity Framework Core 2.x に含まれる、`Microsoft.AspNetCore.All`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="371b0-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="371b0-111">既定のプロジェクト テンプレートは、このパッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="371b0-111">The default project templates use this package.</span></span>

<span data-ttu-id="371b0-112">バージョン番号、 `Microsoft.AspNetCore.All` metapackage ASP.NET Core バージョンおよび Entity Framework Core バージョン (.NET Core バージョンで揃え) を表します。</span><span class="sxs-lookup"><span data-stu-id="371b0-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="371b0-113">使用するアプリケーション、 `Microsoft.AspNetCore.All` metapackage は自動的には、.NET Core ランタイム ストアの利用します。</span><span class="sxs-lookup"><span data-stu-id="371b0-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the .NET Core Runtime Store.</span></span> <span data-ttu-id="371b0-114">ランタイムのストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="371b0-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="371b0-115">使用すると、 `Microsoft.AspNetCore.All` metapackage、**ありません**から ASP.NET Core NuGet パッケージの参照先のアセットは、アプリケーションと共に配置&mdash;.NET Core ランタイム ストアには、これらのアセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="371b0-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="371b0-116"><!-- todo add link to Runtime store -->アプリケーションの起動時間を向上させるためには、ランタイム ストア内の資産がプリコンパイル済みです。</span><span class="sxs-lookup"><span data-stu-id="371b0-116"><!-- todo add link to Runtime store --> The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="371b0-117">使用しないパッケージを削除するパッケージのトリミング処理を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="371b0-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="371b0-118">トリミングされたパッケージは、公開されたアプリケーションの出力に除外されます。</span><span class="sxs-lookup"><span data-stu-id="371b0-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="371b0-119">次*.csproj*ファイル参照、 `Microsoft.AspNetCore.All` metapackage ASP.NET core:</span><span class="sxs-lookup"><span data-stu-id="371b0-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

<span data-ttu-id="371b0-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="371b0-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span></span>
