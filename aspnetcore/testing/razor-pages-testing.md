---
title: "Razor ページの単位と ASP.NET Core でのテストの統合"
author: guardrex
description: "Razor ページのアプリの単体テストや統合のテストを作成する方法を説明します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/razor-pages-testing
ms.openlocfilehash: 3f53924e0b36b7924d82f97a8702aa461d9ebd78
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="razor-pages-unit-and-integration-testing-in-aspnet-core"></a><span data-ttu-id="da204-103">Razor ページの単位と ASP.NET Core でのテストの統合</span><span class="sxs-lookup"><span data-stu-id="da204-103">Razor Pages unit and integration testing in ASP.NET Core</span></span>

<span data-ttu-id="da204-104">作成者: [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="da204-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="da204-105">ASP.NET Core は、ユニットおよび Razor ページのアプリの統合のテストをサポートします。</span><span class="sxs-lookup"><span data-stu-id="da204-105">ASP.NET Core supports unit and integration testing of Razor Pages apps.</span></span> <span data-ttu-id="da204-106">データ アクセス層 (DAL)、ページのモデル、およびコンポーネントの統合ページのテストに役立ちますを確認してください。</span><span class="sxs-lookup"><span data-stu-id="da204-106">Testing the data access layer (DAL), page models, and integrated page components helps ensure:</span></span>

* <span data-ttu-id="da204-107">Razor ページのアプリの部分を使用とは別に単位としてまとめてアプリの構築中にあります。</span><span class="sxs-lookup"><span data-stu-id="da204-107">Parts of a Razor Pages app work independently and together as a unit during app construction.</span></span>
* <span data-ttu-id="da204-108">クラスとメソッドには、責任の範囲が制限されます。</span><span class="sxs-lookup"><span data-stu-id="da204-108">Classes and methods have limited scopes of responsibility.</span></span>
* <span data-ttu-id="da204-109">その他のドキュメントは、アプリの動作に存在します。</span><span class="sxs-lookup"><span data-stu-id="da204-109">Additional documentation exists on how the app should behave.</span></span>
* <span data-ttu-id="da204-110">エラー コードの更新によってもたらさの回帰は、自動ビルドと配置中に検出されました。</span><span class="sxs-lookup"><span data-stu-id="da204-110">Regressions, which are errors brought about by updates to the code, are found during automated building and deployment.</span></span>

<span data-ttu-id="da204-111">このトピックでは、Razor ページ アプリ、単体テスト、および統合の基本的な知識があると仮定をテストします。</span><span class="sxs-lookup"><span data-stu-id="da204-111">This topic assumes that you have a basic understanding of Razor Pages apps, unit testing, and integration testing.</span></span> <span data-ttu-id="da204-112">Razor ページのアプリやテストの概念に習熟していない場合は、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="da204-112">If you're unfamiliar with Razor Pages apps or testing concepts, see the following topics:</span></span>

* [<span data-ttu-id="da204-113">Razor ページを始める</span><span class="sxs-lookup"><span data-stu-id="da204-113">Introduction to Razor Pages</span></span>](xref:mvc/razor-pages/index)
* [<span data-ttu-id="da204-114">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="da204-114">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="da204-115">単体テスト c# dotnet テスト、xUnit を使用して .NET Core</span><span class="sxs-lookup"><span data-stu-id="da204-115">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="da204-116">統合テスト</span><span class="sxs-lookup"><span data-stu-id="da204-116">Integration testing</span></span>](xref:testing/integration-testing)

<span data-ttu-id="da204-117">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="da204-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/razor-pages-testing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="da204-118">サンプル プロジェクトは、2 個のアプリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="da204-118">The sample project is composed of two apps:</span></span>

| <span data-ttu-id="da204-119">アプリ</span><span class="sxs-lookup"><span data-stu-id="da204-119">App</span></span>         | <span data-ttu-id="da204-120">プロジェクト フォルダー</span><span class="sxs-lookup"><span data-stu-id="da204-120">Project folder</span></span>                        | <span data-ttu-id="da204-121">説明</span><span class="sxs-lookup"><span data-stu-id="da204-121">Description</span></span> |
| ----------- | ------------------------------------- | ----------- |
| <span data-ttu-id="da204-122">メッセージ アプリ</span><span class="sxs-lookup"><span data-stu-id="da204-122">Message app</span></span> | <span data-ttu-id="da204-123">*src/RazorPagesTestingSample*</span><span class="sxs-lookup"><span data-stu-id="da204-123">*src/RazorPagesTestingSample*</span></span>         | <span data-ttu-id="da204-124">ユーザーを追加、1 つを削除、削除、およびメッセージを分析できます。</span><span class="sxs-lookup"><span data-stu-id="da204-124">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="da204-125">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="da204-125">Test app</span></span>    | <span data-ttu-id="da204-126">*tests/RazorPagesTestingSample.Tests*</span><span class="sxs-lookup"><span data-stu-id="da204-126">*tests/RazorPagesTestingSample.Tests*</span></span> | <span data-ttu-id="da204-127">メッセージ アプリをテストするために使用します。</span><span class="sxs-lookup"><span data-stu-id="da204-127">Used to test the message app.</span></span><ul><li><span data-ttu-id="da204-128">単体テスト: データ アクセス層 (DAL)、インデックス ページのモデル</span><span class="sxs-lookup"><span data-stu-id="da204-128">Unit tests: Data access layer (DAL), Index page model</span></span></li><li><span data-ttu-id="da204-129">統合テスト: インデックス ページ</span><span class="sxs-lookup"><span data-stu-id="da204-129">Integration tests: Index page</span></span></li></ul> |

<span data-ttu-id="da204-130">などの IDE の組み込みのテスト機能を使用してテストを実行できる[Visual Studio](https://www.visualstudio.com/vs/)です。</span><span class="sxs-lookup"><span data-stu-id="da204-130">The tests can be run using the built-in testing features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="da204-131">使用して場合[Visual Studio Code](https://code.visualstudio.com/)またはコマンド プロンプトで次のコマンドを実行、コマンドライン、 *tests/RazorPagesTestingSample.Tests*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="da204-131">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

```console
dotnet test
```

## <a name="message-app-organization"></a><span data-ttu-id="da204-132">メッセージ アプリ組織</span><span class="sxs-lookup"><span data-stu-id="da204-132">Message app organization</span></span>

<span data-ttu-id="da204-133">メッセージ アプリでは、次の特性を持つ単純な Razor ページ メッセージ システムを示します。</span><span class="sxs-lookup"><span data-stu-id="da204-133">The message app is a simple Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="da204-134">アプリのインデックス ページ (*Pages/Index.cshtml*と*Pages/Index.cshtml.cs*) UI とページを追加、削除、およびメッセージ (メッセージあたりの平均ワード) の分析を制御するモデルのメソッドを提供.</span><span class="sxs-lookup"><span data-stu-id="da204-134">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="da204-135">メッセージは、`Message`クラス (*Data/Message.cs*) 2 つのプロパティを持つ: `Id` (キー) と`Text`(メッセージ)。</span><span class="sxs-lookup"><span data-stu-id="da204-135">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="da204-136">`Text`プロパティは必須であり 200 文字までに制限されます。</span><span class="sxs-lookup"><span data-stu-id="da204-136">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="da204-137">使用してメッセージが格納される[Entity Framework のメモリ内データベース](/ef/core/providers/in-memory/)&#8224;。</span><span class="sxs-lookup"><span data-stu-id="da204-137">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="da204-138">アプリには、データベース コンテキスト クラスのデータ アクセス層 (DAL) が含まれています`AppDbContext`(*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-138">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span> <span data-ttu-id="da204-139">DAL メソッドをマーク`virtual`、これにより、テストで使用するためのメソッドをモックします。</span><span class="sxs-lookup"><span data-stu-id="da204-139">The DAL methods are marked `virtual`, which allows mocking the methods for use in the tests.</span></span>
* <span data-ttu-id="da204-140">データベースがアプリの起動時に空の場合、メッセージ ストアは、3 つのメッセージで初期化されます。</span><span class="sxs-lookup"><span data-stu-id="da204-140">If the database is empty on app startup, the message store is initialized with three messages.</span></span> <span data-ttu-id="da204-141">これら*メッセージをシード*テストにも使用します。</span><span class="sxs-lookup"><span data-stu-id="da204-141">These *seeded messages* are also used in testing.</span></span>

<span data-ttu-id="da204-142">&#8224;です。EF トピック[InMemory でテスト](/ef/core/miscellaneous/testing/in-memory)MSTest でテストするインメモリ データベースを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="da204-142">&#8224;The EF topic, [Testing with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for testing with MSTest.</span></span> <span data-ttu-id="da204-143">このトピックでは、 [xUnit](https://xunit.github.io/)テスト フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="da204-143">This topic uses the [xUnit](https://xunit.github.io/) testing framework.</span></span> <span data-ttu-id="da204-144">テストの概念とさまざまなテスト フレームワークの間でのテストの実装は、似ているが同一でです。</span><span class="sxs-lookup"><span data-stu-id="da204-144">Testing concepts and test implementations across different testing frameworks are similar but not identical.</span></span>

<span data-ttu-id="da204-145">アプリを使用していないが、[リポジトリ パターン](http://martinfowler.com/eaaCatalog/repository.html)の有効な例が表示されない、[作業単位 (UoW) パターン](https://martinfowler.com/eaaCatalog/unitOfWork.html)、Razor ページには、開発のこれらのパターンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="da204-145">Although the app doesn't use the [repository pattern](http://martinfowler.com/eaaCatalog/repository.html) and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="da204-146">詳細については、次を参照してください[インフラストラクチャの永続性レイヤーをデザイン](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)、 [ASP.NET MVC アプリケーションでリポジトリおよび単位の作業パターンを実装する](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)、および[テスト。コント ローラー ロジック](/aspnet/core/mvc/controllers/testing)(このサンプルは、リポジトリ パターンを実装)。</span><span class="sxs-lookup"><span data-stu-id="da204-146">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design), [Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application), and [Testing controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

## <a name="test-app-organization"></a><span data-ttu-id="da204-147">組織のアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="da204-147">Test app organization</span></span>

<span data-ttu-id="da204-148">テスト アプリケーションは、コンソール アプリケーション内、 *tests/RazorPagesTestingSample.Tests*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="da204-148">The test app is a console app inside the *tests/RazorPagesTestingSample.Tests* folder:</span></span>

| <span data-ttu-id="da204-149">App フォルダーのテスト</span><span class="sxs-lookup"><span data-stu-id="da204-149">Test app folder</span></span>    | <span data-ttu-id="da204-150">説明</span><span class="sxs-lookup"><span data-stu-id="da204-150">Description</span></span> |
| ------------------ | ----------- |
| <span data-ttu-id="da204-151">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="da204-151">*IntegrationTests*</span></span> | <ul><li><span data-ttu-id="da204-152">*IndexPageTest.cs*インデックス ページの統合テストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="da204-152">*IndexPageTest.cs* contains the integration tests for the Index page.</span></span></li><li><span data-ttu-id="da204-153">*TestFixture.cs*メッセージ アプリをテストするテストのホストを作成します。</span><span class="sxs-lookup"><span data-stu-id="da204-153">*TestFixture.cs* creates the test host to test the message app.</span></span></li></ul> |
| <span data-ttu-id="da204-154">*UnitTests*</span><span class="sxs-lookup"><span data-stu-id="da204-154">*UnitTests*</span></span>        | <ul><li><span data-ttu-id="da204-155">*DataAccessLayerTest.cs* DAL の単体テストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="da204-155">*DataAccessLayerTest.cs* contains the unit tests for the DAL.</span></span></li><li><span data-ttu-id="da204-156">*IndexPageTest.cs*インデックス ページのモデルの単体テストが含まれています。</span><span class="sxs-lookup"><span data-stu-id="da204-156">*IndexPageTest.cs* contains the unit tests for the Index page model.</span></span></li></ul> |
| <span data-ttu-id="da204-157">*ユーティリティ*</span><span class="sxs-lookup"><span data-stu-id="da204-157">*Utilities*</span></span>        | <span data-ttu-id="da204-158">*Utilities.cs*を格納します。</span><span class="sxs-lookup"><span data-stu-id="da204-158">*Utilities.cs* contains the:</span></span><ul><li><span data-ttu-id="da204-159">`TestingDbContextOptions` メソッドは、データベースがその基準条件テストごとにリセットできるように各 DAL 単体テストのコンテキストのオプションで新しいデータベースを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="da204-159">`TestingDbContextOptions` method used to create new database context options for each DAL unit test so that the database is reset to its baseline condition for each test.</span></span></li><li><span data-ttu-id="da204-160">`GetRequestContentAsync` 準備に使用する方法、`HttpClient`と統合テスト中に、メッセージのアプリに送信される要求のコンテンツ。</span><span class="sxs-lookup"><span data-stu-id="da204-160">`GetRequestContentAsync` method used to prepare the `HttpClient` and content for requests that are sent to the message app during integration testing.</span></span></li></ul>

<span data-ttu-id="da204-161">テスト フレームワークが[xUnit](https://xunit.github.io/)です。</span><span class="sxs-lookup"><span data-stu-id="da204-161">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="da204-162">フレームワークのモック オブジェクトが[Moq](https://github.com/moq/moq4)です。</span><span class="sxs-lookup"><span data-stu-id="da204-162">The object mocking framework is [Moq](https://github.com/moq/moq4).</span></span> <span data-ttu-id="da204-163">使用して統合テストが実施、 [ASP.NET Core テスト ホスト](xref:testing/integration-testing#the-test-host)です。</span><span class="sxs-lookup"><span data-stu-id="da204-163">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span>

## <a name="unit-testing-the-data-access-layer-dal"></a><span data-ttu-id="da204-164">単体テスト、データ アクセス層 (DAL)</span><span class="sxs-lookup"><span data-stu-id="da204-164">Unit testing the data access layer (DAL)</span></span>

<span data-ttu-id="da204-165">メッセージ アプリを持つ、DAL に含まれている 4 つのメソッド、`AppDbContext`クラス (*src/RazorPagesTestingSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-165">The message app has a DAL with four methods contained in the `AppDbContext` class (*src/RazorPagesTestingSample/Data/AppDbContext.cs*).</span></span> <span data-ttu-id="da204-166">各メソッドでは、テスト アプリの 1 つまたは 2 つの単体テストがあります。</span><span class="sxs-lookup"><span data-stu-id="da204-166">Each method has one or two unit tests in the test app.</span></span>

| <span data-ttu-id="da204-167">DAL メソッド</span><span class="sxs-lookup"><span data-stu-id="da204-167">DAL method</span></span>               | <span data-ttu-id="da204-168">関数</span><span class="sxs-lookup"><span data-stu-id="da204-168">Function</span></span>                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | <span data-ttu-id="da204-169">取得、`List<Message>`順に並べ替えて、データベースから、`Text`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="da204-169">Obtains a `List<Message>` from the database sorted by the `Text` property.</span></span> |
| `AddMessageAsync`        | <span data-ttu-id="da204-170">追加、`Message`データベースにします。</span><span class="sxs-lookup"><span data-stu-id="da204-170">Adds a `Message` to the database.</span></span>                                          |
| `DeleteAllMessagesAsync` | <span data-ttu-id="da204-171">すべて削除`Message`データベースからのエントリ。</span><span class="sxs-lookup"><span data-stu-id="da204-171">Deletes all `Message` entries from the database.</span></span>                           |
| `DeleteMessageAsync`     | <span data-ttu-id="da204-172">1 つを削除`Message`によってデータベースから`Id`です。</span><span class="sxs-lookup"><span data-stu-id="da204-172">Deletes a single `Message` from the database by `Id`.</span></span>                      |

<span data-ttu-id="da204-173">DAL の単体テストを必要と[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)新たに作成するときに`AppDbContext`テストごとにします。</span><span class="sxs-lookup"><span data-stu-id="da204-173">Unit tests of the DAL require [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) when creating a new `AppDbContext` for each test.</span></span> <span data-ttu-id="da204-174">作成する方法の 1 つ、`DbContextOptions`各テストは、使用するため、 [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span><span class="sxs-lookup"><span data-stu-id="da204-174">One approach to creating the `DbContextOptions` for each test is to use a [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):</span></span>

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="da204-175">このアプローチの問題は、各テストがどのような状態、前のテストのための状態でデータベースを受信します。</span><span class="sxs-lookup"><span data-stu-id="da204-175">The problem with this approach is that each test receives the database in whatever state the previous test left it.</span></span> <span data-ttu-id="da204-176">これは問題となる互いに干渉しないアトミック単位テストを作成しようとしています。</span><span class="sxs-lookup"><span data-stu-id="da204-176">This can be problematic when trying to write atomic unit tests that don't interfere with each other.</span></span> <span data-ttu-id="da204-177">強制的に、`AppDbContext`するには、新しいデータベース コンテキストを使用して、各テストについて、指定、`DbContextOptions`新しいサービス プロバイダーに基づいているインスタンス。</span><span class="sxs-lookup"><span data-stu-id="da204-177">To force the `AppDbContext` to use a new database context for each test, supply a `DbContextOptions` instance that's based on a new service provider.</span></span> <span data-ttu-id="da204-178">テスト アプリを使用してこれを行う方法を示しています。 その`Utilities`クラス メソッド`TestingDbContextOptions`(*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-178">The test app shows how to do this using its `Utilities` class method `TestingDbContextOptions` (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet1)]

<span data-ttu-id="da204-179">使用して、 `DbContextOptions` DAL 単体テストで、新しいデータベース インスタンスにアトミックに実行するには、各テスト。</span><span class="sxs-lookup"><span data-stu-id="da204-179">Using the `DbContextOptions` in the DAL unit tests allows each test to run atomically with a fresh database instance:</span></span>

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

<span data-ttu-id="da204-180">内の各テスト メソッド、`DataAccessLayerTest`クラス (*UnitTests/DataAccessLayerTest.cs*) のような配置 Act アサート パターンに従います。</span><span class="sxs-lookup"><span data-stu-id="da204-180">Each test method in the `DataAccessLayerTest` class (*UnitTests/DataAccessLayerTest.cs*) follows a similar Arrange-Act-Assert pattern:</span></span>

1. <span data-ttu-id="da204-181">配置: データベースが構成されているテストや、予想される結果が定義されています。</span><span class="sxs-lookup"><span data-stu-id="da204-181">Arrange: The database is configured for the test and/or the expected outcome is defined.</span></span>
1. <span data-ttu-id="da204-182">Act: テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="da204-182">Act: The test is executed.</span></span>
1. <span data-ttu-id="da204-183">アサート: アサーションに対するテスト結果が成功したかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="da204-183">Assert: Assertions are made to determine if the test result is a success.</span></span>

<span data-ttu-id="da204-184">たとえば、`DeleteMessageAsync`で識別される 1 つのメッセージを削除するため、メソッドはその`Id`(*src/RazorPagesTestingSample/Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-184">For example, the `DeleteMessageAsync` method is responsible for removing a single message identified by its `Id` (*src/RazorPagesTestingSample/Data/AppDbContext.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/AppDbContext.cs?name=snippet4)]

<span data-ttu-id="da204-185">このメソッドの 2 つのテストがあります。</span><span class="sxs-lookup"><span data-stu-id="da204-185">There are two tests for this method.</span></span> <span data-ttu-id="da204-186">1 つのテストは、メソッドは、メッセージがデータベースに存在する場合、メッセージを削除を確認します。</span><span class="sxs-lookup"><span data-stu-id="da204-186">One test checks that the method deletes a message when the message is present in the database.</span></span> <span data-ttu-id="da204-187">データベースを変更しないこと、他のメソッド テスト メッセージ`Id`削除が存在しないためです。</span><span class="sxs-lookup"><span data-stu-id="da204-187">The other method tests that the database doesn't change if the message `Id` for deletion doesn't exist.</span></span> <span data-ttu-id="da204-188">`DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="da204-188">The `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound` method is shown below:</span></span>

[!code-csharp[](razor-pages-testing/sample_snapshot/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="da204-189">最初に、メソッドが実行の配置手順 Act 手順の準備が行われる。</span><span class="sxs-lookup"><span data-stu-id="da204-189">First, the method performs the Arrange step, where preparation for the Act step takes place.</span></span> <span data-ttu-id="da204-190">シード処理のメッセージを取得し、保持されている`seedMessages`です。</span><span class="sxs-lookup"><span data-stu-id="da204-190">The seeding messages are obtained and held in `seedMessages`.</span></span> <span data-ttu-id="da204-191">シード処理のメッセージは、データベースに保存されます。</span><span class="sxs-lookup"><span data-stu-id="da204-191">The seeding messages are saved into the database.</span></span> <span data-ttu-id="da204-192">メッセージを`Id`の`1`削除用に設定されています。</span><span class="sxs-lookup"><span data-stu-id="da204-192">The message with an `Id` of `1` is set for deletion.</span></span> <span data-ttu-id="da204-193">ときに、`DeleteMessageAsync`メソッドが実行され、予期されるメッセージには、すべてのメッセージを 1 つを除くが必要な`Id`の`1`します。</span><span class="sxs-lookup"><span data-stu-id="da204-193">When the `DeleteMessageAsync` method is executed, the expected messages should have all of the messages except for the one with an `Id` of `1`.</span></span> <span data-ttu-id="da204-194">`expectedMessages`変数はこの予想される結果を表します。</span><span class="sxs-lookup"><span data-stu-id="da204-194">The `expectedMessages` variable represents this expected outcome.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

<span data-ttu-id="da204-195">メソッドの動作:`DeleteMessageAsync`で渡すメソッドが実行される、`recId`の`1`:</span><span class="sxs-lookup"><span data-stu-id="da204-195">The method acts: The `DeleteMessageAsync` method is executed passing in the `recId` of `1`:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

<span data-ttu-id="da204-196">メソッドの最後に、取得、`Messages`コンテキストからとを比較、 `expectedMessages` 2 つが等しいことをアサートするとします。</span><span class="sxs-lookup"><span data-stu-id="da204-196">Finally, the method obtains the `Messages` from the context and compares it to the `expectedMessages` asserting that the two are equal:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

<span data-ttu-id="da204-197">比較するために、2 つ`List<Message>`は、同じです。</span><span class="sxs-lookup"><span data-stu-id="da204-197">In order to compare that the two `List<Message>` are the same:</span></span>

* <span data-ttu-id="da204-198">メッセージが順`Id`です。</span><span class="sxs-lookup"><span data-stu-id="da204-198">The messages are ordered by `Id`.</span></span>
* <span data-ttu-id="da204-199">メッセージのペアの比較、`Text`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="da204-199">Message pairs are compared on the `Text` property.</span></span>

<span data-ttu-id="da204-200">ようなテスト メソッド、`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`が存在しないメッセージを削除しようとしての結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="da204-200">A similar test method, `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` checks the result of attempting to delete a message that doesn't exist.</span></span> <span data-ttu-id="da204-201">ここでは、データベース内の予期されるメッセージが、実際のメッセージの後に等しいにする必要があります、`DeleteMessageAsync`メソッドを実行します。</span><span class="sxs-lookup"><span data-stu-id="da204-201">In this case, the expected messages in the database should be equal to the actual messages after the `DeleteMessageAsync` method is executed.</span></span> <span data-ttu-id="da204-202">ありません、データベースのコンテンツを変更します。</span><span class="sxs-lookup"><span data-stu-id="da204-202">There should be no change to the database's content:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-testing-the-page-model-methods"></a><span data-ttu-id="da204-203">単体テストのページのモデルのメソッド</span><span class="sxs-lookup"><span data-stu-id="da204-203">Unit testing the page model methods</span></span>

<span data-ttu-id="da204-204">別の一連の単体テストは、ページのモデルのメソッドのテストを担当します。</span><span class="sxs-lookup"><span data-stu-id="da204-204">Another set of unit tests is responsible for testing page model methods.</span></span> <span data-ttu-id="da204-205">メッセージのアプリで、インデックス ページのモデル内にある、`IndexModel`クラス*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*です。</span><span class="sxs-lookup"><span data-stu-id="da204-205">In the message app, the Index page models are found in the `IndexModel` class in *src/RazorPagesTestingSample/Pages/Index.cshtml.cs*.</span></span>

| <span data-ttu-id="da204-206">ページ モデル メソッド</span><span class="sxs-lookup"><span data-stu-id="da204-206">Page model method</span></span> | <span data-ttu-id="da204-207">関数</span><span class="sxs-lookup"><span data-stu-id="da204-207">Function</span></span> |
| ----------------- | -------- | 
| `OnGetAsync` | <span data-ttu-id="da204-208">使用して、UI の DAL からメッセージを取得、`GetMessagesAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="da204-208">Obtains the messages from the DAL for the UI using the `GetMessagesAsync` method.</span></span> |
| `OnPostAddMessageAsync` | <span data-ttu-id="da204-209">場合、`ModelState`が有効では、呼び出しの`AddMessageAsync`をデータベースにメッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="da204-209">If the `ModelState` is valid, calls `AddMessageAsync` to add a message to the database.</span></span> | 
| `OnPostDeleteAllMessagesAsync` | <span data-ttu-id="da204-210">呼び出し`DeleteAllMessagesAsync`をすべてのデータベースにメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="da204-210">Calls `DeleteAllMessagesAsync` to delete all of the messages in the database.</span></span> |
| `OnPostDeleteMessageAsync` | <span data-ttu-id="da204-211">実行`DeleteMessageAsync`でメッセージを削除する、`Id`指定します。</span><span class="sxs-lookup"><span data-stu-id="da204-211">Executes `DeleteMessageAsync` to delete a message with the `Id` specified.</span></span> |
| `OnPostAnalyzeMessagesAsync` | <span data-ttu-id="da204-212">1 つまたは複数のメッセージが、データベース内にある場合は、1 つのメッセージの単語の平均数を計算します。</span><span class="sxs-lookup"><span data-stu-id="da204-212">If one or more messages are in the database, calculates the average number of words per message.</span></span> |

<span data-ttu-id="da204-213">7 つのテストを使用してページ モデル メソッドがテストされます、`IndexPageTest`クラス (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-213">The page model methods are tested using seven tests in the `IndexPageTest` class (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*).</span></span> <span data-ttu-id="da204-214">テストは、使い慣れた配置をアサート Act パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="da204-214">The tests use the familiar Arrange-Assert-Act pattern.</span></span> <span data-ttu-id="da204-215">これらのテストが焦点になりました。</span><span class="sxs-lookup"><span data-stu-id="da204-215">These tests focus on:</span></span>

* <span data-ttu-id="da204-216">かどうか方法に従って、正しい動作を決定するときに、`ModelState`が無効です。</span><span class="sxs-lookup"><span data-stu-id="da204-216">Determining if the methods follow the correct behavior when the `ModelState` is invalid.</span></span>
* <span data-ttu-id="da204-217">正しいメソッドを確認する生成`IActionResult`です。</span><span class="sxs-lookup"><span data-stu-id="da204-217">Confirming the methods produce the correct `IActionResult`.</span></span>
* <span data-ttu-id="da204-218">プロパティ値の割り当てが正しく行われたことを確認しています。</span><span class="sxs-lookup"><span data-stu-id="da204-218">Checking that property value assignments are made correctly.</span></span>

<span data-ttu-id="da204-219">このテストのグループは、多くの場合、ページ モデル メソッドが実行される Act ステップに必要なデータを生成するために DAL のメソッドをモックします。</span><span class="sxs-lookup"><span data-stu-id="da204-219">This group of tests often mock the methods of the DAL to produce expected data for the Act step where a page model method is executed.</span></span> <span data-ttu-id="da204-220">たとえば、`GetMessagesAsync`のメソッド、`AppDbContext`出力を生成するモックします。</span><span class="sxs-lookup"><span data-stu-id="da204-220">For example, the `GetMessagesAsync` method of the `AppDbContext` is mocked to produce output.</span></span> <span data-ttu-id="da204-221">ページのモデル メソッドは、このメソッドを実行するとき、モックは結果を返します。</span><span class="sxs-lookup"><span data-stu-id="da204-221">When a page model method executes this method, the mock returns the result.</span></span> <span data-ttu-id="da204-222">データベースからは、データがありません。</span><span class="sxs-lookup"><span data-stu-id="da204-222">The data doesn't come from the database.</span></span> <span data-ttu-id="da204-223">これには、DAL を使用してページ モデルのテストでの予測可能な信頼性の高いテスト条件が作成されます。</span><span class="sxs-lookup"><span data-stu-id="da204-223">This creates predictable, reliable test conditions for using the DAL in the page model tests.</span></span>

<span data-ttu-id="da204-224">`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`番組をテストする方法、`GetMessagesAsync`メソッドは、ページのモデルのモックします。</span><span class="sxs-lookup"><span data-stu-id="da204-224">The `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` test shows how the `GetMessagesAsync` method is mocked for the page model:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="da204-225">ときに、 `OnGetAsync` Act の手順でメソッドが実行され、ページのモデルの呼び出し`GetMessagesAsync`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="da204-225">When the `OnGetAsync` method is executed in the Act step, it calls the page model's `GetMessagesAsync` method.</span></span>

<span data-ttu-id="da204-226">単体テストの Act の手順 (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-226">Unit test Act step (*tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="da204-227">`IndexPage` ページのモデルの`OnGetAsync`メソッド (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-227">`IndexPage` page model's `OnGetAsync` method (*src/RazorPagesTestingSample/Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

<span data-ttu-id="da204-228">`GetMessagesAsync` DAL でメソッドが、このメソッドの呼び出しの結果を返さない。</span><span class="sxs-lookup"><span data-stu-id="da204-228">The `GetMessagesAsync` method in the DAL doesn't return the result for this method call.</span></span> <span data-ttu-id="da204-229">モック バージョンのメソッドは、結果を返します。</span><span class="sxs-lookup"><span data-stu-id="da204-229">The mocked version of the method returns the result.</span></span>

<span data-ttu-id="da204-230">`Assert`手順、実際のメッセージ (`actualMessages`) から割り当てられた、`Messages`ページ モデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="da204-230">In the `Assert` step, the actual messages (`actualMessages`) are assigned from the `Messages` property of the page model.</span></span> <span data-ttu-id="da204-231">型チェックは、メッセージが割り当てられている場合にも実行されます。</span><span class="sxs-lookup"><span data-stu-id="da204-231">A type check is also performed when the messages are assigned.</span></span> <span data-ttu-id="da204-232">比較に予測と実際のメッセージはその`Text`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="da204-232">The expected and actual messages are compared by their `Text` properties.</span></span> <span data-ttu-id="da204-233">テストをアサートする 2 つ`List<Message>`インスタンスが同じメッセージを格納します。</span><span class="sxs-lookup"><span data-stu-id="da204-233">The test asserts that the two `List<Message>` instances contain the same messages.</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet3)]

<span data-ttu-id="da204-234">このグループの他のテストの作成 ページを含むモデル オブジェクト、 `DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`確立するために、 `PageContext`、 `ViewDataDictionary`、および`PageContext`です。</span><span class="sxs-lookup"><span data-stu-id="da204-234">Other tests in this group create page model objects that include the `DefaultHttpContext`, the `ModelStateDictionary`, an `ActionContext` to establish the `PageContext`, a `ViewDataDictionary`, and a `PageContext`.</span></span> <span data-ttu-id="da204-235">これらは、テストを実施するのに便利です。</span><span class="sxs-lookup"><span data-stu-id="da204-235">These are useful in conducting tests.</span></span> <span data-ttu-id="da204-236">たとえば、メッセージ アプリを確立、`ModelState`でエラーが`AddModelError`ことを確認する有効な`PageResult`ときに返される`OnPostAddMessageAsync`が実行されます。</span><span class="sxs-lookup"><span data-stu-id="da204-236">For example, the message app establishes a `ModelState` error with `AddModelError` to check that a valid `PageResult` is returned when `OnPostAddMessageAsync` is executed:</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/UnitTests/IndexPageTest.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="integration-testing-the-app"></a><span data-ttu-id="da204-237">統合アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="da204-237">Integration testing the app</span></span>

<span data-ttu-id="da204-238">統合は、アプリのコンポーネントが連携して動作するテストでのフォーカスをテストします。</span><span class="sxs-lookup"><span data-stu-id="da204-238">The integration tests focus on testing that the app's components work together.</span></span> <span data-ttu-id="da204-239">使用して統合テストが実施、 [ASP.NET Core テスト ホスト](xref:testing/integration-testing#the-test-host)です。</span><span class="sxs-lookup"><span data-stu-id="da204-239">Integration tests are conducted using the [ASP.NET Core Test Host](xref:testing/integration-testing#the-test-host).</span></span> <span data-ttu-id="da204-240">完全な要求-応答のライフ サイクルの処理をテストします。</span><span class="sxs-lookup"><span data-stu-id="da204-240">Full request-response lifecycle processing is tested.</span></span> <span data-ttu-id="da204-241">これらのテストがページに、正しい状態コードが生成されることをアサートし、`Location`ヘッダー場合に、設定します。</span><span class="sxs-lookup"><span data-stu-id="da204-241">These tests assert that the page produces the correct status code and `Location` header, if set.</span></span>

<span data-ttu-id="da204-242">例のサンプルのテストとの統合メッセージ アプリのインデックス ページの要求の結果を確認する (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-242">An integration testing example from the sample checks the result of requesting the Index page of the message app (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet1)]

<span data-ttu-id="da204-243">配置手順がありません。</span><span class="sxs-lookup"><span data-stu-id="da204-243">There's no Arrange step.</span></span> <span data-ttu-id="da204-244">`GetAsync`メソッドが、`HttpClient`エンドポイントに GET 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="da204-244">The `GetAsync` method is called on the `HttpClient` to send a GET request to the endpoint.</span></span> <span data-ttu-id="da204-245">テストは、結果が 200 OK ステータス コードであることをアサートします。</span><span class="sxs-lookup"><span data-stu-id="da204-245">The test asserts that the result is a 200-OK status code.</span></span>

<span data-ttu-id="da204-246">メッセージ アプリに POST 要求は、アプリのによって自動的に行われた antiforgery チェックを満たす必要があります[データ保護 antiforgery システム](xref:security/data-protection/introduction)です。</span><span class="sxs-lookup"><span data-stu-id="da204-246">Any POST request to the message app must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="da204-247">テストの POST 要求の配置、するために、アプリケーションをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="da204-247">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="da204-248">ページの要求を行います。</span><span class="sxs-lookup"><span data-stu-id="da204-248">Make a request for the page.</span></span>
1. <span data-ttu-id="da204-249">Antiforgery cookie と、応答からの要求の検証トークンを解析します。</span><span class="sxs-lookup"><span data-stu-id="da204-249">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="da204-250">インプレース antiforgery cookie と要求の検証の POST 要求トークンを作成します。</span><span class="sxs-lookup"><span data-stu-id="da204-250">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="da204-251">`Post_AddMessageHandler_ReturnsRedirectToRoot`メソッドをテストします。</span><span class="sxs-lookup"><span data-stu-id="da204-251">The `Post_AddMessageHandler_ReturnsRedirectToRoot` test method:</span></span>

* <span data-ttu-id="da204-252">メッセージを作成し、`HttpClient`です。</span><span class="sxs-lookup"><span data-stu-id="da204-252">Prepares a message and the `HttpClient`.</span></span>
* <span data-ttu-id="da204-253">アプリへの POST 要求を作成します。</span><span class="sxs-lookup"><span data-stu-id="da204-253">Makes a POST request to the app.</span></span>
* <span data-ttu-id="da204-254">応答がインデックス ページにリダイレクトであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="da204-254">Checks the response is a redirect back to the Index page.</span></span>

<span data-ttu-id="da204-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` メソッド (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-255">`Post_AddMessageHandler_ReturnsRedirectToRoot ` method (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet2)]

<span data-ttu-id="da204-256">`GetRequestContentAsync` Antiforgery cookie と検証トークンの要求でクライアントを準備するユーティリティ メソッドを管理します。</span><span class="sxs-lookup"><span data-stu-id="da204-256">The `GetRequestContentAsync` utility method manages preparing the client with the antiforgery cookie and request verification token.</span></span> <span data-ttu-id="da204-257">メソッドの受信に注意してください、`IDictionary`検証トークンの要求と共にエンコードする要求のデータを渡すには、呼び出し元のテスト メソッドを許可する (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span><span class="sxs-lookup"><span data-stu-id="da204-257">Note how the method receives an `IDictionary` that permits the calling test method to pass in data for the request to encode along with the request verification token (*tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/Utilities/Utilities.cs?name=snippet2&highlight=1-2,8-9,29)]

<span data-ttu-id="da204-258">統合テストでは、アプリの応答の動作をテストするアプリに不適切なデータを渡すことができますも。</span><span class="sxs-lookup"><span data-stu-id="da204-258">Integration tests can also pass bad data to the app to test the app's response behavior.</span></span> <span data-ttu-id="da204-259">メッセージ アプリ 200 文字までにメッセージの長さの制限 (*src/RazorPagesTestingSample/Data/Message.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-259">The message app limits message length to 200 characters (*src/RazorPagesTestingSample/Data/Message.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/src/RazorPagesTestingSample/Data/Message.cs?name=snippet1&highlight=7)]

<span data-ttu-id="da204-260">`Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong`テスト`Message`201"X"文字がテキストで明示的に渡します。</span><span class="sxs-lookup"><span data-stu-id="da204-260">The `Post_AddMessageHandler_ReturnsSuccess_WhenMessageTextTooLong` test `Message` explicitly passes in text with 201 "X" characters.</span></span> <span data-ttu-id="da204-261">これは、結果、`ModelState`エラーです。</span><span class="sxs-lookup"><span data-stu-id="da204-261">This results in a `ModelState` error.</span></span> <span data-ttu-id="da204-262">投稿は、インデックスのページに戻るリダイレクトしません。</span><span class="sxs-lookup"><span data-stu-id="da204-262">The POST doesn't redirect back to the Index page.</span></span> <span data-ttu-id="da204-263">200 OK が返されます、 `null` `Location`ヘッダー (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*)。</span><span class="sxs-lookup"><span data-stu-id="da204-263">It returns a 200-OK with a `null` `Location` header (*tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs*):</span></span>

[!code-csharp[](razor-pages-testing/sample/tests/RazorPagesTestingSample.Tests/IntegrationTests/IndexPageTest.cs?name=snippet3&highlight=7,16-17)]

## <a name="see-also"></a><span data-ttu-id="da204-264">関連項目</span><span class="sxs-lookup"><span data-stu-id="da204-264">See also</span></span>

* [<span data-ttu-id="da204-265">単体テスト c# dotnet テスト、xUnit を使用して .NET Core</span><span class="sxs-lookup"><span data-stu-id="da204-265">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="da204-266">統合テスト</span><span class="sxs-lookup"><span data-stu-id="da204-266">Integration testing</span></span>](xref:testing/integration-testing)
* [<span data-ttu-id="da204-267">コントローラーのテスト</span><span class="sxs-lookup"><span data-stu-id="da204-267">Testing controllers</span></span>](xref:mvc/controllers/testing)
* <span data-ttu-id="da204-268">[単体テスト、コード](/visualstudio/test/unit-test-your-code)(Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="da204-268">[Unit Test Your Code](/visualstudio/test/unit-test-your-code) (Visual Studio)</span></span>
* [<span data-ttu-id="da204-269">xUnit.net</span><span class="sxs-lookup"><span data-stu-id="da204-269">xUnit.net</span></span>](https://xunit.github.io/)
* [<span data-ttu-id="da204-270">XUnit.net (.NET Core/ASP.NET Core) の概要</span><span class="sxs-lookup"><span data-stu-id="da204-270">Getting started with xUnit.net (.NET Core/ASP.NET Core)</span></span>](https://xunit.github.io/docs/getting-started-dotnet-core)
* [<span data-ttu-id="da204-271">Moq</span><span class="sxs-lookup"><span data-stu-id="da204-271">Moq</span></span>](https://github.com/moq/moq4)
* [<span data-ttu-id="da204-272">Moq クイック スタート</span><span class="sxs-lookup"><span data-stu-id="da204-272">Moq Quickstart</span></span>](https://github.com/Moq/moq4/wiki/Quickstart)
