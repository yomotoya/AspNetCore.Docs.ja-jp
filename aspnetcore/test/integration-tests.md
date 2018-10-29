---
title: ASP.NET Core で統合テスト
author: guardrex
description: 統合テストによってデータベース、ファイル システム、ネットワークなどのインフラストラクチャ レベルで、アプリのコンポーネントがどのように正しく機能するようになるかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/30/2018
uid: test/integration-tests
ms.openlocfilehash: a136a362cd8973b3684f9a70bd4792d75238eab0
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207876"
---
# <a name="integration-tests-in-aspnet-core"></a><span data-ttu-id="ed417-103">ASP.NET Core で統合テスト</span><span class="sxs-lookup"><span data-stu-id="ed417-103">Integration tests in ASP.NET Core</span></span>

<span data-ttu-id="ed417-104">によって[Luke Latham](https://github.com/guardrex)と[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ed417-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ed417-105">統合テストでは、データベース、ファイル システム、ネットワークなど、アプリのサポート インフラストラクチャを含むレベルでアプリのコンポーネントが正しく動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="ed417-105">Integration tests ensure that an app's components function correctly at a level that includes the app's supporting infrastructure, such as the database, file system, and network.</span></span> <span data-ttu-id="ed417-106">ASP.NET Core では、テスト web ホストとメモリ内のテスト サーバーを単体テスト フレームワークを使用して、統合テストをサポートします。</span><span class="sxs-lookup"><span data-stu-id="ed417-106">ASP.NET Core supports integration tests using a unit test framework with a test web host and an in-memory test server.</span></span>

<span data-ttu-id="ed417-107">このトピックでは、単体テストの基本的な知識を前提とします。</span><span class="sxs-lookup"><span data-stu-id="ed417-107">This topic assumes a basic understanding of unit tests.</span></span> <span data-ttu-id="ed417-108">テストの概念にあまり馴染みがない場合、[.NET Core と .NET Standard の単体テスト](/dotnet/core/testing/) のトピックとそこでリンクされているコンテンツを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ed417-108">If unfamiliar with test concepts, see the [Unit Testing in .NET Core and .NET Standard](/dotnet/core/testing/) topic and its linked content.</span></span>

<span data-ttu-id="ed417-109">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="ed417-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ed417-110">サンプル アプリは、Razor ページ アプリで Razor ページの基本的な知識を前提としています。</span><span class="sxs-lookup"><span data-stu-id="ed417-110">The sample app is a Razor Pages app and assumes a basic understanding of Razor Pages.</span></span> <span data-ttu-id="ed417-111">Razor ページに不慣れな場合は、次のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ed417-111">If unfamiliar with Razor Pages, see the following topics:</span></span>

* [<span data-ttu-id="ed417-112">Razor ページを始める</span><span class="sxs-lookup"><span data-stu-id="ed417-112">Introduction to Razor Pages</span></span>](xref:razor-pages/index)
* [<span data-ttu-id="ed417-113">Razor ページの概要</span><span class="sxs-lookup"><span data-stu-id="ed417-113">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="ed417-114">Razor ページの単体テスト</span><span class="sxs-lookup"><span data-stu-id="ed417-114">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)

> [!NOTE]
> <span data-ttu-id="ed417-115">Spa をテストするには、お勧めするツールなど[Selenium](https://www.seleniumhq.org/)ブラウザーを自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="ed417-115">For testing SPAs, we recommended a tool such as [Selenium](https://www.seleniumhq.org/), which can automate a browser.</span></span>

## <a name="introduction-to-integration-tests"></a><span data-ttu-id="ed417-116">統合テストの概要</span><span class="sxs-lookup"><span data-stu-id="ed417-116">Introduction to integration tests</span></span>

<span data-ttu-id="ed417-117">広範なレベルでのアプリのコンポーネントを評価する統合テスト[単体テスト](/dotnet/core/testing/)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-117">Integration tests evaluate an app's components on a broader level than [unit tests](/dotnet/core/testing/).</span></span> <span data-ttu-id="ed417-118">単体テストは、個々 のクラスのメソッドなどの分離のソフトウェア コンポーネントのテストに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-118">Unit tests are used to test isolated software components, such as individual class methods.</span></span> <span data-ttu-id="ed417-119">統合テストでは、完全に要求を処理するために必要なすべてのコンポーネントを含めることも、予想結果を生成するために、2 つ以上のアプリ コンポーネントが共同作業を確認します。</span><span class="sxs-lookup"><span data-stu-id="ed417-119">Integration tests confirm that two or more app components work together to produce an expected result, possibly including every component required to fully process a request.</span></span>

<span data-ttu-id="ed417-120">これらのより広範なテストは、アプリのインフラストラクチャと多くの場合、次のコンポーネントを含む、フレームワーク全体をテストに使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-120">These broader tests are used to test the app's infrastructure and whole framework, often including the following components:</span></span>

* <span data-ttu-id="ed417-121">データベース</span><span class="sxs-lookup"><span data-stu-id="ed417-121">Database</span></span>
* <span data-ttu-id="ed417-122">ファイル システム</span><span class="sxs-lookup"><span data-stu-id="ed417-122">File system</span></span>
* <span data-ttu-id="ed417-123">ネットワーク アプライアンス</span><span class="sxs-lookup"><span data-stu-id="ed417-123">Network appliances</span></span>
* <span data-ttu-id="ed417-124">要求-応答のパイプライン</span><span class="sxs-lookup"><span data-stu-id="ed417-124">Request-response pipeline</span></span>

<span data-ttu-id="ed417-125">単体テストと呼ばれる使用加工コンポーネント*fakes*または*モック オブジェクト*、インフラストラクチャ コンポーネントの代わりにします。</span><span class="sxs-lookup"><span data-stu-id="ed417-125">Unit tests use fabricated components, known as *fakes* or *mock objects*, in place of infrastructure components.</span></span>

<span data-ttu-id="ed417-126">単体テストとは対照的統合をテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-126">In contrast to unit tests, integration tests:</span></span>

* <span data-ttu-id="ed417-127">運用環境でアプリを使用して、実際のコンポーネントを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed417-127">Use the actual components that the app uses in production.</span></span>
* <span data-ttu-id="ed417-128">多くのコードとデータ処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="ed417-128">Require more code and data processing.</span></span>
* <span data-ttu-id="ed417-129">実行にかかます。</span><span class="sxs-lookup"><span data-stu-id="ed417-129">Take longer to run.</span></span>

<span data-ttu-id="ed417-130">そのため、最も重要なインフラストラクチャへの統合テストの使用を制限します。</span><span class="sxs-lookup"><span data-stu-id="ed417-130">Therefore, limit the use of integration tests to the most important infrastructure scenarios.</span></span> <span data-ttu-id="ed417-131">場合は、動作をテストするには、単体テストまたは統合テストを使用して、単体テストを選択します。</span><span class="sxs-lookup"><span data-stu-id="ed417-131">If a behavior can be tested using either a unit test or an integration test, choose the unit test.</span></span>

> [!TIP]
> <span data-ttu-id="ed417-132">データベースやファイル システムでのデータとファイルのアクセスの順列で可能なすべての統合テストを記述しません。</span><span class="sxs-lookup"><span data-stu-id="ed417-132">Don't write integration tests for every possible permutation of data and file access with databases and file systems.</span></span> <span data-ttu-id="ed417-133">アプリ間で配置を数に関係なくは、データベースとファイル システム、フォーカスのある一連の読み取り、書き込み、更新、および削除の統合テストは、十分にテスト データベースのことし、ファイル システムのコンポーネントと対話します。</span><span class="sxs-lookup"><span data-stu-id="ed417-133">Regardless of how many places across an app interact with databases and file systems, a focused set of read, write, update, and delete integration tests are usually capable of adequately testing database and file system components.</span></span> <span data-ttu-id="ed417-134">これらのコンポーネントと対話する日常的なテスト メソッドのロジックの単位を使用をテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-134">Use unit tests for routine tests of method logic that interact with these components.</span></span> <span data-ttu-id="ed417-135">単体テストでインフラストラクチャを使用して fakes/モック テストの実行を高速化の結果。</span><span class="sxs-lookup"><span data-stu-id="ed417-135">In unit tests, the use of infrastructure fakes/mocks result in faster test execution.</span></span>

> [!NOTE]
> <span data-ttu-id="ed417-136">テスト対象のプロジェクトが頻繁に呼び出される統合テストの説明で、*テスト対象のシステム*、または略して"SUT"。</span><span class="sxs-lookup"><span data-stu-id="ed417-136">In discussions of integration tests, the tested project is frequently called the *system under test*, or "SUT" for short.</span></span>

## <a name="aspnet-core-integration-tests"></a><span data-ttu-id="ed417-137">ASP.NET Core 統合テスト</span><span class="sxs-lookup"><span data-stu-id="ed417-137">ASP.NET Core integration tests</span></span>

<span data-ttu-id="ed417-138">ASP.NET Core で統合テストでは、次の項目が必要です。</span><span class="sxs-lookup"><span data-stu-id="ed417-138">Integration tests in ASP.NET Core require the following:</span></span>

* <span data-ttu-id="ed417-139">含まれてし、テストを実行するテスト プロジェクトが使用されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-139">A test project is used to contain and execute the tests.</span></span> <span data-ttu-id="ed417-140">テスト プロジェクトと呼ばれる、テスト対象の ASP.NET Core プロジェクトを参照している、*テスト対象のシステム*(SUT)。</span><span class="sxs-lookup"><span data-stu-id="ed417-140">The test project has a reference to the tested ASP.NET Core project, called the *system under test* (SUT).</span></span> <span data-ttu-id="ed417-141">_このトピック全体では、"SUT"を使用してテスト済みのアプリを参照してください。_</span><span class="sxs-lookup"><span data-stu-id="ed417-141">_"SUT" is used throughout this topic to refer to the tested app._</span></span>
* <span data-ttu-id="ed417-142">テスト プロジェクトでは、SUT のテスト web ホストを作成し、テスト サーバーのクライアントを使用して、要求と SUT への応答を処理します。</span><span class="sxs-lookup"><span data-stu-id="ed417-142">The test project creates a test web host for the SUT and uses a test server client to handle requests and responses to the SUT.</span></span>
* <span data-ttu-id="ed417-143">テスト ランナーを使用して、テスト結果、テストとレポートを実行できます。</span><span class="sxs-lookup"><span data-stu-id="ed417-143">A test runner is used to execute the tests and report the test results.</span></span>

<span data-ttu-id="ed417-144">統合テストを含む、一般的なイベントのシーケンスのに従って*配置*、 *Act*、および*Assert*テスト ステップ。</span><span class="sxs-lookup"><span data-stu-id="ed417-144">Integration tests follow a sequence of events that include the usual *Arrange*, *Act*, and *Assert* test steps:</span></span>

1. <span data-ttu-id="ed417-145">SUT の web ホストが構成されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-145">The SUT's web host is configured.</span></span>
1. <span data-ttu-id="ed417-146">テスト サーバーのクライアントを作成すると、アプリに要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ed417-146">A test server client is created to submit requests to the app.</span></span>
1. <span data-ttu-id="ed417-147">*配置*テスト ステップが実行されます。 テスト アプリが要求を準備します。</span><span class="sxs-lookup"><span data-stu-id="ed417-147">The *Arrange* test step is executed: The test app prepares a request.</span></span>
1. <span data-ttu-id="ed417-148">*Act*テスト ステップが実行されます。 クライアント要求を送信すると、応答を受信します。</span><span class="sxs-lookup"><span data-stu-id="ed417-148">The *Act* test step is executed: The client submits the request and receives the response.</span></span>
1. <span data-ttu-id="ed417-149">*Assert*テスト ステップが実行された:*実際*として応答が検証された、*渡す*または*失敗*に基づいて、*が必要です*応答します。</span><span class="sxs-lookup"><span data-stu-id="ed417-149">The *Assert* test step is executed: The *actual* response is validated as a *pass* or *fail* based on an *expected* response.</span></span>
1. <span data-ttu-id="ed417-150">すべてのテストが実行されるまで、処理が続きます。</span><span class="sxs-lookup"><span data-stu-id="ed417-150">The process continues until all of the tests are executed.</span></span>
1. <span data-ttu-id="ed417-151">テスト結果が報告されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-151">The test results are reported.</span></span>

<span data-ttu-id="ed417-152">通常、テスト web ホストではテスト用のアプリの通常の web ホストの実行よりも構成されますが異なります。</span><span class="sxs-lookup"><span data-stu-id="ed417-152">Usually, the test web host is configured differently than the app's normal web host for the test runs.</span></span> <span data-ttu-id="ed417-153">たとえば、別のデータベースまたは別のアプリ設定は、テストに使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ed417-153">For example, a different database or different app settings might be used for the tests.</span></span>

<span data-ttu-id="ed417-154">テスト web ホストのメモリ内のテスト サーバーなどのインフラストラクチャ コンポーネント ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)) が提供されているかによって管理される、 [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="ed417-154">Infrastructure components, such as the test web host and in-memory test server ([TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)), are provided or managed by the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package.</span></span> <span data-ttu-id="ed417-155">このパッケージの使用には、テストの作成と実行が効率化されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-155">Use of this package streamlines test creation and execution.</span></span>

<span data-ttu-id="ed417-156">`Microsoft.AspNetCore.Mvc.Testing`パッケージは、次のタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="ed417-156">The `Microsoft.AspNetCore.Mvc.Testing` package handles the following tasks:</span></span>

* <span data-ttu-id="ed417-157">依存関係ファイルのコピー (*\*.deps*) にテスト プロジェクトの SUT から*bin*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="ed417-157">Copies the dependencies file (*\*.deps*) from the SUT into the test project's *bin* folder.</span></span>
* <span data-ttu-id="ed417-158">静的ファイルとページ/ビューは、テストの実行時に検出できるように、コンテンツのルートを SUT のプロジェクトのルートに設定します。</span><span class="sxs-lookup"><span data-stu-id="ed417-158">Sets the content root to the SUT's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="ed417-159">提供、 [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)で SUT をブートス トラップを効率化するクラス`TestServer`します。</span><span class="sxs-lookup"><span data-stu-id="ed417-159">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the SUT with `TestServer`.</span></span>

<span data-ttu-id="ed417-160">[単体テスト](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)ドキュメントが名前のテストにテストと方法に関する推奨事項を実行して、クラスをテストする方法の詳細な手順と共に、テスト プロジェクトとテスト ランナーを設定する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed417-160">The [unit tests](/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation describes how to set up a test project and test runner, along with detailed instructions on how to run tests and recommendations for how to name tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="ed417-161">アプリのテスト プロジェクトを作成する場合は、別々 のプロジェクトに統合テストから単体テストを区切ります。</span><span class="sxs-lookup"><span data-stu-id="ed417-161">When creating a test project for an app, separate the unit tests from the integration tests into different projects.</span></span> <span data-ttu-id="ed417-162">こうことからテストのインフラストラクチャ コンポーネントの単体テストに含まれる誤っていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ed417-162">This helps ensure that infrastructure testing components aren't accidently included in the unit tests.</span></span> <span data-ttu-id="ed417-163">単体テストや統合テストの分離により、テストのどのセットに対してが実行を制御します。</span><span class="sxs-lookup"><span data-stu-id="ed417-163">Separation of unit and integration tests also allows control over which set of tests are run.</span></span>

<span data-ttu-id="ed417-164">これは、Razor ページ アプリのテストの構成と MVC アプリの違いはほとんどありません。</span><span class="sxs-lookup"><span data-stu-id="ed417-164">There's virtually no difference between the configuration for tests of Razor Pages apps and MVC apps.</span></span> <span data-ttu-id="ed417-165">唯一の違いは、テストの名前付け方法です。</span><span class="sxs-lookup"><span data-stu-id="ed417-165">The only difference is in how the tests are named.</span></span> <span data-ttu-id="ed417-166">Razor ページ アプリでページのエンドポイントのテストの名前は通常、ページ モデル クラスの後 (たとえば、`IndexPageTests`インデックス ページのコンポーネントの統合をテストする)。</span><span class="sxs-lookup"><span data-stu-id="ed417-166">In a Razor Pages app, tests of page endpoints are usually named after the page model class (for example, `IndexPageTests` to test component integration for the Index page).</span></span> <span data-ttu-id="ed417-167">MVC アプリでテストは通常は別に整理されたコント ローラー クラスとテスト コント ローラーにちなんだ名前 (たとえば、 `HomeControllerTests` Home コント ローラー用のコンポーネントの統合をテストする)。</span><span class="sxs-lookup"><span data-stu-id="ed417-167">In an MVC app, tests are usually organized by controller classes and named after the controllers they test (for example, `HomeControllerTests` to test component integration for the Home controller).</span></span>

## <a name="test-app-prerequisites"></a><span data-ttu-id="ed417-168">アプリの前提条件をテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-168">Test app prerequisites</span></span>

<span data-ttu-id="ed417-169">テスト プロジェクトが必要です。</span><span class="sxs-lookup"><span data-stu-id="ed417-169">The test project must:</span></span>

* <span data-ttu-id="ed417-170">次のパッケージを参照します。</span><span class="sxs-lookup"><span data-stu-id="ed417-170">Reference the following packages:</span></span>
  - [<span data-ttu-id="ed417-171">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="ed417-171">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)
  - [<span data-ttu-id="ed417-172">Microsoft.AspNetCore.Mvc.Testing</span><span class="sxs-lookup"><span data-stu-id="ed417-172">Microsoft.AspNetCore.Mvc.Testing</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/)
* <span data-ttu-id="ed417-173">Web SDK をプロジェクト ファイルで指定 (`<Project Sdk="Microsoft.NET.Sdk.Web">`)。</span><span class="sxs-lookup"><span data-stu-id="ed417-173">Specify the Web SDK in the project file (`<Project Sdk="Microsoft.NET.Sdk.Web">`).</span></span> <span data-ttu-id="ed417-174">Web SDK は、参照するときに必要な[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-174">The Web SDK is required when referencing the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="ed417-175">これらの前提条件がわかるように、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-175">These prerequisites can be seen in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/).</span></span> <span data-ttu-id="ed417-176">検査、 *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj*ファイル。</span><span class="sxs-lookup"><span data-stu-id="ed417-176">Inspect the *tests/RazorPagesProject.Tests/RazorPagesProject.Tests.csproj* file.</span></span> <span data-ttu-id="ed417-177">サンプル アプリでは、 [xUnit](https://xunit.github.io/)テスト フレームワークと[AngleSharp](https://anglesharp.github.io/)サンプル アプリを参照しているため、パーサー ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="ed417-177">The sample app uses the [xUnit](https://xunit.github.io/) test framework and the [AngleSharp](https://anglesharp.github.io/) parser library, so the sample app also references:</span></span>

* [<span data-ttu-id="ed417-178">xunit</span><span class="sxs-lookup"><span data-stu-id="ed417-178">xunit</span></span>](https://www.nuget.org/packages/xunit/)
* [<span data-ttu-id="ed417-179">xunit.runner.visualstudio</span><span class="sxs-lookup"><span data-stu-id="ed417-179">xunit.runner.visualstudio</span></span>](https://www.nuget.org/packages/xunit.runner.visualstudio/)
* [<span data-ttu-id="ed417-180">AngleSharp</span><span class="sxs-lookup"><span data-stu-id="ed417-180">AngleSharp</span></span>](https://www.nuget.org/packages/AngleSharp/)

## <a name="basic-tests-with-the-default-webapplicationfactory"></a><span data-ttu-id="ed417-181">既定値 WebApplicationFactory の基本的なテスト</span><span class="sxs-lookup"><span data-stu-id="ed417-181">Basic tests with the default WebApplicationFactory</span></span>

<span data-ttu-id="ed417-182">[WebApplicationFactory&lt;TEntryPoint&gt; ](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1)を作成するために使用する[TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)統合テスト用です。</span><span class="sxs-lookup"><span data-stu-id="ed417-182">[WebApplicationFactory&lt;TEntryPoint&gt;](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) is used to create a [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver) for the integration tests.</span></span> <span data-ttu-id="ed417-183">`TEntryPoint` SUT のエントリ ポイント クラスは、通常、`Startup`クラス。</span><span class="sxs-lookup"><span data-stu-id="ed417-183">`TEntryPoint` is the entry point class of the SUT, usually the `Startup` class.</span></span>

<span data-ttu-id="ed417-184">テスト クラスの実装を*クラス フィクスチャ*インターフェイス (`IClassFixture`) を示す、クラスは、テストが含まれ、クラスのテストの間で共有されたオブジェクトのインスタンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="ed417-184">Test classes implement a *class fixture* interface (`IClassFixture`) to indicate the class contains tests and provide shared object instances across the tests in the class.</span></span>

### <a name="basic-test-of-app-endpoints"></a><span data-ttu-id="ed417-185">アプリのエンドポイントの基本的なテスト</span><span class="sxs-lookup"><span data-stu-id="ed417-185">Basic test of app endpoints</span></span>

<span data-ttu-id="ed417-186">次のテスト クラス、`BasicTests`を使用して、 `WebApplicationFactory` SUT を起動して、提供する、 [HttpClient](/dotnet/api/system.net.http.httpclient)テスト メソッドに`Get_EndpointsReturnSuccessAndCorrectContentType`します。</span><span class="sxs-lookup"><span data-stu-id="ed417-186">The following test class, `BasicTests`, uses the `WebApplicationFactory` to bootstrap the SUT and provide an [HttpClient](/dotnet/api/system.net.http.httpclient) to a test method, `Get_EndpointsReturnSuccessAndCorrectContentType`.</span></span> <span data-ttu-id="ed417-187">応答ステータス コードが成功したかどうか、メソッドを確認します (状態コードが 200-299 の範囲) と`Content-Type`ヘッダーが`text/html; charset=utf-8`のいくつかのアプリ ページ。</span><span class="sxs-lookup"><span data-stu-id="ed417-187">The method checks if the response status code is successful (status codes in the range 200-299) and the `Content-Type` header is `text/html; charset=utf-8` for several app pages.</span></span>

<span data-ttu-id="ed417-188">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)のインスタンスを作成します`HttpClient`を自動的にリダイレクトに依存して cookie を処理します。</span><span class="sxs-lookup"><span data-stu-id="ed417-188">[CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) creates an instance of `HttpClient` that automatically follows redirects and handles cookies.</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet1)]

### <a name="test-a-secure-endpoint"></a><span data-ttu-id="ed417-189">セキュリティで保護されたエンドポイントをテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-189">Test a secure endpoint</span></span>

<span data-ttu-id="ed417-190">別のテストで、`BasicTests`クラスは、セキュリティで保護されたエンドポイントがアプリのログイン ページに未認証のユーザーをリダイレクトすることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ed417-190">Another test in the `BasicTests` class checks that a secure endpoint redirects an unauthenticated user to the app's Login page.</span></span>

<span data-ttu-id="ed417-191">SUT で、`/SecurePage`ページ使用して、 [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage)を適用する規則、 [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter)ページにします。</span><span class="sxs-lookup"><span data-stu-id="ed417-191">In the SUT, the `/SecurePage` page uses an [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention to apply an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page.</span></span> <span data-ttu-id="ed417-192">詳細については、次を参照してください。 [Razor ページの承認規則](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-192">For more information, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization#require-authorization-to-access-a-page).</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet1)]

<span data-ttu-id="ed417-193">`Get_SecurePageRequiresAnAuthenticatedUser` 、テスト、 [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)を設定してリダイレクトを許可しないように設定されている[AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect)に`false`:</span><span class="sxs-lookup"><span data-stu-id="ed417-193">In the `Get_SecurePageRequiresAnAuthenticatedUser` test, a [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) is set to disallow redirects by setting [AllowAutoRedirect](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) to `false`:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/BasicTests.cs?name=snippet2)]

<span data-ttu-id="ed417-194">リダイレクトを実行するクライアントを禁止することにより、次のチェックを変更できます。</span><span class="sxs-lookup"><span data-stu-id="ed417-194">By disallowing the client to follow the redirect, the following checks can be made:</span></span>

* <span data-ttu-id="ed417-195">SUT によって返されるステータス コードをチェックして、予想されるに対して[HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode)結果、可能性のあるログイン ページへのリダイレクトの後に最終的な状態コードではなく[HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span><span class="sxs-lookup"><span data-stu-id="ed417-195">The status code returned by the SUT can be checked against the expected [HttpStatusCode.Redirect](/dotnet/api/system.net.httpstatuscode) result, not the final status code after the redirect to the Login page, which would be [HttpStatusCode.OK](/dotnet/api/system.net.httpstatuscode).</span></span>
* <span data-ttu-id="ed417-196">`Location`で始まることを確認する応答ヘッダーのヘッダーの値がオンになって`http://localhost/Identity/Account/Login`、いない最終的なログイン ページの応答、場所、`Location`ヘッダーが存在するでしょう。</span><span class="sxs-lookup"><span data-stu-id="ed417-196">The `Location` header value in the response headers is checked to confirm that it starts with `http://localhost/Identity/Account/Login`, not the final Login page response, where the `Location` header wouldn't be present.</span></span>

<span data-ttu-id="ed417-197">詳細については`WebApplicationFactoryClientOptions`を参照してください、[クライアント オプション](#client-options)セクション。</span><span class="sxs-lookup"><span data-stu-id="ed417-197">For more information on `WebApplicationFactoryClientOptions`, see the [Client options](#client-options) section.</span></span>

## <a name="customize-webapplicationfactory"></a><span data-ttu-id="ed417-198">WebApplicationFactory をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ed417-198">Customize WebApplicationFactory</span></span>

<span data-ttu-id="ed417-199">継承することによってテスト クラスとは無関係に web ホストの構成を作成できます`WebApplicationFactory`1 つまたは複数のカスタム ファクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed417-199">Web host configuration can be created independently of the test classes by inheriting from `WebApplicationFactory` to create one or more custom factories:</span></span>

1. <span data-ttu-id="ed417-200">継承`WebApplicationFactory`オーバーライドと[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-200">Inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost).</span></span> <span data-ttu-id="ed417-201">[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)サービス コレクションの構成を可能に[ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span><span class="sxs-lookup"><span data-stu-id="ed417-201">The [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) allows the configuration of the service collection with [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configureservices):</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/CustomWebApplicationFactory.cs?name=snippet1)]

   <span data-ttu-id="ed417-202">データベースでシード処理で、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)によって実行されます、`InitializeDbForTests`メソッド。</span><span class="sxs-lookup"><span data-stu-id="ed417-202">Database seeding in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) is performed by the `InitializeDbForTests` method.</span></span> <span data-ttu-id="ed417-203">メソッドが記載されて、[統合サンプルのテスト: 組織のアプリをテスト](#test-app-organization)セクション。</span><span class="sxs-lookup"><span data-stu-id="ed417-203">The method is described in the [Integration tests sample: Test app organization](#test-app-organization) section.</span></span>

2. <span data-ttu-id="ed417-204">ユーザー設定を使用して、`CustomWebApplicationFactory`テスト クラスにします。</span><span class="sxs-lookup"><span data-stu-id="ed417-204">Use the custom `CustomWebApplicationFactory` in test classes.</span></span> <span data-ttu-id="ed417-205">次のコードの例では、工場で、`IndexPageTests`クラス。</span><span class="sxs-lookup"><span data-stu-id="ed417-205">The following example uses the factory in the `IndexPageTests` class:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet1)]

   <span data-ttu-id="ed417-206">防ぐために、サンプル アプリのクライアントが構成されている、`HttpClient`次のリダイレクトから。</span><span class="sxs-lookup"><span data-stu-id="ed417-206">The sample app's client is configured to prevent the `HttpClient` from following redirects.</span></span> <span data-ttu-id="ed417-207">説明したように、[セキュリティで保護されたエンドポイントをテスト](#test-a-secure-endpoint) セクションで、これにより、アプリの初回の応答の結果を確認するテストです。</span><span class="sxs-lookup"><span data-stu-id="ed417-207">As explained in the [Test a secure endpoint](#test-a-secure-endpoint) section, this permits tests to check the result of the app's first response.</span></span> <span data-ttu-id="ed417-208">最初の応答は、リダイレクトでこれらのテストの多くで、`Location`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="ed417-208">The first response is a redirect in many of these tests with a `Location` header.</span></span>

3. <span data-ttu-id="ed417-209">一般的なテストを使用して、`HttpClient`と、要求と応答を処理するヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="ed417-209">A typical test uses the `HttpClient` and helper methods to process the request and the response:</span></span>

   [!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet2)]

<span data-ttu-id="ed417-210">SUT への POST 要求は、アプリのによって自動的に行われた偽造防止チェックを満たす必要があります[データ保護の偽造防止システム](xref:security/data-protection/introduction)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-210">Any POST request to the SUT must satisfy the antiforgery check that's automatically made by the app's [data protection antiforgery system](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="ed417-211">テストの POST 要求を配置するために、テスト アプリが必要です。</span><span class="sxs-lookup"><span data-stu-id="ed417-211">In order to arrange for a test's POST request, the test app must:</span></span>

1. <span data-ttu-id="ed417-212">ページの要求を行います。</span><span class="sxs-lookup"><span data-stu-id="ed417-212">Make a request for the page.</span></span>
1. <span data-ttu-id="ed417-213">偽造防止 cookie と、応答からの要求検証トークンを解析します。</span><span class="sxs-lookup"><span data-stu-id="ed417-213">Parse the antiforgery cookie and request validation token from the response.</span></span>
1. <span data-ttu-id="ed417-214">インプレース偽造防止 cookie と要求の検証を含む POST 要求トークンを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed417-214">Make the POST request with the antiforgery cookie and request validation token in place.</span></span>

<span data-ttu-id="ed417-215">`SendAsync`ヘルパー拡張メソッド (*Helpers/HttpClientExtensions.cs*) および`GetDocumentAsync`ヘルパー メソッド (*Helpers/HtmlHelpers.cs*) で、 [のサンプルアプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/)を使用して、 [AngleSharp](https://anglesharp.github.io/)偽造防止チェックを次の方法で処理するためにパーサー。</span><span class="sxs-lookup"><span data-stu-id="ed417-215">The `SendAsync` helper extension methods (*Helpers/HttpClientExtensions.cs*) and the `GetDocumentAsync` helper method (*Helpers/HtmlHelpers.cs*) in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples/) use the [AngleSharp](https://anglesharp.github.io/) parser to handle the antiforgery check with the following methods:</span></span>

* <span data-ttu-id="ed417-216">`GetDocumentAsync` &ndash; 受信、 [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage)を返します、`IHtmlDocument`します。</span><span class="sxs-lookup"><span data-stu-id="ed417-216">`GetDocumentAsync` &ndash; Receives the [HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) and returns an `IHtmlDocument`.</span></span> <span data-ttu-id="ed417-217">`GetDocumentAsync` 準備するファクトリを使用して、*仮想応答*元に基づいて`HttpResponseMessage`します。</span><span class="sxs-lookup"><span data-stu-id="ed417-217">`GetDocumentAsync` uses a factory that prepares a *virtual response* based on the original `HttpResponseMessage`.</span></span> <span data-ttu-id="ed417-218">詳細については、次を参照してください。、 [AngleSharp ドキュメント](https://github.com/AngleSharp/AngleSharp#documentation)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-218">For more information, see the [AngleSharp documentation](https://github.com/AngleSharp/AngleSharp#documentation).</span></span>
* <span data-ttu-id="ed417-219">`SendAsync` 拡張メソッド、 `HttpClient` compose、 [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage)を呼び出すと[SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) SUT に要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ed417-219">`SendAsync` extension methods for the `HttpClient` compose an [HttpRequestMessage](/dotnet/api/system.net.http.httprequestmessage) and call [SendAsync(HttpRequestMessage)](/dotnet/api/system.net.http.httpclient.sendasync#System_Net_Http_HttpClient_SendAsync_System_Net_Http_HttpRequestMessage_) to submit requests to the SUT.</span></span> <span data-ttu-id="ed417-220">オーバー ロード`SendAsync`HTML フォームを受け入れる (`IHtmlFormElement`) および次。</span><span class="sxs-lookup"><span data-stu-id="ed417-220">Overloads for `SendAsync` accept the HTML form (`IHtmlFormElement`) and the following:</span></span>
  - <span data-ttu-id="ed417-221">フォームのボタンの送信 (`IHtmlElement`)</span><span class="sxs-lookup"><span data-stu-id="ed417-221">Submit button of the form (`IHtmlElement`)</span></span>
  - <span data-ttu-id="ed417-222">フォーム値のコレクション (`IEnumerable<KeyValuePair<string, string>>`)</span><span class="sxs-lookup"><span data-stu-id="ed417-222">Form values collection (`IEnumerable<KeyValuePair<string, string>>`)</span></span>
  - <span data-ttu-id="ed417-223">送信ボタン (`IHtmlElement`) の値を形成し、(`IEnumerable<KeyValuePair<string, string>>`)</span><span class="sxs-lookup"><span data-stu-id="ed417-223">Submit button (`IHtmlElement`) and form values (`IEnumerable<KeyValuePair<string, string>>`)</span></span>

> [!NOTE]
> <span data-ttu-id="ed417-224">[AngleSharp](https://anglesharp.github.io/)サード パーティがこのトピックおよびサンプル アプリケーションのデモンストレーションを目的として使用されるライブラリを解析します。</span><span class="sxs-lookup"><span data-stu-id="ed417-224">[AngleSharp](https://anglesharp.github.io/) is a third-party parsing library used for demonstration purposes in this topic and the sample app.</span></span> <span data-ttu-id="ed417-225">AngleSharp はサポートされているがないか、ASP.NET Core アプリの統合をテストするために必要です。</span><span class="sxs-lookup"><span data-stu-id="ed417-225">AngleSharp isn't supported or required for integration testing of ASP.NET Core apps.</span></span> <span data-ttu-id="ed417-226">その他のパーサーができますなど、 [Html 機敏性パック (HAP)](http://html-agility-pack.net/)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-226">Other parsers can be used, such as the [Html Agility Pack (HAP)](http://html-agility-pack.net/).</span></span> <span data-ttu-id="ed417-227">別の方法では、偽造防止システムの要求検証トークンと偽造防止 cookie を直接処理するコードを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed417-227">Another approach is to write code to handle the antiforgery system's request verification token and antiforgery cookie directly.</span></span>

## <a name="customize-the-client-with-withwebhostbuilder"></a><span data-ttu-id="ed417-228">WithWebHostBuilder でクライアントをカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ed417-228">Customize the client with WithWebHostBuilder</span></span>

<span data-ttu-id="ed417-229">追加の構成がテスト メソッド内で必要な場合[WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder)新たに作成します`WebApplicationFactory`で、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)するが、構成でさらにカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="ed417-229">When additional configuration is required within a test method, [WithWebHostBuilder](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.withwebhostbuilder) creates a new `WebApplicationFactory` with an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) that is further customized by configuration.</span></span>

<span data-ttu-id="ed417-230">`Post_DeleteMessageHandler_ReturnsRedirectToRoot`テストのメソッド、[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)の使用方法を示します`WithWebHostBuilder`します。</span><span class="sxs-lookup"><span data-stu-id="ed417-230">The `Post_DeleteMessageHandler_ReturnsRedirectToRoot` test method of the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) demonstrates the use of `WithWebHostBuilder`.</span></span> <span data-ttu-id="ed417-231">このテストは、SUT でフォームの送信をトリガーすることによって、データベース内のレコードの削除を実行します。</span><span class="sxs-lookup"><span data-stu-id="ed417-231">This test performs a record delete in the database by triggering a form submission in the SUT.</span></span>

<span data-ttu-id="ed417-232">別のテストのため、`IndexPageTests`クラスは、すべてのデータベース内のレコードを削除し、前に実行される可能性が操作を実行、`Post_DeleteMessageHandler_ReturnsRedirectToRoot`メソッド、データベースのレコードが削除 SUT に存在することを確認するには、このテスト メソッドでシード処理します。</span><span class="sxs-lookup"><span data-stu-id="ed417-232">Because another test in the `IndexPageTests` class performs an operation that deletes all of the records in the database and may run before the `Post_DeleteMessageHandler_ReturnsRedirectToRoot` method, the database is seeded in this test method to ensure that a record is present for the SUT to delete.</span></span> <span data-ttu-id="ed417-233">選択すると、`deleteBtn1`のボタン、 `messages` SUT でフォームが SUT への要求でシミュレートされました。</span><span class="sxs-lookup"><span data-stu-id="ed417-233">Selecting the `deleteBtn1` button of the `messages` form in the SUT is simulated in the request to the SUT:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet3)]

