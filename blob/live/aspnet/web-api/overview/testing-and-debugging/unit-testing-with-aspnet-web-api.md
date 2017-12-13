---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "単体テストの ASP.NET Web API 2 |Microsoft ドキュメント"
author: tfitzmac
description: "このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テスト プロジェクトを含める方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 13211ee4543e17a4bfb2f83495f4041880f37df2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="1952a-104">単体テストの ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="1952a-104">Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1952a-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1952a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="1952a-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1952a-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="1952a-107">このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="1952a-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="1952a-108">このチュートリアルでは、単体テスト プロジェクトをソリューションに含めるし、コント ローラー メソッドから返された値を確認するテスト メソッドを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1952a-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
> 
> <span data-ttu-id="1952a-109">このチュートリアルでは、ASP.NET Web API の基本概念を使い慣れて前提としています。</span><span class="sxs-lookup"><span data-stu-id="1952a-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="1952a-110">入門チュートリアルでは、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="1952a-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> <span data-ttu-id="1952a-111">このトピックの単体テストは、単純なデータ シナリオに意図的に制限されます。</span><span class="sxs-lookup"><span data-stu-id="1952a-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="1952a-112">単体テスト データの高度なシナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)です。</span><span class="sxs-lookup"><span data-stu-id="1952a-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1952a-113">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="1952a-113">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="1952a-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1952a-114">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="1952a-115">Web API 2</span><span class="sxs-lookup"><span data-stu-id="1952a-115">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="1952a-116">このトピックの内容</span><span class="sxs-lookup"><span data-stu-id="1952a-116">In this topic</span></span>

