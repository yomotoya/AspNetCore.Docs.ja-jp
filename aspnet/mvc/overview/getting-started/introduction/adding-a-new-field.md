---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 新しいフィールドの追加 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: a4eb759646fc77bbba687d8b4914465d58cd3aaa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819852"
---
<a name="adding-a-new-field"></a><span data-ttu-id="327b3-102">新しいフィールドの追加</span><span class="sxs-lookup"><span data-stu-id="327b3-102">Adding a New Field</span></span>
====================
<span data-ttu-id="327b3-103">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="327b3-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="327b3-104">このセクションでは、変更がデータベースに適用されるため、モデル クラスにいくつかの変更を移行するのに Entity Framework Code First Migrations を使用します。</span><span class="sxs-lookup"><span data-stu-id="327b3-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="327b3-105">既定を使用する Entity Framework Code First のデータベースを自動的に作成するように、このチュートリアルで前に行ったときに Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデル クラスと同期されているかどうかを追跡できます。</span><span class="sxs-lookup"><span data-stu-id="327b3-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="327b3-106">同期していない、Entity Framework は、エラーをスローします。</span><span class="sxs-lookup"><span data-stu-id="327b3-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="327b3-107">これにより、表示されている可能性がありますそれ以外の場合のみ (原因不明のエラー) を実行時に、開発時に問題を追跡しやすくします。</span><span class="sxs-lookup"><span data-stu-id="327b3-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="327b3-108">モデルの変更の Code First Migrations の設定</span><span class="sxs-lookup"><span data-stu-id="327b3-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="327b3-109">ソリューション エクスプ ローラーに移動します。</span><span class="sxs-lookup"><span data-stu-id="327b3-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="327b3-110">右クリックして、 *Movies.mdf*ファイルおよび選択**削除**ムービー データベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="327b3-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="327b3-111">表示されない場合、 *Movies.mdf*ファイルで、をクリックして、 **すべてのファイル**アイコンを赤色のアウトラインに示します。</span><span class="sxs-lookup"><span data-stu-id="327b3-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="327b3-112">エラーがないかどうかを確認するアプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="327b3-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="327b3-113">**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**し**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="327b3-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![パックのマニュアルを追加します。](adding-a-new-field/_static/image2.png)