## <a name="client-options"></a><span data-ttu-id="ed417-234">クライアント オプション</span><span class="sxs-lookup"><span data-stu-id="ed417-234">Client options</span></span>

<span data-ttu-id="ed417-235">次の表は、既定[WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions)作成するときに使用可能な`HttpClient`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ed417-235">The following table shows the default [WebApplicationFactoryClientOptions](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions) available when creating `HttpClient` instances.</span></span>

| <span data-ttu-id="ed417-236">オプション</span><span class="sxs-lookup"><span data-stu-id="ed417-236">Option</span></span> | <span data-ttu-id="ed417-237">説明</span><span class="sxs-lookup"><span data-stu-id="ed417-237">Description</span></span> | <span data-ttu-id="ed417-238">既定値</span><span class="sxs-lookup"><span data-stu-id="ed417-238">Default</span></span> |
| ------ | ----------- | ------- |
| [<span data-ttu-id="ed417-239">AllowAutoRedirect</span><span class="sxs-lookup"><span data-stu-id="ed417-239">AllowAutoRedirect</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.allowautoredirect) | <span data-ttu-id="ed417-240">取得または設定するかどうか`HttpClient`インスタンスは、リダイレクト応答に自動的に従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed417-240">Gets or sets whether or not `HttpClient` instances should automatically follow redirect responses.</span></span> | `true` |
| [<span data-ttu-id="ed417-241">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="ed417-241">BaseAddress</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.baseaddress) | <span data-ttu-id="ed417-242">取得または設定のベース アドレス`HttpClient`インスタンス。</span><span class="sxs-lookup"><span data-stu-id="ed417-242">Gets or sets the base address of `HttpClient` instances.</span></span> | `http://localhost` |
| [<span data-ttu-id="ed417-243">HandleCookies</span><span class="sxs-lookup"><span data-stu-id="ed417-243">HandleCookies</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.handlecookies) | <span data-ttu-id="ed417-244">取得または設定するかどうか`HttpClient`インスタンスは、cookie を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed417-244">Gets or sets whether `HttpClient` instances should handle cookies.</span></span> | `true` |
| [<span data-ttu-id="ed417-245">MaxAutomaticRedirections</span><span class="sxs-lookup"><span data-stu-id="ed417-245">MaxAutomaticRedirections</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactoryclientoptions.maxautomaticredirections) | <span data-ttu-id="ed417-246">リダイレクト応答の最大数の設定を取得または`HttpClient`インスタンスが従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed417-246">Gets or sets the maximum number of redirect responses that `HttpClient` instances should follow.</span></span> | <span data-ttu-id="ed417-247">7</span><span class="sxs-lookup"><span data-stu-id="ed417-247">7</span></span> |

