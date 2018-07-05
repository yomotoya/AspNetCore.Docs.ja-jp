---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server データベースの更新 - 12 の 11 を展開する |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816633"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="d8da3-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: SQL Server データベースの更新 - 12 の 11 を展開します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="d8da3-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d8da3-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d8da3-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="d8da3-106">この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d8da3-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="d8da3-107">Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="d8da3-108">シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="d8da3-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Windows Azure Web サイトをデプロイする方法を示しますチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="d8da3-110">概要</span><span class="sxs-lookup"><span data-stu-id="d8da3-110">Overview</span></span>

<span data-ttu-id="d8da3-111">このチュートリアルでは、完全な SQL Server データベースにデータベースの更新を展開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="d8da3-112">Code First Migrations は、データベースの更新のすべての作業を行う、ため、プロセスの SQL Server Compact で行った場合とほぼ同じ、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="d8da3-113">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="d8da3-114">テーブルに新しい列の追加</span><span class="sxs-lookup"><span data-stu-id="d8da3-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="d8da3-115">このチュートリアルのこのセクションで変更をデータベースと対応するコードを変更し、テストおよび運用環境に展開するための準備で Visual Studio でテストを行います。</span><span class="sxs-lookup"><span data-stu-id="d8da3-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="d8da3-116">変更は、追加、`OfficeHours`列を`Instructor`エンティティとの新しい情報を表示する、 **Instructors** web ページ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="d8da3-117">ContosoUniversity.DAL プロジェクトで開きます*Instructor.cs*間で、次のプロパティを追加し、`HireDate`と`Courses`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="d8da3-118">テスト データを含む新しい列のシードを設定するために、初期化子クラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="d8da3-119">開いている*migrations \configuration.cs*と置換を開始するコード ブロック`var instructors = new List<Instructor>`と新しい列を含む次のコード ブロック。</span><span class="sxs-lookup"><span data-stu-id="d8da3-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="d8da3-120">ContosoUniversity プロジェクトで開きます*Instructors.aspx*の業務時間終了直前に新しいテンプレート フィールドを追加および`</Columns>`最初のタグ`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="d8da3-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="d8da3-121">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-121">Build the solution.</span></span>

<span data-ttu-id="d8da3-122">開く、**パッケージ マネージャー コンソール**ウィンドウ、および select ContosoUniversity.DAL として、**既定のプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="d8da3-123">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="d8da3-124">アプリケーションを実行し、選択、 **Instructors**ページ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="d8da3-125">ページでは読み込むには、Entity Framework がデータベースを再作成し、テスト データのシードを設定するために通常より少し長くなります。</span><span class="sxs-lookup"><span data-stu-id="d8da3-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="d8da3-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8da3-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="d8da3-127">データベースの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="d8da3-128">Code First Migrations を使用する場合に SQL Server データベースの変更を展開するためのメソッドと同じ SQL Server Compact です。</span><span class="sxs-lookup"><span data-stu-id="d8da3-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="d8da3-129">ただし、テストを変更する必要があるため、SQL Server Compact から SQL Server に移行するよう設定されても、発行プロファイル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="d8da3-130">最初の手順は、前のチュートリアルで作成した接続文字列の変換を削除します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="d8da3-131">これらが不要になったため、発行プロファイルの接続文字列の変換を指定ように構成する前に行ったよう、**パッケージ化/発行 SQL** SQL Server への移行 タブ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="d8da3-132">開く、 *Web.Test.config*ファイルし、削除、`connectionStrings`要素。</span><span class="sxs-lookup"><span data-stu-id="d8da3-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="d8da3-133">残り変換だけで、 *Web.Test.config*ファイルは、`Environment`値、`appSettings`要素。</span><span class="sxs-lookup"><span data-stu-id="d8da3-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="d8da3-134">今すぐテスト環境には、発行プロファイルと発行を更新できます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="d8da3-135">開く、 **Web の発行**ウィザード、および、スイッチ、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="d8da3-136">選択、**テスト**プロファイルを公開します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="d8da3-137">選択、**設定**タブ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="d8da3-138">クリックして**発行機能の向上、新しいデータベースを有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="d8da3-139">接続文字列 ボックスで**SchoolContext**で使用した同じ値を入力、 *Web.Test.config*前のチュートリアルでは変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="d8da3-140">選択**実行 Code First Migrations (アプリケーションの起動時に実行)** します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="d8da3-141">(Visual Studio のバージョンによって、チェック ボックスをラベル可能性があります**適用の Code First Migrations**)。</span><span class="sxs-lookup"><span data-stu-id="d8da3-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="d8da3-142">接続文字列 ボックスで**DefaultConnection**で使用した同じ値を入力、 *Web.Test.config*前のチュートリアルでは変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="d8da3-143">ままに**Update database**オフにします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="d8da3-144">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-144">Click **Publish**.</span></span>

