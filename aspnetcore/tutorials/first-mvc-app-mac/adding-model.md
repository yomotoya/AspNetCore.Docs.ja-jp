---
title: ".ASP.NET Core MVC アプリへのモデルの追加"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: bf4d5d289266b585cbdfbb70c7482620fd4ced54
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="e0fd8-103">*Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="e0fd8-104">**[新しいファイル]** ダイアログで次を実行します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="e0fd8-105">左側のウィンドウで **[全般]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="e0fd8-106">中央ウィンドウで **[空のクラス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="e0fd8-107">クラスに **Movie** という名前を付け、**[新規]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="e0fd8-108">`Movie` クラスに次のプロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="e0fd8-109">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="e0fd8-110">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="e0fd8-111">これで、**M**VC アプリに、**モ**デルがあることになります。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="e0fd8-112">スキャフォールディング用のプロジェクトの準備</span><span class="sxs-lookup"><span data-stu-id="e0fd8-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="e0fd8-113">プロジェクト ファイルを右クリックし、**[ツール]、[ファイルの編集]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![前述の手順を参照](adding-model/_static/1.png)

- <span data-ttu-id="e0fd8-115">*MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="e0fd8-116">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-116">Save the file.</span></span>

- <span data-ttu-id="e0fd8-117">*Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="e0fd8-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="e0fd8-118">*Startup.cs* ファイルを開き、using を 2 つ追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="e0fd8-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="e0fd8-119">*Startup.cs* ファイルに、次のデータベース コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="e0fd8-120">これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="e0fd8-121">データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="e0fd8-122">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="e0fd8-123">MovieController のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="e0fd8-123">Scaffold the MovieController</span></span>

<span data-ttu-id="e0fd8-124">プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="e0fd8-125">`No executable found matching command "dotnet-aspnet-codegenerator", verify`エラーが発生した場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="e0fd8-126">プロジェクト ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-126">You are in the project directory.</span></span> <span data-ttu-id="e0fd8-127">プロジェクト ディレクトリには、*Program.cs*、*Startup.cs* および *.csproj* ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="e0fd8-128">dotnet バージョンは 1.1 以上です。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="e0fd8-129">`dotnet` を実行し、バージョンを取得します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="e0fd8-130">[MvcMovie.csproj file](#prepare-the-project-for-scaffolding) に `<DotNetCliToolReference>` 要素が追加されています。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="e0fd8-131">スキャフォールディング エンジンによって次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="e0fd8-132">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e0fd8-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e0fd8-133">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e0fd8-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="e0fd8-134">[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="e0fd8-135">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="e0fd8-136">Visual Studio へのファイルの追加</span><span class="sxs-lookup"><span data-stu-id="e0fd8-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="e0fd8-137">Visual Studio プロジェクトに、*MovieController.cs* ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="e0fd8-138">*Controllers* フォルダーを右クリックし、**[追加]、[ファイルの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="e0fd8-139">*MovieController.cs* ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="e0fd8-140">*Movies* フォルダーおよびビューを追加します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="e0fd8-141">*Views* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="e0fd8-142">*Views* フォルダーに移動し、*Views\Movies* を選択し、**[開く]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="e0fd8-143">**[Select files to add from Movies]\(Movies から追加するファイルを選択\)** ダイアログ ボックスで、**[Include All]\(すべて含める\)**、**[OK]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="e0fd8-144">これで、データを表示、編集、更新および削除できるデータベースができました。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="e0fd8-145">次のチュートリアルでは、そのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="e0fd8-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0fd8-146">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="e0fd8-146">Additional resources</span></span>

* [<span data-ttu-id="e0fd8-147">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="e0fd8-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e0fd8-148">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="e0fd8-148">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="e0fd8-149">[前のチュートリアル ビューの追加](adding-view.md)
[次のチュートリアル SQL の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e0fd8-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