<span data-ttu-id="ed417-248">作成、`WebApplicationFactoryClientOptions`クラスに渡すと、 [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient)メソッド (既定値はコード例に示した)。</span><span class="sxs-lookup"><span data-stu-id="ed417-248">Create the `WebApplicationFactoryClientOptions` class and pass it to the [CreateClient](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.createclient) method (default values are shown in the code example):</span></span>

```csharp
// Default client option values are shown
var clientOptions = new WebApplicationFactoryClientOptions();
clientOptions.AllowAutoRedirect = true;
clientOptions.BaseAddress = new Uri("http://localhost");
clientOptions.HandleCookies = true;
clientOptions.MaxAutomaticRedirections = 7;

_client = _factory.CreateClient(clientOptions);
```

## <a name="inject-mock-services"></a><span data-ttu-id="ed417-249">モック サービスを挿入します。</span><span class="sxs-lookup"><span data-stu-id="ed417-249">Inject mock services</span></span>

<span data-ttu-id="ed417-250">サービスへの呼び出しでのテストでオーバーライドできます[ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices)ホスト ビルダーにします。</span><span class="sxs-lookup"><span data-stu-id="ed417-250">Services can be overridden in a test with a call to [ConfigureTestServices](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.configuretestservices) on the host builder.</span></span> <span data-ttu-id="ed417-251">**モック サービスを挿入する SUT があります、`Startup`クラス、`Startup.ConfigureServices`メソッド。**</span><span class="sxs-lookup"><span data-stu-id="ed417-251">**To inject mock services, the SUT must have a `Startup` class with a `Startup.ConfigureServices` method.**</span></span>

