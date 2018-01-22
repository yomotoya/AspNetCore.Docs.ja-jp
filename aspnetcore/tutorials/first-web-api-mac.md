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
manager: wpickett
ms.openlocfilehash: 4f2643a91e1523008b68df670a9734e3d4dea5a8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="9e327-103">ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="9e327-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="9e327-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="9e327-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="9e327-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="9e327-105">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="9e327-106">UI は構築しません。</span><span class="sxs-lookup"><span data-stu-id="9e327-106">You won’t build a UI.</span></span>

<span data-ttu-id="9e327-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="9e327-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="9e327-108">macOS: Visual Studio for Mac で Web API を作成する (このチュートリアル)</span><span class="sxs-lookup"><span data-stu-id="9e327-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="9e327-109">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="9e327-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="9e327-110">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="9e327-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="9e327-111">永続的なデータベースを使用する例については、「[Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index)」 (Mac または Linux での ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9e327-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e327-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="9e327-112">Prerequisites</span></span>

<span data-ttu-id="9e327-113">以下をインストールします。</span><span class="sxs-lookup"><span data-stu-id="9e327-113">Install the following:</span></span>

- <span data-ttu-id="9e327-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 以降</span><span class="sxs-lookup"><span data-stu-id="9e327-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="9e327-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9e327-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="9e327-116">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="9e327-116">Create the project</span></span>

<span data-ttu-id="9e327-117">Visual Studio で **[ファイル]、[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS の新しいソリューション](first-web-api-mac/_static/sln.png)

<span data-ttu-id="9e327-119">**[.NET Core アプリ]、[ASP.NET Core Web API]、[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](first-web-api-mac/_static/1.png)

