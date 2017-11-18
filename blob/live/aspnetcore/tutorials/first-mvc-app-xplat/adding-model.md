---
title: ".ASP.NET Core MVC アプリへのモデルの追加。"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, サービス, HTTP サービス, VS Code"
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="5ac13-104">*Movie.cs* という名前の *Models* フォルダーにクラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="5ac13-105">*Models/Movie.cs* ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="5ac13-106">`ID` フィールドは、データベースで主キー用に必要です。</span><span class="sxs-lookup"><span data-stu-id="5ac13-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="5ac13-107">エラーがないことを確認するためにアプリを構築すると、**M**VC アプリに **モ**デルが追加されます。</span><span class="sxs-lookup"><span data-stu-id="5ac13-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="5ac13-108">スキャフォールディング用のプロジェクトの準備</span><span class="sxs-lookup"><span data-stu-id="5ac13-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="5ac13-109">*MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="5ac13-110">ファイルを保存し、[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージに対し、**[復元]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="5ac13-111">*Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="5ac13-112">*Startup.cs* ファイルを開き、using を 2 つ追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="5ac13-113">*Startup.cs* ファイルに、次のデータベース コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="5ac13-114">これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="5ac13-115">データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="5ac13-116">プロジェクトをビルドして、エラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="5ac13-117">MovieController のスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="5ac13-117">Scaffold the MovieController</span></span>

<span data-ttu-id="5ac13-118">プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="5ac13-119">スキャフォールディング コマンドの実行時にエラーが表示される場合、解決策として[スキャフォールディング リポジトリの問題 444](https://github.com/aspnet/scaffolding/issues/444)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5ac13-119">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="5ac13-120">スキャフォールディング エンジンによって次が作成されます。</span><span class="sxs-lookup"><span data-stu-id="5ac13-120">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="5ac13-121">ムービー コントローラー (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="5ac13-121">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="5ac13-122">作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="5ac13-122">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="5ac13-123">[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。</span><span class="sxs-lookup"><span data-stu-id="5ac13-123">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="5ac13-124">ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-124">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="5ac13-125">これで、データを表示、編集、更新および削除できるデータベースができました。</span><span class="sxs-lookup"><span data-stu-id="5ac13-125">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="5ac13-126">次のチュートリアルでは、そのデータベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="5ac13-126">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="5ac13-127">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5ac13-127">Additional resources</span></span>

* [<span data-ttu-id="5ac13-128">タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="5ac13-128">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="5ac13-129">グローバライズとローカライズ</span><span class="sxs-lookup"><span data-stu-id="5ac13-129">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="5ac13-130">[前のチュートリアル - ビューの追加](adding-view.md)
[次のチュートリアル - SQL Lite の使用](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="5ac13-130">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