<span data-ttu-id="ed417-252">SUT サンプルには、見積もりが返すスコープ化されたサービスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed417-252">The sample SUT includes a scoped service that returns a quote.</span></span> <span data-ttu-id="ed417-253">見積もりは、インデックス ページが要求されたときに、インデックス ページ上の非表示フィールドに埋め込まれます。</span><span class="sxs-lookup"><span data-stu-id="ed417-253">The quote is embedded in a hidden field on the Index page when the Index page is requested.</span></span>

<span data-ttu-id="ed417-254">*Services/IQuoteService.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-254">*Services/IQuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/IQuoteService.cs?name=snippet1)]

<span data-ttu-id="ed417-255">*Services/QuoteService.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-255">*Services/QuoteService.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Services/QuoteService.cs?name=snippet1)]

<span data-ttu-id="ed417-256">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-256">*Startup.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Startup.cs?name=snippet2)]

<span data-ttu-id="ed417-257">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-257">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml.cs?name=snippet1&highlight=4,9,20,26)]

<span data-ttu-id="ed417-258">*Pages/Index.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-258">*Pages/Index.cs*:</span></span>

[!code-cshtml[](integration-tests/samples/2.x/IntegrationTestsSample/src/RazorPagesProject/Pages/Index.cshtml?name=snippet_Quote)]

