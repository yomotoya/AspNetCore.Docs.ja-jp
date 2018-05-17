---
title: ASP.NET Core と Visual Studio for Windows で Web API を作成する
author: rick-anderson
description: ASP.NET Core MVC と Visual Studio for Windows で Web API を構築する
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="f73e5-103">ASP.NET Core と Visual Studio for Windows で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="f73e5-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="f73e5-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f73e5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f73e5-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f73e5-106">ユーザー インターフェイス (UI) は作成されません。</span><span class="sxs-lookup"><span data-stu-id="f73e5-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="f73e5-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="f73e5-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f73e5-108">Windows: Visual Studio for Windows で Web API を作成する (このチュートリアル)</span><span class="sxs-lookup"><span data-stu-id="f73e5-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="f73e5-109">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f73e5-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f73e5-110">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="f73e5-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="f73e5-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="f73e5-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="f73e5-112">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="f73e5-112">Create the project</span></span>

<span data-ttu-id="f73e5-113">Visual Studio で次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="f73e5-114">**[ファイル]** メニューで、**[新規作成]**、**[プロジェクト]** の順に作成します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f73e5-115">**[ASP.NET Core Web アプリケーション]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="f73e5-116">プロジェクトに「*TodoApi*」という名前を付け、**[OK]** をクリックます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="f73e5-117">**[New ASP.NET Core Web Application - TodoApi]\(新しい ASP.NET Core Web アプリケーション - TodoApi\)** ダイアログで、ASP.NET Core のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="f73e5-118">**API** テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="f73e5-119">**[Enable Docker Support]\(Docker サポートを有効にする\)** は**選択しないで**ください。</span><span class="sxs-lookup"><span data-stu-id="f73e5-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="f73e5-120">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="f73e5-120">Launch the app</span></span>

<span data-ttu-id="f73e5-121">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f73e5-122">Visual Studio でブラウザーが起動し、`http://localhost:<port>/api/values` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="f73e5-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f73e5-123">Chrome、Microsoft Edge、Firefox では次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="f73e5-124">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="f73e5-124">Add a model class</span></span>

<span data-ttu-id="f73e5-125">モデルは、アプリのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="f73e5-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="f73e5-126">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="f73e5-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f73e5-127">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="f73e5-128">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f73e5-129">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="f73e5-130">モデル クラスはプロジェクト内のどこでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="f73e5-131">通例として、モデル クラス用に *Models* フォルダーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="f73e5-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="f73e5-132">ソリューション エクスプローラーで、*[Models]* フォルダーを右クリックし、**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f73e5-133">クラスに「*TodoItem*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="f73e5-134">`TodoItem` クラスを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f73e5-135">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="f73e5-136">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="f73e5-136">Create the database context</span></span>

<span data-ttu-id="f73e5-137">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="f73e5-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f73e5-138">このクラスは `Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f73e5-139">ソリューション エクスプローラーで、*[Models]* フォルダーを右クリックし、**[追加]**、**[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f73e5-140">クラスに「*TodoContext*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="f73e5-141">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="f73e5-142">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="f73e5-142">Add a controller</span></span>

<span data-ttu-id="f73e5-143">ソリューション エクスプローラーで、*Controllers* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="f73e5-144">**[追加]** > **[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="f73e5-145">**[新しい項目の追加]** ダイアログで、**[API コントローラー クラス]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="f73e5-146">クラスに「*TodoController*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f73e5-146">Name the class *TodoController*, and click **Add**.</span></span>

![[新しい項目の追加] ダイアログ。検索ボックスに「controller」と入力されています。Web API コントローラーが選択されています。](first-web-api/_static/new_controller.png)

<span data-ttu-id="f73e5-148">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f73e5-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f73e5-149">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="f73e5-149">Launch the app</span></span>

<span data-ttu-id="f73e5-150">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="f73e5-151">Visual Studio でブラウザーが起動し、`http://localhost:<port>/api/values` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="f73e5-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f73e5-152">`http://localhost:<port>/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="f73e5-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
