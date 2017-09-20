---
title: "ASP.NET MVC Core アプリにモデルを追加する"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
keywords: "ASP.NET Core, MVC, スキャフォールディング, スキャフォールディング"
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/15/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
helpviewer_keywords: aspnet, csharp, mvc
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 36819284073eb1cb20b19c41512944e34c54c6d3
ms.sourcegitcommit: 3fece4e2869581df72090ff5e82af1a09d927699
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="1759c-104">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-104">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="1759c-105">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="1759c-105">In the **New File** dialog:</span></span>

  * <span data-ttu-id="1759c-106">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-106">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="1759c-107">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-107">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="1759c-108">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-108">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="1759c-109">`Movie` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="1759c-109">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="1759c-110">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="1759c-110">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="1759c-111">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1759c-111">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="1759c-112">これで、**M**VC アプリに、**モ**デルがあることになります。</span><span class="sxs-lookup"><span data-stu-id="1759c-112">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="1759c-113">スキャフォールディング用のプロジェクトの準備</span><span class="sxs-lookup"><span data-stu-id="1759c-113">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="1759c-114">プロジェクト ファイルを右クリックし、**[ツール]、[ファイルの編集]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-114">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![前述の手順を参照](adding-model/_static/1.png)

- <span data-ttu-id="1759c-116">*MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="1759c-116">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="1759c-117">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="1759c-117">Save the file.</span></span>

- <span data-ttu-id="1759c-118">*Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="1759c-118">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="1759c-119">*Startup.cs* ファイルを開き、using を 2 つ追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="1759c-119">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="1759c-120">*Startup.cs* ファイルに、次のデータベース コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="1759c-120">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="1759c-121">これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。</span><span class="sxs-lookup"><span data-stu-id="1759c-121">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="1759c-122">データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。</span><span class="sxs-lookup"><span data-stu-id="1759c-122">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="1759c-123">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="1759c-123">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="1759c-124">MovieController のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="1759c-124">Scaffold the MovieController</span></span>

<span data-ttu-id="1759c-125">プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1759c-125">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="1759c-126">`No executable found matching command "dotnet-aspnet-codegenerator", verify`エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="1759c-126">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="1759c-127">プロジェクト ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="1759c-127">You are in the project directory.</span></span> <span data-ttu-id="1759c-128">プロジェクト ディレクトリには、*Program.cs*、*Startup.cs* および *.csproj* ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="1759c-128">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="1759c-129">dotnet バージョンは 1.1 以上です。</span><span class="sxs-lookup"><span data-stu-id="1759c-129">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="1759c-130">`dotnet` を実行し、バージョンを取得します。</span><span class="sxs-lookup"><span data-stu-id="1759c-130">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="1759c-131">[MvcMovie.csproj ファイル](#prepare-the-project-for-scaffolding)に `<DotNetCliToolReference>` 要素が追加されました。</span><span class="sxs-lookup"><span data-stu-id="1759c-131">You have added the `<DotNetCliToolReference>` elment to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="1759c-132">スキャフォールディング エンジンによって次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="1759c-132">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="1759c-133">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="1759c-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="1759c-134">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="1759c-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="1759c-135">[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="1759c-135">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="1759c-136">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="1759c-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="1759c-137">Visual Studio へのファイルの追加</span><span class="sxs-lookup"><span data-stu-id="1759c-137">Add the files to Visual Studio</span></span>

* <span data-ttu-id="1759c-138">Visual Studio プロジェクトに、*MovieController.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="1759c-138">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="1759c-139">*Controllers* フォルダーを右クリックし、**[追加]、[ファイルの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-139">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="1759c-140">*MovieController.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-140">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="1759c-141">*Movies* フォルダーおよびビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="1759c-141">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="1759c-142">*Views* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-142">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="1759c-143">*Views* フォルダーに移動し、*Views\Movies* を選択し、**[開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-143">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="1759c-144">**[Select files to add from Movies]\(Movies から追加するファイルを選択\)** ダイアログ ボックスで、**[Include All]\(すべて含める\)**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="1759c-144">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="1759c-145">これで、データを表示、編集、更新および削除できるデータベースができました。</span><span class="sxs-lookup"><span data-stu-id="1759c-145">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="1759c-146">次のチュートリアルでは、そのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="1759c-146">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1759c-147">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="1759c-147">Additional resources</span></span>

* [<span data-ttu-id="1759c-148">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="1759c-148">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="1759c-149">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="1759c-149">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="1759c-150">[前のチュートリアル ビューの追加](adding-view.md)
[次のチュートリアル SQL の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="1759c-150">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
