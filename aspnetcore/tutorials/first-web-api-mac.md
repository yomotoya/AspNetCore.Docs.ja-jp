---
title: ASP.NET Core と Visual Studio for Mac で Web API を作成する
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio for Mac で Web API を作成する
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011626"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="d18c7-103">ASP.NET Core と Visual Studio for Mac で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="d18c7-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="d18c7-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d18c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d18c7-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d18c7-106">UI が構成されていません。</span><span class="sxs-lookup"><span data-stu-id="d18c7-106">The UI isn't constructed.</span></span>

<span data-ttu-id="d18c7-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="d18c7-108">macOS: Visual Studio for Mac で Web API を作成する (このチュートリアル)</span><span class="sxs-lookup"><span data-stu-id="d18c7-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="d18c7-109">Windows: [Visual Studio for Windows で Web API を作成する](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="d18c7-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="d18c7-110">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="d18c7-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="d18c7-111">永続的なデータベースを使用する例については、「[Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index)」 (macOS または Linux での ASP.NET Core MVC の概要) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d18c7-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d18c7-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="d18c7-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="d18c7-113">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="d18c7-113">Create the project</span></span>

<span data-ttu-id="d18c7-114">Visual Studio で **[ファイル]**、 > **[新しいソリューション]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS の新しいソリューション](first-web-api-mac/_static/sln.png)

<span data-ttu-id="d18c7-116">**[.NET Core アプリ]**、 > **[ASP.NET Core Web API]**、 > **[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS の [新しいプロジェクト] ダイアログ](first-web-api-mac/_static/1.png)

