---
title: ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージがその依存関係と共に含まれています。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123828"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="fea6e-103">ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="fea6e-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="fea6e-104">ASP.NET Core 2.1 以降を対象とするアプリケーションは、このパッケージではなく [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) を使うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fea6e-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="fea6e-105">この記事の「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](#migrate)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="fea6e-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="fea6e-106">この機能では、.NET Core 2.x を対象とする ASP.NET Core 2.x が必要です。</span><span class="sxs-lookup"><span data-stu-id="fea6e-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="fea6e-107">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fea6e-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="fea6e-108">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="fea6e-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="fea6e-109">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="fea6e-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="fea6e-110">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="fea6e-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="fea6e-111">`Microsoft.AspNetCore.All` パッケージには、ASP.NET Core 2.x および Entity Framework Core 2.x のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="fea6e-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="fea6e-112">ASP.NET Core 2.0 を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="fea6e-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="fea6e-113">`Microsoft.AspNetCore.All` メタパッケージのバージョン番号は、ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="fea6e-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="fea6e-114">`Microsoft.AspNetCore.All` メタパッケージを使用するアプリケーションでは、[.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)が自動的に利用されます。</span><span class="sxs-lookup"><span data-stu-id="fea6e-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="fea6e-115">このランタイム ストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fea6e-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="fea6e-116">`Microsoft.AspNetCore.All` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**。 .NET Core ランタイム ストアにこれらの資産が含まれています。</span><span class="sxs-lookup"><span data-stu-id="fea6e-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="fea6e-117">ランタイム ストア内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="fea6e-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="fea6e-118">使用しないパッケージは、パッケージのトリミング処理を使用して削除することができます。</span><span class="sxs-lookup"><span data-stu-id="fea6e-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="fea6e-119">トリミング処理されたパッケージは、公開済みのアプリケーション出力から除外されます。</span><span class="sxs-lookup"><span data-stu-id="fea6e-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="fea6e-120">次の *.csproj* ファイルは、ASP.NET Core の `Microsoft.AspNetCore.All` メタパッケージを参照しています。</span><span class="sxs-lookup"><span data-stu-id="fea6e-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="fea6e-121">Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行</span><span class="sxs-lookup"><span data-stu-id="fea6e-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="fea6e-122">以下のパッケージは、`Microsoft.AspNetCore.All` には含まれますが `Microsoft.AspNetCore.App` パッケージには含まれません。</span><span class="sxs-lookup"><span data-stu-id="fea6e-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="fea6e-123">アプリで上記のパッケージまたは上記のパッケージによって取り込まれるパッケージに含まれるいずれかの API を使っている場合、`Microsoft.AspNetCore.All` から `Microsoft.AspNetCore.App` に移行するには、これらのパッケージへの参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="fea6e-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="fea6e-124">上記のパッケージの依存関係のうち、他の部分で `Microsoft.AspNetCore.App` の依存関係になっていないものは、暗黙的に含まれることはありません。</span><span class="sxs-lookup"><span data-stu-id="fea6e-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="fea6e-125">例:</span><span class="sxs-lookup"><span data-stu-id="fea6e-125">For example:</span></span>

* <span data-ttu-id="fea6e-126">`Microsoft.Extensions.Caching.Redis` の依存関係としての `StackExchange.Redis`</span><span class="sxs-lookup"><span data-stu-id="fea6e-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="fea6e-127">`Microsoft.AspNetCore.ApplicationInsights.HostingStartup` の依存関係としての `Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="fea6e-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
