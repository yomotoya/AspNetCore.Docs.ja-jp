---
title: ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージは、ASP.NET Core 2.1 以降では推奨されていません。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2018
uid: fundamentals/metapackage
ms.openlocfilehash: b1924e07acd2b4feb25c69b8c4674002e6ba0464
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325680"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="b7cd3-103">ASP.NET Core 2.0 用の Microsoft.AspNetCore.All メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="b7cd3-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="b7cd3-104">ASP.NET Core 2.1 以降を対象とするアプリケーションは、このパッケージではなく [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) を使うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="b7cd3-105">この記事の「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](#migrate)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="b7cd3-106">この機能では、.NET Core 2.x を対象とする ASP.NET Core 2.x が必要です。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="b7cd3-107">ASP.NET Core の [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) メタパッケージには、次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="b7cd3-108">ASP.NET Core チームでサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="b7cd3-109">Entity Framework Core でサポートされるすべてのパッケージ。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="b7cd3-110">ASP.NET Core および Entity Framework Core で使用される内部およびサードパーティの依存関係。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="b7cd3-111">`Microsoft.AspNetCore.All` パッケージには、ASP.NET Core 2.x および Entity Framework Core 2.x のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="b7cd3-112">ASP.NET Core 2.0 を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="b7cd3-113">`Microsoft.AspNetCore.All` メタパッケージのバージョン番号は、ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="b7cd3-114">`Microsoft.AspNetCore.All` メタパッケージを使用するアプリケーションでは、[.NET Core ランタイム ストア](https://docs.microsoft.com/dotnet/core/deploying/runtime-store)が自動的に利用されます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="b7cd3-115">このランタイム ストアには、ASP.NET Core 2.x アプリケーションの実行に必要なすべてのランタイム アセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="b7cd3-116">`Microsoft.AspNetCore.All` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**。 .NET Core ランタイム ストアにこれらの資産が含まれています。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="b7cd3-117">ランタイム ストア内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="b7cd3-118">使用しないパッケージは、パッケージのトリミング処理を使用して削除することができます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="b7cd3-119">トリミング処理されたパッケージは、公開済みのアプリケーション出力から除外されます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="b7cd3-120">次の *.csproj* ファイルは、ASP.NET Core の `Microsoft.AspNetCore.All` メタパッケージを参照しています。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="b7cd3-121">Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行</span><span class="sxs-lookup"><span data-stu-id="b7cd3-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="b7cd3-122">以下のパッケージは、`Microsoft.AspNetCore.All` には含まれますが `Microsoft.AspNetCore.App` パッケージには含まれません。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

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

<span data-ttu-id="b7cd3-123">アプリで上記のパッケージまたは上記のパッケージによって取り込まれるパッケージに含まれるいずれかの API を使っている場合、`Microsoft.AspNetCore.All` から `Microsoft.AspNetCore.App` に移行するには、これらのパッケージへの参照をプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="b7cd3-124">上記のパッケージの依存関係のうち、他の部分で `Microsoft.AspNetCore.App` の依存関係になっていないものは、暗黙的に含まれることはありません。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="b7cd3-125">例:</span><span class="sxs-lookup"><span data-stu-id="b7cd3-125">For example:</span></span>

* <span data-ttu-id="b7cd3-126">`Microsoft.Extensions.Caching.Redis` の依存関係としての `StackExchange.Redis`</span><span class="sxs-lookup"><span data-stu-id="b7cd3-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="b7cd3-127">`Microsoft.AspNetCore.ApplicationInsights.HostingStartup` の依存関係としての `Microsoft.ApplicationInsights`</span><span class="sxs-lookup"><span data-stu-id="b7cd3-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="b7cd3-128">ASP.NET Core 2.1 を更新する</span><span class="sxs-lookup"><span data-stu-id="b7cd3-128">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="b7cd3-129">2.1 以降用の `Microsoft.AspNetCore.App` メタパッケージに移行することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-129">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="b7cd3-130">`Microsoft.AspNetCore.All` メタパッケージを引き続き使用し、最新のバージョンの修正プログラムが配置されていることを確認するには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-130">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="b7cd3-131">開発用コンピューターやビルド サーバー上: 最新の [.NET Core SDK](https://www.microsoft.com/net/download) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-131">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="b7cd3-132">展開サーバー上: 最新の [.NET Core ランタイム](https://www.microsoft.com/net/download)をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-132">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="b7cd3-133">ご利用のアプリは、アプリケーションの再起動時にインストールされている最新バージョンにロールフォワードされます。</span><span class="sxs-lookup"><span data-stu-id="b7cd3-133">Your app will roll forward to the latest installed version on an application restart.</span></span>