<span data-ttu-id="9e327-121">**[プロジェクト名]** に「**TodoApi**」と入力し、[作成] を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![構成ダイアログ](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="9e327-123">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="9e327-123">Launch the app</span></span>

<span data-ttu-id="9e327-124">Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="9e327-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="9e327-125">Visual Studio によってブラウザーが起動され、`http://localhost:5000` に移動されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="9e327-126">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="9e327-127">URL を `http://localhost:port/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="9e327-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="9e327-128">`ValuesController` データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="9e327-129">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="9e327-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="9e327-130">[Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="9e327-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="9e327-131">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用すことが許可されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="9e327-132">**[プロジェクト]** メニューの **[Add NuGet Packages]\(NuGet パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="9e327-133">または、**[依存関係]** を右クリックし、**[Add Packages]\(パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="9e327-134">検索ボックスに、`EntityFrameworkCore.InMemory` と入力します。</span><span class="sxs-lookup"><span data-stu-id="9e327-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="9e327-135">`Microsoft.EntityFrameworkCore.InMemory` を選択し、**[Add Package]\(パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="9e327-136">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="9e327-136">Add a model class</span></span>

<span data-ttu-id="9e327-137">モデルとは、アプリケーションのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="9e327-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="9e327-138">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="9e327-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="9e327-139">*Models* という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-139">Add a folder named *Models*.</span></span> <span data-ttu-id="9e327-140">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="9e327-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="9e327-141">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="9e327-142">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9e327-142">Name the folder *Models*.</span></span>

![新しいフォルダー](first-web-api-mac/_static/folder.png)

<span data-ttu-id="9e327-144">注: モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="9e327-145">`TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="9e327-146">*Models* フォルダーを右クリックし、**[追加]、[新しいファイル]、[全般]、[Empty Class]\(空のクラス)\** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="9e327-147">クラスに `TodoItem` という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="9e327-148">生成されたコードを次と置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9e327-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="9e327-149">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="9e327-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="9e327-150">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="9e327-150">Create the database context</span></span>

<span data-ttu-id="9e327-151">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="9e327-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="9e327-152">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="9e327-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="9e327-153">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="9e327-154">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="9e327-154">Add a controller</span></span>

<span data-ttu-id="9e327-155">ソリューション エクスプローラーの *Controllers* フォルダーに、`TodoController` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="9e327-156">生成されたコードを次に置き換えます (閉じかっこを追加します)。</span><span class="sxs-lookup"><span data-stu-id="9e327-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="9e327-157">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="9e327-157">Launch the app</span></span>

<span data-ttu-id="9e327-158">Visual Studio で、**[実行]、[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="9e327-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="9e327-159">Visual Studio でブラウザーが起動し、`http://localhost:port` にアクセスします。*ポート*はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="9e327-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="9e327-160">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="9e327-161">URL を `http://localhost:port/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="9e327-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="9e327-162">`ValuesController` データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="9e327-163">`http://localhost:port/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="9e327-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="9e327-164">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="9e327-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="9e327-165">コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="9e327-166">これらはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。</span><span class="sxs-lookup"><span data-stu-id="9e327-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="9e327-167">プロジェクトは、コードの追加または変更後にビルドします。</span><span class="sxs-lookup"><span data-stu-id="9e327-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="9e327-168">作成</span><span class="sxs-lookup"><span data-stu-id="9e327-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="9e327-169">これは、HTTP POST メソッドです。[`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 属性を使用します。</span><span class="sxs-lookup"><span data-stu-id="9e327-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="9e327-170">MVC は、[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="9e327-171">`CreatedAtRoute` メソッドは、サーバーに新しいリソースを作成する HTTP POST メソッドの標準の応答である 201 の応答を返します。</span><span class="sxs-lookup"><span data-stu-id="9e327-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="9e327-172">`CreatedAtRoute` では、応答に場所ヘッダーも追加されます。</span><span class="sxs-lookup"><span data-stu-id="9e327-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="9e327-173">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="9e327-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="9e327-174">「[10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9e327-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="9e327-175">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="9e327-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="9e327-176">アプリを起動します (**[実行]、[デバッグありで開始]**)。</span><span class="sxs-lookup"><span data-stu-id="9e327-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="9e327-177">Postman を起動します。</span><span class="sxs-lookup"><span data-stu-id="9e327-177">Start Postman.</span></span>

![Postman のコンソール](first-web-api/_static/pmc.png)

* <span data-ttu-id="9e327-179">HTTP メソッド名を `POST` に設定します。</span><span class="sxs-lookup"><span data-stu-id="9e327-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="9e327-180">**[Body]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="9e327-181">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="9e327-182">型を JSON に設定します。</span><span class="sxs-lookup"><span data-stu-id="9e327-182">Set the type to JSON</span></span>
* <span data-ttu-id="9e327-183">キー/値エディターで次のような To do 項目を追加します。</span><span class="sxs-lookup"><span data-stu-id="9e327-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="9e327-184">**[Send]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="9e327-184">Select **Send**</span></span>

* <span data-ttu-id="9e327-185">次のように、下のウィンドウの [Headers] タブを選択して、**[Location]** ヘッダーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="9e327-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman コンソールの [Headers] タブ](first-web-api/_static/pmget.png)

<span data-ttu-id="9e327-187">[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="9e327-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="9e327-188">`GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="9e327-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="9e327-189">更新</span><span class="sxs-lookup"><span data-stu-id="9e327-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="9e327-190">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="9e327-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="9e327-191">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="9e327-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="9e327-192">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="9e327-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="9e327-193">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="9e327-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="9e327-195">削除</span><span class="sxs-lookup"><span data-stu-id="9e327-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="9e327-196">応答は [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="9e327-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="9e327-198">次の手順</span><span class="sxs-lookup"><span data-stu-id="9e327-198">Next steps</span></span>

* [<span data-ttu-id="9e327-199">コントローラー アクションへのルーティング</span><span class="sxs-lookup"><span data-stu-id="9e327-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="9e327-200">API の配置については、[ホストおよび配置](xref:host-and-deploy/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9e327-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="9e327-201">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="9e327-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="9e327-202">Postman</span><span class="sxs-lookup"><span data-stu-id="9e327-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="9e327-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="9e327-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
