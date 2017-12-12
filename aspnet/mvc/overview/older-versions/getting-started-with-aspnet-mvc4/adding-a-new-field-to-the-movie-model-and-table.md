---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: "ムービーのモデルとテーブルに新しいフィールドを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "注: このチュートリアルの最新バージョンはここで ASP.NET MVC 5 と Visual Studio 2013 を使用します。 安全な非常に簡単に従い、デモをお勧めしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0c094c4d4c99702a5b513717126872a254ca3e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="b2690-104">ムービーのモデルとテーブルに新しいフィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="b2690-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="b2690-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b2690-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="b2690-106">このチュートリアルの更新バージョンが利用可能な[ここ](../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="b2690-107">より安全な非常に簡単に従うしより多くの機能を示します。</span><span class="sxs-lookup"><span data-stu-id="b2690-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="b2690-108">このセクションでは、データベースに、変更が適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="b2690-109">既定を使用して Entity Framework Code First を自動的に、データベースの作成と同様、このチュートリアルで前に Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデルのクラスと同期されているかどうかを追跡できます。</span><span class="sxs-lookup"><span data-stu-id="b2690-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="b2690-110">同期そうでない場合、Entity Framework は、エラーをスローします。</span><span class="sxs-lookup"><span data-stu-id="b2690-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="b2690-111">これにより、それ以外の場合のみがあります (あいまいなエラー) を実行時に開発時に問題を追跡しやすくします。</span><span class="sxs-lookup"><span data-stu-id="b2690-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="b2690-112">モデルの変更の Code First Migrations を設定します。</span><span class="sxs-lookup"><span data-stu-id="b2690-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="b2690-113">Visual Studio 2012 を使用している場合をダブルクリックして、 *Movies.mdf*ソリューション エクスプ ローラーを開くには、データベース ツールからファイルです。</span><span class="sxs-lookup"><span data-stu-id="b2690-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="b2690-114">Visual Studio の Express for Web が表示されますデータベース エクスプ ローラーで、Visual Studio 2012 サーバー エクスプ ローラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b2690-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="b2690-115">Visual Studio 2010 を使用している場合は、SQL Server オブジェクト エクスプ ローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="b2690-116">ツールでは、データベース (データベース エクスプ ローラー、サーバー エクスプ ローラーまたは SQL Server オブジェクト エクスプ ローラー) を右クリックして`MovieDBContext`選択**削除**映画データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="b2690-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="b2690-117">ソリューション エクスプ ローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="b2690-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="b2690-118">右クリックして、 *Movies.mdf*ファイルおよび選択した**削除**映画データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="b2690-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="b2690-119">アプリケーションをビルドしてエラーがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b2690-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="b2690-120">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**し**Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="b2690-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![パックのマニュアルを追加します。](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="b2690-122">**Package Manager Console**ウィンドウで、`PM>`プロンプト"Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext"を入力してください。</span><span class="sxs-lookup"><span data-stu-id="b2690-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="b2690-123">**Enable-migrations**コマンド (上記) を作成、*される Configuration.cs*を新しいファイル*移行*フォルダーです。</span><span class="sxs-lookup"><span data-stu-id="b2690-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="b2690-124">Visual Studio を開き、*される Configuration.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2690-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="b2690-125">置換、`Seed`メソッドで、*される Configuration.cs*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="b2690-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="b2690-126">下の赤い波線を右クリックして`Movie`選択**を解決する**し**を使用して** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="b2690-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="b2690-127">これにより、次を追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="b2690-128">Code First Migrations 呼び出し、`Seed`メソッドすべての移行した後に (つまり、呼び出し**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在していません。</span><span class="sxs-lookup"><span data-stu-id="b2690-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="b2690-129">**プロジェクトをビルドするには CTRL-shift キーを押し-B を押します。**(場合に、次の手順は失敗、この時点で構築しない)。</span><span class="sxs-lookup"><span data-stu-id="b2690-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="b2690-130">次の手順が作成するには、`DbMigration`初期の移行のためのクラスです。</span><span class="sxs-lookup"><span data-stu-id="b2690-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="b2690-131">移行を作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でのファイルです。</span><span class="sxs-lookup"><span data-stu-id="b2690-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="b2690-132">**Package Manager Console**ウィンドウで、「追加移行初期」コマンドを入力して初期の移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="b2690-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="b2690-133">名前「初期」は任意され、作成した移行ファイルの名前に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b2690-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="b2690-134">Code First Migrations で別のクラス ファイルの作成、*移行*フォルダー (名前を持つ*{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b2690-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="b2690-135">移行ファイル名は、順序付けに役立てるために事前、タイムスタンプを持つ固定されています。</span><span class="sxs-lookup"><span data-stu-id="b2690-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="b2690-136">確認、 *{日付スタンプ}\_Initial.cs*ファイル、ムービー DB の映画のテーブルを作成する手順があります。</span><span class="sxs-lookup"><span data-stu-id="b2690-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="b2690-137">以下、この手順では、データベースを更新すると*{日付スタンプ}\_Initial.cs*ファイルを作成して、データベースのスキーマです。</span><span class="sxs-lookup"><span data-stu-id="b2690-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the the DB schema.</span></span> <span data-ttu-id="b2690-138">続いて、**シード**DB にテスト データを設定するメソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="b2690-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="b2690-139">**Package Manager Console**、コマンド"更新プログラム-データベースを入力します"、データベースを作成および実行する、**シード**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="b2690-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="b2690-140">テーブルは既に存在し、作成することはできませんを示すエラーが発生すると考えられますが、データベースを削除した後、および実行する前に、アプリケーションが実行された`update-database`です。</span><span class="sxs-lookup"><span data-stu-id="b2690-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="b2690-141">その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。</span><span class="sxs-lookup"><span data-stu-id="b2690-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="b2690-142">依然としてエラーが発生した場合に、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除である、 *Movies.mdf*ファイル、Enable-migrations に進みます)。</span><span class="sxs-lookup"><span data-stu-id="b2690-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="b2690-143">アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="b2690-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="b2690-144">シード データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="b2690-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="b2690-145">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="b2690-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="b2690-146">新しく追加することによって開始`Rating`を既存のプロパティ`Movie`クラスです。</span><span class="sxs-lookup"><span data-stu-id="b2690-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="b2690-147">開く、 *Models\Movie.cs*ファイルを追加、`Rating`次のようなプロパティ。</span><span class="sxs-lookup"><span data-stu-id="b2690-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="b2690-148">完全な`Movie`クラスの現在のような次のコード。</span><span class="sxs-lookup"><span data-stu-id="b2690-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="b2690-149">使用して、アプリケーションをビルド、**ビルド** &gt;**作成の映画**メニュー コマンドまたは ctrl キーを押し-b shift キーを押すとして</span><span class="sxs-lookup"><span data-stu-id="b2690-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="b2690-150">これで、更新した、`Model`クラスも更新する必要が、 *\Views\Movies\Index.cshtml*と*\Views\Movies\Create.cshtml*新しいを表示するためにテンプレートの表示`Rating`ブラウザー ビューのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="b2690-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="b2690-151">開く、*\Views\Movies\Index.cshtml*ファイルを追加、`<th>Rating</th>`列見出し直後、**価格**列です。</span><span class="sxs-lookup"><span data-stu-id="b2690-151">Open the*\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="b2690-152">追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。</span><span class="sxs-lookup"><span data-stu-id="b2690-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="b2690-153">以下はどのような更新*Index.cshtml*ビュー テンプレートはようになります。</span><span class="sxs-lookup"><span data-stu-id="b2690-153">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="b2690-154">次に、開く、 *\Views\Movies\Create.cshtml*ファイルおよびフォームの末尾付近の次のマークアップを追加します。</span><span class="sxs-lookup"><span data-stu-id="b2690-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="b2690-155">これにより、評価を指定するには、新しいムービーの作成時にできるように、テキスト ボックスが表示します。</span><span class="sxs-lookup"><span data-stu-id="b2690-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="b2690-156">新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティです。</span><span class="sxs-lookup"><span data-stu-id="b2690-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="b2690-157">これで、アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="b2690-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="b2690-158">これを行うときにが表示されます、次のエラーのいずれか。</span><span class="sxs-lookup"><span data-stu-id="b2690-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="b2690-159">このエラーが表示されている、更新された`Movie`アプリケーションのモデル クラスがのスキーマと異なるようになりました、`Movie`既存のデータベースのテーブルです。</span><span class="sxs-lookup"><span data-stu-id="b2690-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="b2690-160">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="b2690-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="b2690-161">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="b2690-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="b2690-162">Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="b2690-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="b2690-163">テスト データベース; 上のアクティブな開発を行うときに、この方法は非常に便利簡単にモデルとデータベースのスキーマを一緒に発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="b2690-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="b2690-164">、欠点は、データベース内の既存のデータが失われること、— ようにする*しない*を実稼働データベースでこの方法を使用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="b2690-165">初期化子を使用してデータベースにテスト データを自動的にシードするは、多くの場合、アプリケーションを開発する生産性の高い方法です。</span><span class="sxs-lookup"><span data-stu-id="b2690-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="b2690-166">Entity Framework データベースの初期化子の詳細については、Tom Dykstra を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="b2690-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="b2690-167">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="b2690-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="b2690-168">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="b2690-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="b2690-169">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="b2690-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="b2690-170">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="b2690-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="b2690-171">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="b2690-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="b2690-172">新しい列の値を提供できるようにシード メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="b2690-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="b2690-173">Migrations\Configuration.cs ファイルを開き、ムービーの各オブジェクトに格納された Rating フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="b2690-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="b2690-174">ソリューションをビルドし、開きます、**パッケージ マネージャー コンソール**ウィンドウと、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="b2690-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="b2690-175">`add-migration`コマンドを現在のムービー DB スキーマと、現在のムービー モデルを調査して、新しいモデルにデータベースを移行するために必要なコードを作成する移行フレームワークに指示します。</span><span class="sxs-lookup"><span data-stu-id="b2690-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="b2690-176">AddRatingMig は任意し、移行ファイルの名前に使用されます。</span><span class="sxs-lookup"><span data-stu-id="b2690-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="b2690-177">移行手順のわかりやすい名前を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b2690-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="b2690-178">このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラスを派生し、、`Up`方法、新しい列を作成するコードを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="b2690-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="b2690-179">ソリューションをビルドし、コマンドを入力して、「データベースの更新」で、 **Package Manager Console**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="b2690-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="b2690-180">次の図での出力、 **Package Manager Console**ウィンドウ (AddRatingMig を付加すること、日付スタンプされるが異なります)。</span><span class="sxs-lookup"><span data-stu-id="b2690-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="b2690-181">アプリケーションを再実行し、/Movies の URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="b2690-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="b2690-182">新しい評価 フィールドを表示できます。</span><span class="sxs-lookup"><span data-stu-id="b2690-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="b2690-183">をクリックして、**新規作成**リンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="b2690-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="b2690-184">評価を追加できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b2690-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="b2690-186">
              **[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b2690-186">Click **Create**.</span></span> <span data-ttu-id="b2690-187">この評価を含む、新しいムービーに表示されます、映画を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b2690-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="b2690-189">追加することも必要があります、`Rating`テンプレートの表示、編集、詳細 SearchIndex するフィールドです。</span><span class="sxs-lookup"><span data-stu-id="b2690-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="b2690-190">「データベースの更新」コマンドを入力する可能性があります、 **Package Manager Console**ウィンドウをもう一度と変更はありません行われる、スキーマが、モデルと一致するためです。</span><span class="sxs-lookup"><span data-stu-id="b2690-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="b2690-191">このセクションではモデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="b2690-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="b2690-192">また、シナリオを実行するためのサンプル データ、新しく作成されたデータベースに設定する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="b2690-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="b2690-193">次に、モデル クラスをより詳細な検証ロジックを追加し、適用するビジネス ルールの一部を有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="b2690-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b2690-194">[前へ](examining-the-edit-methods-and-edit-view.md)
[次へ](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="b2690-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
