---
title: ASP.NET Core と Visual Studio で Web API を作成する
author: rick-anderson
description: ASP.NET Core MVC と Windows の Visual Studio で Web API を構築する
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450698"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="800d1-103">ASP.NET Core と Visual Studio で Web API を作成する</span><span class="sxs-lookup"><span data-stu-id="800d1-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="800d1-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="800d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="800d1-105">このチュートリアルでは、"to-do" 項目の一覧を管理する Web API を構築します。</span><span class="sxs-lookup"><span data-stu-id="800d1-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="800d1-106">ユーザー インターフェイス (UI) は作成されません。</span><span class="sxs-lookup"><span data-stu-id="800d1-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="800d1-107">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="800d1-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="800d1-108">Windows: Windows の Visual Studio で Web API を作成する (このチュートリアル。[ビデオ版](https://www.youtube.com/watch?v=TTkhEyGBfAk)を参照)</span><span class="sxs-lookup"><span data-stu-id="800d1-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="800d1-109">macOS: [Visual Studio for Mac で Web API を作成する](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="800d1-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="800d1-110">macOS、Linux、Windows: [Visual Studio Code で Web API を作成する](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="800d1-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="800d1-111">ASP.NET Core の目次の提案された新しい構造の有用性をテストしています。</span><span class="sxs-lookup"><span data-stu-id="800d1-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="800d1-112">現在または提案された目次で 7 つのトピックを探す演習をする時間がある場合は、[ここをクリックして、調査に参加してください](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5)。</span><span class="sxs-lookup"><span data-stu-id="800d1-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="800d1-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="800d1-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="800d1-114">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="800d1-114">Create the project</span></span>

<span data-ttu-id="800d1-115">Visual Studio で次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="800d1-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="800d1-116">**[ファイル]** メニューで、**[新規作成]**、 > **[プロジェクト]** の順に作成します。</span><span class="sxs-lookup"><span data-stu-id="800d1-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="800d1-117">**[ASP.NET Core Web アプリケーション]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="800d1-118">プロジェクトに「*TodoApi*」という名前を付け、**[OK]** をクリックます。</span><span class="sxs-lookup"><span data-stu-id="800d1-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="800d1-119">**[New ASP.NET Core Web Application - TodoApi]\(新しい ASP.NET Core Web アプリケーション - TodoApi\)** ダイアログで、ASP.NET Core のバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="800d1-120">**API** テンプレートを選択し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="800d1-121">**[Enable Docker Support]\(Docker サポートを有効にする\)** は**選択しないで**ください。</span><span class="sxs-lookup"><span data-stu-id="800d1-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="800d1-122">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="800d1-122">Launch the app</span></span>

<span data-ttu-id="800d1-123">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="800d1-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="800d1-124">Visual Studio でブラウザーが起動し、`http://localhost:<port>/api/values` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="800d1-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="800d1-125">Chrome、Microsoft Edge、Firefox では次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="800d1-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="800d1-126">Internet Explorer を使っている場合、*values.json* ファイルを保存するように求められます。</span><span class="sxs-lookup"><span data-stu-id="800d1-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="800d1-127">モデル クラスの追加</span><span class="sxs-lookup"><span data-stu-id="800d1-127">Add a model class</span></span>

<span data-ttu-id="800d1-128">モデルは、アプリのデータを表すオブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="800d1-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="800d1-129">ここでは、唯一のモデルが to-do 項目です。</span><span class="sxs-lookup"><span data-stu-id="800d1-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="800d1-130">ソリューション エクスプローラーで、プロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="800d1-131">**[追加]** > **[新しいフォルダー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="800d1-132">フォルダーに *Models* という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="800d1-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="800d1-133">モデル クラスはプロジェクト内のどこでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="800d1-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="800d1-134">通例として、モデル クラス用に *Models* フォルダーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="800d1-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="800d1-135">ソリューション エクスプローラーで、*[Models]* フォルダーを右クリックし、**[追加]**、 > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="800d1-136">クラスに「*TodoItem*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="800d1-137">`TodoItem` クラスを次のコードで更新します。</span><span class="sxs-lookup"><span data-stu-id="800d1-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="800d1-138">`TodoItem` が作成されるとき、データベースは `Id` を生成します。</span><span class="sxs-lookup"><span data-stu-id="800d1-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="800d1-139">データベース コンテキストの作成</span><span class="sxs-lookup"><span data-stu-id="800d1-139">Create the database context</span></span>

<span data-ttu-id="800d1-140">*データベース コンテキスト*は、所与のデータ モデルに対し、Entity Framework 機能を調整するメイン クラスです。</span><span class="sxs-lookup"><span data-stu-id="800d1-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="800d1-141">このクラスは `Microsoft.EntityFrameworkCore.DbContext` クラスから派生させて作成します。</span><span class="sxs-lookup"><span data-stu-id="800d1-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="800d1-142">ソリューション エクスプローラーで、*[Models]* フォルダーを右クリックし、**[追加]**、 > **[クラス]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="800d1-143">クラスに「*TodoContext*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="800d1-144">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="800d1-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="800d1-145">コントローラーの追加</span><span class="sxs-lookup"><span data-stu-id="800d1-145">Add a controller</span></span>

<span data-ttu-id="800d1-146">ソリューション エクスプローラーで、*Controllers* フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="800d1-147">**[追加]** > **[新しい項目]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="800d1-148">**[新しい項目の追加]** ダイアログで、**[API コントローラー クラス]** テンプレートを選択します。</span><span class="sxs-lookup"><span data-stu-id="800d1-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="800d1-149">クラスに「*TodoController*」という名前を付け、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="800d1-149">Name the class *TodoController*, and click **Add**.</span></span>

![[新しい項目の追加] ダイアログ。検索ボックスに「controller」と入力されています。Web API コントローラーが選択されています。](first-web-api/_static/new_controller.png)

<span data-ttu-id="800d1-151">このクラスを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="800d1-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="800d1-152">アプリの起動</span><span class="sxs-lookup"><span data-stu-id="800d1-152">Launch the app</span></span>

<span data-ttu-id="800d1-153">Visual Studio で、CTRL を押しながら F5 を押し、アプリを起動します。</span><span class="sxs-lookup"><span data-stu-id="800d1-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="800d1-154">Visual Studio でブラウザーが起動し、`http://localhost:<port>/api/values` にアクセスします。ここで、`<port>` はランダムに選択されたポート番号になります。</span><span class="sxs-lookup"><span data-stu-id="800d1-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="800d1-155">`http://localhost:<port>/api/todo` の `Todo` コントローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="800d1-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
