---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio を使用して ASP.NET Web 展開: データベース更新の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817624"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="80074-103">Visual Studio を使用して ASP.NET Web 展開: データベース更新の展開</span><span class="sxs-lookup"><span data-stu-id="80074-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="80074-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="80074-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="80074-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="80074-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="80074-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="80074-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="80074-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="80074-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="80074-108">概要</span><span class="sxs-lookup"><span data-stu-id="80074-108">Overview</span></span>

<span data-ttu-id="80074-109">このチュートリアルで行ったデータベースの変更と関連するコードの変更、Visual Studio での変更をテストし、テスト、ステージング、実稼働環境に更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="80074-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="80074-110">DbDacFx プロバイダーを使用してデータベースを更新する方法を示します後で、およびチュートリアルが最初に Code First Migrations で管理されているデータベースを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="80074-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="80074-111">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。</span><span class="sxs-lookup"><span data-stu-id="80074-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="80074-112">Code First Migrations を使用してデータベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="80074-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="80074-113">このセクションに生年月日の列を追加、`Person`の基本クラス、`Student`と`Instructor`エンティティ。</span><span class="sxs-lookup"><span data-stu-id="80074-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="80074-114">新しい列を表示するように、インストラクター データを表示するページを更新します。</span><span class="sxs-lookup"><span data-stu-id="80074-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="80074-115">最後に、テスト、ステージング、および運用環境に変更をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="80074-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="80074-116">アプリケーション データベース内のテーブルに列を追加します。</span><span class="sxs-lookup"><span data-stu-id="80074-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="80074-117">*ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (あります終わり波かっこの後に 2 つ)。</span><span class="sxs-lookup"><span data-stu-id="80074-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="80074-118">次に、更新、`Seed`メソッドを新しい列の値を提供するようにします。</span><span class="sxs-lookup"><span data-stu-id="80074-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="80074-119">開いている*migrations \configuration.cs*と置換を開始するコード ブロック`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。</span><span class="sxs-lookup"><span data-stu-id="80074-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="80074-120">ソリューションをビルドしを開き、**パッケージ マネージャー コンソール**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="80074-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="80074-121">として ContosoUniversity.DAL がまだ選択されていることを確認、**既定のプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="80074-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="80074-122">**パッケージ マネージャー コンソール**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="80074-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="80074-123">このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`メソッド、新しい列を作成するコードを表示できます。</span><span class="sxs-lookup"><span data-stu-id="80074-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="80074-124">`Up`メソッドは、変更を実装するときに、列を作成し、`Down`メソッドは、変更をロールバックするときに列を削除します。</span><span class="sxs-lookup"><span data-stu-id="80074-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="80074-126">ソリューションをビルドおよびでは、次のコマンドを入力し、**パッケージ マネージャー コンソール**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="80074-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="80074-127">Entity Framework の実行、`Up`メソッドとし、実行、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="80074-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="80074-128">Instructors ページに新しい列を表示します。</span><span class="sxs-lookup"><span data-stu-id="80074-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="80074-129">ContosoUniversity プロジェクトで開きます*Instructors.aspx*し、誕生日を表示する新しいテンプレート フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="80074-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="80074-130">雇用日とオフィスの割り当てのためのものの間に追加します。</span><span class="sxs-lookup"><span data-stu-id="80074-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="80074-131">(コードのインデントとれていない場合は、することができますキーを押して CTRL + K し CTRL-D ファイルを自動的に再フォーマットします。)</span><span class="sxs-lookup"><span data-stu-id="80074-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="80074-132">アプリケーションを実行し、をクリックして、 **Instructors**リンク。</span><span class="sxs-lookup"><span data-stu-id="80074-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="80074-133">ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールド。</span><span class="sxs-lookup"><span data-stu-id="80074-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Instructors ページには birthdate で](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="80074-135">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="80074-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="80074-136">データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="80074-136">Deploy the database update</span></span>

1. <span data-ttu-id="80074-137">**ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="80074-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="80074-138">**Web の 1 クリックして発行**ツールバーで、をクリックして、**テスト**発行プロファイルをクリックして**Web の発行**します。</span><span class="sxs-lookup"><span data-stu-id="80074-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="80074-139">(ツールバーが無効になっている場合で ContosoUniversity プロジェクトを選択します**ソリューション エクスプ ローラー**。)。</span><span class="sxs-lookup"><span data-stu-id="80074-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="80074-140">Visual Studio が更新されたアプリケーションを配置し、ホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="80074-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="80074-141">実行、 **Instructors**ページを更新プログラムが正常にデプロイされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80074-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="80074-142">Code First アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッド。</span><span class="sxs-lookup"><span data-stu-id="80074-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="80074-143">予期される確認ページが表示されたら、**生年月日**で日付を含む列。</span><span class="sxs-lookup"><span data-stu-id="80074-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="80074-144">**Web の 1 クリックして発行**ツールバーで、をクリックして、**ステージング**発行プロファイルをクリックして**Web の発行**します。</span><span class="sxs-lookup"><span data-stu-id="80074-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="80074-145">実行、 **Instructors**  ページで、ステージング、更新プログラムが正常にデプロイされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80074-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="80074-146">**Web の 1 クリックして発行**ツールバーで、をクリックして、**運用**発行プロファイルをクリックして**Web の発行**します。</span><span class="sxs-lookup"><span data-stu-id="80074-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="80074-147">実行、 **Instructors**更新プログラムが正常にデプロイされたことを確認するには、実稼働環境でのページ。</span><span class="sxs-lookup"><span data-stu-id="80074-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="80074-148">データベースの変更を含む実際の運用アプリケーションの更新プログラムを使用してデプロイ時にアプリケーションをオフラインをも通常取得するが*アプリ\_offline.htm*、前のチュートリアルで学習したようにします。</span><span class="sxs-lookup"><span data-stu-id="80074-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="80074-149">DbDacFx プロバイダーを使用してデータベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="80074-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="80074-150">このセクションでは追加、*コメント*列を*ユーザー*メンバーシップ データベース内のテーブルし、表示し、各ユーザーのコメントを編集することができます ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="80074-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="80074-151">変更は、テスト、ステージング、および運用環境に配置します。</span><span class="sxs-lookup"><span data-stu-id="80074-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="80074-152">メンバーシップ データベース内のテーブルに列を追加します。</span><span class="sxs-lookup"><span data-stu-id="80074-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="80074-153">Visual Studio で開く**SQL Server オブジェクト エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="80074-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="80074-154">展開 **(localdb) \v11.0**、展開**データベース**、展開**aspnet ContosoUniversity** (いない**aspnet-ContosoUniversity-運用**)順に展開**テーブル**します。</span><span class="sxs-lookup"><span data-stu-id="80074-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="80074-155">表示されない場合 **(localdb) \v11.0**下、 **SQL Server**ノードを右クリックし、 **SQL Server**ノードをクリックします**SQL Server の追加**します。</span><span class="sxs-lookup"><span data-stu-id="80074-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="80074-156">**サーバーへの接続**ダイアログ ボックスの入力 *(localdb) \v11.0*として、**サーバー名**、 をクリックし、 **Connect**。</span><span class="sxs-lookup"><span data-stu-id="80074-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="80074-157">表示されない場合**aspnet ContosoUniversity**、プロジェクトを実行しを使用してログイン、*管理者*資格情報 (パスワードが*devpwd*)、し、更新、 **SQL Server オブジェクト エクスプ ローラー**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="80074-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="80074-158">右クリックし、**ユーザー**テーブル、およびクリックして**ビュー デザイナー**します。</span><span class="sxs-lookup"><span data-stu-id="80074-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX のビュー デザイナー](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="80074-160">デザイナーで、追加、*コメント*列、それを*nvarchar (128)* と null を許容すると、順にクリックします**Update**します。</span><span class="sxs-lookup"><span data-stu-id="80074-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Comments 列を追加します。](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="80074-162">**データベース更新のプレビュー**ボックスで、 **Update Database**します。</span><span class="sxs-lookup"><span data-stu-id="80074-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![データベース更新のプレビュー](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="80074-164">表示し、新しい列を編集するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="80074-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="80074-165">**ソリューション エクスプ ローラー**を右クリックし、**アカウント**ContosoUniversity プロジェクトのフォルダーをクリックして**追加**、 をクリックし、**新しい項目の**.</span><span class="sxs-lookup"><span data-stu-id="80074-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="80074-166">新規作成**Web フォームを使用してマスター ページ**名前を付けます*UserInfo.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="80074-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="80074-167">既定値を受け入れる*Site.Master*のマスター ページとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="80074-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="80074-168">次のマークアップをコピー、 `MainContent` `Content`要素 (最後に、3`Content`要素)。</span><span class="sxs-lookup"><span data-stu-id="80074-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="80074-169">右クリックし、 *UserInfo.aspx*ページし、クリックして**ブラウザーで表示**します。</span><span class="sxs-lookup"><span data-stu-id="80074-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="80074-170">ログイン、*管理者*ユーザーの資格情報 (パスワードが*devpwd*) し、ユーザーは、ページが正しく動作することを確認するいくつかのコメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="80074-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![UserInfo ページ](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="80074-172">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="80074-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="80074-173">データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="80074-173">Deploy the database update</span></span>

<span data-ttu-id="80074-174">を dbDacFx プロバイダーを使用して展開するには、だけを選択する必要が、 **Update database** 、発行プロファイルのオプション。</span><span class="sxs-lookup"><span data-stu-id="80074-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="80074-175">ただし、初期のデプロイにこのオプションを使用したときにも構成したいくつか追加の SQL スクリプトを実行する: プロファイルにまだいるし、再び実行されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="80074-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="80074-176">開く、 **Web の発行**を ContosoUniversity プロジェクトを右クリックしてウィザード**発行**します。</span><span class="sxs-lookup"><span data-stu-id="80074-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="80074-177">選択、**テスト**プロファイル。</span><span class="sxs-lookup"><span data-stu-id="80074-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="80074-178">をクリックして、**設定**タブ。</span><span class="sxs-lookup"><span data-stu-id="80074-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="80074-179">**DefaultConnection**、 **Update database**。</span><span class="sxs-lookup"><span data-stu-id="80074-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="80074-180">初期デプロイを実行するように構成する追加のスクリプトを無効にします。</span><span class="sxs-lookup"><span data-stu-id="80074-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="80074-181">クリックして**データベース更新を構成する**します。</span><span class="sxs-lookup"><span data-stu-id="80074-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="80074-182">**データベース更新を構成する**ダイアログ ボックスで、クリア、チェック ボックスを横に*Grant.sql*と*aspnet-データ-dev.sql*します。</span><span class="sxs-lookup"><span data-stu-id="80074-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="80074-183">**[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80074-183">Click **Close**.</span></span>
6. <span data-ttu-id="80074-184">をクリックして、**プレビュー**タブ。</span><span class="sxs-lookup"><span data-stu-id="80074-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="80074-185">**データベース**の右側に**DefaultConnection**、クリックして、**プレビュー データベース**リンク。</span><span class="sxs-lookup"><span data-stu-id="80074-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![データベースのプレビュー](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="80074-187">プレビュー ウィンドウには、ソース データベースのスキーマに一致するデータベース スキーマを転送先データベースで実行されるスクリプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80074-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="80074-188">スクリプトには、新しい列を追加する ALTER TABLE コマンドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="80074-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="80074-189">閉じる、**データベース プレビュー**クリックしてダイアログ ボックスで、**発行**します。</span><span class="sxs-lookup"><span data-stu-id="80074-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="80074-190">Visual Studio が更新されたアプリケーションを配置し、ホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="80074-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="80074-191">UserInfo ページの実行 (追加*Account/UserInfo.aspx*ホーム ページの URL に)、更新プログラムが正常にデプロイされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80074-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="80074-192">入力してログインする必要があります*管理者*と*devpwd*します。</span><span class="sxs-lookup"><span data-stu-id="80074-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="80074-193">既定では、テーブル内のデータを展開していないと、開発で追加したコメントを検索しないように、実行するにはデータ デプロイ スクリプトを構成しませんでした。</span><span class="sxs-lookup"><span data-stu-id="80074-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="80074-194">新しいコメントを今すぐ追加するには、変更がデータベースに展開されたことと、ページが正常に機能を確認するステージングします。</span><span class="sxs-lookup"><span data-stu-id="80074-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="80074-195">ステージングと運用環境に展開するのと同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="80074-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="80074-196">忘れずに余分なスクリプトを無効にします。</span><span class="sxs-lookup"><span data-stu-id="80074-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="80074-197">テスト プロファイルに対する唯一の違いは、ステージング環境で 1 つだけのスクリプトを無効にするには、実稼働プロファイルのみを実行するように構成されたため、 *aspnet-prod-data.sql*します。</span><span class="sxs-lookup"><span data-stu-id="80074-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="80074-198">ステージングと運用環境の資格情報は、管理者と prodpwd です。</span><span class="sxs-lookup"><span data-stu-id="80074-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="80074-199">データベースの変更を含む実際の運用アプリケーションの更新プログラムをアップロードしてデプロイ時にアプリケーションをオフラインをも通常実行するは*アプリ\_offline.htm*発行を削除する前にその後で見たよう[、前のチュートリアル](deploying-a-code-update.md)します。</span><span class="sxs-lookup"><span data-stu-id="80074-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="80074-200">まとめ</span><span class="sxs-lookup"><span data-stu-id="80074-200">Summary</span></span>

<span data-ttu-id="80074-201">Code First Migrations と dbDacFx プロバイダーの両方を使用してデータベースの変更を含むアプリケーションの更新プログラムをデプロイしたようになりました。</span><span class="sxs-lookup"><span data-stu-id="80074-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Instructors ページには birthdate で](deploying-a-database-update/_static/image8.png)

![UserInfo ページ](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="80074-204">次のチュートリアルでは、コマンドラインを使用してデプロイを実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="80074-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80074-205">[前へ](deploying-a-code-update.md)
> [次へ](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="80074-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
