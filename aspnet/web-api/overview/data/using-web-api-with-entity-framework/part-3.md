---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Code First Migrations を使用して、データベースのシードを |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 733b1c343d774e5fa8757808be07a9ae67481d84
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910500"
---
<a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="7452c-102">Code First Migrations を使用して、データベースのシード</span><span class="sxs-lookup"><span data-stu-id="7452c-102">Use Code First Migrations to Seed the Database</span></span>
====================
<span data-ttu-id="7452c-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7452c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7452c-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="7452c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7452c-105">このセクションでは使用して[Code First Migrations](https://msdn.microsoft.com/data/jj591621)テスト データでデータベースをシードする EF でします。</span><span class="sxs-lookup"><span data-stu-id="7452c-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="7452c-106">**ツール**メニューの  **NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="7452c-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7452c-107">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="7452c-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="7452c-108">このコマンドは、プロジェクトに移行をという名前のフォルダーとという名前の Migrations フォルダーに Configuration.cs コード ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="7452c-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="7452c-109">Configuration.cs ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="7452c-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="7452c-110">次の追加**を使用して**ステートメント。</span><span class="sxs-lookup"><span data-stu-id="7452c-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="7452c-111">次のコードを追加し、 **Configuration.Seed**メソッド。</span><span class="sxs-lookup"><span data-stu-id="7452c-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="7452c-112">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="7452c-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="7452c-113">最初のコマンドは、データベースを作成するコードを生成し、2 番目のコマンドは、そのコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="7452c-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="7452c-114">データベースがローカルで作成されるを使用して[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7452c-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="7452c-115">(省略可能) の API を確認します。</span><span class="sxs-lookup"><span data-stu-id="7452c-115">Explore the API (Optional)</span></span>

<span data-ttu-id="7452c-116">F5 キーを押して、デバッグ モードでアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="7452c-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="7452c-117">Visual Studio では、IIS Express を起動し、web アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="7452c-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="7452c-118">Visual Studio は、ブラウザーを起動し、アプリのホーム ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="7452c-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="7452c-119">Visual Studio で web プロジェクトを実行すると、ポート番号が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="7452c-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="7452c-120">次の図では、ポート番号は、50524 です。</span><span class="sxs-lookup"><span data-stu-id="7452c-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="7452c-121">アプリケーションを実行するときに、別のポート番号を確認します。</span><span class="sxs-lookup"><span data-stu-id="7452c-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="7452c-122">ホーム ページは、ASP.NET MVC を使用して実装されます。</span><span class="sxs-lookup"><span data-stu-id="7452c-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="7452c-123">ページの上部にある"API"というリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="7452c-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="7452c-124">このリンクを自動生成されたヘルプ ページに web API に表示されます。</span><span class="sxs-lookup"><span data-stu-id="7452c-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="7452c-125">(このヘルプ ページを生成する方法と、ページを独自のドキュメントを追加する方法については、次を参照してください[ASP.NET Web API のヘルプ ページを作成する](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)。)。リンクをクリックして ヘルプ ページを要求と応答の形式を含む、API の詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="7452c-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="7452c-126">この API は、データベースに対する CRUD 操作を使用します。</span><span class="sxs-lookup"><span data-stu-id="7452c-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="7452c-127">次に、API の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="7452c-127">The following summarizes the API.</span></span>

