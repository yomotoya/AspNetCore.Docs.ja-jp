---
title: "ASP.NET core Microsoft.AspNetCore.All metapackage 2.x 以降"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage には、その依存関係と共に、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="a6628-103">ASP.NET core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="a6628-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="a6628-104">この機能では ASP.NET Core 2.x の対象とする .NET 2.x のコアです。</span><span class="sxs-lookup"><span data-stu-id="a6628-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="a6628-105">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="a6628-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="a6628-106">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="a6628-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="a6628-107">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="a6628-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="a6628-108">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="a6628-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="a6628-109">ASP.NET Core のすべての機能 2.x および Entity Framework Core 2.x に含まれる、`Microsoft.AspNetCore.All`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="a6628-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="a6628-110">既定のプロジェクト テンプレートは、このパッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="a6628-110">The default project templates use this package.</span></span>

<span data-ttu-id="a6628-111">バージョン番号、 `Microsoft.AspNetCore.All` metapackage ASP.NET Core バージョンおよび Entity Framework Core バージョン (.NET Core バージョンで揃え) を表します。</span><span class="sxs-lookup"><span data-stu-id="a6628-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="a6628-112">使用するアプリケーション、 `Microsoft.AspNetCore.All` metapackage を自動的に利用、 [.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)です。</span><span class="sxs-lookup"><span data-stu-id="a6628-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="a6628-113">ランタイムのストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a6628-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="a6628-114">使用すると、 `Microsoft.AspNetCore.All` metapackage、**ありません**から ASP.NET Core NuGet パッケージの参照先のアセットは、アプリケーションと共に配置&mdash;.NET Core ランタイム ストアには、これらのアセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="a6628-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="a6628-115">アプリケーションの起動時間を向上させるためには、ランタイム ストア内の資産がプリコンパイル済みです。</span><span class="sxs-lookup"><span data-stu-id="a6628-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="a6628-116">使用しないパッケージを削除するパッケージのトリミング処理を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="a6628-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="a6628-117">トリミングされたパッケージは、公開されたアプリケーションの出力に除外されます。</span><span class="sxs-lookup"><span data-stu-id="a6628-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="a6628-118">次*.csproj*ファイル参照、 `Microsoft.AspNetCore.All` metapackage ASP.NET core:</span><span class="sxs-lookup"><span data-stu-id="a6628-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
