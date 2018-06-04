---
title: ASP.NET Core での SQL Server LocalDB の使用
author: rick-anderson
description: 単純な ASP.NET Core MVC アプリで SQL Server LocalDB を使用する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: b00d709f3009f63431becf24797269ad5988a20b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688375"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="a5b41-103">ASP.NET Core での SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="a5b41-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="a5b41-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a5b41-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a5b41-105">`MvcMovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="a5b41-106">データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a5b41-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="a5b41-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="a5b41-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="a5b41-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

<span data-ttu-id="a5b41-109">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="a5b41-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="a5b41-110">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="a5b41-111">テストまたは実稼働サーバーにアプリを配置する場合は、環境変数または別の方法を使用して、実際の SQL Server に接続文字列を設定できます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-111">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="a5b41-112">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a5b41-112">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="a5b41-113">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="a5b41-113">SQL Server Express LocalDB</span></span>

<span data-ttu-id="a5b41-114">LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。</span><span class="sxs-lookup"><span data-stu-id="a5b41-114">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="a5b41-115">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="a5b41-115">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="a5b41-116">既定では、LocalDB データベースは *C:/Users/\<user\>* ディレクトリに "\*.mdf" ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-116">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="a5b41-117">**[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-117">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![[View] メニュー](working-with-sql/_static/ssox.png)

* <span data-ttu-id="a5b41-119">`Movie` テーブルを右クリックして **[デザイナーの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-119">Right click on the `Movie` table **> View Designer**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](working-with-sql/_static/dv.png)

<span data-ttu-id="a5b41-122">`ID` の横のキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a5b41-122">Note the key icon next to `ID`.</span></span> <span data-ttu-id="a5b41-123">既定では、EF は `ID` という名前のプロパティを主キーにします。</span><span class="sxs-lookup"><span data-stu-id="a5b41-123">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="a5b41-124">`Movie` テーブルを右クリックして **[データの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-124">Right click on the `Movie` table **> View Data**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/ssox2.png)

  ![開いた Movie テーブルにテーブル データが表示されています](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="a5b41-127">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="a5b41-127">Seed the database</span></span>

<span data-ttu-id="a5b41-128">*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="a5b41-129">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-129">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="a5b41-130">DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。</span><span class="sxs-lookup"><span data-stu-id="a5b41-130">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="a5b41-131">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="a5b41-131">Add the seed initializer</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="a5b41-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="a5b41-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a5b41-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a5b41-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a5b41-134">次のように、*Program.cs* ファイルで `Main` メソッドにシード初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-134">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="a5b41-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span><span class="sxs-lookup"><span data-stu-id="a5b41-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a5b41-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a5b41-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a5b41-137">次のように、*Startup.cs* ファイルで `Configure` メソッドの末尾にシード初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-137">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

<span data-ttu-id="a5b41-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span><span class="sxs-lookup"><span data-stu-id="a5b41-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span></span>

---
::: moniker-end

<span data-ttu-id="a5b41-139">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="a5b41-139">Test the app</span></span>

* <span data-ttu-id="a5b41-140">DB 内のすべてのレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-140">Delete all the records in the DB.</span></span> <span data-ttu-id="a5b41-141">これはブラウザーの削除リンクで行うか、SSOX から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-141">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="a5b41-142">アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="a5b41-142">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="a5b41-143">強制的に初期化するには、IIS Express を停止してから再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a5b41-143">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="a5b41-144">これは次の方法のいずれかを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-144">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="a5b41-145">通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="a5b41-145">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express システム トレイ アイコン](working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="a5b41-148">非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-148">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="a5b41-149">デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a5b41-149">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="a5b41-150">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5b41-150">The app shows the seeded data.</span></span>

![ムービー データが表示された、Microsoft Edge で開かれている MVC ムービー アプリケーション](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="a5b41-152">[前へ](adding-model.md)
> [次へ](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="a5b41-152">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