<span data-ttu-id="1952a-117">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="1952a-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="1952a-118">前提条件</span><span class="sxs-lookup"><span data-stu-id="1952a-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="1952a-119">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1952a-119">Download code</span></span>](#download)
- [<span data-ttu-id="1952a-120">単体テスト プロジェクトとアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="1952a-120">Create application with unit test project</span></span>](#appwithunittest)

    - [<span data-ttu-id="1952a-121">アプリケーションを作成するときに、単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="1952a-122">既存のアプリケーションの単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="1952a-123">Web API 2 アプリケーションを設定します。</span><span class="sxs-lookup"><span data-stu-id="1952a-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="1952a-124">テスト プロジェクトの NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1952a-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="1952a-125">テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="1952a-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="1952a-126">テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="1952a-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="1952a-127">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1952a-127">Prerequisites</span></span>

<span data-ttu-id="1952a-128">Visual Studio 2017 Community、Professional edition または Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="1952a-128">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="1952a-129">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="1952a-129">Download code</span></span>

<span data-ttu-id="1952a-130">ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)です。</span><span class="sxs-lookup"><span data-stu-id="1952a-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="1952a-131">ダウンロード可能なプロジェクトには、このトピックと単体テスト コードが含まれています、 [Entity Framework のモック作成時に単体テストの ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)トピックです。</span><span class="sxs-lookup"><span data-stu-id="1952a-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="1952a-132">単体テスト プロジェクトとアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="1952a-132">Create application with unit test project</span></span>

<span data-ttu-id="1952a-133">アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、既存のアプリケーションに単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="1952a-134">このチュートリアルでは、単体テスト プロジェクトを作成するための両方のメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1952a-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="1952a-135">このチュートリアルを実行するには、いずれかの方法を使用できます。</span><span class="sxs-lookup"><span data-stu-id="1952a-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="1952a-136">アプリケーションを作成するときに、単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="1952a-137">という名前の新しい ASP.NET Web アプリケーションを作成する**StoreApp**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="1952a-139">Windows では、新しい ASP.NET プロジェクト、選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアです。</span><span class="sxs-lookup"><span data-stu-id="1952a-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="1952a-140">選択、**単体テストを追加**オプション。</span><span class="sxs-lookup"><span data-stu-id="1952a-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="1952a-141">単体テスト プロジェクトの名前が自動的に**StoreApp.Tests**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="1952a-142">この名前を保持することができます。</span><span class="sxs-lookup"><span data-stu-id="1952a-142">You can keep this name.</span></span>

![単体テスト プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="1952a-144">アプリケーションを作成した後は、2 つのプロジェクトが含まれている参照してください。</span><span class="sxs-lookup"><span data-stu-id="1952a-144">After creating the application, you will see it contains two projects.</span></span>

![2 つのプロジェクト](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="1952a-146">既存のアプリケーションの単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="1952a-147">アプリケーションを作成したときに、単体テスト プロジェクトを作成しなかった場合は、いつでも追加できます。</span><span class="sxs-lookup"><span data-stu-id="1952a-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="1952a-148">たとえば、StoreApp、という名前のアプリケーションが既にあるし、単体テストを追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="1952a-149">単体テスト プロジェクトを追加するには、ソリューションを右クリックして**追加**と**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![新しいプロジェクトをソリューションに追加します。](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="1952a-151">選択**テスト**クリックし、左側のウィンドウで**単体テスト プロジェクト**プロジェクトの種類。</span><span class="sxs-lookup"><span data-stu-id="1952a-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="1952a-152">プロジェクトに名前を**StoreApp.Tests**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-152">Name the project **StoreApp.Tests**.</span></span>

![単体テスト プロジェクトを追加します。](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="1952a-154">ソリューションの単体テスト プロジェクトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="1952a-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="1952a-155">単体テスト プロジェクトでは、元のプロジェクトへのプロジェクト参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="1952a-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="1952a-156">Web API 2 アプリケーションを設定します。</span><span class="sxs-lookup"><span data-stu-id="1952a-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="1952a-157">StoreApp プロジェクトにクラス ファイルを追加、**モデル**という名前のフォルダー **Product.cs**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="1952a-158">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1952a-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="1952a-159">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="1952a-159">Build the solution.</span></span>

<span data-ttu-id="1952a-160">Controllers フォルダーを右クリックし **追加**と**スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="1952a-161">選択**Web API 2 コント ローラー - 空**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-161">Select **Web API 2 Controller - Empty**.</span></span>

![新しいコント ローラーを追加します。](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="1952a-163">コント ローラー名を設定します**SimpleProductController**、 をクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![コント ローラーを指定します。](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="1952a-165">既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1952a-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="1952a-166">この例を簡略化、データは、データベースではなく、一覧に格納されます。</span><span class="sxs-lookup"><span data-stu-id="1952a-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="1952a-167">このクラスで定義されている一覧では、実稼働データを表します。</span><span class="sxs-lookup"><span data-stu-id="1952a-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="1952a-168">コント ローラーに製品オブジェクトのリストをパラメーターとして受け取るコンス トラクターが含まれることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="1952a-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="1952a-169">このコンス トラクターを使用すると、テスト データを渡すときに単体テストします。</span><span class="sxs-lookup"><span data-stu-id="1952a-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="1952a-170">コント ローラーも含まれています 2 **async**メソッドを単体テストの非同期メソッドを示しています。</span><span class="sxs-lookup"><span data-stu-id="1952a-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="1952a-171">これらの非同期メソッドを呼び出すことによって実装された**Task.FromResult** 、余分なコードが、通常のメソッドを最小限に抑えるにリソースを消費する操作が含まれます。</span><span class="sxs-lookup"><span data-stu-id="1952a-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="1952a-172">GetProduct メソッドのインスタンスを返します、 **IHttpActionResult**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="1952a-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="1952a-173">IHttpActionResult Web API 2 の新機能の 1 つは、単体テストの開発が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="1952a-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="1952a-174">IHttpActionResult インターフェイスを実装するクラスは、 [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)名前空間。</span><span class="sxs-lookup"><span data-stu-id="1952a-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="1952a-175">これらのクラスは、操作要求から応答できるを表し、HTTP ステータス コードに対応します。</span><span class="sxs-lookup"><span data-stu-id="1952a-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="1952a-176">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="1952a-176">Build the solution.</span></span>

<span data-ttu-id="1952a-177">テスト プロジェクトを設定する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="1952a-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="1952a-178">テスト プロジェクトの NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1952a-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="1952a-179">空のテンプレートを使用してアプリケーションを作成するときに、単体テスト プロジェクト (StoreApp.Tests) は、インストールされている NuGet パッケージが含まれません。</span><span class="sxs-lookup"><span data-stu-id="1952a-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="1952a-180">Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで一部の NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1952a-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="1952a-181">このチュートリアルでは、テスト プロジェクトに、Microsoft ASP.NET Web API 2 Core パッケージを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="1952a-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="1952a-182">StoreApp.Tests プロジェクトを右クリックし  **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="1952a-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="1952a-183">そのプロジェクトにパッケージを追加するプロジェクトを StoreApp.Tests を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1952a-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![パッケージを管理します。](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="1952a-185">検索し、Microsoft ASP.NET Web API 2 Core パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1952a-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![web api core パッケージをインストールします。](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="1952a-187">NuGet パッケージの管理ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="1952a-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="1952a-188">テストの作成</span><span class="sxs-lookup"><span data-stu-id="1952a-188">Create tests</span></span>

<span data-ttu-id="1952a-189">既定では、テスト プロジェクトには、UnitTest1.cs をという名前の空のテスト ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1952a-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="1952a-190">このファイルは、テスト メソッドの作成に使用する属性を示します。</span><span class="sxs-lookup"><span data-stu-id="1952a-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="1952a-191">単体テストの場合は、このファイルを使用するか、独自のファイルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="1952a-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="1952a-193">このチュートリアルでは、独自のテスト クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="1952a-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="1952a-194">UnitTest1.cs ファイルを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="1952a-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="1952a-195">という名前のクラスを追加**TestSimpleProductController.cs**コードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="1952a-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="1952a-196">テストの実行</span><span class="sxs-lookup"><span data-stu-id="1952a-196">Run tests</span></span>

<span data-ttu-id="1952a-197">テストを実行する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="1952a-197">You are now ready to run the tests.</span></span> <span data-ttu-id="1952a-198">マークされたメソッドのすべての**TestMethod**属性がテストされます。</span><span class="sxs-lookup"><span data-stu-id="1952a-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="1952a-199">**テスト**メニュー項目、テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="1952a-199">From the **Test** menu item, run the tests.</span></span>

![テストの実行](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="1952a-201">開く、**テスト エクスプ ローラー**ウィンドウ、およびテストの結果に注意してください。</span><span class="sxs-lookup"><span data-stu-id="1952a-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![テスト結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="1952a-203">概要</span><span class="sxs-lookup"><span data-stu-id="1952a-203">Summary</span></span>

<span data-ttu-id="1952a-204">このチュートリアルを完了しました。</span><span class="sxs-lookup"><span data-stu-id="1952a-204">You have completed this tutorial.</span></span> <span data-ttu-id="1952a-205">このチュートリアルでは、データは、単体テストの条件に焦点を絞る意図的に簡素化されます。</span><span class="sxs-lookup"><span data-stu-id="1952a-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="1952a-206">単体テスト データの高度なシナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)です。</span><span class="sxs-lookup"><span data-stu-id="1952a-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