<span data-ttu-id="ed417-259">SUT アプリの実行時に、次のマークアップが生成されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-259">The following markup is generated when the SUT app is run:</span></span>

```html
<input id="quote" type="hidden" value="Come on, Sarah. We&#x27;ve an appointment in 
    London, and we&#x27;re already 30,000 years late.">
```

<span data-ttu-id="ed417-260">統合テストでは、サービスおよび見積もりの挿入をテストするには、モック サービスは、テストによって SUT に組み込まれます。</span><span class="sxs-lookup"><span data-stu-id="ed417-260">To test the service and quote injection in an integration test, a mock service is injected into the SUT by the test.</span></span> <span data-ttu-id="ed417-261">モック サービスが、アプリの置き換えられます`QuoteService`テスト アプリによって提供されるサービスという`TestQuoteService`:</span><span class="sxs-lookup"><span data-stu-id="ed417-261">The mock service replaces the app's `QuoteService` with a service provided by the test app, called `TestQuoteService`:</span></span>

<span data-ttu-id="ed417-262">*IntegrationTests.IndexPageTests.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed417-262">*IntegrationTests.IndexPageTests.cs*:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet4)]

<span data-ttu-id="ed417-263">`ConfigureTestServices` 呼び出されると、スコープ化されたサービスが登録されているとします。</span><span class="sxs-lookup"><span data-stu-id="ed417-263">`ConfigureTestServices` is called, and the scoped service is registered:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/IntegrationTests/IndexPageTests.cs?name=snippet5&highlight=7-10,17,20-21)]

