---
title: "ASP.NET Core と Visual Studio for Mac で Web API を作成する"
description: "ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, サービス, HTTP サービス"
manager: wpickett
ms.openlocfilehash: 6835cdefcc001452a3ffc8f4fd6a2f55f7274692
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="3b2b4-104">ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="3b2b4-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="3b2b4-105">[Rick Anderson](https://twitter.com/RickAndMSFT) および [Mike Wasson](https://github.com/mikewasson) 著</span><span class="sxs-lookup"><span data-stu-id="3b2b4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="3b2b4-106">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="3b2b4-107">UI は構築しません。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-107">You won’t build a UI.</span></span>

<span data-ttu-id="3b2b4-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="3b2b4-109">macOS: Visual Studio for Mac で Web API を作成する (このチュートリアル)</span><span class="sxs-lookup"><span data-stu-id="3b2b4-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="3b2b4-110">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="3b2b4-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="3b2b4-111">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="3b2b4-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="3b2b4-112">永続的なデータベースを使用する例については、「[Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index)」 (Mac または Linux での ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b2b4-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3b2b4-113">Prerequisites</span></span>

<span data-ttu-id="3b2b4-114">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-114">Install the following:</span></span>

- [<span data-ttu-id="3b2b4-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3b2b4-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="3b2b4-116">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3b2b4-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="3b2b4-117">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="3b2b4-117">Create the project</span></span>

<span data-ttu-id="3b2b4-118">Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-118">From Visual Studio, select **File > New Solution**.</span></span>

![macOS の新しいソリューション](first-web-api-mac/_static/sln.png)

<span data-ttu-id="3b2b4-120">**[.NET Core アプリ]、[ASP.NET Core Web API]、[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](first-web-api-mac/_static/1.png)

<span data-ttu-id="3b2b4-122">**[プロジェクト名]** に「**TodoApi**」と入力し、[作成] を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![構成ダイアログ](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="3b2b4-124">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="3b2b4-124">Launch the app</span></span>

<span data-ttu-id="3b2b4-125">Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="3b2b4-126">Visual Studio によってブラウザーが起動され、`http://localhost:5000` に移動されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="3b2b4-127">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="3b2b4-128">URL を `http://localhost:port/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="3b2b4-129">`ValuesController` データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="3b2b4-130">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="3b2b4-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="3b2b4-131">[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="3b2b4-132">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用すことが許可されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="3b2b4-133">**[プロジェクト]** メニューの **[Add NuGet Packages]\(NuGet パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="3b2b4-134">または、**[依存関係]** を右クリックし、**[Add Packages]\(パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="3b2b4-135">検索ボックスに、`EntityFrameworkCore.InMemory` と入力します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="3b2b4-136">`Microsoft.EntityFrameworkCore.InMemory` を選択し、**[Add Package]\(パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="3b2b4-137">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="3b2b4-137">Add a model class</span></span>

<span data-ttu-id="3b2b4-138">モデルとは、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="3b2b4-139">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="3b2b4-140">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-140">Add a folder named *Models*.</span></span> <span data-ttu-id="3b2b4-141">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="3b2b4-142">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3b2b4-143">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-143">Name the folder *Models*.</span></span>

![新しいフォルダー](first-web-api-mac/_static/folder.png)

<span data-ttu-id="3b2b4-145">注: モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="3b2b4-146">`TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="3b2b4-147">*Models* フォルダーを右クリックし、**[追加]、[新しいファイル]、[全般]、[Empty Class]\(空のクラス)\** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="3b2b4-148">クラスに `TodoItem` という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="3b2b4-149">生成されたコードを次と置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-149">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="3b2b4-150">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-150">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="3b2b4-151">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="3b2b4-151">Create the database context</span></span>

<span data-ttu-id="3b2b4-152">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-152">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="3b2b4-153">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-153">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="3b2b4-154">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-154">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="3b2b4-155">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="3b2b4-155">Add a controller</span></span>

<span data-ttu-id="3b2b4-156">ソリューション エクスプローラーの *Controllers* フォルダーに、`TodoController` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-156">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="3b2b4-157">生成されたコードを次に置き換えます (閉じかっこを追加します)。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-157">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="3b2b4-158">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="3b2b4-158">Launch the app</span></span>

<span data-ttu-id="3b2b4-159">Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-159">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="3b2b4-160">Visual Studio でブラウザーが起動し、`http://localhost:port` にアクセスします。*ポート*はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-160">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="3b2b4-161">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-161">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="3b2b4-162">URL を `http://localhost:port/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-162">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="3b2b4-163">`ValuesController` データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-163">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="3b2b4-164">`http://localhost:port/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-164">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="3b2b4-165">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="3b2b4-165">Implement the other CRUD operations</span></span>

<span data-ttu-id="3b2b4-166">コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-166">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="3b2b4-167">これらはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-167">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="3b2b4-168">プロジェクトは、コードの追加または変更後にビルドします。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-168">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="3b2b4-169">作成</span><span class="sxs-lookup"><span data-stu-id="3b2b4-169">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="3b2b4-170">これは、HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-170">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3b2b4-171">MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-171">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="3b2b4-172">`CreatedAtRoute` メソッドは、サーバーに新しいリソースを作成する HTTP POST メソッドの標準の応答である 201 の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-172">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="3b2b4-173">`CreatedAtRoute` では、応答に場所ヘッダーも追加されます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="3b2b4-174">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="3b2b4-175">「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-175">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="3b2b4-176">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="3b2b4-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="3b2b4-177">アプリを起動します (**[実行]、[デバッグありで開始]**)。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-177">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="3b2b4-178">Postman を起動します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-178">Start Postman.</span></span>

![Postman のコンソール](first-web-api/_static/pmc.png)

* <span data-ttu-id="3b2b4-180">HTTP メソッド名を `POST` に設定します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-180">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="3b2b4-181">**[Body]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-181">Select the **Body** radio button</span></span>
* <span data-ttu-id="3b2b4-182">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-182">Select the **raw** radio button</span></span>
* <span data-ttu-id="3b2b4-183">型を JSON に設定します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-183">Set the type to JSON</span></span>
* <span data-ttu-id="3b2b4-184">キー/値エディターで次のような To do 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-184">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="3b2b4-185">**[Send]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-185">Select **Send**</span></span>

* <span data-ttu-id="3b2b4-186">次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-186">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman コンソールの [Headers] タブ](first-web-api/_static/pmget.png)

<span data-ttu-id="3b2b4-188">[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-188">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="3b2b4-189">`GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-189">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="3b2b4-190">更新</span><span class="sxs-lookup"><span data-stu-id="3b2b4-190">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="3b2b4-191">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-191">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="3b2b4-192">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-192">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="3b2b4-193">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-193">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="3b2b4-194">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-194">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="3b2b4-196">削除</span><span class="sxs-lookup"><span data-stu-id="3b2b4-196">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="3b2b4-197">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-197">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="3b2b4-199">次のステップ</span><span class="sxs-lookup"><span data-stu-id="3b2b4-199">Next steps</span></span>

* [<span data-ttu-id="3b2b4-200">コントローラー アクションへのルーティング</span><span class="sxs-lookup"><span data-stu-id="3b2b4-200">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="3b2b4-201">API の配置については、[発行および配置](../publishing/index.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3b2b4-201">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="3b2b4-202">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="3b2b4-202">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="3b2b4-203">Postman</span><span class="sxs-lookup"><span data-stu-id="3b2b4-203">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="3b2b4-204">Fiddler</span><span class="sxs-lookup"><span data-stu-id="3b2b4-204">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
