---
title: "ASP.NET Core と Entity Framework 6 の概要"
author: tdykstra
description: "この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。"
ms.author: tdykstra
manager: wpickett
ms.date: 02/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/entity-framework-6
ms.openlocfilehash: 7f3c1f28c1e0b3a68db7f6f84c56b18643b56cc8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-aspnet-core-and-entity-framework-6"></a><span data-ttu-id="53a15-103">ASP.NET Core と Entity Framework 6 の概要</span><span class="sxs-lookup"><span data-stu-id="53a15-103">Getting started with ASP.NET Core and Entity Framework 6</span></span>

<span data-ttu-id="53a15-104">によって[Paweł Grudzień](https://github.com/pgrudzien12)、 [Damien Pontifex](https://github.com/DamienPontifex)、および[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="53a15-104">By [Paweł Grudzień](https://github.com/pgrudzien12), [Damien Pontifex](https://github.com/DamienPontifex), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="53a15-105">この記事では、ASP.NET Core アプリケーションで Entity Framework 6 を使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="53a15-105">This article shows how to use Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="overview"></a><span data-ttu-id="53a15-106">概要</span><span class="sxs-lookup"><span data-stu-id="53a15-106">Overview</span></span>

<span data-ttu-id="53a15-107">Entity Framework 6 を使用するのには、Entity Framework 6 .NET Core をサポートするいないとも、プロジェクトには .NET フレームワークに対してコンパイルがします。</span><span class="sxs-lookup"><span data-stu-id="53a15-107">To use Entity Framework 6, your project has to compile against .NET Framework, as Entity Framework 6 doesn't support .NET Core.</span></span> <span data-ttu-id="53a15-108">クロスプラット フォームの機能が必要な場合にアップグレードする必要があります。 [Entity Framework Core](https://docs.microsoft.com/ef/)です。</span><span class="sxs-lookup"><span data-stu-id="53a15-108">If you need cross-platform features you will need to upgrade to [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

<span data-ttu-id="53a15-109">ASP.NET Core アプリケーションで Entity Framework 6 を使用することをお勧め EF6 コンテキストには、プロジェクトを対象とする完全なフレームワークのクラス ライブラリでモデル クラス。</span><span class="sxs-lookup"><span data-stu-id="53a15-109">The recommended way to use Entity Framework 6 in an ASP.NET Core application is to put the EF6 context and model classes in a class library project that targets the full framework.</span></span> <span data-ttu-id="53a15-110">ASP.NET Core プロジェクトから、クラス ライブラリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="53a15-110">Add a reference to the class library from the ASP.NET Core project.</span></span> <span data-ttu-id="53a15-111">このサンプルを参照してください[EF6 および ASP.NET Core プロジェクトと Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)です。</span><span class="sxs-lookup"><span data-stu-id="53a15-111">See the sample [Visual Studio solution with EF6 and ASP.NET Core projects](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/).</span></span>

<span data-ttu-id="53a15-112">.NET Core プロジェクトには、すべての機能 EF6 などのコマンドをサポートしないので、プロジェクトで ASP.NET Core EF6 コンテキストを配置することはできません*Enable-migrations*を必要とします。</span><span class="sxs-lookup"><span data-stu-id="53a15-112">You can't put an EF6 context in an ASP.NET Core project because .NET Core projects don't support all of the functionality that EF6 commands such as *Enable-Migrations* require.</span></span>

<span data-ttu-id="53a15-113">EF6 コンテキストを検索するプロジェクトの種類に関係なく EF6 コマンド ライン ツールだけは EF6 コンテキストを使用します。</span><span class="sxs-lookup"><span data-stu-id="53a15-113">Regardless of project type in which you locate your EF6 context, only EF6 command-line tools work with an EF6 context.</span></span> <span data-ttu-id="53a15-114">たとえば、`Scaffold-DbContext`は Entity Framework のコアでのみ使用します。</span><span class="sxs-lookup"><span data-stu-id="53a15-114">For example, `Scaffold-DbContext` is only available in Entity Framework Core.</span></span> <span data-ttu-id="53a15-115">EF6 モデルにリバース エンジニア リングのデータベースについて実行する必要がある場合は、次を参照してください。[既存のデータベースに Code First](https://msdn.microsoft.com/jj200620)です。</span><span class="sxs-lookup"><span data-stu-id="53a15-115">If you need to do reverse engineering of a database into an EF6 model, see [Code First to an Existing Database](https://msdn.microsoft.com/jj200620).</span></span>

## <a name="reference-full-framework-and-ef6-in-the-aspnet-core-project"></a><span data-ttu-id="53a15-116">参照を完全なフレームワークおよび ASP.NET Core プロジェクトに EF6</span><span class="sxs-lookup"><span data-stu-id="53a15-116">Reference full framework and EF6 in the ASP.NET Core project</span></span>

<span data-ttu-id="53a15-117">ASP.NET Core プロジェクトは、.NET framework と EF6 を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="53a15-117">Your ASP.NET Core project needs to reference .NET framework and EF6.</span></span> <span data-ttu-id="53a15-118">たとえば、 *.csproj* ASP.NET Core プロジェクトのファイルは次の例のようになります (ファイルの関連する部分のみが表示されます)。</span><span class="sxs-lookup"><span data-stu-id="53a15-118">For example, the *.csproj* file of your ASP.NET Core project will look similar to the following example (only relevant parts of the file are shown).</span></span>

[!code-xml[](entity-framework-6/sample/MVCCore/MVCCore.csproj?range=3-9&highlight=2)]

<span data-ttu-id="53a15-119">新しいプロジェクトを作成する場合を使用して、 **ASP.NET Core Web アプリケーション (.NET Framework)**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="53a15-119">If you’re creating a new project, use the **ASP.NET Core Web Application (.NET Framework)** template.</span></span>

## <a name="handle-connection-strings"></a><span data-ttu-id="53a15-120">接続文字列を処理します。</span><span class="sxs-lookup"><span data-stu-id="53a15-120">Handle connection strings</span></span>

<span data-ttu-id="53a15-121">EF6 クラス ライブラリ プロジェクトを使用して、EF6 コマンド ライン ツールには、コンテキストのインスタンスを作成するための既定のコンス トラクターが必要があります。</span><span class="sxs-lookup"><span data-stu-id="53a15-121">The EF6 command-line tools that you'll use in the EF6 class library project require a default constructor so they can instantiate the context.</span></span> <span data-ttu-id="53a15-122">ASP.NET Core のプロジェクトである場合、コンテキスト コンス トラクターを使用する接続文字列は、接続文字列に渡すことができますをパラメーターを持つ必要がありますを指定することが可能性があります。</span><span class="sxs-lookup"><span data-stu-id="53a15-122">But you'll probably want to specify the connection string to use in the ASP.NET Core project, in which case your context constructor must have a parameter that lets you pass in the connection string.</span></span> <span data-ttu-id="53a15-123">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="53a15-123">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContext.cs?name=snippet_Constructor)]