| <span data-ttu-id="7452c-128">Authors</span><span class="sxs-lookup"><span data-stu-id="7452c-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="7452c-129">Api/作成者します。</span><span class="sxs-lookup"><span data-stu-id="7452c-129">GET api/authors</span></span> | <span data-ttu-id="7452c-130">すべての著者を取得します。</span><span class="sxs-lookup"><span data-stu-id="7452c-130">Get all authors.</span></span> |
| <span data-ttu-id="7452c-131">GET api の作成者/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-131">GET api/authors/{id}</span></span> | <span data-ttu-id="7452c-132">ID で作成者を取得します。</span><span class="sxs-lookup"><span data-stu-id="7452c-132">Get an author by ID.</span></span> |
| <span data-ttu-id="7452c-133">投稿の作成者/api</span><span class="sxs-lookup"><span data-stu-id="7452c-133">POST /api/authors</span></span> | <span data-ttu-id="7452c-134">新しい作成者を作成します。</span><span class="sxs-lookup"><span data-stu-id="7452c-134">Create a new author.</span></span> |
| <span data-ttu-id="7452c-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="7452c-136">既存の作成者を更新します。</span><span class="sxs-lookup"><span data-stu-id="7452c-136">Update an existing author.</span></span> |
| <span data-ttu-id="7452c-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="7452c-138">作成者を削除します。</span><span class="sxs-lookup"><span data-stu-id="7452c-138">Delete an author.</span></span> |

| <span data-ttu-id="7452c-139">ブック</span><span class="sxs-lookup"><span data-stu-id="7452c-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="7452c-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="7452c-140">GET /api/books</span></span> | <span data-ttu-id="7452c-141">すべてのブックを取得します。</span><span class="sxs-lookup"><span data-stu-id="7452c-141">Get all books.</span></span> |
| <span data-ttu-id="7452c-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-142">GET /api/books/{id}</span></span> | <span data-ttu-id="7452c-143">ID での書籍を入手します。</span><span class="sxs-lookup"><span data-stu-id="7452c-143">Get a book by ID.</span></span> |
| <span data-ttu-id="7452c-144">Api/ブックを投稿します。</span><span class="sxs-lookup"><span data-stu-id="7452c-144">POST /api/books</span></span> | <span data-ttu-id="7452c-145">新しいブックを作成します。</span><span class="sxs-lookup"><span data-stu-id="7452c-145">Create a new book.</span></span> |
| <span data-ttu-id="7452c-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="7452c-147">既存のブックを更新します。</span><span class="sxs-lookup"><span data-stu-id="7452c-147">Update an existing book.</span></span> |
| <span data-ttu-id="7452c-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="7452c-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="7452c-149">ブックを削除します。</span><span class="sxs-lookup"><span data-stu-id="7452c-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="7452c-150">(省略可能) データベースを表示します。</span><span class="sxs-lookup"><span data-stu-id="7452c-150">View the Database (Optional)</span></span>

<span data-ttu-id="7452c-151">EF がデータベースを作成しと呼ばれる、Update-database コマンドを実行したときに、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="7452c-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="7452c-152">EF を使用して、アプリケーションをローカルで実行するときに[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="7452c-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="7452c-153">Visual Studio でデータベースを表示することができます。</span><span class="sxs-lookup"><span data-stu-id="7452c-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="7452c-154">**ビュー**メニューの  **SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="7452c-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="7452c-155">**サーバーへの接続**ダイアログで、**サーバー名**編集ボックスに、"(localdb) \v11.0"を入力します。</span><span class="sxs-lookup"><span data-stu-id="7452c-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="7452c-156">ままに、**認証**として [Windows 認証] オプション。</span><span class="sxs-lookup"><span data-stu-id="7452c-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="7452c-157">**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7452c-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="7452c-158">Visual Studio では、LocalDB に接続し、SQL Server オブジェクト エクスプ ローラー ウィンドウで、既存のデータベースを示しています。</span><span class="sxs-lookup"><span data-stu-id="7452c-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="7452c-159">EF を作成したテーブルを表示するノードを展開することができます。</span><span class="sxs-lookup"><span data-stu-id="7452c-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="7452c-160">データを表示するには、テーブルを右クリックして**ビュー データ**します。</span><span class="sxs-lookup"><span data-stu-id="7452c-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="7452c-161">次のスクリーン ショットは、ブックの「テーブルの結果を示しています。</span><span class="sxs-lookup"><span data-stu-id="7452c-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="7452c-162">EF がデータをシードとの表に、使用してデータベースを設定することに注意してくださいには、Authors テーブルへの外部キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="7452c-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="7452c-163">[前へ](part-2.md)
> [次へ](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="7452c-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