<span data-ttu-id="d18c7-118">**[プロジェクト名]** に「*TodoApi*」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![構成ダイアログ](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="d18c7-120">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="d18c7-120">Launch the app</span></span>

<span data-ttu-id="d18c7-121">Visual Studio で、**[実行]**、**[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="d18c7-122">Visual Studio によってブラウザーが起動され、`http://localhost:5000` に移動されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="d18c7-123">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="d18c7-124">URL を `http://localhost:<port>/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="d18c7-125">`ValuesController` のデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="d18c7-126">Entity Framework Core のサポートの追加</span><span class="sxs-lookup"><span data-stu-id="d18c7-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="d18c7-127">[Entity Framework Core InMemory](/ef/core/providers/in-memory/) データベース プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="d18c7-128">このデータベース プロバイダーにより、メモリ内のデータベースで Entity Framework Core を使用すことが許可されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="d18c7-129">**[プロジェクト]** メニューの **[Add NuGet Packages]\(NuGet パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="d18c7-130">または、**[依存関係]** を右クリックし、**[パッケージを追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="d18c7-131">検索ボックスに、`EntityFrameworkCore.InMemory` と入力します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="d18c7-132">`Microsoft.EntityFrameworkCore.InMemory` を選択し、**[Add Package]\(パッケージの追加\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="d18c7-133">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="d18c7-133">Add a model class</span></span>

<span data-ttu-id="d18c7-134">モデルは、アプリのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="d18c7-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="d18c7-135">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="d18c7-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d18c7-136">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="d18c7-137">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="d18c7-138">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-138">Name the folder *Models*.</span></span>

![新しいフォルダー](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="d18c7-140">モデル クラスはプロジェクト内のどこでも配置できますが、慣習で *Models* フォルダーが利用されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="d18c7-141">*Models* フォルダーを右クリックし、**[追加]**、**[新しいファイル]**、**[全般]**、**[空のクラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="d18c7-142">クラスに「*TodoItem*」という名前を付け、**[新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="d18c7-143">生成されたコードを次と置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d18c7-144">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="d18c7-145">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="d18c7-145">Create the database context</span></span>

<span data-ttu-id="d18c7-146">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="d18c7-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d18c7-147">このクラスは、`Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d18c7-148">*Models* フォルダーに `TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="d18c7-149">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="d18c7-149">Add a controller</span></span>

<span data-ttu-id="d18c7-150">ソリューション エクスプローラーの *Controllers* フォルダーに、`TodoController` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="d18c7-151">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d18c7-152">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="d18c7-152">Launch the app</span></span>

<span data-ttu-id="d18c7-153">Visual Studio で、**[実行]**、**[デバッグありで開始]** の順に選択してアプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="d18c7-154">Visual Studio でブラウザーが起動し、`http://localhost:<port>` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="d18c7-155">HTTP 404 (Not Found) エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="d18c7-156">URL を `http://localhost:<port>/api/values` に変更します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="d18c7-157">`ValuesController` のデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="d18c7-158">`http://localhost:<port>/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="d18c7-159">次の JSON が返されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="d18c7-160">その他の CRUD 操作の実装</span><span class="sxs-lookup"><span data-stu-id="d18c7-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="d18c7-161">コントローラーに、`Create`、`Update`、および `Delete` メソッドを追加します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="d18c7-162">これらのメソッドはテーマのバリエーションなので、単にコードを示し、主な違いを強調します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="d18c7-163">プロジェクトは、コードの追加または変更後にビルドします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="d18c7-164">作成</span><span class="sxs-lookup"><span data-stu-id="d18c7-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="d18c7-165">前述のメソッドは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されている HTTP POST に対応します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="d18c7-166">MVC は、[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 属性により、HTTP 要求の本文から to-do 項目の値を取得するよう指示されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="d18c7-167">前述のメソッドは、[[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 属性で示されている HTTP POST に対応します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="d18c7-168">MVC は、HTTP 要求の本文から to-do 項目の値を取得します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>

::: moniker-end

<span data-ttu-id="d18c7-169">`CreatedAtRoute` メソッドにより、201 の応答があります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="d18c7-170">これは、サーバーに新しいリソースを作成する HTTP POST メソッドに対する標準の応答です。</span><span class="sxs-lookup"><span data-stu-id="d18c7-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="d18c7-171">`CreatedAtRoute` では、応答に場所ヘッダーも追加されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="d18c7-172">場所ヘッダーでは、新しく作成された To Do アイテムの URI を指定します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="d18c7-173">「[10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)」 (10.2.2 201 生成) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d18c7-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="d18c7-174">Postman を使用した作成要求の送信</span><span class="sxs-lookup"><span data-stu-id="d18c7-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="d18c7-175">アプリを起動します (**[実行]**、**[デバッグありで開始]**)。</span><span class="sxs-lookup"><span data-stu-id="d18c7-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="d18c7-176">Postman を開きます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-176">Open Postman.</span></span>

![Postman コンソール](first-web-api/_static/pmc.png)

* <span data-ttu-id="d18c7-178">localhost URL のポート番号を更新します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="d18c7-179">HTTP メソッドを *POST* に設定します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="d18c7-180">**[Body]** タブをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="d18c7-181">**[raw]** ラジオ ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="d18c7-182">型を *[JSON (application/json)]* に設定します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="d18c7-183">次の JSON のような、to-do 項目を含む要求本文を入力します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="d18c7-184">**[Send]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> <span data-ttu-id="d18c7-185">**[Send]** をクリックしても応答が表示されない場合、**[SSL certification verification]** オプションを無効にします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="d18c7-186">これは、**[File]**、**[Settings]** の下にあります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="d18c7-187">設定を無効にした後にもう一度 **[Send]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-187">Click the **Send** button again after disabling the setting.</span></span>

::: moniker-end

<span data-ttu-id="d18c7-188">次のように、**[Response]** ウィンドウの **[Headers]** タブをクリックして、**[Location]** ヘッダー値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="d18c7-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman コンソールの [Headers] タブ](first-web-api/_static/pmc2.png)

<span data-ttu-id="d18c7-190">[Location] ヘッダーの URI を使用して、作成したリソースにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="d18c7-191">`Create` メソッドにより [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_) が返されます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="d18c7-192">`CreatedAtRoute` に渡された最初のパラメーターは、URL の生成に使用される名前のルートを表します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="d18c7-193">`GetById` メソッドによって、`"GetTodo"` という名前のルートが作成されたことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="d18c7-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="d18c7-194">更新</span><span class="sxs-lookup"><span data-stu-id="d18c7-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

::: moniker-end

<span data-ttu-id="d18c7-195">`Update` は `Create` と似ていますが、HTTP PUT を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="d18c7-196">応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="d18c7-197">HTTP 仕様に従って、PUT 要求では、デルタのみでなく、更新されたエンティティ全体を送信するようクライアントに求めます。</span><span class="sxs-lookup"><span data-stu-id="d18c7-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="d18c7-198">部分的な更新をサポートするには、HTTP PATCH を使用します。</span><span class="sxs-lookup"><span data-stu-id="d18c7-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="d18c7-200">削除</span><span class="sxs-lookup"><span data-stu-id="d18c7-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="d18c7-201">応答は [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) となります。</span><span class="sxs-lookup"><span data-stu-id="d18c7-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![204 (No Content) の応答を示す Postman コンソール](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
