---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Visual Studio を使用した ASP.NET Web 展開: データベース更新の展開 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892622"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="543ed-103">Visual Studio を使用した ASP.NET Web 展開: データベース更新の展開</span><span class="sxs-lookup"><span data-stu-id="543ed-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="543ed-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="543ed-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="543ed-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="543ed-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="543ed-106">この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。</span><span class="sxs-lookup"><span data-stu-id="543ed-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="543ed-107">系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="543ed-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="543ed-108">概要</span><span class="sxs-lookup"><span data-stu-id="543ed-108">Overview</span></span>

<span data-ttu-id="543ed-109">このチュートリアルで行うデータベースの変更と関連するコードの変更、Visual Studio で、変更をテストし、テスト、ステージング、および運用環境を更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="543ed-110">DbDacFx プロバイダーを使用してデータベースを更新する方法を示します後で、およびチュートリアルが最初に Code First Migrations で管理されているデータベースを更新する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="543ed-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="543ed-111">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。</span><span class="sxs-lookup"><span data-stu-id="543ed-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="543ed-112">Code First Migrations を使用して、データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="543ed-113">このセクションで、生年月日に列を追加する、`Person`の基本クラス、`Student`と`Instructor`エンティティです。</span><span class="sxs-lookup"><span data-stu-id="543ed-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="543ed-114">新しい列を表示するように、インストラクター データを表示するページを更新します。</span><span class="sxs-lookup"><span data-stu-id="543ed-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="543ed-115">最後に、テスト、ステージング、および実稼働環境に変更を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="543ed-116">アプリケーション データベース内のテーブルに列を追加します。</span><span class="sxs-lookup"><span data-stu-id="543ed-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="543ed-117">*ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (存在する必要があります終わり波かっこの後に続く 2)。</span><span class="sxs-lookup"><span data-stu-id="543ed-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="543ed-118">次に、更新、`Seed`メソッドが新しい列の値を提供するようにします。</span><span class="sxs-lookup"><span data-stu-id="543ed-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="543ed-119">開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。</span><span class="sxs-lookup"><span data-stu-id="543ed-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="543ed-120">ソリューションをビルドし、開きます、 **Package Manager Console**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="543ed-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="543ed-121">ContosoUniversity.DAL として選択されていることを確認してください、**既定のプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="543ed-122">**Package Manager Console**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="543ed-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="543ed-123">このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`方法、新しい列を作成するコードを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="543ed-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="543ed-124">`Up`メソッドは、変更を実装しているときに、列を作成し、`Down`メソッドは、変更をロールバックするときに、列を削除します。</span><span class="sxs-lookup"><span data-stu-id="543ed-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="543ed-126">ソリューションをビルドしに次のコマンドを入力、 **Package Manager Console**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。</span><span class="sxs-lookup"><span data-stu-id="543ed-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="543ed-127">Entity Framework が実行される、`Up`メソッドとし、実行、`Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="543ed-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="543ed-128">講習においてインストラクター ページで、新しい列を表示します。</span><span class="sxs-lookup"><span data-stu-id="543ed-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="543ed-129">ContosoUniversity プロジェクトで開きます*Instructors.aspx*生年月日を表示する新しいテンプレート フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="543ed-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="543ed-130">雇用日とオフィス割り当てのためのものの間に追加します。</span><span class="sxs-lookup"><span data-stu-id="543ed-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="543ed-131">(場合はコードのインデントがとれていない状態を取得することができますキーを押して CTRL K し CTRL-D ファイルを自動的に書式設定を変更します。)</span><span class="sxs-lookup"><span data-stu-id="543ed-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="543ed-132">アプリケーションを実行し、をクリックして、**講習においてインストラクター**リンクします。</span><span class="sxs-lookup"><span data-stu-id="543ed-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="543ed-133">ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールドです。</span><span class="sxs-lookup"><span data-stu-id="543ed-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![講習においてインストラクター birthdate ページ](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="543ed-135">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="543ed-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="543ed-136">データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-136">Deploy the database update</span></span>

