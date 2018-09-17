---
title: ASP.NET Core 2.1 以降用 Microsoft.AspNetCore.App メタパッケージ
author: Rick-Anderson
description: Microsoft.AspNetCore.All メタパッケージには、サポートされているすべての ASP.NET Core および Entity Framework Core パッケージが含まれています。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538454"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="d7465-103">ASP.NET Core 2.1 用の Microsoft.AspNetCore.App メタパッケージ</span><span class="sxs-lookup"><span data-stu-id="d7465-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="d7465-104">この機能では、.NET Core 2.1 以降を対象とする ASP.NET Core 2.1 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="d7465-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="d7465-105">ASP.NET Core 用の [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [メタパッケージ](/dotnet/core/packages#metapackages):</span><span class="sxs-lookup"><span data-stu-id="d7465-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="d7465-106">[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/)、[IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) を除き、サードパーティの依存関係は含まれません。</span><span class="sxs-lookup"><span data-stu-id="d7465-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="d7465-107">含まれるサードパーティの依存関係は、主要なフレームワークの機能を保証するために必要なものです。</span><span class="sxs-lookup"><span data-stu-id="d7465-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="d7465-108">サードパーティの依存関係 (上で示したもの以外) を含むものを除き、ASP.NET Core チームによってサポートされているすべてのパッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7465-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="d7465-109">サードパーティの依存関係 (上で示したもの以外) を含むものを除き、Entity Framework Core チームによってサポートされているすべてのパッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7465-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="d7465-110">`Microsoft.AspNetCore.App` パッケージには、ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降のすべての機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7465-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="d7465-111">ASP.NET Core 2.1 以降を対象とする既定のプロジェクト テンプレートは、このパッケージを使用します。</span><span class="sxs-lookup"><span data-stu-id="d7465-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="d7465-112">ASP.NET Core 2.1 以降および Entity Framework Core 2.1 以降を対象とするアプリケーションは、`Microsoft.AspNetCore.App` パッケージを使うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d7465-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="d7465-113">`Microsoft.AspNetCore.App` メタパッケージのバージョン番号は、ASP.NET Core のバージョンと Entity Framework Core のバージョンを表します。</span><span class="sxs-lookup"><span data-stu-id="d7465-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="d7465-114">`Microsoft.AspNetCore.App` メタパッケージを使うと、アプリを保護するバージョンの制限が提供されます。</span><span class="sxs-lookup"><span data-stu-id="d7465-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="d7465-115">`Microsoft.AspNetCore.App` 内のパッケージに対して (直接的ではなく) 推移的な依存関係を持つパッケージが含まれ、それらのバージョン番号が異なる場合、NuGet はエラーを生成します。</span><span class="sxs-lookup"><span data-stu-id="d7465-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="d7465-116">アプリに追加される他のパッケージは、`Microsoft.AspNetCore.App` に含まれるパッケージのバージョンを変更できません。</span><span class="sxs-lookup"><span data-stu-id="d7465-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="d7465-117">バージョンの整合性により信頼性の高いエクスペリエンスが保証されます。</span><span class="sxs-lookup"><span data-stu-id="d7465-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="d7465-118">`Microsoft.AspNetCore.App` は、関連するビットのテストされていないバージョンの組み合わせが同じアプリ内で一緒に使われるのを防ぐように設計されました。</span><span class="sxs-lookup"><span data-stu-id="d7465-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="d7465-119">`Microsoft.AspNetCore.App` メタパッケージを使うアプリケーションでは、ASP.NET Core 共有フレームワークが自動的に利用されます。</span><span class="sxs-lookup"><span data-stu-id="d7465-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="d7465-120">`Microsoft.AspNetCore.App` メタパッケージを使用する場合、参照される ASP.NET Core NuGet パッケージの資産は、アプリケーションと共に配置**されません**&mdash;ASP.NET Core 共有フレームワークにはこれらの資産が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d7465-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application&mdash;the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="d7465-121">共有フレームワーク内の資産は、アプリケーションの起動時間を向上させるためにプリコンパイルされています。</span><span class="sxs-lookup"><span data-stu-id="d7465-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="d7465-122">詳しくは、「[.NET Core の配布パッケージ](/dotnet/core/build/distribution-packaging)」で "共有フレームワーク" に関する説明をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d7465-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="d7465-123">次のプロジェクト ファイルは ASP.NET Core の `Microsoft.AspNetCore.App` メタパッケージを参照し、一般的な ASP.NET Core 2.1 のテンプレートを表します。</span><span class="sxs-lookup"><span data-stu-id="d7465-123">The following project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core and represents a typical ASP.NET Core 2.1 template:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="d7465-124">`Microsoft.AspNetCore.App` 参照のバージョン番号では、共有フレームワークのバージョンが選択されることが保証**されません**。</span><span class="sxs-lookup"><span data-stu-id="d7465-124">The version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be used.</span></span> <span data-ttu-id="d7465-125">たとえば、バージョン `2.1.1` が指定されているのに、インストールされているのは `2.1.3` であるものとします。</span><span class="sxs-lookup"><span data-stu-id="d7465-125">For example, suppose version `2.1.1` is specified, but `2.1.3` is installed.</span></span> <span data-ttu-id="d7465-126">この場合、アプリは `2.1.3` を使います。</span><span class="sxs-lookup"><span data-stu-id="d7465-126">In that case, the app uses `2.1.3`.</span></span> <span data-ttu-id="d7465-127">お勧めしませんが、ロールフォワードの動作 (パッチとマイナーの両方または一方) を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="d7465-127">Although not recommended, you can disable roll-forward behavior (patch and/or minor).</span></span> <span data-ttu-id="d7465-128">パッケージ バージョンのロールフォワード動作の詳細については、[dotnet ホストのロールフォワード](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7465-128">For more information on package version roll-forward behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

