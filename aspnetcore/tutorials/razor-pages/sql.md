---
title: データベースと ASP.NET Core を使用する
author: rick-anderson
description: データベースと ASP.NET Core の使用について説明します。
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 3e05f5dbc73c35f1f938346b2eaab8c0fa7d8ab9
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410298"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="0001d-103">データベースと ASP.NET Core を使用する</span><span class="sxs-lookup"><span data-stu-id="0001d-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="0001d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) および [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="0001d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="0001d-105">`RazorPagesMovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="0001d-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="0001d-106">データベース コンテキストは、*Startup.cs* の `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0001d-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0001d-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0001d-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0001d-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0001d-109">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0001d-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="0001d-110">`ConfigureServices` で使用されているメソッドの詳細については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0001d-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="0001d-111">[ASP.NET Core での `CookiePolicyOptions` 用の EU の一般データ保護規制 (GDPR) のサポート](xref:security/gdpr)</span><span class="sxs-lookup"><span data-stu-id="0001d-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="0001d-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="0001d-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="0001d-113">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="0001d-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="0001d-114">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="0001d-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0001d-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0001d-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0001d-116">データベースの名前の値は (`Database={Database name}`) ユーザーが生成したコードでは異なります。</span><span class="sxs-lookup"><span data-stu-id="0001d-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="0001d-117">名前の値は任意です。</span><span class="sxs-lookup"><span data-stu-id="0001d-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0001d-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0001d-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0001d-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0001d-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="0001d-120">アプリがテスト サーバーまたは運用サーバーにデプロイされると、環境変数を使用して接続文字列を実際のデータベース サーバーに設定できます。</span><span class="sxs-lookup"><span data-stu-id="0001d-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="0001d-121">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="0001d-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0001d-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0001d-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="0001d-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="0001d-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="0001d-124">LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。</span><span class="sxs-lookup"><span data-stu-id="0001d-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="0001d-125">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="0001d-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="0001d-126">LocalDB データベースでは、既定で `C:/Users/<user/>` ディレクトリに `*.mdf` ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="0001d-127">**[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="0001d-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![[View] メニュー](sql/_static/ssox.png)

* <span data-ttu-id="0001d-129">`Movie` テーブルを右クリックし、**[デザイナーの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0001d-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](sql/_static/dv.png)

<span data-ttu-id="0001d-132">`ID` の横のキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0001d-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="0001d-133">既定では、EF で主キーに `ID` という名前のプロパティが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="0001d-134">`Movie` テーブルを右クリックし、**[データの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0001d-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="0001d-135">![開いた Movie テーブルにテーブル データが表示されています](sql/_static/vd22.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="0001d-135">![Movie table open showing table data](sql/_static/vd22.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0001d-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0001d-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0001d-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0001d-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="0001d-138">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="0001d-138">Seed the database</span></span>

<span data-ttu-id="0001d-139">次のコードを使用して、*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="0001d-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="0001d-140">DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。</span><span class="sxs-lookup"><span data-stu-id="0001d-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="0001d-141">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="0001d-141">Add the seed initializer</span></span>

<span data-ttu-id="0001d-142">*Program.cs* で、次を実行するように `Main` メソッドを変更します。</span><span class="sxs-lookup"><span data-stu-id="0001d-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="0001d-143">依存関係挿入コンテナーから DB コンテキスト インスタンスを取得します。</span><span class="sxs-lookup"><span data-stu-id="0001d-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="0001d-144">seed メソッドを呼び出し、コンテキストを渡します。</span><span class="sxs-lookup"><span data-stu-id="0001d-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="0001d-145">seed メソッドが完了したら、コンテキストを破棄します。</span><span class="sxs-lookup"><span data-stu-id="0001d-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="0001d-146">次は、更新された *Program.cs* ファイルのコードです。</span><span class="sxs-lookup"><span data-stu-id="0001d-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="0001d-147">運用アプリは `Database.Migrate` を呼び出しません。</span><span class="sxs-lookup"><span data-stu-id="0001d-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="0001d-148">これは、`Update-Database` が実行されていないとき、前述のコードに追加され、次の例外を阻止します。</span><span class="sxs-lookup"><span data-stu-id="0001d-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="0001d-149">SqlException:Cannot open database "RazorPagesMovieContext-21" requested by the login. (SqlException: ログインで要求されている "RazorPagesMovieContext-21" データベースを開くことができませんでした。)</span><span class="sxs-lookup"><span data-stu-id="0001d-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="0001d-150">The login failed.\(ログインに失敗しました。\)</span><span class="sxs-lookup"><span data-stu-id="0001d-150">The login failed.</span></span>
<span data-ttu-id="0001d-151">Login failed for user 'user name'.\(ユーザー 'ユーザー名' はログインできませんでした。\)</span><span class="sxs-lookup"><span data-stu-id="0001d-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="0001d-152">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="0001d-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0001d-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0001d-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0001d-154">DB 内のすべてのレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="0001d-154">Delete all the records in the DB.</span></span> <span data-ttu-id="0001d-155">これはブラウザーの削除リンクで行うか、[SSOX](xref:tutorials/razor-pages/new-field#ssox) から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0001d-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="0001d-156">アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="0001d-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="0001d-157">強制的に初期化するには、IIS Express を停止してから再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0001d-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="0001d-158">これは次の方法のいずれかを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0001d-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="0001d-159">通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="0001d-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![IIS Express システム トレイ アイコン](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](sql/_static/stopIIS.png)

    * <span data-ttu-id="0001d-162">非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="0001d-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="0001d-163">デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="0001d-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0001d-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0001d-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="0001d-165">DB 内のすべてのレコードを削除します (そのため Seed メソッドが実行されます)。</span><span class="sxs-lookup"><span data-stu-id="0001d-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="0001d-166">アプリを停止および起動して、データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="0001d-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="0001d-167">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0001d-168">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="0001d-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0001d-169">DB 内のすべてのレコードを削除します (そのため Seed メソッドが実行されます)。</span><span class="sxs-lookup"><span data-stu-id="0001d-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="0001d-170">アプリを停止および起動して、データベースをシードします。</span><span class="sxs-lookup"><span data-stu-id="0001d-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="0001d-171">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="0001d-172">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0001d-172">The app shows the seeded data:</span></span>

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

<span data-ttu-id="0001d-174">次のチュートリアルでは、データの表示をクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="0001d-174">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0001d-175">[前へ:スキャフォールディングされた Razor Pages](xref:tutorials/razor-pages/page)
> [次:ページの更新](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="0001d-175">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