<span data-ttu-id="ed417-264">テストの実行中に生成されたマークアップの反映によって提供される見積もりテキスト`TestQuoteService`、そのため、アサーションのパス。</span><span class="sxs-lookup"><span data-stu-id="ed417-264">The markup produced during the test's execution reflects the quote text supplied by `TestQuoteService`, thus the assertion passes:</span></span>

```html
<input id="quote" type="hidden" value="Something&#x27;s interfering with time, 
    Mr. Scarman, and time is my business.">
```

## <a name="how-the-test-infrastructure-infers-the-app-content-root-path"></a><span data-ttu-id="ed417-265">テスト インフラストラクチャが、アプリのコンテンツ ルート パスを推論する方法</span><span class="sxs-lookup"><span data-stu-id="ed417-265">How the test infrastructure infers the app content root path</span></span>

<span data-ttu-id="ed417-266">`WebApplicationFactory`コンス トラクターを検索して、アプリのコンテンツ ルート パスの推測を[WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute)と等しいキーを使用して、統合テストを含むアセンブリを`TEntryPoint`アセンブリ`System.Reflection.Assembly.FullName`.</span><span class="sxs-lookup"><span data-stu-id="ed417-266">The `WebApplicationFactory` constructor infers the app content root path by searching for a [WebApplicationFactoryContentRootAttribute](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactorycontentrootattribute) on the assembly containing the integration tests with a key equal to the `TEntryPoint` assembly `System.Reflection.Assembly.FullName`.</span></span> <span data-ttu-id="ed417-267">正しいキーを持つ属性が見つからない場合に`WebApplicationFactory`ソリューション ファイルの検索にフォールバック (*\*.sln*) し、追加、`TEntryPoint`ソリューション ディレクトリにアセンブリ名。</span><span class="sxs-lookup"><span data-stu-id="ed417-267">In case an attribute with the correct key isn't found, `WebApplicationFactory` falls back to searching for a solution file (*\*.sln*) and appends the `TEntryPoint` assembly name to the solution directory.</span></span> <span data-ttu-id="ed417-268">アプリのルート ディレクトリ (コンテンツ ルート パス) を使用して、ビューとコンテンツ ファイルを検出します。</span><span class="sxs-lookup"><span data-stu-id="ed417-268">The app root directory (the content root path) is used to discover views and content files.</span></span>

<span data-ttu-id="ed417-269">ほとんどの場合、必要はありません、アプリのコンテンツ ルートを明示的に設定するとしての検索ロジックが実行時に、通常の適切なコンテンツのルートを検索します。</span><span class="sxs-lookup"><span data-stu-id="ed417-269">In most cases, it isn't necessary to explicitly set the app content root, as the search logic usually finds the correct content root at runtime.</span></span> <span data-ttu-id="ed417-270">コンテンツのルートが見つからないの特殊なシナリオでは、アプリのコンテンツ ルートは、明示的にまたはカスタム ロジックを使用して指定できます、組み込みの検索アルゴリズムを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed417-270">In special scenarios where the content root isn't found using the built-in search algorithm, the app content root can be specified explicitly or by using custom logic.</span></span> <span data-ttu-id="ed417-271">これらのシナリオでは、アプリのコンテンツ ルートを設定するには、呼び出し、`UseSolutionRelativeContentRoot`から拡張メソッド、 [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost)パッケージ。</span><span class="sxs-lookup"><span data-stu-id="ed417-271">To set the app content root in those scenarios, call the `UseSolutionRelativeContentRoot` extension method from the [Microsoft.AspNetCore.TestHost](https://www.nuget.org/packages/Microsoft.AspNetCore.TestHost) package.</span></span> <span data-ttu-id="ed417-272">ソリューションの相対パスと省略可能なソリューション ファイルの名前または glob パターンを指定 (既定 = `*.sln`)。</span><span class="sxs-lookup"><span data-stu-id="ed417-272">Supply the solution's relative path and optional solution file name or glob pattern (default = `*.sln`).</span></span>

<span data-ttu-id="ed417-273">呼び出す、 [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot)メソッドを使用して拡張*1 つ*の次の方法。</span><span class="sxs-lookup"><span data-stu-id="ed417-273">Call the [UseSolutionRelativeContentRoot](/dotnet/api/microsoft.aspnetcore.testhost.webhostbuilderextensions.usesolutionrelativecontentroot) extension method using *ONE* of the following approaches:</span></span>

* <span data-ttu-id="ed417-274">テスト クラスを構成するときに`WebApplicationFactory`、カスタム構成での提供、 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):</span><span class="sxs-lookup"><span data-stu-id="ed417-274">When configuring test classes with `WebApplicationFactory`, provide a custom configuration with the [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder):</span></span>

   ```csharp
   public IndexPageTests(
       WebApplicationFactory<RazorPagesProject.Startup> factory)
   {
       var _factory = factory.WithWebHostBuilder(builder =>
       {
           builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

           ...
       });
   }
   ```

