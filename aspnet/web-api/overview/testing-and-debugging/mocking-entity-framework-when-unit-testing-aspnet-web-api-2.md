---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework をモック作成時に単体テストの ASP.NET Web API 2 |Microsoft ドキュメント
author: tfitzmac
description: このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。 変更する方法を示しますが、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152866"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="4fa3b-104">Entity Framework をモック作成時に単体テストの ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="4fa3b-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="4fa3b-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4fa3b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="4fa3b-106">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> <span data-ttu-id="4fa3b-107">このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="4fa3b-108">Entity Framework を使用するテスト オブジェクトを作成する方法とテストは、コンテキスト オブジェクトを渡すことを有効にするスキャフォールディング コント ローラーを変更する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
> 
> <span data-ttu-id="4fa3b-109">ASP.NET Web API を使用した単体テストの概要については、次を参照してください。 [ASP.NET Web API 2 による単体テスト](unit-testing-with-aspnet-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
> 
> <span data-ttu-id="4fa3b-110">このチュートリアルでは、ASP.NET Web API の基本概念を使い慣れて前提としています。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="4fa3b-111">入門チュートリアルでは、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4fa3b-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="4fa3b-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="4fa3b-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4fa3b-113">Visual Studio 2017</span></span>](https://www.visualstudio.com/vs/)
> - <span data-ttu-id="4fa3b-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="4fa3b-114">Web API 2</span></span>


## <a name="in-this-topic"></a><span data-ttu-id="4fa3b-115">このトピックの内容</span><span class="sxs-lookup"><span data-stu-id="4fa3b-115">In this topic</span></span>

<span data-ttu-id="4fa3b-116">このトピックは、次のセクションで構成されています。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4fa3b-117">前提条件</span><span class="sxs-lookup"><span data-stu-id="4fa3b-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="4fa3b-118">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-118">Download code</span></span>](#download)
- [<span data-ttu-id="4fa3b-119">単体テスト プロジェクトとアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="4fa3b-120">モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="4fa3b-121">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="4fa3b-122">依存関係の挿入を追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="4fa3b-123">テスト プロジェクトの NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="4fa3b-124">テスト コンテキストを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="4fa3b-125">テストを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="4fa3b-126">テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-126">Run tests</span></span>](#runtests)

<span data-ttu-id="4fa3b-127">」の手順を既に完了している場合[ASP.NET Web API 2 による単体テスト](unit-testing-with-aspnet-web-api.md)、セクションにスキップできます[コント ローラーを追加](#controller)です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="4fa3b-128">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="4fa3b-128">Prerequisites</span></span>

<span data-ttu-id="4fa3b-129">Visual Studio 2017 Community、Professional edition または Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="4fa3b-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="4fa3b-130">コードをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-130">Download code</span></span>

<span data-ttu-id="4fa3b-131">ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="4fa3b-132">ダウンロード可能なプロジェクトには、このトピックと単体テスト コードが含まれています、 [ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)トピックです。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="4fa3b-133">単体テスト プロジェクトとアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-133">Create application with unit test project</span></span>

<span data-ttu-id="4fa3b-134">アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、既存のアプリケーションに単体テスト プロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="4fa3b-135">このチュートリアルでは、アプリケーションを作成するときに、単体テスト プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="4fa3b-136">という名前の新しい ASP.NET Web アプリケーションを作成する**StoreApp**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="4fa3b-137">Windows では、新しい ASP.NET プロジェクト、選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアです。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="4fa3b-138">選択、**単体テストを追加**オプション。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="4fa3b-139">単体テスト プロジェクトの名前が自動的に**StoreApp.Tests**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="4fa3b-140">この名前を保持することができます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-140">You can keep this name.</span></span>

![単体テスト プロジェクトを作成します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="4fa3b-142">アプリケーションを作成するには、後に表示されます - 2 つのプロジェクトが含まれている**StoreApp**と**StoreApp.Tests**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="4fa3b-143">モデル クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-143">Create the model class</span></span>

<span data-ttu-id="4fa3b-144">StoreApp プロジェクトにクラス ファイルを追加、**モデル**という名前のフォルダー **Product.cs**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="4fa3b-145">ファイルの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="4fa3b-146">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="4fa3b-147">コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-147">Add the controller</span></span>

<span data-ttu-id="4fa3b-148">Controllers フォルダーを右クリックし **追加**と**スキャフォールディングされた新しい項目**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="4fa3b-149">Entity Framework を使用して、アクションがある Web API 2 コント ローラーを選択します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![新しいコント ローラーを追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="4fa3b-151">次の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-151">Set the following values:</span></span>

- <span data-ttu-id="4fa3b-152">コント ローラー名: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="4fa3b-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="4fa3b-153">モデル クラス:**製品**</span><span class="sxs-lookup"><span data-stu-id="4fa3b-153">Model class: **Product**</span></span>
- <span data-ttu-id="4fa3b-154">データ コンテキスト クラス: [選択**新しいデータ コンテキスト**ボタン以下に示す値を設定する]</span><span class="sxs-lookup"><span data-stu-id="4fa3b-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![コント ローラーを指定します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="4fa3b-156">をクリックして**追加**を自動的に生成されたコードで、コント ローラーを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="4fa3b-157">コードには、作成、取得、更新、および Product クラスのインスタンスを削除するためのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="4fa3b-158">次のコードは、メソッドの製品に追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="4fa3b-159">メソッドがのインスタンスを返すことに注意してください。 **IHttpActionResult**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="4fa3b-160">IHttpActionResult Web API 2 の新機能の 1 つは、単体テストの開発が簡単になります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="4fa3b-161">次のセクションでは、容易にするために生成されたコードをカスタマイズするコント ローラーにテスト オブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="4fa3b-162">依存関係の挿入を追加します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-162">Add dependency injection</span></span>

<span data-ttu-id="4fa3b-163">現時点では、ProductController クラスでは、StoreAppContext クラスのインスタンスを使用するハード コーディングされたです。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="4fa3b-164">アプリケーションを変更し、ハード コーディングされた依存関係を削除する依存関係の挿入と呼ばれるパターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="4fa3b-165">この依存関係を分割することにより、モック オブジェクト内に渡すことができますをテストする場合。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="4fa3b-166">右クリックし、**モデル**フォルダー、という名前の新しいインターフェイスを追加および**IStoreAppContext**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="4fa3b-167">このコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="4fa3b-168">StoreAppContext.cs ファイルを開き、次の強調表示された変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="4fa3b-169">重要な変更点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-169">The important changes to note are:</span></span>

- <span data-ttu-id="4fa3b-170">StoreAppContext クラス IStoreAppContext インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="4fa3b-171">MarkAsModified メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-171">MarkAsModified method is implemented</span></span>


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="4fa3b-172">ProductController.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="4fa3b-173">強調表示されたコードのように既存のコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="4fa3b-174">これらの変更は StoreAppContext で依存関係を中断し、別のオブジェクト コンテキスト クラスを渡すには、他のクラスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="4fa3b-175">この変更は、単体テスト中に、テスト コンテキストに渡すことができますを有効になります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="4fa3b-176">1 つの以上変更 ProductController で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="4fa3b-177">**PutProduct**メソッドは、置換するエンティティの状態を設定する行が MarkAsModified メソッドへの呼び出しで変更します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="4fa3b-178">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-178">Build the solution.</span></span>

<span data-ttu-id="4fa3b-179">テスト プロジェクトを設定する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="4fa3b-180">テスト プロジェクトの NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="4fa3b-181">空のテンプレートを使用してアプリケーションを作成するときに、単体テスト プロジェクト (StoreApp.Tests) は、インストールされている NuGet パッケージが含まれません。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="4fa3b-182">Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで一部の NuGet パッケージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="4fa3b-183">このチュートリアルでは、Entity Framework パッケージとテスト プロジェクトに Microsoft ASP.NET Web API 2 Core パッケージを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-183">For this tutorial, you must include the Entity Framework packge and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="4fa3b-184">StoreApp.Tests プロジェクトを右クリックし  **NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="4fa3b-185">そのプロジェクトにパッケージを追加するプロジェクトを StoreApp.Tests を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![パッケージを管理します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="4fa3b-187">オンラインのパッケージから検索して EntityFramework パッケージ (バージョン 6.0 またはそれ以降) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="4fa3b-188">EntityFramework パッケージが既にインストールされている場合は、StoreApp.Tests プロジェクトではなく StoreApp プロジェクトを選択した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Entity Framework を追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="4fa3b-190">検索し、Microsoft ASP.NET Web API 2 Core パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![web api core パッケージをインストールします。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="4fa3b-192">NuGet パッケージの管理ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="4fa3b-193">テスト コンテキストを作成します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-193">Create test context</span></span>

<span data-ttu-id="4fa3b-194">という名前のクラスを追加**TestDbSet**テスト プロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="4fa3b-195">このクラスは、テスト データ セットの基本クラスとして機能します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="4fa3b-196">このコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="4fa3b-197">という名前のクラスを追加**TestProductDbSet**を次のコードを含むテスト プロジェクトをします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="4fa3b-198">という名前のクラスを追加**TestStoreAppContext**し、既存のコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="4fa3b-199">テストの作成</span><span class="sxs-lookup"><span data-stu-id="4fa3b-199">Create tests</span></span>

<span data-ttu-id="4fa3b-200">既定では、テスト プロジェクトには、という名前の空のテスト ファイルが含まれる**UnitTest1.cs**です。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="4fa3b-201">このファイルは、テスト メソッドの作成に使用する属性を示します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="4fa3b-202">このチュートリアルでは、新しいテスト クラスを追加するために、このファイルを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="4fa3b-203">という名前のクラスを追加**TestProductController**テスト プロジェクトにします。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="4fa3b-204">このコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="4fa3b-205">テストの実行</span><span class="sxs-lookup"><span data-stu-id="4fa3b-205">Run tests</span></span>

<span data-ttu-id="4fa3b-206">テストを実行する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-206">You are now ready to run the tests.</span></span> <span data-ttu-id="4fa3b-207">マークされたメソッドのすべての**TestMethod**属性がテストされます。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="4fa3b-208">**テスト**メニュー項目、テストを実行します。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-208">From the **Test** menu item, run the tests.</span></span>

![テストの実行](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="4fa3b-210">開く、**テスト エクスプ ローラー**ウィンドウ、およびテストの結果に注意してください。</span><span class="sxs-lookup"><span data-stu-id="4fa3b-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![テスト結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