## <a name="update-aspnet-core"></a><span data-ttu-id="d7465-129">ASP.NET Core の更新</span><span class="sxs-lookup"><span data-stu-id="d7465-129">Update ASP.NET Core</span></span>

<span data-ttu-id="d7465-130">`Microsoft.AspNetCore.App` [メタパッケージ](/dotnet/core/packages#metapackages)は、NuGet から更新される従来型のパッケージではありません。</span><span class="sxs-lookup"><span data-stu-id="d7465-130">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) isn't a traditional package that's updated from NuGet.</span></span> <span data-ttu-id="d7465-131">`Microsoft.NETCore.App` に似て、`Microsoft.AspNetCore.App` は共有ランタイムを表しており、NuGet の外部で処理される特殊なバージョン管理セマンティクスを持ちます。</span><span class="sxs-lookup"><span data-stu-id="d7465-131">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="d7465-132">詳しくは、「[パッケージ、メタパッケージ、フレームワーク](/dotnet/core/packages)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d7465-132">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="d7465-133">ASP.NET Core を更新するには:</span><span class="sxs-lookup"><span data-stu-id="d7465-133">To update ASP.NET Core:</span></span>

* <span data-ttu-id="d7465-134">開発用コンピューターやビルド サーバー上: [.NET Core SDK](https://www.microsoft.com/net/download) をダウンロードしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="d7465-134">On development machines and build servers: Download and install the [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="d7465-135">配置サーバー上: [.NET Core ランタイム](https://www.microsoft.com/net/download)をダウンロードしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="d7465-135">On deployment servers: Download and install the [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>

 <span data-ttu-id="d7465-136">アプリケーションは、アプリケーションの再起動時にインストールされている最新バージョンにロールフォワードされます。</span><span class="sxs-lookup"><span data-stu-id="d7465-136">Applications will roll forward to the latest installed version on application restart.</span></span> <span data-ttu-id="d7465-137">プロジェクト ファイル内で `Microsoft.AspNetCore.App` バージョン番号を更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d7465-137">It's not necessary to update the `Microsoft.AspNetCore.App` version number in the project file.</span></span> <span data-ttu-id="d7465-138">詳細については、「[フレームワーク依存のアプリをロールフォワードする](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d7465-138">For more information, see [Framework-dependent apps roll forward](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="d7465-139">アプリケーションで以前に `Microsoft.AspNetCore.All` を使っていた場合は、「[Microsoft.AspNetCore.All から Microsoft.AspNetCore.App への移行](xref:fundamentals/metapackage#migrate)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d7465-139">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>
