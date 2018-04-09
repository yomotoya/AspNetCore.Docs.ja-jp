---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server データベースの更新 - 11/12 を展開する |。Microsoft ドキュメント
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="cbad4-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します SQL Server データベースの更新 - 11/12 を展開する。</span><span class="sxs-lookup"><span data-stu-id="cbad4-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="cbad4-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cbad4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="cbad4-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="cbad4-106">この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="cbad4-107">Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="cbad4-108">系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="cbad4-109">Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示しています、および Windows Azure Web サイトを展開する方法を示しますをチュートリアルでは、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="cbad4-110">概要</span><span class="sxs-lookup"><span data-stu-id="cbad4-110">Overview</span></span>

<span data-ttu-id="cbad4-111">このチュートリアルでは、SQL Server データベースの完全にデータベースの更新を展開する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="cbad4-112">Code First Migrations は、データベースの更新のすべての作業を行う、ため、プロセスは SQL Server Compact での作業した内容とほぼ同じ、[データベース更新の展開](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="cbad4-113">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="cbad4-114">テーブルに新しい列を追加します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="cbad4-115">チュートリアルのこのセクションで、データベースの変更と対応するコードの変更、その後、テスト環境や実稼働環境に展開するための準備で Visual Studio でテストを作ります。</span><span class="sxs-lookup"><span data-stu-id="cbad4-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="cbad4-116">変更は、追加、`OfficeHours`列を`Instructor`エンティティとの新しい情報を表示する、**講習においてインストラクター** web ページ。</span><span class="sxs-lookup"><span data-stu-id="cbad4-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="cbad4-117">ContosoUniversity.DAL プロジェクトで開きます*Instructor.cs*間で、次のプロパティを追加し、`HireDate`と`Courses`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="cbad4-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="cbad4-118">テスト データを含む新しい列のシードを設定するように、初期化子のクラスを更新します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="cbad4-119">開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`を新しい列を含む次のコード ブロックで。</span><span class="sxs-lookup"><span data-stu-id="cbad4-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="cbad4-120">ContosoUniversity プロジェクトで開きます*Instructors.aspx* office 時間の終了の直前に新しいテンプレート フィールドを追加および`</Columns>`、最初のタグ`GridView`コントロール。</span><span class="sxs-lookup"><span data-stu-id="cbad4-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="cbad4-121">ソリューションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-121">Build the solution.</span></span>

<span data-ttu-id="cbad4-122">開く、 **Package Manager Console**ウィンドウ、および select ContosoUniversity.DAL として、**既定のプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="cbad4-123">次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="cbad4-124">アプリケーションの実行を選択して、**講習においてインストラクター**ページ。</span><span class="sxs-lookup"><span data-stu-id="cbad4-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="cbad4-125">ページが読み込むには、Entity Framework は、データベースを再作成され、テスト データのシードを設定するために通常よりも少し長くかかります。</span><span class="sxs-lookup"><span data-stu-id="cbad4-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="cbad4-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cbad4-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="cbad4-127">データベースの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="cbad4-128">Code First Migrations を使用して、SQL Server にデータベースの変更を展開するためのメソッドは、同じ SQL Server Compact です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="cbad4-129">ただし、テストを変更する必要があるがまだ設定して SQL Server Compact から SQL Server に移行するために、プロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="cbad4-130">最初の手順では、前のチュートリアルで作成した接続文字列の変換を削除します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="cbad4-131">これらは不要になったため、発行プロファイルで接続文字列の変換を指定ように構成する前に実行した方法で、**パッケージ化/発行 SQL** SQL Server への移行 タブ。</span><span class="sxs-lookup"><span data-stu-id="cbad4-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="cbad4-132">開く、 *Web.Test.config*ファイルし、削除、`connectionStrings`要素。</span><span class="sxs-lookup"><span data-stu-id="cbad4-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="cbad4-133">内でのみの残りの変換、 *Web.Test.config*用のファイルは、`Environment`値で、`appSettings`要素。</span><span class="sxs-lookup"><span data-stu-id="cbad4-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="cbad4-134">今すぐテスト環境に発行プロファイルと発行を更新できます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="cbad4-135">開く、 **Web の発行**ウィザード、しに切り替えると、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="cbad4-136">選択、**テスト**プロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="cbad4-137">選択、**設定**タブです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="cbad4-138">をクリックして**の機能強化を公開する新しいデータベースを有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="cbad4-139">接続文字列 ボックスで**SchoolContext**で使用したものと同じ値を入力、 *Web.Test.config*前のチュートリアルでの変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="cbad4-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="cbad4-140">選択**実行 Code First Migrations (アプリケーション開始時に実行されます)**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="cbad4-141">(チェック ボックスにラベルを付けることがあります、バージョンの Visual Studio で**適用の Code First Migrations**)。</span><span class="sxs-lookup"><span data-stu-id="cbad4-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="cbad4-142">接続文字列 ボックスで**DefaultConnection**で使用したものと同じ値を入力、 *Web.Test.config*前のチュートリアルでの変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="cbad4-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="cbad4-143">ままにして**更新データベース**オフにします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="cbad4-144">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-144">Click **Publish**.</span></span>

<span data-ttu-id="cbad4-145">Visual Studio では、コードの変更をテスト環境に展開し、Contoso 大学のホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="cbad4-146">講習においてインストラクター ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-146">Select the Instructors page.</span></span>

<span data-ttu-id="cbad4-147">アプリケーションがこのページを実行すると、データベースにアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="cbad4-148">Code First Migrations は、データベースが最新では、検索を最新の移行がまだ適用されていないかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="cbad4-149">Code First Migrations 最新の移行を適用する場合は、実行、`Seed`メソッド、し、ページが正常に実行します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="cbad4-150">シードのデータの新しい勤務時間列を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cbad4-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="cbad4-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="cbad4-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="cbad4-152">データベースの更新を実稼働環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="cbad4-153">また、運用環境の発行プロファイルを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cbad4-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="cbad4-154">ここでは、既存のプロファイルの削除を更新された .publishsettings ファイルをインポートして新しいものを作成します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="cbad4-155">更新されたファイルには、Cytanium で SQL Server データベースの接続文字列が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="cbad4-156">接続文字列の変換が不要になったテスト環境に展開したときに説明したとおり、 *Web.Production.config*変換ファイル。</span><span class="sxs-lookup"><span data-stu-id="cbad4-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="cbad4-157">ファイルし、削除を開く、`connectionStrings`要素。</span><span class="sxs-lookup"><span data-stu-id="cbad4-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="cbad4-158">残りの変換は、`Environment`値で、`appSettings`要素および`location`Elmah エラー レポートにアクセスを制限する要素。</span><span class="sxs-lookup"><span data-stu-id="cbad4-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="cbad4-159">実稼働環境用の新しい発行プロファイルを作成する前に、更新された .publishsettings ファイルをダウンロード前に実行したのと同様、[実稼働環境に展開する](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)チュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="cbad4-160">(Cytanium コントロール パネル で、 **Web サイト**、をクリックし、 **contosouniversity.com** web サイトです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="cbad4-161">Select、 **Web 公開** タブをクリックして**この web サイトの発行プロファイルのダウンロード**)。これを行う理由は、.publishsettings ファイル内のデータベース接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="cbad4-162">まだを使用していた SQL Server Compact しておらず、SQL Server データベース Cytanium にまだ作成しているので、ファイルをダウンロードする最初の時間を使用可能な接続文字列がありませんでした。</span><span class="sxs-lookup"><span data-stu-id="cbad4-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="cbad4-163">今すぐ、運用環境に発行プロファイルと発行を更新できます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="cbad4-164">開く、 **Web の発行**ウィザード、しに切り替えると、**プロファイル**タブです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="cbad4-165">をクリックして**プロファイルの管理**、実稼働プロファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="cbad4-166">閉じる、 **Web の発行**ウィザードにこの変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="cbad4-167">開く、 **Web の発行**クリックしてウィザードをもう一度、**インポート**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="cbad4-168">**接続** タブで、変更**送信先 URL**一時的な URL を使用している場合は、適切な値にします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="cbad4-169">**[次へ]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-169">Click **Next**.</span></span>

<span data-ttu-id="cbad4-170">**設定** タブで、をクリックして**の機能強化を公開する新しいデータベースを有効にする**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="cbad4-171">接続文字列 ドロップダウン リストに**SchoolContext**、Cytanium 接続文字列を選択します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="cbad4-173">選択**実行 Code First migrations (アプリケーション開始時に実行されます)**です。</span><span class="sxs-lookup"><span data-stu-id="cbad4-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="cbad4-174">接続文字列 ドロップダウン リストに**DefaultConnection**、Cytanium 接続文字列を選択します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="cbad4-175">選択、**プロファイル** タブで、をクリックして**プロファイルの管理**、"Production"に"contosouniversity.com - Web Deploy"から、プロファイルの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="cbad4-176">発行プロファイルを変更を保存するを閉じてから、もう一度開きます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="cbad4-177">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cbad4-177">Click **Publish**.</span></span> <span data-ttu-id="cbad4-178">(コピーする実際の運用 web サイト、*アプリ\_offline.htm*と put の実稼働環境に発行する前に、プロジェクト フォルダーでを削除して、展開が完了するとします)。</span><span class="sxs-lookup"><span data-stu-id="cbad4-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="cbad4-179">Visual Studio では、コードの変更をテスト環境に展開し、Contoso 大学のホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="cbad4-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="cbad4-180">講習においてインストラクター ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-180">Select the Instructors page.</span></span>

<span data-ttu-id="cbad4-181">Code First Migrations は、テスト環境で、同じ方法、データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="cbad4-182">シードのデータの新しい勤務時間列を参照してください。</span><span class="sxs-lookup"><span data-stu-id="cbad4-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="cbad4-184">今すぐが正常に展開したデータベースの変更が含まれているアプリケーションの更新プログラム、SQL Server データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="cbad4-185">説明</span><span class="sxs-lookup"><span data-stu-id="cbad4-185">More Information</span></span>

<span data-ttu-id="cbad4-186">これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションの配置に関するチュートリアルを完了します。</span><span class="sxs-lookup"><span data-stu-id="cbad4-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="cbad4-187">これらのチュートリアルで説明されているトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN web サイトです。</span><span class="sxs-lookup"><span data-stu-id="cbad4-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="cbad4-188">謝辞</span><span class="sxs-lookup"><span data-stu-id="cbad4-188">Acknowledgements</span></span>

<span data-ttu-id="cbad4-189">このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。</span><span class="sxs-lookup"><span data-stu-id="cbad4-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="cbad4-190">Alberto Poblacion、MVP &amp; MCT、スペイン</span><span class="sxs-lookup"><span data-stu-id="cbad4-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="cbad4-191">Jarod Ferguson、データ プラットフォームの開発 MVP、United States</span><span class="sxs-lookup"><span data-stu-id="cbad4-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="cbad4-192">過酷 Mittal、Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbad4-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="cbad4-193">Kristina Olson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbad4-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="cbad4-194">Mike 教皇、Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbad4-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="cbad4-195">Mohit Srivastava、Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbad4-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="cbad4-196">Raffaele Rialdi (イタリア)</span><span class="sxs-lookup"><span data-stu-id="cbad4-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="cbad4-197">Rick Anderson、Microsoft</span><span class="sxs-lookup"><span data-stu-id="cbad4-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="cbad4-198">[Sayed Hashimi、Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="cbad4-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="cbad4-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="cbad4-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="cbad4-200">[Scott ハンター、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="cbad4-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="cbad4-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="cbad4-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="cbad4-202">[Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="cbad4-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cbad4-203">[前へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [次へ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="cbad4-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