<span data-ttu-id="327b3-115">**パッケージ マネージャー コンソール**ウィンドウで、`PM>`プロンプトを入力します</span><span class="sxs-lookup"><span data-stu-id="327b3-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="327b3-116">Enable-migrations-ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="327b3-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="327b3-117">**Enable-migrations**コマンド (上記) を作成、 *Configuration.cs*で新しいファイル*移行*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="327b3-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="327b3-118">Visual Studio を開き、 *Configuration.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="327b3-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="327b3-119">置換、`Seed`メソッドで、 *Configuration.cs*を次のコード ファイル。</span><span class="sxs-lookup"><span data-stu-id="327b3-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="327b3-120">下の赤の波線をポイント`Movie`クリック`Show Potential Fixes`順にクリックします**を使用して** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="327b3-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="327b3-121">これにより、次を追加ステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="327b3-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="327b3-122">Code First Migrations を呼び出し、`Seed`すべて移行後のメソッド (呼び出しは、**データベースを更新**パッケージ マネージャー コンソールで)、このメソッドは、既に挿入されている、または場合に、それらを挿入する行を更新し、まだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="327b3-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="327b3-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドは、次のコードでは、"upsert"操作を実行します。</span><span class="sxs-lookup"><span data-stu-id="327b3-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="327b3-124">[シード](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)メソッドは、すべての移行を実行、データベースを作成する最初の移行後に追加しようとしている行があるなる既にためだけデータを挿入できません。</span><span class="sxs-lookup"><span data-stu-id="327b3-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="327b3-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)"操作が既に存在する行を挿入しようとする場合に発生するとエラーを防ぐことが、アプリケーションのテスト中に行ったデータに対する変更を上書きします。</span><span class="sxs-lookup"><span data-stu-id="327b3-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="327b3-126">いくつかのテーブルでのテスト データたくないことが起こる: 場合によってはテスト中にデータを変更すると、変更するデータベースの更新後に残します。</span><span class="sxs-lookup"><span data-stu-id="327b3-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="327b3-127">条件付きの insert 操作を実行する場合: 存在しない場合にのみ行を挿入します。</span><span class="sxs-lookup"><span data-stu-id="327b3-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="327b3-128">渡される最初のパラメーター、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを使用して、行が既に存在するかを確認するプロパティを指定します。</span><span class="sxs-lookup"><span data-stu-id="327b3-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="327b3-129">テスト ビデオ データを提供するには`Title`プロパティは、リスト内の各タイトルは一意であるために、この目的に使用できます。</span><span class="sxs-lookup"><span data-stu-id="327b3-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="327b3-130">このコードでは、タイトルが一意であることを前提としています。</span><span class="sxs-lookup"><span data-stu-id="327b3-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="327b3-131">タイトルの重複を手動で追加する場合、次回の移行を実行する次の例外が表示されます。</span><span class="sxs-lookup"><span data-stu-id="327b3-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="327b3-132">*シーケンスには、1 つ以上の要素が含まれています。*</span><span class="sxs-lookup"><span data-stu-id="327b3-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="327b3-133">詳細については、 [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)メソッドを参照してください[EF 4.3 AddOrUpdate メソッドを使用して対処](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).</span><span class="sxs-lookup"><span data-stu-id="327b3-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="327b3-134">**CTRL + SHIFT+B プロジェクトをビルドするキーを押します。**(次の手順は、この時点で構築しない場合失敗します)。</span><span class="sxs-lookup"><span data-stu-id="327b3-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="327b3-135">次の手順が作成するには、`DbMigration`初回移行のためのクラス。</span><span class="sxs-lookup"><span data-stu-id="327b3-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="327b3-136">この移行が作成することは、新しいデータベースを削除する、 *movie.mdf*前の手順でファイル。</span><span class="sxs-lookup"><span data-stu-id="327b3-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="327b3-137">**パッケージ マネージャー コンソール**ウィンドウ、コマンドを入力して`add-migration Initial`初期移行を作成します。</span><span class="sxs-lookup"><span data-stu-id="327b3-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="327b3-138">「初期」という名前は任意なので名前を移行ファイルを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="327b3-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="327b3-139">Code First Migrations は、別のクラス ファイルを作成、*移行*フォルダー (名前を持つ *{日付スタンプ}\_Initial.cs* )、このクラスには、データベース スキーマを作成するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="327b3-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="327b3-140">移行ファイル名は事前のタイムスタンプを持つ順序付けに関するヘルプを修正しました。</span><span class="sxs-lookup"><span data-stu-id="327b3-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="327b3-141">確認、 *{日付スタンプ}\_Initial.cs*ファイルを作成する手順がある、 `Movies` Movie DB のテーブル。</span><span class="sxs-lookup"><span data-stu-id="327b3-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="327b3-142">以下、この手順では、データベースを更新すると *{日付スタンプ}\_Initial.cs*ファイルは実行され、DB のスキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="327b3-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="327b3-143">次に、**シード**DB にテスト データを設定するメソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="327b3-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="327b3-144">**パッケージ マネージャー コンソール**、コマンドを入力して`update-database`、データベースを作成し、実行、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="327b3-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="327b3-145">テーブルが既に存在し、作成することはできませんを示すエラーが発生する場合は可能性があります、データベースを削除した後、および実行する前にアプリケーションを実行するため、`update-database`します。</span><span class="sxs-lookup"><span data-stu-id="327b3-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="327b3-146">その場合は、削除、 *Movies.mdf*ファイルをもう一度やり直して、`update-database`コマンド。</span><span class="sxs-lookup"><span data-stu-id="327b3-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="327b3-147">依然としてエラーが発生した場合、migrations フォルダーと内容を削除し、このページの上部にある手順を開始 (削除は、 *Movies.mdf*ファイルを Enable-migrations に進みます)。</span><span class="sxs-lookup"><span data-stu-id="327b3-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="327b3-148">それでもエラーが発生する場合は、SQL Server オブジェクト エクスプ ローラーを開き、一覧からデータベースを削除します。</span><span class="sxs-lookup"><span data-stu-id="327b3-148">If you still get an eror, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="327b3-149">アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="327b3-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="327b3-150">シード データが表示されます。</span><span class="sxs-lookup"><span data-stu-id="327b3-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="327b3-151">ムービー モデルへの評価プロパティの追加</span><span class="sxs-lookup"><span data-stu-id="327b3-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="327b3-152">まず、新しい追加`Rating`プロパティを既存の`Movie`クラス。</span><span class="sxs-lookup"><span data-stu-id="327b3-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="327b3-153">開く、 *Models\Movie.cs*追加ファイルを開き、`Rating`次のようなプロパティ。</span><span class="sxs-lookup"><span data-stu-id="327b3-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="327b3-154">完全な`Movie`今のような次のコードをクラスします。</span><span class="sxs-lookup"><span data-stu-id="327b3-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="327b3-155">(Ctrl + Shift + B) アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="327b3-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="327b3-156">新しいフィールドを追加したので、`Movie`クラスもする必要があるバインドを更新*ホワイト リスト*この新しいプロパティが含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="327b3-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="327b3-157">更新プログラム、`bind`属性`Create`と`Edit`アクション メソッドに含める、`Rating`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="327b3-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="327b3-158">ブラウザー ビューで新しい `Rating` プロパティを表示、作成、編集する目的でビュー テンプレートを更新する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="327b3-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="327b3-159">開く、 *\Views\Movies\Index.cshtml*追加ファイルを開き、`<th>Rating</th>`直後の列ヘッダー、**価格**列。</span><span class="sxs-lookup"><span data-stu-id="327b3-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="327b3-160">追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。</span><span class="sxs-lookup"><span data-stu-id="327b3-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="327b3-161">以下はどのような更新*Index.cshtml*ビュー テンプレートのようになります。</span><span class="sxs-lookup"><span data-stu-id="327b3-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="327b3-162">次に、開く、 *\Views\Movies\Create.cshtml*追加ファイルを開き、`Rating`フィールドには、次のハイライト マークアップ。</span><span class="sxs-lookup"><span data-stu-id="327b3-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="327b3-163">これにより、新しいムービーの作成時に、評価を指定できるように、テキスト ボックスを表示します。</span><span class="sxs-lookup"><span data-stu-id="327b3-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="327b3-164">新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="327b3-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="327b3-165">アプリケーションを実行しに移動し、 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="327b3-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="327b3-166">これを行うときに、表示されます、次のエラーのいずれか。</span><span class="sxs-lookup"><span data-stu-id="327b3-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="327b3-167">データベースが作成されたために、'MovieDBContext' コンテキストのバックアップ モデルが変更されました。</span><span class="sxs-lookup"><span data-stu-id="327b3-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="327b3-168">データベースを更新する Code First Migrations を使用する (https://go.microsoft.com/fwlink/?LinkId=238269)します。</span><span class="sxs-lookup"><span data-stu-id="327b3-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="327b3-169">ため、このエラーが表示されている更新された`Movie`アプリケーションでモデル クラスは、スキーマの異なる、`Movie`既存のデータベースのテーブル。</span><span class="sxs-lookup"><span data-stu-id="327b3-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="327b3-170">(データベース テーブルに `Rating` 列はありません)。</span><span class="sxs-lookup"><span data-stu-id="327b3-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="327b3-171">このエラーを解決するための手法がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="327b3-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="327b3-172">Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。</span><span class="sxs-lookup"><span data-stu-id="327b3-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="327b3-173">この手法は、開発周期の早い段階で、テスト データベースで開発しているときに非常に便利です。モデルとデータベース スキーマを一緒に短期間で発展させることができます。</span><span class="sxs-lookup"><span data-stu-id="327b3-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="327b3-174">短所は、データベース内の既存のデータが失われる-ようにする*しない*実稼働データベースでこのアプローチを使用する!</span><span class="sxs-lookup"><span data-stu-id="327b3-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="327b3-175">初期化子を利用し、データベースにテスト データを自動的に初期投入します。多くの場合、アプリケーション開発の手法として有益な方法です。</span><span class="sxs-lookup"><span data-stu-id="327b3-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="327b3-176">Entity Framework データベースの初期化子の詳細については、次を参照してください。 [ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="327b3-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="327b3-177">モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。</span><span class="sxs-lookup"><span data-stu-id="327b3-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="327b3-178">この手法の長所は、データが維持されることです。</span><span class="sxs-lookup"><span data-stu-id="327b3-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="327b3-179">この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="327b3-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="327b3-180">Code First Migrations を使用して、データベース スキーマを更新します。</span><span class="sxs-lookup"><span data-stu-id="327b3-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="327b3-181">このチュートリアルでは、Code First Migrations を利用します。</span><span class="sxs-lookup"><span data-stu-id="327b3-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="327b3-182">新しい列の値を提供するように、Seed メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="327b3-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="327b3-183">Migrations \configuration.cs ファイルを開き、ムービーの各オブジェクトに Rating フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="327b3-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="327b3-184">ソリューションをビルドし、開き、**パッケージ マネージャー コンソール**ウィンドウし、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="327b3-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="327b3-185">`add-migration`コマンドは現在のムービー DB スキーマを使用して現在のムービー モデルを調べるし、DB を新しいモデルに移行するために必要なコードを作成するために移行フレームワークに指示します。</span><span class="sxs-lookup"><span data-stu-id="327b3-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="327b3-186">名前*評価*任意であり、移行ファイルの名前に使用されます。</span><span class="sxs-lookup"><span data-stu-id="327b3-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="327b3-187">移行手順のわかりやすい名前を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="327b3-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="327b3-188">このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMigration`クラスを派生し、`Up`メソッド、新しい列を作成するコードを表示できます。</span><span class="sxs-lookup"><span data-stu-id="327b3-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="327b3-189">ソリューションをビルドし、入力、`update-database`コマンド、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="327b3-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="327b3-190">次の図は、出力で、**パッケージ マネージャー コンソール**ウィンドウ (日付スタンプ プリペンド*評価*異なるものになります)。</span><span class="sxs-lookup"><span data-stu-id="327b3-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="327b3-191">アプリケーションを再実行し、/Movies URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="327b3-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="327b3-192">新しい Rating フィールドを確認できます。</span><span class="sxs-lookup"><span data-stu-id="327b3-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="327b3-193">をクリックして、**新規作成**のリンクを新しいムービーを追加します。</span><span class="sxs-lookup"><span data-stu-id="327b3-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="327b3-194">評価を追加できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="327b3-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="327b3-196">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="327b3-196">Click **Create**.</span></span> <span data-ttu-id="327b3-197">評価を含む、新しいムービーに表示されるビデオを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="327b3-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="327b3-199">これで、プロジェクトは、移行を使用して、新しいフィールドを追加またはそれ以外の場合、スキーマを更新するときに、データベースを削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="327b3-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="327b3-200">次のセクションではスキーマの変更がお migrations を使用してデータベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="327b3-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="327b3-201">追加することも必要があります、`Rating`フィールドを編集、詳細、および Delete ビュー テンプレートにします。</span><span class="sxs-lookup"><span data-stu-id="327b3-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="327b3-202">「データベースの更新」のコマンドを入力する可能性があります、**パッケージ マネージャー コンソール**ウィンドウをもう一度と移行コードが実行、スキーマには、モデルが一致するためです。</span><span class="sxs-lookup"><span data-stu-id="327b3-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="327b3-203">ただし、「データベースの更新」を実行しているが実行されます、`Seed`メソッドを再度とシード データを変更した場合、変更は失われますので、`Seed`メソッド アップサート データ。</span><span class="sxs-lookup"><span data-stu-id="327b3-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="327b3-204">詳細をご覧ください、`Seed`メソッド Tom Dykstra の人気のある[ASP.NET MVC/Entity Framework チュートリアル](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="327b3-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="327b3-205">このセクションでは、モデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="327b3-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="327b3-206">シナリオを試すことができますので、サンプル データを使って新しく作成したデータベースを設定する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="327b3-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="327b3-207">これは、Code First に概要を参照してください[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)サブジェクトに関するより詳細なチュートリアルについてはします。</span><span class="sxs-lookup"><span data-stu-id="327b3-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="327b3-208">次に、高度な検証ロジックはモデル クラスを追加し、いくつかのビジネス ルールを適用するを有効にする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="327b3-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="327b3-209">[前へ](adding-search.md)
> [次へ](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="327b3-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
