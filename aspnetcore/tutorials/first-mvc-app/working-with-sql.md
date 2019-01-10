---
title: ASP.NET Core MVC アプリでの SQL の操作
author: rick-anderson
description: ASP.NET Core MVC アプリ内で SQL Server LocalDB または SQLite を使用する方法について説明します。
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 3757b972694a41cb87beb8ebee818cd498be6764
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382031"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="279f1-103">ASP.NET Core での SQL の操作</span><span class="sxs-lookup"><span data-stu-id="279f1-103">Work with SQL in ASP.NET Core</span></span>

<span data-ttu-id="279f1-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="279f1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="279f1-105">`MvcMovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="279f1-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="279f1-106">データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="279f1-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="279f1-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="279f1-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="279f1-108">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="279f1-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="279f1-109">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="279f1-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="279f1-110">Visual Studio Code/Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="279f1-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="279f1-111">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="279f1-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="279f1-112">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="279f1-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="279f1-113">テストまたは実稼働サーバーにアプリを配置する場合は、環境変数または別の方法を使用して、実際の SQL Server に接続文字列を設定できます。</span><span class="sxs-lookup"><span data-stu-id="279f1-113">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="279f1-114">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="279f1-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="279f1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="279f1-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="279f1-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="279f1-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="279f1-117">LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。</span><span class="sxs-lookup"><span data-stu-id="279f1-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="279f1-118">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="279f1-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="279f1-119">既定では、LocalDB データベースによって *C:/Users/{user}* ディレクトリに *.mdf* ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="279f1-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="279f1-120">**[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="279f1-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![[View] メニュー](working-with-sql/_static/ssox.png)

* <span data-ttu-id="279f1-122">`Movie` テーブルを右クリックして **[デザイナーの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="279f1-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](working-with-sql/_static/dv.png)

<span data-ttu-id="279f1-125">`ID` の横のキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="279f1-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="279f1-126">既定では、EF は `ID` という名前のプロパティを主キーにします。</span><span class="sxs-lookup"><span data-stu-id="279f1-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="279f1-127">`Movie` テーブルを右クリックして **[データの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="279f1-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/ssox2.png)

  ![開いた Movie テーブルにテーブル データが表示されています](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="279f1-130">Visual Studio Code/Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="279f1-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="279f1-131">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="279f1-131">Seed the database</span></span>

<span data-ttu-id="279f1-132">*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="279f1-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="279f1-133">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="279f1-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="279f1-134">DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。</span><span class="sxs-lookup"><span data-stu-id="279f1-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="279f1-135">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="279f1-135">Add the seed initializer</span></span>

<span data-ttu-id="279f1-136">*Program.cs* の内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="279f1-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="279f1-137">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="279f1-137">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="279f1-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="279f1-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="279f1-139">DB 内のすべてのレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="279f1-139">Delete all the records in the DB.</span></span> <span data-ttu-id="279f1-140">これはブラウザーの削除リンクで行うか、SSOX から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="279f1-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="279f1-141">アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="279f1-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="279f1-142">強制的に初期化するには、IIS Express を停止してから再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="279f1-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="279f1-143">これは次の方法のいずれかを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="279f1-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="279f1-144">通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="279f1-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express システム トレイ アイコン](working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="279f1-147">非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="279f1-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="279f1-148">デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="279f1-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="279f1-149">Visual Studio Code/Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="279f1-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="279f1-150">DB 内のすべてのレコードを削除します (そのため Seed メソッドが実行されます)。</span><span class="sxs-lookup"><span data-stu-id="279f1-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="279f1-151">アプリを停止および起動して、データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="279f1-151">Stop and start the app to seed the database.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="279f1-152">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="279f1-152">The app shows the seeded data.</span></span>

![ムービー データが表示された、Microsoft Edge で開かれている MVC ムービー アプリケーション](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="279f1-154">[前へ](adding-model.md)
> [次へ](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="279f1-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
