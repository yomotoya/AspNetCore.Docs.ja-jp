---
title: "ASP.NET Core と Visual Studio for Windows で Web API を作成する"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio for Windows で Web API を構築する"
keywords: "ASP.NET Core、WebAPI、Web API、REST、HTTP、Service、HTTP サービス"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: da47296fd952300ce60121603834a9f22be3c339
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="93da3-104">ASP.NET Core と Visual Studio for Windows で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="93da3-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="93da3-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="93da3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="93da3-106">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="93da3-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="93da3-107">ユーザー インターフェイス (UI) は作成しません。</span><span class="sxs-lookup"><span data-stu-id="93da3-107">A user interface (UI) is not created.</span></span>

<span data-ttu-id="93da3-108">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="93da3-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="93da3-109">Windows: Visual Studio for Windows で Web API を作成する (このチュートリアル)</span><span class="sxs-lookup"><span data-stu-id="93da3-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="93da3-110">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="93da3-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="93da3-111">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="93da3-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="93da3-112">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="93da3-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="93da3-113">ASP.NET Core 1.1 バージョンについては、[この PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="93da3-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="93da3-114">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="93da3-114">Create the project</span></span>

<span data-ttu-id="93da3-115">Visual Studio で、**[ファイル]** メニュー、**[新規]** > **[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="93da3-116">**[ASP.NET Core Web アプリケーション (.NET Core)]** プロジェクト テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="93da3-117">プロジェクトに `TodoApi` という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="93da3-117">Name the project `TodoApi` and select **OK**.</span></span>

![[新しいプロジェクト] ダイアログ](first-web-api/_static/new-project.png)

<span data-ttu-id="93da3-119">**[New ASP.NET Core Web Application - TodoApi]\(新しい ASP.NET Core Web アプリケーション - TodoApi\)** ダイアログで、**[Web API]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="93da3-120">**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-120">Select **OK**.</span></span> <span data-ttu-id="93da3-121">**[Enable Docker Support]\(Docker サポートを有効にする\)** は**選択しないで**ください。</span><span class="sxs-lookup"><span data-stu-id="93da3-121">Do **not** select **Enable Docker Support**.</span></span>

![[新しい ASP.NET Core Web アプリケーション] ダイアログ。ASP.NET Core テンプレートから Web API プロジェクト テンプレートが選択されています。](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="93da3-123">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="93da3-123">Launch the app</span></span>

<span data-ttu-id="93da3-124">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="93da3-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="93da3-125">Visual Studio でブラウザーが起動し、`http://localhost:port/api/values` にアクセスします。*ポート*はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="93da3-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="93da3-126">Chrome、Microsoft Edge、Firefox では次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="93da3-126">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="93da3-127">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="93da3-127">Add a model class</span></span>

<span data-ttu-id="93da3-128">モデルは、アプリのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="93da3-128">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="93da3-129">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="93da3-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="93da3-130">"Models" という名前のフォルダーを追加します。</span><span class="sxs-lookup"><span data-stu-id="93da3-130">Add a folder named "Models".</span></span> <span data-ttu-id="93da3-131">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="93da3-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="93da3-132">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="93da3-133">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="93da3-133">Name the folder *Models*.</span></span>

<span data-ttu-id="93da3-134">注: モデル クラスはプロジェクト内のどこでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="93da3-134">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="93da3-135">通例として、モデル クラス用に *Models* フォルダーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="93da3-135">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="93da3-136">`TodoItem` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="93da3-136">Add a `TodoItem` class.</span></span> <span data-ttu-id="93da3-137">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="93da3-137">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="93da3-138">クラスに `TodoItem` という名前を付け、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-138">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="93da3-139">`TodoItem` クラスを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="93da3-139">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="93da3-140">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="93da3-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="93da3-141">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="93da3-141">Create the database context</span></span>

<span data-ttu-id="93da3-142">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="93da3-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="93da3-143">このクラスは `Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="93da3-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="93da3-144">`TodoContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="93da3-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="93da3-145">*Models* フォルダーを右クリックし、**[追加]** > **[クラス]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="93da3-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="93da3-146">クラスに `TodoContext` という名前を付け、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="93da3-147">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="93da3-147">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="93da3-148">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="93da3-148">Add a controller</span></span>

<span data-ttu-id="93da3-149">ソリューション エクスプローラーで、*Controllers* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="93da3-149">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="93da3-150">**[追加]** > **[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-150">Select **Add** > **New Item**.</span></span> <span data-ttu-id="93da3-151">**[新しい項目の追加]** ダイアログで、**[Web API コントローラー クラス]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="93da3-151">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="93da3-152">クラスに `TodoController` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="93da3-152">Name the class `TodoController`.</span></span>

![[新しい項目の追加] ダイアログ。検索ボックスに「controller」と入力されています。Web API コントローラーが選択されています。](first-web-api/_static/new_controller.png)

<span data-ttu-id="93da3-154">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="93da3-154">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="93da3-155">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="93da3-155">Launch the app</span></span>

<span data-ttu-id="93da3-156">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="93da3-156">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="93da3-157">Visual Studio でブラウザーが起動し、`http://localhost:port/api/values` にアクセスします。*ポート*はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="93da3-157">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="93da3-158">`http://localhost:port/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="93da3-158">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