<span data-ttu-id="d8da3-145">Visual Studio では、コードの変更をテスト環境に展開し、Contoso University のホーム ページをブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="d8da3-146">Instructors ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-146">Select the Instructors page.</span></span>

<span data-ttu-id="d8da3-147">アプリケーションは、このページを実行すると、データベースにアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="d8da3-148">Code First Migrations は、データベースが現在があり、最新の移行がまだ適用されていないいる検索かどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="d8da3-149">Code First Migrations は、最新の移行を適用する、実行、`Seed`メソッド、し、ページが正常に実行します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="d8da3-150">シードされたデータで新しい Office 時間列が参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8da3-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="d8da3-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d8da3-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="d8da3-152">運用環境にデータベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="d8da3-153">また、運用環境の発行プロファイルを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8da3-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="d8da3-154">この場合、既存のプロファイルを削除し、更新された .publishsettings ファイルをインポートして、新しく作成します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="d8da3-155">更新されたファイルは、Cytanium で SQL Server データベースの接続文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="d8da3-156">内の接続文字列の変換が不要になったテスト環境にデプロイしたときに、学習したように、 *Web.Production.config*変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="d8da3-157">開くファイルを削除、`connectionStrings`要素。</span><span class="sxs-lookup"><span data-stu-id="d8da3-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="d8da3-158">残りの変換は、`Environment`値、`appSettings`要素と`location`Elmah エラー レポートにアクセスを制限する要素。</span><span class="sxs-lookup"><span data-stu-id="d8da3-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="d8da3-159">運用環境用の新しい発行プロファイルを作成する前に、更新された .publishsettings ファイルをダウンロード前に行ったのと同様、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアル。</span><span class="sxs-lookup"><span data-stu-id="d8da3-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="d8da3-160">(Cytanium コントロール パネル で、 **Websites**、順にクリックします、 **contosouniversity.com** web サイト。</span><span class="sxs-lookup"><span data-stu-id="d8da3-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="d8da3-161">選択、 **Web 公開**タブをクリックし、をクリックし、**この web サイトの発行プロファイルのダウンロード**)。これを実行する理由は、.publishsettings ファイル内のデータベース接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="d8da3-162">初めておらず、SQL Server データベース Cytanium にまだ作成して SQL Server Compact まだ使っていたため、ファイルのダウンロードを使用可能な接続文字列がありませんでした。</span><span class="sxs-lookup"><span data-stu-id="d8da3-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="d8da3-163">今すぐ、運用環境に発行プロファイルと発行を更新できます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="d8da3-164">開く、 **Web の発行**ウィザード、および、スイッチ、**プロファイル**タブ。</span><span class="sxs-lookup"><span data-stu-id="d8da3-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="d8da3-165">クリックして**プロファイルの管理**、実稼働プロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="d8da3-166">閉じる、 **Web の発行**ウィザードでこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="d8da3-167">開く、 **Web の発行**クリックしてウィザードをもう一度、**インポート**します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="d8da3-168">**接続** タブで、変更**送信先 URL**一時的な URL を使用している場合は、適切な値にします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="d8da3-169">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-169">Click **Next**.</span></span>

<span data-ttu-id="d8da3-170">**設定**] タブで [**発行機能の向上、新しいデータベースを有効にする**します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="d8da3-171">接続文字列のドロップダウン リストで**SchoolContext**、Cytanium 接続文字列を選択します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="d8da3-173">選択**実行 Code First migrations (アプリケーションの起動時に実行)** します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="d8da3-174">接続文字列のドロップダウン リストで**DefaultConnection**、Cytanium 接続文字列を選択します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="d8da3-175">選択、**プロファイル** タブで、をクリックして**プロファイルの管理**、"Production"を"contosouniversity.com - Web Deploy"から、プロファイルの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="d8da3-176">変更を保存する発行プロファイルを閉じてからもう一度開きます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="d8da3-177">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d8da3-177">Click **Publish**.</span></span> <span data-ttu-id="d8da3-178">(実際の運用 web サイトでコピーする*アプリ\_offline.htm*を本番環境と put、パブリッシュする前に、プロジェクト フォルダーで削除して、デプロイが完了します)。</span><span class="sxs-lookup"><span data-stu-id="d8da3-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="d8da3-179">Visual Studio では、コードの変更をテスト環境に展開し、Contoso University のホーム ページをブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="d8da3-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="d8da3-180">Instructors ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-180">Select the Instructors page.</span></span>

<span data-ttu-id="d8da3-181">Code First Migrations は、テスト環境では、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="d8da3-182">シードされたデータで新しい Office 時間列が参照してください。</span><span class="sxs-lookup"><span data-stu-id="d8da3-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="d8da3-184">正常に配置されましたデータベースの変更を含むアプリケーションの更新プログラム、SQL Server データベースを使用しています。</span><span class="sxs-lookup"><span data-stu-id="d8da3-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="d8da3-185">説明</span><span class="sxs-lookup"><span data-stu-id="d8da3-185">More Information</span></span>

<span data-ttu-id="d8da3-186">これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションを展開する方法のチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="d8da3-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="d8da3-187">これらのチュートリアルで取り上げるトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN web サイト。</span><span class="sxs-lookup"><span data-stu-id="d8da3-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="d8da3-188">謝辞</span><span class="sxs-lookup"><span data-stu-id="d8da3-188">Acknowledgements</span></span>

<span data-ttu-id="d8da3-189">このチュートリアル シリーズのコンテンツに多大な貢献を行った以下の方々 に感謝したいと思います。</span><span class="sxs-lookup"><span data-stu-id="d8da3-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="d8da3-190">Alberto Poblacion、MVP &amp; MCT、スペイン</span><span class="sxs-lookup"><span data-stu-id="d8da3-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="d8da3-191">Jarod Ferguson、データ プラットフォームの開発の MVP、United States</span><span class="sxs-lookup"><span data-stu-id="d8da3-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="d8da3-192">Harsh Mittal、Microsoft</span><span class="sxs-lookup"><span data-stu-id="d8da3-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="d8da3-193">Kristina Olson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="d8da3-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="d8da3-194">Mike 教皇、Microsoft</span><span class="sxs-lookup"><span data-stu-id="d8da3-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="d8da3-195">Mohit Srivastava、Microsoft</span><span class="sxs-lookup"><span data-stu-id="d8da3-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="d8da3-196">Raffaele Rialdi (イタリア)</span><span class="sxs-lookup"><span data-stu-id="d8da3-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="d8da3-197">Rick Anderson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="d8da3-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="d8da3-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="d8da3-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="d8da3-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="d8da3-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="d8da3-200">[Scott Hunter、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="d8da3-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="d8da3-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="d8da3-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="d8da3-202">[Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="d8da3-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8da3-203">[前へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [次へ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="d8da3-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