* <span data-ttu-id="ed417-275">カスタムのテスト クラスを構成するときに`WebApplicationFactory`、継承`WebApplicationFactory`オーバーライドと[ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):</span><span class="sxs-lookup"><span data-stu-id="ed417-275">When configuring test classes with a custom `WebApplicationFactory`, inherit from `WebApplicationFactory` and override [ConfigureWebHost](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1.configurewebhost):</span></span>

   ```csharp
   public class CustomWebApplicationFactory<TStartup>
       : WebApplicationFactory<RazorPagesProject.Startup>
   {
       protected override void ConfigureWebHost(IWebHostBuilder builder)
       {
           builder.ConfigureServices(services =>
           {
               builder.UseSolutionRelativeContentRoot("<SOLUTION-RELATIVE-PATH>");

               ...
           });
       }
   }
   ```

## <a name="disable-shadow-copying"></a><span data-ttu-id="ed417-276">シャドウ コピーを無効にします。</span><span class="sxs-lookup"><span data-stu-id="ed417-276">Disable shadow copying</span></span>

<span data-ttu-id="ed417-277">シャドウ コピーすると、出力フォルダーとは別のフォルダーで実行するテストが発生します。</span><span class="sxs-lookup"><span data-stu-id="ed417-277">Shadow copying causes the tests to execute in a different folder than the output folder.</span></span> <span data-ttu-id="ed417-278">正常に動作するテストでは、シャドウ コピーする必要があります無効になります。</span><span class="sxs-lookup"><span data-stu-id="ed417-278">For tests to work properly, shadow copying must be disabled.</span></span> <span data-ttu-id="ed417-279">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)xUnit を使用し、シャドウ コピーを含めることで xunit を無効にする*xunit.runner.json*適切な構成設定ファイル。</span><span class="sxs-lookup"><span data-stu-id="ed417-279">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) uses xUnit and disables shadow copying for xUnit by including an *xunit.runner.json* file with the correct configuration setting.</span></span> <span data-ttu-id="ed417-280">詳細については、次を参照してください。 [xUnit.net を構成する JSON で](https://xunit.github.io/docs/configuring-with-json.html)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-280">For more information, see [Configuring xUnit.net with JSON](https://xunit.github.io/docs/configuring-with-json.html).</span></span>

<span data-ttu-id="ed417-281">追加、 *xunit.runner.json*以下の内容のテスト プロジェクトのルートにファイル。</span><span class="sxs-lookup"><span data-stu-id="ed417-281">Add the *xunit.runner.json* file to root of the test project with the following content:</span></span>

```json
{
  "shadowCopy": false
}
```

## <a name="integration-tests-sample"></a><span data-ttu-id="ed417-282">統合テストのサンプル</span><span class="sxs-lookup"><span data-stu-id="ed417-282">Integration tests sample</span></span>

<span data-ttu-id="ed417-283">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples)は 2 つのアプリで構成されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-283">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/integration-tests/samples) is composed of two apps:</span></span>

| <span data-ttu-id="ed417-284">アプリ</span><span class="sxs-lookup"><span data-stu-id="ed417-284">App</span></span> | <span data-ttu-id="ed417-285">プロジェクト フォルダー</span><span class="sxs-lookup"><span data-stu-id="ed417-285">Project folder</span></span> | <span data-ttu-id="ed417-286">説明</span><span class="sxs-lookup"><span data-stu-id="ed417-286">Description</span></span> |
| --- | -------------- | ----------- |
| <span data-ttu-id="ed417-287">メッセージ アプリ (SUT)</span><span class="sxs-lookup"><span data-stu-id="ed417-287">Message app (the SUT)</span></span> | <span data-ttu-id="ed417-288">*src/RazorPagesProject*</span><span class="sxs-lookup"><span data-stu-id="ed417-288">*src/RazorPagesProject*</span></span> | <span data-ttu-id="ed417-289">により、ユーザーの追加、削除のいずれか、すべてを削除、およびメッセージを分析できます。</span><span class="sxs-lookup"><span data-stu-id="ed417-289">Allows a user to add, delete one, delete all, and analyze messages.</span></span> |
| <span data-ttu-id="ed417-290">テスト アプリ</span><span class="sxs-lookup"><span data-stu-id="ed417-290">Test app</span></span> | <span data-ttu-id="ed417-291">*tests/RazorPagesProject.Tests*</span><span class="sxs-lookup"><span data-stu-id="ed417-291">*tests/RazorPagesProject.Tests*</span></span> | <span data-ttu-id="ed417-292">統合テスト SUT するために使用します。</span><span class="sxs-lookup"><span data-stu-id="ed417-292">Used to integration test the SUT.</span></span> |

