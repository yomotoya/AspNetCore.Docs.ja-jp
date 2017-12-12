---
title: "ASP.NET Core と Entity Framework 6 の概要"
author: tdykstra
description: "この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。"
keywords: "ASP.NET Core、Entity Framework EF 6"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.assetid: 016cc836-4c43-45a4-b9a7-9efaf53350df
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 8abec95c591f20069e20eec55fd21503e74f8606
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="48be2-104">ASP.NET Core と Entity Framework 6 の概要</span><span class="sxs-lookup"><span data-stu-id="48be2-104">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="48be2-105">によって[Paweł Grudzień](https://github.com/pgrudzien12)、 [Damien Pontifex](https://github.com/DamienPontifex)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="48be2-105">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="48be2-106">この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="48be2-106">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="48be2-107">概要</span><span class="sxs-lookup"><span data-stu-id="48be2-107">Overview</span></span>

<span data-ttu-id="48be2-108">Entity Framework 6 を使用するのには、Entity Framework 6 では .NET Core はサポートされていないも、プロジェクトは .NET フレームワークに対してコンパイルにします。</span><span class="sxs-lookup"><span data-stu-id="48be2-108">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 does not support .NET Core.</span></span> <span data-ttu-id="48be2-109">クロスプラット フォームの機能が必要な場合にアップグレードする必要があります。 [Entity Framework Core](https://docs.microsoft.com/ef/)です。</span><span class="sxs-lookup"><span data-stu-id="48be2-109">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="48be2-110">ASP.NET Core アプリケーションで Entity Framework 6 を使用することをお勧め EF6 コンテキストには、プロジェクトを対象とする完全なフレームワークのクラス ライブラリでモデル クラス。</span><span class="sxs-lookup"><span data-stu-id="48be2-110">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="48be2-111">ASP.NET Core プロジェクトから、クラス ライブラリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="48be2-111">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="48be2-112">このサンプルを参照してください[EF6 および ASP.NET Core プロジェクトと Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)です。</span><span class="sxs-lookup"><span data-stu-id="48be2-112">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="48be2-113">.NET Core プロジェクトには、すべての機能 EF6 などのコマンドをサポートしないので、プロジェクトで ASP.NET Core EF6 コンテキストを配置することはできません*Enable-migrations*を必要とします。</span><span class="sxs-lookup"><span data-stu-id="48be2-113">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="48be2-114">EF6 コンテキストを検索するプロジェクトの種類に関係なく EF6 コマンド ライン ツールだけは EF6 コンテキストを使用します。</span><span class="sxs-lookup"><span data-stu-id="48be2-114">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="48be2-115">たとえば、`Scaffold-DbContext`は Entity Framework のコアでのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="48be2-115">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="48be2-116">EF6 モデルにリバース エンジニア リングのデータベースについて実行する必要がある場合は、次を参照してください。[既存のデータベースに Code First](https://msdn.microsoft.com/jj200620)です。</span><span class="sxs-lookup"><span data-stu-id="48be2-116">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="48be2-117">参照を完全なフレームワークおよび ASP.NET Core プロジェクトに EF6</span><span class="sxs-lookup"><span data-stu-id="48be2-117">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="48be2-118">ASP.NET Core プロジェクトは、.NET framework と EF6 を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="48be2-118">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="48be2-119">たとえば、 *.csproj* ASP.NET Core プロジェクトのファイルは次の例のようになります (ファイルの関連する部分のみが表示されます)。</span><span class="sxs-lookup"><span data-stu-id="48be2-119">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="48be2-120">新しいプロジェクトを作成する場合を使用して、 **ASP.NET Core Web アプリケーション (.NET Framework)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="48be2-120">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="48be2-121">接続文字列を処理します。</span><span class="sxs-lookup"><span data-stu-id="48be2-121">Handle connection strings</span></span>

<span data-ttu-id="48be2-122">EF6 クラス ライブラリ プロジェクトを使用して、EF6 コマンド ライン ツールには、コンテキストのインスタンスを作成するための既定のコンス トラクターが必要があります。</span><span class="sxs-lookup"><span data-stu-id="48be2-122">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="48be2-123">ASP.NET Core のプロジェクトである場合、コンテキスト コンス トラクターを使用する接続文字列は、接続文字列に渡すことができますをパラメーターを持つ必要がありますを指定することが可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48be2-123">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="48be2-124">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48be2-124">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="48be2-125">実装を提供する EF6 プロジェクトが、EF6 コンテキストでは、パラメーターなしのコンス トラクターがあるないため[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)です。</span><span class="sxs-lookup"><span data-stu-id="48be2-125">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="48be2-126">EF6 コマンド ライン ツールを見つけて、コンテキストをインスタンス化することができますのでこの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="48be2-126">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="48be2-127">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48be2-127">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="48be2-128">このサンプル コードで、`IDbContextFactory`実装のハード コーディングされた接続文字列に渡します。</span><span class="sxs-lookup"><span data-stu-id="48be2-128">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="48be2-129">これは、コマンド ライン ツールで使用される接続文字列です。</span><span class="sxs-lookup"><span data-stu-id="48be2-129">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="48be2-130">クラス ライブラリで呼び出し元のアプリケーションを使用する同じ接続文字列を使用することを確認するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="48be2-130">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="48be2-131">たとえば、両方のプロジェクトでの環境変数から値を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="48be2-131">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="48be2-132">ASP.NET Core プロジェクトに依存関係の挿入を設定します。</span><span class="sxs-lookup"><span data-stu-id="48be2-132">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="48be2-133">中核となるプロジェクトの*Startup.cs*で依存性の注入 (DI) の EF6 コンテキストを設定して、ファイル`ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="48be2-133">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="48be2-134">EF コンテキスト オブジェクトは、要求ごとの有効期間中にスコープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="48be2-134">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="48be2-135">DI を使用して、コント ローラーでのコンテキストのインスタンスを取得できます。</span><span class="sxs-lookup"><span data-stu-id="48be2-135">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="48be2-136">コードは、EF コア コンテキストを記述して新機能と同様です。</span><span class="sxs-lookup"><span data-stu-id="48be2-136">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="48be2-137">サンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="48be2-137">Sample application</span></span>

<span data-ttu-id="48be2-138">実際のサンプル アプリケーションでは、次を参照してください。、[サンプル Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)この記事に付属しています。</span><span class="sxs-lookup"><span data-stu-id="48be2-138">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="48be2-139">このサンプルは、Visual Studio で次の手順で、ゼロから作成することができます。</span><span class="sxs-lookup"><span data-stu-id="48be2-139">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="48be2-140">ソリューションを作成します。</span><span class="sxs-lookup"><span data-stu-id="48be2-140">Create a solution.</span></span>

* <span data-ttu-id="48be2-141">**新しいプロジェクトの追加 > Web > ASP.NET Core Web アプリケーション (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="48be2-141">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="48be2-142">**新しいプロジェクトの追加 > Windows クラシック デスクトップ > クラス ライブラリ (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="48be2-142">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="48be2-143">**Package Manager Console** (PMC) 両方のプロジェクトでは、コマンドを実行`Install-Package Entityframework`です。</span><span class="sxs-lookup"><span data-stu-id="48be2-143">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="48be2-144">クラス ライブラリ プロジェクトで、データ モデルのクラスと、コンテキスト クラスの実装を作成する`IDbContextFactory`です。</span><span class="sxs-lookup"><span data-stu-id="48be2-144">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="48be2-145">クラス ライブラリ プロジェクトの PMC でのコマンドを実行`Enable-Migrations`と`Add-Migration Initial`です。</span><span class="sxs-lookup"><span data-stu-id="48be2-145">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="48be2-146">スタートアップ プロジェクトとして ASP.NET Core プロジェクトを設定する場合、追加`-StartupProjectName EF6`これらのコマンドにします。</span><span class="sxs-lookup"><span data-stu-id="48be2-146">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="48be2-147">コア プロジェクトでは、クラス ライブラリ プロジェクトへのプロジェクト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="48be2-147">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="48be2-148">コア プロジェクト内で*Startup.cs*DI のコンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="48be2-148">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="48be2-149">コア プロジェクト内で*される appsettings.json*、接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="48be2-149">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="48be2-150">コア プロジェクトでは、コント ローラーとの読み取りおよびデータを書き込むことを確認するビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="48be2-150">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="48be2-151">(ASP.NET Core MVC のスキャフォールディングをクラス ライブラリから参照されている EF6 コンテキストで動作しないことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="48be2-151">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="48be2-152">概要</span><span class="sxs-lookup"><span data-stu-id="48be2-152">Summary</span></span>

<span data-ttu-id="48be2-153">この記事は、ASP.NET Core アプリケーションで Entity Framework 6 を使用するための基本的なガイダンスを提供しています。</span><span class="sxs-lookup"><span data-stu-id="48be2-153">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48be2-154">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="48be2-154">Additional Resources</span></span>

* [<span data-ttu-id="48be2-155">Entity Framework のコード ベースの構成</span><span class="sxs-lookup"><span data-stu-id="48be2-155">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
