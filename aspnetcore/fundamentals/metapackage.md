---
title: "ASP.NET Core 2.x 以降用 Microsoft.AspNetCore.All メタパッケージ"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージがその依存関係と共に含まれています。"
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 07220fdae299723088fa85e452cedff5e5685bd7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="cc396-103">ASP.NET Core 2.x 用 Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="cc396-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="cc396-104">この機能では、.NET Core 2.x を対象とする ASP.NET Core 2.x が必要です。</span><span class="sxs-lookup"><span data-stu-id="cc396-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="cc396-105">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cc396-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="cc396-106">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="cc396-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="cc396-107">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="cc396-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="cc396-108">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="cc396-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="cc396-109">`Microsoft.AspNetCore.All` パッケージには、ASP.NET Core 2.x および Entity Framework Core 2.x のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cc396-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="cc396-110">このパッケージは、既定のプロジェクト テンプレートで使用されます。</span><span class="sxs-lookup"><span data-stu-id="cc396-110">The default project templates use this package.</span></span>

<span data-ttu-id="cc396-111">`Microsoft.AspNetCore.All` メタパッケージのバージョン番号は、(.NET Core バージョンと連携している) ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="cc396-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="cc396-112">`Microsoft.AspNetCore.All` メタパッケージを使用するアプリケーションでは、[.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)が自動的に利用されます。</span><span class="sxs-lookup"><span data-stu-id="cc396-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="cc396-113">このランタイム ストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cc396-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="cc396-114">`Microsoft.AspNetCore.All` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**。 .NET Core ランタイム ストアにこれらの資産が含まれています。</span><span class="sxs-lookup"><span data-stu-id="cc396-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="cc396-115">ランタイム ストア内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="cc396-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="cc396-116">使用しないパッケージは、パッケージのトリミング処理を使用して削除することができます。</span><span class="sxs-lookup"><span data-stu-id="cc396-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="cc396-117">トリミング処理されたパッケージは、公開済みのアプリケーション出力から除外されます。</span><span class="sxs-lookup"><span data-stu-id="cc396-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="cc396-118">次の *.csproj* ファイルは、ASP.NET Core の `Microsoft.AspNetCore.All` メタパッケージを参照しています。</span><span class="sxs-lookup"><span data-stu-id="cc396-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
