---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: 単体テストの ASP.NET Web API 2 |Microsoft Docs
author: Rick-Anderson
description: このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テストのプロジェクトを含める方法を使用しています.
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 915610e6646ebe86dd8f16f290ecabd36bf7f48d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020833"
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="10903-104">単体テストの ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="10903-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="10903-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="10903-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="10903-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="10903-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="10903-107">このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="10903-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="10903-108">このチュートリアルでは、単体テスト プロジェクトをソリューションに含めるし、コント ローラー メソッドから返された値をチェックするテスト メソッドを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10903-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="10903-109">このチュートリアルでは、ASP.NET Web API の基本的な概念に慣れてを前提としています。</span><span class="sxs-lookup"><span data-stu-id="10903-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="10903-110">入門チュートリアルについては、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="10903-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="10903-111">このトピックの「単体テストは、単純なデータ シナリオに意図的に制限されます。</span><span class="sxs-lookup"><span data-stu-id="10903-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="10903-112">単体テストより高度なデータ シナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="10903-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="10903-113">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="10903-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="10903-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="10903-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="10903-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="10903-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="10903-116">このトピックの内容</span><span class="sxs-lookup"><span data-stu-id="10903-116">In this topic</span></span>

<span data-ttu-id="10903-117">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="10903-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="10903-118">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="10903-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="10903-119">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="10903-119">Download code</span></span>](#download)
- [<span data-ttu-id="10903-120">単体テスト プロジェクトでアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="10903-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="10903-121">アプリケーションを作成するときに、単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="10903-122">単体テスト プロジェクトを既存のアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="10903-123">Web API 2 アプリケーションを設定します。</span><span class="sxs-lookup"><span data-stu-id="10903-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="10903-124">テスト プロジェクトで NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="10903-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="10903-125">テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="10903-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="10903-126">テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="10903-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="10903-127">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="10903-127">Prerequisites</span></span>

<span data-ttu-id="10903-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、Professional または Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="10903-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="10903-129">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="10903-129">Download code</span></span>

<span data-ttu-id="10903-130">ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)します。</span><span class="sxs-lookup"><span data-stu-id="10903-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="10903-131">ダウンロード可能なプロジェクトには、このトピックでは、単体テストのコードが含まれています、 [Entity Framework のモック作成時に単体テストの ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)トピック。</span><span class="sxs-lookup"><span data-stu-id="10903-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="10903-132">単体テスト プロジェクトでアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="10903-132">Create application with unit test project</span></span>

<span data-ttu-id="10903-133">アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、単体テスト プロジェクトを既存のアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="10903-134">このチュートリアルでは、2 つの単体テスト プロジェクトの作成方法を示します。</span><span class="sxs-lookup"><span data-stu-id="10903-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="10903-135">このチュートリアルを実行するには、どちらの方法を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="10903-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="10903-136">アプリケーションを作成するときに、単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="10903-137">という名前の新しい ASP.NET Web アプリケーション作成**StoreApp**します。</span><span class="sxs-lookup"><span data-stu-id="10903-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="10903-139">新しい ASP.NET プロジェクト ウィンドウで選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアしします。</span><span class="sxs-lookup"><span data-stu-id="10903-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="10903-140">選択、**単体テストを追加**オプション。</span><span class="sxs-lookup"><span data-stu-id="10903-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="10903-141">単体テスト プロジェクトが自動的に名前付き**StoreApp.Tests**します。</span><span class="sxs-lookup"><span data-stu-id="10903-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="10903-142">この名前を保持することができます。</span><span class="sxs-lookup"><span data-stu-id="10903-142">You can keep this name.</span></span>

![単体テスト プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="10903-144">アプリケーションを作成した後は、2 つのプロジェクトが含まれている参照してください。</span><span class="sxs-lookup"><span data-stu-id="10903-144">After creating the application, you will see it contains two projects.</span></span>

![2 つのプロジェクト](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="10903-146">単体テスト プロジェクトを既存のアプリケーションに追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="10903-147">アプリケーションを作成したときに単体テスト プロジェクトを作成しなかった場合は、いつでも追加できます。</span><span class="sxs-lookup"><span data-stu-id="10903-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="10903-148">たとえば、StoreApp、という名前のアプリケーションが既にあるし、単体テストを追加したいとします。</span><span class="sxs-lookup"><span data-stu-id="10903-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="10903-149">単体テスト プロジェクトを追加するには、ソリューションを右クリックして**追加**と**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="10903-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![新しいプロジェクトをソリューションに追加します。](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="10903-151">選択**テスト**クリックし、左側のウィンドウで**単体テスト プロジェクト**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="10903-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="10903-152">プロジェクトに名前を**StoreApp.Tests**します。</span><span class="sxs-lookup"><span data-stu-id="10903-152">Name the project **StoreApp.Tests**.</span></span>

![単体テスト プロジェクトを追加します。](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="10903-154">ソリューションの単体テスト プロジェクトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="10903-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="10903-155">単体テスト プロジェクトでは、元のプロジェクトへのプロジェクト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="10903-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="10903-156">Web API 2 アプリケーションを設定します。</span><span class="sxs-lookup"><span data-stu-id="10903-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="10903-157">StoreApp プロジェクトで、追加のクラス ファイルを**モデル**という名前のフォルダー **Product.cs**します。</span><span class="sxs-lookup"><span data-stu-id="10903-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="10903-158">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10903-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="10903-159">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="10903-159">Build the solution.</span></span>

<span data-ttu-id="10903-160">Controllers フォルダーを右クリックして**追加**と**スキャフォールディングされた新しい項目**します。</span><span class="sxs-lookup"><span data-stu-id="10903-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="10903-161">選択**Web API 2 コント ローラー - 空**します。</span><span class="sxs-lookup"><span data-stu-id="10903-161">Select **Web API 2 Controller - Empty**.</span></span>

![新しいコント ローラーを追加します。](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="10903-163">コント ローラー名を設定**SimpleProductController**、 をクリック**追加**します。</span><span class="sxs-lookup"><span data-stu-id="10903-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![コント ローラーを指定します。](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="10903-165">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10903-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="10903-166">この例を簡略化するのには、データがデータベースではなく、リストに格納されます。</span><span class="sxs-lookup"><span data-stu-id="10903-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="10903-167">このクラスで定義されている一覧は、実稼働データを表します。</span><span class="sxs-lookup"><span data-stu-id="10903-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="10903-168">コント ローラーが Product オブジェクトの一覧をパラメーターとして受け取るコンス トラクターが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="10903-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="10903-169">このコンス トラクターを使用すると、テスト データを渡すときに単体テストします。</span><span class="sxs-lookup"><span data-stu-id="10903-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="10903-170">2 つは、コント ローラーも含む**async**メソッドの単体テストの非同期メソッドを説明するためにします。</span><span class="sxs-lookup"><span data-stu-id="10903-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="10903-171">これらの非同期メソッドを呼び出すことによって実装された**Task.FromResult** 、余分なコードが、通常、メソッドを最小限に抑えるにリソースを消費する操作が含まれます。</span><span class="sxs-lookup"><span data-stu-id="10903-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="10903-172">GetProduct メソッドのインスタンスを返します、 **IHttpActionResult**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="10903-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="10903-173">IHttpActionResult は、Web API 2 では、新機能の 1 つと、単体テストの開発が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="10903-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="10903-174">IHttpActionResult インターフェイスを実装するクラスは、 [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="10903-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="10903-175">これらのクラスは、操作要求からのような応答を表し、HTTP 状態コードに対応します。</span><span class="sxs-lookup"><span data-stu-id="10903-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="10903-176">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="10903-176">Build the solution.</span></span>

<span data-ttu-id="10903-177">テスト プロジェクトを設定する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="10903-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="10903-178">テスト プロジェクトで NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="10903-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="10903-179">空のテンプレートを使用してアプリケーションを作成するときに単体テスト プロジェクト (StoreApp.Tests) にインストールされている NuGet パッケージは含まれません。</span><span class="sxs-lookup"><span data-stu-id="10903-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="10903-180">Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで NuGet パッケージ一部にはが含まれます。</span><span class="sxs-lookup"><span data-stu-id="10903-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="10903-181">このチュートリアルでは、Microsoft ASP.NET Web API 2 Core パッケージをテスト プロジェクトを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="10903-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="10903-182">StoreApp.Tests プロジェクトを右クリックして**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="10903-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="10903-183">そのプロジェクトにパッケージを追加する StoreApp.Tests プロジェクトを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="10903-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![パッケージを管理します。](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="10903-185">検索して、Microsoft ASP.NET Web API 2 コア パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="10903-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![web api のコア パッケージをインストールします。](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="10903-187">NuGet パッケージの管理ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="10903-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="10903-188">テストの作成</span><span class="sxs-lookup"><span data-stu-id="10903-188">Create tests</span></span>

<span data-ttu-id="10903-189">既定では、テスト プロジェクトには、UnitTest1.cs をという名前を空のテスト ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="10903-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="10903-190">このファイルは、テスト メソッドの作成に使用する属性を示します。</span><span class="sxs-lookup"><span data-stu-id="10903-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="10903-191">単体テスト用には、このファイルを使用するか、独自のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="10903-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="10903-193">このチュートリアルでは、独自のテスト クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="10903-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="10903-194">UnitTest1.cs ファイルを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="10903-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="10903-195">という名前のクラスを追加**TestSimpleProductController.cs**コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="10903-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="10903-196">テストの実行</span><span class="sxs-lookup"><span data-stu-id="10903-196">Run tests</span></span>

<span data-ttu-id="10903-197">テストを実行する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="10903-197">You are now ready to run the tests.</span></span> <span data-ttu-id="10903-198">マークされているメソッドはすべて、 **TestMethod**属性がテストされます。</span><span class="sxs-lookup"><span data-stu-id="10903-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="10903-199">**テスト**メニュー項目、テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="10903-199">From the **Test** menu item, run the tests.</span></span>

![テストの実行](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="10903-201">開く、**テスト エクスプ ローラー**ウィンドウで、テストの結果を注意してください。</span><span class="sxs-lookup"><span data-stu-id="10903-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![テスト結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="10903-203">まとめ</span><span class="sxs-lookup"><span data-stu-id="10903-203">Summary</span></span>

<span data-ttu-id="10903-204">このチュートリアルを完了しました。</span><span class="sxs-lookup"><span data-stu-id="10903-204">You have completed this tutorial.</span></span> <span data-ttu-id="10903-205">このチュートリアルでは、データは、単体テストの条件に重点を置く意図的に簡略化されました。</span><span class="sxs-lookup"><span data-stu-id="10903-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="10903-206">単体テストより高度なデータ シナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)します。</span><span class="sxs-lookup"><span data-stu-id="10903-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
