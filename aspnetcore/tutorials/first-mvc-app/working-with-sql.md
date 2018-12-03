---
title: ASP.NET Core MVC アプリでの SQL Server LocalDB の操作
author: rick-anderson
description: 単純な ASP.NET Core MVC アプリで SQL Server LocalDB を使用する方法について説明します。
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 49615c25d51cfa671157c2e56b8e0753719c678a
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710102"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="2dd4c-103">ASP.NET Core での SQL Server LocalDB の使用</span><span class="sxs-lookup"><span data-stu-id="2dd4c-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="2dd4c-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2dd4c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2dd4c-105">`MvcMovieContext` オブジェクトは、データベースへの接続と、データベース レコードへの `Movie` オブジェクトのマッピングのタスクを処理します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="2dd4c-106">データベース コンテキストは、*Startup.cs* ファイルの `ConfigureServices` メソッドで[依存性の注入](xref:fundamentals/dependency-injection)コンテナーに登録されます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

<span data-ttu-id="2dd4c-107">ASP.NET Core の[構成](xref:fundamentals/configuration/index)システムは `ConnectionString` を読み取ります。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="2dd4c-108">ローカルで開発する場合は、*appsettings.json* ファイルから接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="2dd4c-109">テストまたは実稼働サーバーにアプリを配置する場合は、環境変数または別の方法を使用して、実際の SQL Server に接続文字列を設定できます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="2dd4c-110">詳細については、[構成](xref:fundamentals/configuration/index)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="2dd4c-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="2dd4c-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="2dd4c-112">LocalDB は、プログラム開発を対象にした、SQL Server Express データベース エンジンの軽量版です。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="2dd4c-113">LocalDB は要求時に開始され、ユーザー モードで実行されるため、複雑な構成はありません。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="2dd4c-114">既定では、LocalDB データベースは *C:/Users/\<user\>* ディレクトリに "\*.mdf" ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="2dd4c-115">**[表示]** メニューの **[SQL Server オブジェクト エクスプローラー]** (SSOX) を開きます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![[View] メニュー](working-with-sql/_static/ssox.png)

* <span data-ttu-id="2dd4c-117">`Movie` テーブルを右クリックして **[デザイナーの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/design.png)

  ![デザイナーに開かれた Movie テーブル](working-with-sql/_static/dv.png)

<span data-ttu-id="2dd4c-120">`ID` の横のキー アイコンに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="2dd4c-121">既定では、EF は `ID` という名前のプロパティを主キーにします。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="2dd4c-122">`Movie` テーブルを右クリックして **[データの表示]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Movie テーブルで開かれたコンテキスト メニュー](working-with-sql/_static/ssox2.png)

  ![開いた Movie テーブルにテーブル データが表示されています](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="2dd4c-125">データベースのシード</span><span class="sxs-lookup"><span data-stu-id="2dd4c-125">Seed the database</span></span>

<span data-ttu-id="2dd4c-126">*Models* フォルダーに `SeedData` という名前の新しいクラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="2dd4c-127">生成されたコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-127">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="2dd4c-128">DB にムービーがある場合、シード初期化子が返され、ムービーは追加されません。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="2dd4c-129">シード初期化子の追加</span><span class="sxs-lookup"><span data-stu-id="2dd4c-129">Add the seed initializer</span></span>

<span data-ttu-id="2dd4c-130">*Program.cs* の内容を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-130">Replace the contents of *Program.cs* with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="2dd4c-131">次のように、*Program.cs* ファイルで `Main` メソッドにシード初期化子を追加します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

::: moniker-end

<span data-ttu-id="2dd4c-132">アプリのテスト</span><span class="sxs-lookup"><span data-stu-id="2dd4c-132">Test the app</span></span>

* <span data-ttu-id="2dd4c-133">DB 内のすべてのレコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-133">Delete all the records in the DB.</span></span> <span data-ttu-id="2dd4c-134">これはブラウザーの削除リンクで行うか、SSOX から行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-134">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="2dd4c-135">アプリを強制的に初期化して (`Startup` クラスでメソッドを呼び出す)、シード メソッドが実行されるようにします。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-135">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="2dd4c-136">強制的に初期化するには、IIS Express を停止してから再起動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-136">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="2dd4c-137">これは次の方法のいずれかを使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-137">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="2dd4c-138">通知領域の IIS Express システム トレイ アイコンを右クリックし、**[終了]** または **[サイトの停止]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-138">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![IIS Express システム トレイ アイコン](working-with-sql/_static/iisExIcon.png)

    ![コンテキスト メニュー](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="2dd4c-141">非デバッグ モードで VS を実行していた場合は、F5 キーを押してデバッグ モードで実行します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-141">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="2dd4c-142">デバッグ モードで VS を実行していた場合は、デバッガーを停止して、F5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-142">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="2dd4c-143">アプリにシードされたデータが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2dd4c-143">The app shows the seeded data.</span></span>

![ムービー データが表示された、Microsoft Edge で開かれている MVC ムービー アプリケーション](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="2dd4c-145">[前へ](adding-model.md)
> [次へ](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="2dd4c-145">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
