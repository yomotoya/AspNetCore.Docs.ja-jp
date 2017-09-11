---
title: "モデルの追加"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 054ef316461447ab651cca8c4f324e7b4e98f856
ms.sourcegitcommit: d9e2c99c837078fcac0e315025f8fbfbd45ea6e8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="badf5-104">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="badf5-105">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-105">Add the following code to the *Models/Movie.cs* file:</span></span>

<span data-ttu-id="badf5-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="badf5-106">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]</span></span>

<span data-ttu-id="badf5-107">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="badf5-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="badf5-108">エラーがないことを確認するためにアプリを構築すると、**M**VC アプリに **モ**デルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="badf5-108">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="badf5-109">スキャフォールディング用のプロジェクトの準備</span><span class="sxs-lookup"><span data-stu-id="badf5-109">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="badf5-110">*MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-110">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   <span data-ttu-id="badf5-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span><span class="sxs-lookup"><span data-stu-id="badf5-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]</span></span>

- <span data-ttu-id="badf5-112">ファイルを保存し、[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージに対し、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="badf5-112">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="badf5-113">*Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-113">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   <span data-ttu-id="badf5-114">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="badf5-114">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="badf5-115">*Startup.cs* ファイルを開き、using を 2 つ追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-115">Open the *Startup.cs* file and add two usings:</span></span>

   <span data-ttu-id="badf5-116">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="badf5-116">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="badf5-117">*Startup.cs* ファイルに、次のデータベース コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="badf5-117">Add the database context to the *Startup.cs* file:</span></span>

   <span data-ttu-id="badf5-118">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="badf5-118">[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]</span></span>

  <span data-ttu-id="badf5-119">これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。</span><span class="sxs-lookup"><span data-stu-id="badf5-119">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="badf5-120">データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。</span><span class="sxs-lookup"><span data-stu-id="badf5-120">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="badf5-121">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="badf5-121">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="badf5-122">MovieController のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="badf5-122">Scaffold the MovieController</span></span>

<span data-ttu-id="badf5-123">プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="badf5-123">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="badf5-124">スキャフォールディング コマンドの実行時にエラーが表示される場合、解決策として[スキャフォールディング リポジトリの問題 444](https://github.com/aspnet/scaffolding/issues/444)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="badf5-124">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="badf5-125">スキャフォールディング エンジンによって次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="badf5-125">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="badf5-126">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="badf5-126">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="badf5-127">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="badf5-127">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="badf5-128">[CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="badf5-128">The automatic creation of [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="badf5-129">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="badf5-129">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="badf5-130">これで、データを表示、編集、更新および削除できるデータベースができました。</span><span class="sxs-lookup"><span data-stu-id="badf5-130">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="badf5-131">次のチュートリアルでは、そのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="badf5-131">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="badf5-132">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="badf5-132">Additional resources</span></span>

* [<span data-ttu-id="badf5-133">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="badf5-133">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="badf5-134">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="badf5-134">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="badf5-135">[前のチュートリアル - ビューの追加](adding-view.md)
[次のチュートリアル - SQL Lite の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="badf5-135">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
