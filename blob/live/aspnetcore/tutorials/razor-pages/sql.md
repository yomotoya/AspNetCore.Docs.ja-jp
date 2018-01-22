---
title: "SQL Server LocalDB と ASP.NET Core の使用"
author: rick-anderson
description: "SQL Server LocalDB と ASP.NET Core の使用について説明します。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 90aa194eda1c52afb1f299a0b95c7040e32a02fc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="aab71-103">SQL Server LocalDB と ASP.NET Core の使用</span><span class="sxs-lookup"><span data-stu-id="aab71-103">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="aab71-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="aab71-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="aab71-105">`MovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="aab71-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="aab71-106">データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="aab71-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="aab71-107">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="aab71-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="aab71-108">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="aab71-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="aab71-109">テストまたは実稼働サーバーにアプリを配置する場合は、環境変数または別の方法を使用して、実際の SQL Server に接続文字列を設定できます。</span><span class="sxs-lookup"><span data-stu-id="aab71-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="aab71-110">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="aab71-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="aab71-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="aab71-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="aab71-112">LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。</span><span class="sxs-lookup"><span data-stu-id="aab71-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="aab71-113">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="aab71-113">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="aab71-114">既定では、LocalDB データベースは *C:/Users/\<user\>* ディレクトリに "\*.mdf" ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="aab71-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="aab71-115">**[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="aab71-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![[View] メニュー](sql/_static/ssox.png)

* <span data-ttu-id="aab71-117">`Movie` テーブルを右クリックし、**[デザイナーの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="aab71-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](sql/_static/dv.png)

<span data-ttu-id="aab71-120">`ID` の横のキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="aab71-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="aab71-121">既定では、EF で主キーに `ID` という名前のプロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="aab71-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="aab71-122">`Movie` テーブルを右クリックし、**[データの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="aab71-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![開いた Movie テーブルにテーブル データが表示されています](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="aab71-124">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="aab71-124">Seed the database</span></span>

<span data-ttu-id="aab71-125">*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="aab71-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="aab71-126">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="aab71-126">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="aab71-127">DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。</span><span class="sxs-lookup"><span data-stu-id="aab71-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="aab71-128">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="aab71-128">Add the seed initializer</span></span>

<span data-ttu-id="aab71-129">次のように、*Program.cs* ファイルで `Main` メソッドの末尾にシード初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="aab71-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="aab71-130">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="aab71-130">Test the app</span></span>

* <span data-ttu-id="aab71-131">DB 内のすべてのレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="aab71-131">Delete all the records in the DB.</span></span> <span data-ttu-id="aab71-132">これはブラウザーの削除リンクで行うか、[SSOX](xref:tutorials/razor-pages/new-field#ssox) から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="aab71-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="aab71-133">アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="aab71-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="aab71-134">強制的に初期化するには、IIS Express を停止してから再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="aab71-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="aab71-135">これは次の方法のいずれかを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="aab71-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="aab71-136">通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="aab71-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express システム トレイ アイコン](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](sql/_static/stopIIS.png)

   * <span data-ttu-id="aab71-139">非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="aab71-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="aab71-140">デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="aab71-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="aab71-141">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="aab71-141">The app shows the seeded data:</span></span>

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

<span data-ttu-id="aab71-143">次のチュートリアルでは、データの表示をクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="aab71-143">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="aab71-144">[前: スキャフォールディングされた Razor ページ](xref:tutorials/razor-pages/page)
[次: ページの更新](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="aab71-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
