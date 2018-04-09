---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベースの更新を展開する |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="0aea4-103">SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="0aea4-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0aea4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="0aea4-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="0aea4-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="0aea4-106">この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="0aea4-107">Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="0aea4-108">系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="0aea4-109">チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="0aea4-110">概要</span><span class="sxs-lookup"><span data-stu-id="0aea4-110">Overview</span></span>

<span data-ttu-id="0aea4-111">このチュートリアルで行うデータベースの変更と関連するコードの変更、Visual Studio で、変更をテストし、テストと実稼働環境に、更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="0aea4-112">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="0aea4-113">テーブルに新しい列を追加します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="0aea4-114">このセクションで、生年月日に列を追加する、`Person`の基本クラス、`Student`と`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="0aea4-115">新しい列を表示するように、インストラクター データを表示するページを更新します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="0aea4-116">*ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (存在する必要があります終わり波かっこの後に続く 2)。</span><span class="sxs-lookup"><span data-stu-id="0aea4-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="0aea4-117">次に、新しい列の値を提供できるようにシード メソッドを更新します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="0aea4-118">開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。</span><span class="sxs-lookup"><span data-stu-id="0aea4-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="0aea4-119">ContosoUniversity プロジェクトで開きます*Instructors.aspx*生年月日を表示する新しいテンプレート フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="0aea4-120">雇用日とオフィス割り当てのためのものの間に追加します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="0aea4-121">(場合はコードのインデントがとれていない状態を取得することができますキーを押して CTRL K し CTRL-D ファイルを自動的に書式設定を変更します。)</span><span class="sxs-lookup"><span data-stu-id="0aea4-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="0aea4-122">ソリューションをビルドし、開きます、 **Package Manager Console**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="0aea4-123">ContosoUniversity.DAL として選択されていることを確認してください、**既定のプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="0aea4-124">**Package Manager Console**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="0aea4-125">このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`方法、新しい列を作成するコードを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="0aea4-127">ソリューションをビルドしに次のコマンドを入力、 **Package Manager Console**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="0aea4-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="0aea4-128">コマンドが完了したら、アプリケーションを実行し、インストラクター ページを選択します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="0aea4-129">ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールドです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="0aea4-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="0aea4-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="0aea4-131">データベースの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="0aea4-132">**ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="0aea4-133">**Web 1 つをクリックして 発行**ツールバーで、**テスト**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="0aea4-134">(ツールバーを無効にした場合で ContosoUniversity プロジェクトを選択**ソリューション エクスプ ローラー**)。</span><span class="sxs-lookup"><span data-stu-id="0aea4-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="0aea4-135">Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="0aea4-136">更新プログラムが正常に展開されたことを確認するインストラクター ページを実行します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="0aea4-137">Code First、アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="0aea4-138">予期される表示ページが表示されたら、**生年月日**で日付を含む列。</span><span class="sxs-lookup"><span data-stu-id="0aea4-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="0aea4-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0aea4-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="0aea4-140">データベースの更新を実稼働環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="0aea4-141">これで、実稼働環境に配置できます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-141">You can now deploy to production.</span></span> <span data-ttu-id="0aea4-142">唯一の違いは、使用すること*アプリ\_offline.htm*サイトへのアクセスと変更を展開しているときに、データベースが更新を禁止します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="0aea4-143">運用環境のデプロイには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="0aea4-144">アップロード、*アプリ\_offline.htm*ファイルを実稼働サイトです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="0aea4-145">Visual Studio で、プロファイルを選択して、実稼働環境で、 **Web 1 つをクリックして 発行**ツールバーとクリック**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="0aea4-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="0aea4-146">削除、*アプリ\_offline.htm*実稼働サイトからのファイルです。</span><span class="sxs-lookup"><span data-stu-id="0aea4-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="0aea4-147">アプリケーションが、実稼働環境で使用するときは、バックアップ計画を実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0aea4-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="0aea4-148">つまり、する必要がある定期的にコピーして、*学校 Prod.sdf*と*aspnet Prod.sdf*セキュリティで保護された記憶域の場所にサイトを運用環境からのファイルと、このようないくつかの世代を保存する必要がありますバックアップします。</span><span class="sxs-lookup"><span data-stu-id="0aea4-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="0aea4-149">データベースを更新するときに、変更の直前からバックアップ コピーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0aea4-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="0aea4-150">次に、更新を間違えたして実稼働環境に配置した後まで、検出されない場合もことができますが破損する前に、の状態にデータベースを回復します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="0aea4-151">Visual Studio は、ブラウザーで、ホーム ページの URL を開くときに、*アプリ\_offline.htm*ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="0aea4-152">削除した後、*アプリ\_offline.htm*ファイル、更新プログラムが正常に展開されたことを確認するには、もう一度、ホーム ページを閲覧することができます。</span><span class="sxs-lookup"><span data-stu-id="0aea4-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="0aea4-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="0aea4-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="0aea4-154">テストと実稼働環境の両方にデータベースの変更が含まれているアプリケーションの更新プログラムを展開したようになりました。</span><span class="sxs-lookup"><span data-stu-id="0aea4-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="0aea4-155">次のチュートリアルでは、SQL Server Compact から SQL Server Express と SQL Server にデータベースを移行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0aea4-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0aea4-156">[前へ](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [次へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="0aea4-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