1. <span data-ttu-id="543ed-137">**ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="543ed-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="543ed-138">**Web 1 つをクリックして 発行**ツールバーで、をクリックして、**テスト**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="543ed-139">(ツールバーを無効にした場合で ContosoUniversity プロジェクトを選択**ソリューション エクスプ ローラー**)。</span><span class="sxs-lookup"><span data-stu-id="543ed-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="543ed-140">Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="543ed-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="543ed-141">実行、**講習においてインストラクター**ページを更新プログラムが正常に展開されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="543ed-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="543ed-142">Code First、アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="543ed-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="543ed-143">予期される表示ページが表示されたら、**生年月日**で日付を含む列。</span><span class="sxs-lookup"><span data-stu-id="543ed-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="543ed-144">**Web 1 つをクリックして 発行**ツールバーで、をクリックして、**ステージング**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="543ed-145">実行、**講習においてインストラクター**ステージング環境の更新プログラムが正常に展開されたことを確認するページ。</span><span class="sxs-lookup"><span data-stu-id="543ed-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="543ed-146">**Web 1 つをクリックして 発行**ツールバーで、をクリックして、**運用**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="543ed-147">実行、**講習においてインストラクター**更新プログラムが正常に展開されたことを確認するには、実稼働環境でのページです。</span><span class="sxs-lookup"><span data-stu-id="543ed-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="543ed-148">データベースの変更を含む実際の運用アプリケーションの更新プログラムの通常実行する配置時にアプリケーションをオフラインを使用して*アプリ\_offline.htm*前のチュートリアルで説明したとおり、します。</span><span class="sxs-lookup"><span data-stu-id="543ed-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="543ed-149">DbDacFx プロバイダーを使用して、データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="543ed-150">このセクションでは追加、*コメント*列を*ユーザー*メンバーシップ データベースにテーブルが表示され、ページを表示し、各ユーザーのコメントを編集することができますを作成します。</span><span class="sxs-lookup"><span data-stu-id="543ed-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="543ed-151">その変更は、テスト、ステージング、および実稼働環境を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="543ed-152">メンバーシップ データベース内のテーブルに列を追加します。</span><span class="sxs-lookup"><span data-stu-id="543ed-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="543ed-153">Visual Studio で開く**SQL Server オブジェクト エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="543ed-154">展開 **(localdb) \v11.0**、展開**データベース**、展開**aspnet ContosoUniversity** (いない**aspnet ContosoUniversity 本番**)展開し、**テーブル**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="543ed-155">表示されない場合 **(localdb) \v11.0**下にある、 **SQL Server**  ノードを右クリックし、 **SQL Server**ノードをクリック**SQL Server の追加**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="543ed-156">**サーバーへの接続** ダイアログ ボックスに「 *(localdb) \v11.0*として、**サーバー名**、クリックして**接続**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="543ed-157">表示されない場合**aspnet ContosoUniversity**、プロジェクトを実行しを使用してログイン、 *admin*資格情報 (パスワードが*devpwd*)、し、更新、 **SQL Server オブジェクト エクスプ ローラー**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="543ed-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="543ed-158">右クリックし、**ユーザー**テーブル、およびクリックして**ビュー デザイナー**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX ビュー デザイナー](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="543ed-160">デザイナーで、追加、*コメント*列、それを*nvarchar (128)* null を許可すると、をクリックし、**更新**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Comments 列を追加します。](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="543ed-162">**データベース更新のプレビュー**ボックスで、クリックして**更新データベース**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![データベース更新のプレビュー](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="543ed-164">表示および新しい列を編集するページを作成します。</span><span class="sxs-lookup"><span data-stu-id="543ed-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="543ed-165">**ソリューション エクスプ ローラー**を右クリックし、**アカウント**ContosoUniversity プロジェクト内のフォルダーをクリックして**追加**、クリックして**新しい項目の**.</span><span class="sxs-lookup"><span data-stu-id="543ed-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="543ed-166">新しい**Web フォームを使用してマスター ページ**し名前を付けます*UserInfo.aspx*です。</span><span class="sxs-lookup"><span data-stu-id="543ed-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="543ed-167">既定値を受け入れる*Site.Master*マスター ページとしてファイル。</span><span class="sxs-lookup"><span data-stu-id="543ed-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="543ed-168">次のマークアップをコピー、 `MainContent` `Content`要素 (3 の最後の`Content`要素)。</span><span class="sxs-lookup"><span data-stu-id="543ed-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="543ed-169">右クリックし、 *UserInfo.aspx*ページし、をクリックして**ブラウザーで表示**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="543ed-170">ログインに使用、 *admin*ユーザーの資格情報 (パスワードが*devpwd*) し、ページが正しく動作することを確認するユーザーにいくつかのコメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="543ed-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![ユーザー情報 ページ](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="543ed-172">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="543ed-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="543ed-173">データベースの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="543ed-173">Deploy the database update</span></span>

<span data-ttu-id="543ed-174">DbDacFx プロバイダーを使用して、展開するだけ選択する必要が、**更新データベース**発行プロファイルでオプションです。</span><span class="sxs-lookup"><span data-stu-id="543ed-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="543ed-175">ただし、最初の展開は、このオプションを使用したときにも構成したいくつか追加の SQL スクリプトを実行する: まだプロファイルであるものと、もう一度実行されないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="543ed-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="543ed-176">開く、 **Web の発行**ContosoUniversity プロジェクトを右クリックしをクリックしてウィザード**発行**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="543ed-177">選択、**テスト**プロファイル。</span><span class="sxs-lookup"><span data-stu-id="543ed-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="543ed-178">クリックして、**設定**タブです。</span><span class="sxs-lookup"><span data-stu-id="543ed-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="543ed-179">**DefaultConnection****更新データベース**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="543ed-180">最初の展開を実行するように構成する追加のスクリプトを無効にします。</span><span class="sxs-lookup"><span data-stu-id="543ed-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="543ed-181">をクリックして**データベース更新を構成する**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="543ed-182">**データベース更新を構成する**ダイアログ ボックスで、チェック ボックスをオフのチェック ボックス の横に*Grant.sql*と*aspnet データ-dev.sql*です。</span><span class="sxs-lookup"><span data-stu-id="543ed-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="543ed-183">**[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="543ed-183">Click **Close**.</span></span>
6. <span data-ttu-id="543ed-184">クリックして、**プレビュー**タブです。</span><span class="sxs-lookup"><span data-stu-id="543ed-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="543ed-185">[**データベース**の右側および**DefaultConnection**、] をクリックして、**プレビューのデータベース**リンクします。</span><span class="sxs-lookup"><span data-stu-id="543ed-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![データベースのプレビュー](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="543ed-187">プレビュー ウィンドウでは、ソース データベースのスキーマに一致するデータベース スキーマを作成する対象データベースで実行されるスクリプトを示します。</span><span class="sxs-lookup"><span data-stu-id="543ed-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="543ed-188">スクリプトには、新しい列を追加する ALTER TABLE コマンドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="543ed-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="543ed-189">閉じる、**データベース プレビュー**クリックしてダイアログ ボックスで、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="543ed-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="543ed-190">Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。</span><span class="sxs-lookup"><span data-stu-id="543ed-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="543ed-191">ユーザー情報 ページの実行 (追加*Account/UserInfo.aspx*ホーム ページの URL を)、更新プログラムが正常に展開されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="543ed-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="543ed-192">入力してログインする必要があります*admin*と*devpwd*です。</span><span class="sxs-lookup"><span data-stu-id="543ed-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="543ed-193">既定では、テーブル内のデータが展開されていないと、開発で追加したコメントを検索しないように、実行するには、データの配置スクリプトを構成しませんでした。</span><span class="sxs-lookup"><span data-stu-id="543ed-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="543ed-194">新しいコメントを今すぐ追加するにはステージング環境の変更がデータベースに配置され、ページが正しく動作することを確認します。</span><span class="sxs-lookup"><span data-stu-id="543ed-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="543ed-195">ステージングと運用環境に展開する同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="543ed-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="543ed-196">余分なスクリプトを無効にしてください。</span><span class="sxs-lookup"><span data-stu-id="543ed-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="543ed-197">テストのプロファイルと比較して唯一の違いは、ステージング環境の 1 つだけのスクリプトを無効にするには、実稼働プロファイルのみを実行するように構成されたため*aspnet prod-data.sql*です。</span><span class="sxs-lookup"><span data-stu-id="543ed-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="543ed-198">ステージングと運用環境の資格情報は、管理者、prodpwd がします。</span><span class="sxs-lookup"><span data-stu-id="543ed-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="543ed-199">データベースの変更を含む実際の運用アプリケーションの更新プログラムのも通常実行する配置時にアプリケーションをオフラインをアップロードして*アプリ\_offline.htm*発行を削除する前にその後で学習したよう[前のチュートリアル](deploying-a-code-update.md)です。</span><span class="sxs-lookup"><span data-stu-id="543ed-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="543ed-200">まとめ</span><span class="sxs-lookup"><span data-stu-id="543ed-200">Summary</span></span>

<span data-ttu-id="543ed-201">Code First Migrations および dbDacFx プロバイダーの両方を使用してデータベースの変更が含まれているアプリケーションの更新プログラムを展開したようになりました。</span><span class="sxs-lookup"><span data-stu-id="543ed-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![講習においてインストラクター birthdate ページ](deploying-a-database-update/_static/image8.png)

![ユーザー情報 ページ](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="543ed-204">次のチュートリアルでは、コマンドラインを使用して、展開を実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="543ed-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="543ed-205">[前へ](deploying-a-code-update.md)
> [次へ](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="543ed-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