<span data-ttu-id="53a15-124">実装を提供する EF6 プロジェクトが、EF6 コンテキストでは、パラメーターなしのコンス トラクターがあるないため[IDbContextFactory](https://msdn.microsoft.com/library/hh506876)です。</span><span class="sxs-lookup"><span data-stu-id="53a15-124">Since your EF6 context doesn't have a parameterless constructor, your EF6 project has to provide an implementation of [IDbContextFactory](https://msdn.microsoft.com/library/hh506876).</span></span> <span data-ttu-id="53a15-125">EF6 コマンド ライン ツールを見つけて、コンテキストをインスタンス化することができますのでこの実装を使用します。</span><span class="sxs-lookup"><span data-stu-id="53a15-125">The EF6 command-line tools will find and use that implementation so they can instantiate the context.</span></span> <span data-ttu-id="53a15-126">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="53a15-126">Here's an example.</span></span>

[!code-csharp[](entity-framework-6/sample/EF6/SchoolContextFactory.cs?name=snippet_IDbContextFactory)]

<span data-ttu-id="53a15-127">このサンプル コードで、`IDbContextFactory`実装のハード コーディングされた接続文字列に渡します。</span><span class="sxs-lookup"><span data-stu-id="53a15-127">In this sample code, the `IDbContextFactory` implementation passes in a hard-coded connection string.</span></span> <span data-ttu-id="53a15-128">これは、コマンド ライン ツールで使用される接続文字列です。</span><span class="sxs-lookup"><span data-stu-id="53a15-128">This is the connection string that the command-line tools will use.</span></span> <span data-ttu-id="53a15-129">クラス ライブラリで呼び出し元のアプリケーションを使用する同じ接続文字列を使用することを確認するための戦略を実装します。</span><span class="sxs-lookup"><span data-stu-id="53a15-129">You'll want to implement a strategy to ensure that the class library uses the same connection string that the calling application uses.</span></span> <span data-ttu-id="53a15-130">たとえば、両方のプロジェクトでの環境変数から値を取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="53a15-130">For example, you could get the value from an environment variable in both projects.</span></span>

## <a name="set-up-dependency-injection-in-the-aspnet-core-project"></a><span data-ttu-id="53a15-131">ASP.NET Core プロジェクトに依存関係の挿入を設定します。</span><span class="sxs-lookup"><span data-stu-id="53a15-131">Set up dependency injection in the ASP.NET Core project</span></span>

<span data-ttu-id="53a15-132">中核となるプロジェクトの*Startup.cs*で依存性の注入 (DI) の EF6 コンテキストを設定して、ファイル`ConfigureServices`です。</span><span class="sxs-lookup"><span data-stu-id="53a15-132">In the Core project's *Startup.cs* file, set up the EF6 context for dependency injection (DI) in `ConfigureServices`.</span></span> <span data-ttu-id="53a15-133">EF コンテキスト オブジェクトは、要求ごとの有効期間中にスコープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="53a15-133">EF context objects should be scoped for a per-request lifetime.</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

<span data-ttu-id="53a15-134">DI を使用して、コント ローラーでのコンテキストのインスタンスを取得できます。</span><span class="sxs-lookup"><span data-stu-id="53a15-134">You can then get an instance of the context in your controllers by using DI.</span></span> <span data-ttu-id="53a15-135">コードは、EF コア コンテキストを記述して新機能と同様です。</span><span class="sxs-lookup"><span data-stu-id="53a15-135">The code is similar to what you'd write for an EF Core context:</span></span>

[!code-csharp[](entity-framework-6/sample/MVCCore/Controllers/StudentsController.cs?name=snippet_ContextInController)]

## <a name="sample-application"></a><span data-ttu-id="53a15-136">サンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="53a15-136">Sample application</span></span>

<span data-ttu-id="53a15-137">実際のサンプル アプリケーションでは、次を参照してください。、[サンプル Visual Studio ソリューション](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/)この記事に付属しています。</span><span class="sxs-lookup"><span data-stu-id="53a15-137">For a working sample application, see the [sample Visual Studio solution](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/entity-framework-6/sample/) that accompanies this article.</span></span>

<span data-ttu-id="53a15-138">このサンプルは、Visual Studio で次の手順で、ゼロから作成することができます。</span><span class="sxs-lookup"><span data-stu-id="53a15-138">This sample can be created from scratch by the following steps in Visual Studio:</span></span>

* <span data-ttu-id="53a15-139">ソリューションを作成します。</span><span class="sxs-lookup"><span data-stu-id="53a15-139">Create a solution.</span></span>

* <span data-ttu-id="53a15-140">**新しいプロジェクトの追加 > Web > ASP.NET Core Web アプリケーション (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="53a15-140">**Add New Project > Web > ASP.NET Core Web Application (.NET Framework)**</span></span>

* <span data-ttu-id="53a15-141">**新しいプロジェクトの追加 > Windows クラシック デスクトップ > クラス ライブラリ (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="53a15-141">**Add New Project > Windows Classic Desktop > Class Library (.NET Framework)**</span></span>

* <span data-ttu-id="53a15-142">**Package Manager Console** (PMC) 両方のプロジェクトでは、コマンドを実行`Install-Package Entityframework`です。</span><span class="sxs-lookup"><span data-stu-id="53a15-142">In **Package Manager Console** (PMC) for both projects, run the command `Install-Package Entityframework`.</span></span>

* <span data-ttu-id="53a15-143">クラス ライブラリ プロジェクトで、データ モデルのクラスと、コンテキスト クラスの実装を作成する`IDbContextFactory`です。</span><span class="sxs-lookup"><span data-stu-id="53a15-143">In the class library project, create data model classes and a context class, and an implementation of `IDbContextFactory`.</span></span>

* <span data-ttu-id="53a15-144">クラス ライブラリ プロジェクトの PMC でのコマンドを実行`Enable-Migrations`と`Add-Migration Initial`です。</span><span class="sxs-lookup"><span data-stu-id="53a15-144">In PMC for the class library project, run the commands `Enable-Migrations` and `Add-Migration Initial`.</span></span> <span data-ttu-id="53a15-145">スタートアップ プロジェクトとして ASP.NET Core プロジェクトを設定する場合、追加`-StartupProjectName EF6`これらのコマンドにします。</span><span class="sxs-lookup"><span data-stu-id="53a15-145">If you have set the ASP.NET Core project as the startup project, add `-StartupProjectName EF6` to these commands.</span></span>

* <span data-ttu-id="53a15-146">コア プロジェクトでは、クラス ライブラリ プロジェクトへのプロジェクト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="53a15-146">In the Core project, add a project reference to the class library project.</span></span>

* <span data-ttu-id="53a15-147">コア プロジェクト内で*Startup.cs*DI のコンテキストを登録します。</span><span class="sxs-lookup"><span data-stu-id="53a15-147">In the Core project, in *Startup.cs*, register the context for DI.</span></span>

* <span data-ttu-id="53a15-148">コア プロジェクト内で*される appsettings.json*、接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="53a15-148">In the Core project, in *appsettings.json*, add the connection string.</span></span>

* <span data-ttu-id="53a15-149">コア プロジェクトでは、コント ローラーとの読み取りおよびデータを書き込むことを確認するビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="53a15-149">In the Core project, add a controller and view(s) to verify that you can read and write data.</span></span> <span data-ttu-id="53a15-150">(ASP.NET Core MVC のスキャフォールディングをクラス ライブラリから参照されている EF6 コンテキストで動作しないことに注意してください)。</span><span class="sxs-lookup"><span data-stu-id="53a15-150">(Note that ASP.NET Core MVC scaffolding won't work with the EF6 context referenced from the class library.)</span></span>

## <a name="summary"></a><span data-ttu-id="53a15-151">まとめ</span><span class="sxs-lookup"><span data-stu-id="53a15-151">Summary</span></span>

<span data-ttu-id="53a15-152">この記事は、ASP.NET Core アプリケーションで Entity Framework 6 を使用するための基本的なガイダンスを提供しています。</span><span class="sxs-lookup"><span data-stu-id="53a15-152">This article has provided basic guidance for using Entity Framework 6 in an ASP.NET Core application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53a15-153">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="53a15-153">Additional Resources</span></span>

* [<span data-ttu-id="53a15-154">Entity Framework のコード ベースの構成</span><span class="sxs-lookup"><span data-stu-id="53a15-154">Entity Framework - Code-Based Configuration</span></span>](https://msdn.microsoft.com/data/jj680699.aspx)