<span data-ttu-id="ed417-293">などの IDE、組み込みのテスト機能を使用して、テストを実行できる[Visual Studio](https://www.visualstudio.com/vs/)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-293">The tests can be run using the built-in test features of an IDE, such as [Visual Studio](https://www.visualstudio.com/vs/).</span></span> <span data-ttu-id="ed417-294">使用して場合[Visual Studio Code](https://code.visualstudio.com/)またはコマンド プロンプトで次のコマンドを実行するコマンドライン、 *tests/RazorPagesProject.Tests*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="ed417-294">If using [Visual Studio Code](https://code.visualstudio.com/) or the command line, execute the following command at a command prompt in the *tests/RazorPagesProject.Tests* folder:</span></span>

```console
dotnet test
```

### <a name="message-app-sut-organization"></a><span data-ttu-id="ed417-295">メッセージ アプリ (SUT) 組織</span><span class="sxs-lookup"><span data-stu-id="ed417-295">Message app (SUT) organization</span></span>

<span data-ttu-id="ed417-296">SUT は、次の特性を持つ、Razor ページ メッセージ システムを示します。</span><span class="sxs-lookup"><span data-stu-id="ed417-296">The SUT is a Razor Pages message system with the following characteristics:</span></span>

* <span data-ttu-id="ed417-297">アプリのインデックス ページ (*Pages/Index.cshtml*と*Pages/Index.cshtml.cs*) UI とページ モデルのメソッドを追加、削除、およびメッセージ (メッセージあたりの平均の単語) の分析の制御を提供します.</span><span class="sxs-lookup"><span data-stu-id="ed417-297">The Index page of the app (*Pages/Index.cshtml* and *Pages/Index.cshtml.cs*) provides a UI and page model methods to control the addition, deletion, and analysis of messages (average words per message).</span></span>
* <span data-ttu-id="ed417-298">メッセージは、`Message`クラス (*Data/Message.cs*) 2 つのプロパティを持つ: `Id` (キー) と`Text`(メッセージ)。</span><span class="sxs-lookup"><span data-stu-id="ed417-298">A message is described by the `Message` class (*Data/Message.cs*) with two properties: `Id` (key) and `Text` (message).</span></span> <span data-ttu-id="ed417-299">`Text`プロパティは必須であり 200 文字に制限されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-299">The `Text` property is required and limited to 200 characters.</span></span>
* <span data-ttu-id="ed417-300">使用してメッセージを保存する[Entity Framework のメモリ内データベース](/ef/core/providers/in-memory/)&#8224;します。</span><span class="sxs-lookup"><span data-stu-id="ed417-300">Messages are stored using [Entity Framework's in-memory database](/ef/core/providers/in-memory/)&#8224;.</span></span>
* <span data-ttu-id="ed417-301">アプリのデータベース コンテキスト クラス内でデータ アクセス層 (DAL) に含まれる`AppDbContext`(*Data/AppDbContext.cs*)。</span><span class="sxs-lookup"><span data-stu-id="ed417-301">The app contains a data access layer (DAL) in its database context class, `AppDbContext` (*Data/AppDbContext.cs*).</span></span>
* <span data-ttu-id="ed417-302">データベースがアプリの起動時に空の場合、メッセージ ストアは、3 つのメッセージで初期化されます。</span><span class="sxs-lookup"><span data-stu-id="ed417-302">If the database is empty on app startup, the message store is initialized with three messages.</span></span>
* <span data-ttu-id="ed417-303">アプリを`/SecurePage`を認証されたユーザーのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="ed417-303">The app includes a `/SecurePage` that can only be accessed by an authenticated user.</span></span>

<span data-ttu-id="ed417-304">&#8224;EF トピック[InMemory のテスト](/ef/core/miscellaneous/testing/in-memory)MSTest を使用したテストにメモリ内データベースを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ed417-304">&#8224;The EF topic, [Test with InMemory](/ef/core/miscellaneous/testing/in-memory), explains how to use an in-memory database for tests with MSTest.</span></span> <span data-ttu-id="ed417-305">このトピックでは、 [xUnit](https://xunit.github.io/)テスト フレームワーク。</span><span class="sxs-lookup"><span data-stu-id="ed417-305">This topic uses the [xUnit](https://xunit.github.io/) test framework.</span></span> <span data-ttu-id="ed417-306">テストの概念と別のテスト フレームワーク間でのテストの実装は似ていますが、同一ではないです。</span><span class="sxs-lookup"><span data-stu-id="ed417-306">Test concepts and test implementations across different test frameworks are similar but not identical.</span></span>

<span data-ttu-id="ed417-307">アプリは、リポジトリ パターンを使用しないしの有効な例はありませんが、 [Unit of Work (UoW) パターン](https://martinfowler.com/eaaCatalog/unitOfWork.html)、Razor ページは、これらのパターンの開発をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="ed417-307">Although the app doesn't use the repository pattern and isn't an effective example of the [Unit of Work (UoW) pattern](https://martinfowler.com/eaaCatalog/unitOfWork.html), Razor Pages supports these patterns of development.</span></span> <span data-ttu-id="ed417-308">詳細については、次を参照してください。[インフラストラクチャの永続レイヤーの設計](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)と[テスト コント ローラー ロジック](/aspnet/core/mvc/controllers/testing)(このサンプルは、リポジトリ パターンを実装)。</span><span class="sxs-lookup"><span data-stu-id="ed417-308">For more information, see [Designing the infrastructure persistence layer](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design) and [Test controller logic](/aspnet/core/mvc/controllers/testing) (the sample implements the repository pattern).</span></span>

### <a name="test-app-organization"></a><span data-ttu-id="ed417-309">組織のアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-309">Test app organization</span></span>

<span data-ttu-id="ed417-310">テスト アプリが内部でコンソール アプリ、 *tests/RazorPagesProject.Tests*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="ed417-310">The test app is a console app inside the *tests/RazorPagesProject.Tests* folder.</span></span>

| <span data-ttu-id="ed417-311">アプリ フォルダーのテスト</span><span class="sxs-lookup"><span data-stu-id="ed417-311">Test app folder</span></span> | <span data-ttu-id="ed417-312">説明</span><span class="sxs-lookup"><span data-stu-id="ed417-312">Description</span></span> |
| --------------- | ----------- |
| <span data-ttu-id="ed417-313">*BasicTests*</span><span class="sxs-lookup"><span data-stu-id="ed417-313">*BasicTests*</span></span> | <span data-ttu-id="ed417-314">*BasicTests.cs*ルーティング、未認証のユーザーによってセキュリティで保護されたページにアクセスして、GitHub のユーザー プロファイルの取得およびプロファイルのユーザーのログインを確認用のテスト メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ed417-314">*BasicTests.cs* contains test methods for routing, accessing a secure page by an unauthenticated user, and obtaining a GitHub user profile and checking the profile's user login.</span></span> |
| <span data-ttu-id="ed417-315">*IntegrationTests*</span><span class="sxs-lookup"><span data-stu-id="ed417-315">*IntegrationTests*</span></span> | <span data-ttu-id="ed417-316">*IndexPageTests.cs*カスタムを使用して、インデックス ページに対する統合テストを含む`WebApplicationFactory`クラス。</span><span class="sxs-lookup"><span data-stu-id="ed417-316">*IndexPageTests.cs* contains the integration tests for the Index page using custom `WebApplicationFactory` class.</span></span> |
| <span data-ttu-id="ed417-317">*ヘルパー/ユーティリティ*</span><span class="sxs-lookup"><span data-stu-id="ed417-317">*Helpers/Utilities*</span></span> | <ul><li><span data-ttu-id="ed417-318">*Utilities.cs*が含まれています、`InitializeDbForTests`メソッドがテスト データでデータベースをシードするために使用します。</span><span class="sxs-lookup"><span data-stu-id="ed417-318">*Utilities.cs* contains the `InitializeDbForTests` method used to seed the database with test data.</span></span></li><li><span data-ttu-id="ed417-319">*HtmlHelpers.cs* 、AngleSharp を返すメソッドを提供します。`IHtmlDocument`テスト メソッドで使用します。</span><span class="sxs-lookup"><span data-stu-id="ed417-319">*HtmlHelpers.cs* provides a method to return an AngleSharp `IHtmlDocument` for use by the test methods.</span></span></li><li><span data-ttu-id="ed417-320">*HttpClientExtensions.cs*のオーバー ロードを用意`SendAsync`SUT に要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="ed417-320">*HttpClientExtensions.cs* provide overloads for `SendAsync` to submit requests to the SUT.</span></span></li></ul> |

<span data-ttu-id="ed417-321">テスト フレームワークは[xUnit](https://xunit.github.io/)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-321">The test framework is [xUnit](https://xunit.github.io/).</span></span> <span data-ttu-id="ed417-322">使用して統合テストが行われ、 [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost)が含まれています、 [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver)します。</span><span class="sxs-lookup"><span data-stu-id="ed417-322">Integration tests are conducted using the [Microsoft.AspNetCore.TestHost](/dotnet/api/microsoft.aspnetcore.testhost), which includes the [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span> <span data-ttu-id="ed417-323">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing)テスト ホストとテスト サーバーを構成するパッケージが使用される、`TestHost`と`TestServer`パッケージは、テスト アプリのプロジェクト ファイルの直接のパッケージ参照を必要としないまたはテスト アプリ開発者用の構成。</span><span class="sxs-lookup"><span data-stu-id="ed417-323">Because the [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing) package is used to configure the test host and test server, the `TestHost` and `TestServer` packages don't require direct package references in the test app's project file or developer configuration in the test app.</span></span>

<span data-ttu-id="ed417-324">**テスト用データベースをシード処理**</span><span class="sxs-lookup"><span data-stu-id="ed417-324">**Seeding the database for testing**</span></span>

<span data-ttu-id="ed417-325">統合テストは、通常、テストの実行前に、データベース内の小規模なデータセットを要求します。</span><span class="sxs-lookup"><span data-stu-id="ed417-325">Integration tests usually require a small dataset in the database prior to the test execution.</span></span> <span data-ttu-id="ed417-326">など、データベースは削除要求を成功させるには少なくとも 1 つのレコードがある必要であるために、削除はデータベース レコードの削除の呼び出しをテストします。</span><span class="sxs-lookup"><span data-stu-id="ed417-326">For example, a delete test calls for a database record deletion, so the database must have at least one record for the delete request to succeed.</span></span>

<span data-ttu-id="ed417-327">サンプル アプリで次の 3 つのメッセージを使用したデータベースをシード処理*Utilities.cs*テストで実行するときに使用できること。</span><span class="sxs-lookup"><span data-stu-id="ed417-327">The sample app seeds the database with three messages in *Utilities.cs* that tests can use when they execute:</span></span>

[!code-csharp[](integration-tests/samples/2.x/IntegrationTestsSample/tests/RazorPagesProject.Tests/Helpers/Utilities.cs?name=snippet1)]

## <a name="additional-resources"></a><span data-ttu-id="ed417-328">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="ed417-328">Additional resources</span></span>

* [<span data-ttu-id="ed417-329">単体テスト</span><span class="sxs-lookup"><span data-stu-id="ed417-329">Unit tests</span></span>](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="ed417-330">Razor ページの単体テスト</span><span class="sxs-lookup"><span data-stu-id="ed417-330">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
* [<span data-ttu-id="ed417-331">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="ed417-331">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ed417-332">テスト コントローラー</span><span class="sxs-lookup"><span data-stu-id="ed417-332">Test controllers</span></span>](xref:mvc/controllers/testing)
