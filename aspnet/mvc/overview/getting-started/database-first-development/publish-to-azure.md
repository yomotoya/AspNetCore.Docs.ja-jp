---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Azure Database First の MVC サイトを発行 |Microsoft Docs
author: Rick-Anderson
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021639"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="0e904-104">Azure Database First の MVC サイトを発行します。</span><span class="sxs-lookup"><span data-stu-id="0e904-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="0e904-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0e904-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0e904-106">MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="0e904-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="0e904-107">このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0e904-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="0e904-108">生成されたコードは、データベース テーブル内の列に対応します。</span><span class="sxs-lookup"><span data-stu-id="0e904-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="0e904-109">シリーズのこの部分は、web アプリとデータベースを Azure に発行について説明します。</span><span class="sxs-lookup"><span data-stu-id="0e904-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="0e904-110">Web アプリとデータベースを発行する方法について説明しますが、実際には、チュートリアルの先頭から開始する必要がありますの手順を実行する、このトピックを読むことができます。</span><span class="sxs-lookup"><span data-stu-id="0e904-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="0e904-111">参照してください[Getting Started](setting-up-database.md)します。</span><span class="sxs-lookup"><span data-stu-id="0e904-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="0e904-112">Azure で web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="0e904-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="0e904-113">Azure アカウントに、このチュートリアルを完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="0e904-114">できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="0e904-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="0e904-115">できます[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="0e904-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="0e904-116">Web アプリを発行するには、プロジェクトを右クリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![サイトを発行します。](publish-to-azure/_static/image1.png)

<span data-ttu-id="0e904-118">Microsoft Azure の web サイトを選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-118">Select Microsoft Azure Websites.</span></span>

![Azure を選択します。](publish-to-azure/_static/image2.png)

<span data-ttu-id="0e904-120">Azure にサインインしていない場合は、Azure アカウントの資格情報を提供します。</span><span class="sxs-lookup"><span data-stu-id="0e904-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="0e904-121">次に、新しい web アプリを作成する新規を選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-121">Then, select New to create a new web app.</span></span>

![新しいサイト](publish-to-azure/_static/image3.png)

<span data-ttu-id="0e904-123">Web アプリの一意の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="0e904-123">Create a unique name for your web app.</span></span> <span data-ttu-id="0e904-124">[名前] フィールドの右側に緑色のチェック マークが表示される場合、名前は一意ですがするわかります。</span><span class="sxs-lookup"><span data-stu-id="0e904-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="0e904-125">Web アプリのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-125">Select a region for your web app.</span></span> <span data-ttu-id="0e904-126">選択**新しいサーバーの作成**データベースのこの新しいデータベース サーバーのユーザー名とパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="0e904-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="0e904-127">完了したら、クリックして**作成**です。</span><span class="sxs-lookup"><span data-stu-id="0e904-127">When finished, click **Create**.</span></span>

![サイトを作成します。](publish-to-azure/_static/image4.png)

<span data-ttu-id="0e904-129">接続の値がすべて設定されました。</span><span class="sxs-lookup"><span data-stu-id="0e904-129">Your connection values are now all set.</span></span> <span data-ttu-id="0e904-130">これらの値をそのままにしておくことができます。</span><span class="sxs-lookup"><span data-stu-id="0e904-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="0e904-132">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e904-132">Click **Next**.</span></span>

<span data-ttu-id="0e904-133">設定については、通知の 2 つのデータベース接続が指定された - ApplicationDBContext と ContosoUniversityDataEntities します。</span><span class="sxs-lookup"><span data-stu-id="0e904-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="0e904-134">ApplicationDBContext は、ユーザー アカウントのテーブルの接続です。</span><span class="sxs-lookup"><span data-stu-id="0e904-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="0e904-135">これらの値は、データベースの接続文字列のみを表示します。</span><span class="sxs-lookup"><span data-stu-id="0e904-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="0e904-136">サイトを発行するときに、これらのデータベースを公開することは限りません。</span><span class="sxs-lookup"><span data-stu-id="0e904-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="0e904-137">Web アプリの発行が完了した後、データベース プロジェクトが発行されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="0e904-138">データベース接続の横にある省略記号 (...) では、接続文字列の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="0e904-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="0e904-139">ContosoUniversityDataEntities の横にある省略記号をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e904-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="0e904-140">データベース サーバーとデータベースの名前に注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="0e904-141">サーバー名がランダムに生成されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-141">The server name is randomly generated.</span></span> <span data-ttu-id="0e904-142">データベース名は単純なを使用してサイトの名前 **\_db**追加されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="0e904-143">データベースを発行するときに、両方の名前を後で必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="0e904-144">クリックして**OK**データベース接続文字列 ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="0e904-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="0e904-145">Web の発行] ウィンドウで、[**次**プレビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="0e904-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="0e904-146">発行するファイルの一覧を表示するには、プレビューの開始をクリックすることができます。</span><span class="sxs-lookup"><span data-stu-id="0e904-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="0e904-147">これは、このサイトを発行した最初の時間であるために、一覧は、すべての関連するプロジェクト ファイルになります。</span><span class="sxs-lookup"><span data-stu-id="0e904-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="0e904-148">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e904-148">Click **Publish**.</span></span>

<span data-ttu-id="0e904-149">[出力] ウィンドウ、パブリケーションの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="0e904-150">発行した後は、サイトは、web ブラウザーで起動 immedialely が。</span><span class="sxs-lookup"><span data-stu-id="0e904-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="0e904-151">サイトがデプロイされ、サイトに新しいユーザーを登録することができます。ただし、ContosoUniversityData プロジェクトにテーブルがまだ公開されていません。</span><span class="sxs-lookup"><span data-stu-id="0e904-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="0e904-152">受講者のリンクの一覧をクリックすると、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="0e904-153">データベースを SQL Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="0e904-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="0e904-154">データベースを発行する前に、ローカル コンピューターは、データベース サーバーに接続できることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="0e904-155">データベース サーバーのファイアウォールは、マシンがデータベースに接続可能を制限します。</span><span class="sxs-lookup"><span data-stu-id="0e904-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="0e904-156">ファイアウォールの許可された IP アドレスに、コンピューターの IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="0e904-157">Azure portal で Azure アカウントにログインします。</span><span class="sxs-lookup"><span data-stu-id="0e904-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="0e904-158">新しいデータベースを選択し、選択**管理**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-158">Select your new database and select **Manage**.</span></span>

![管理します。](publish-to-azure/_static/image10.png)

<span data-ttu-id="0e904-160">お使いのコンピューターから接続を許可するデータベース サーバーを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="0e904-161">管理 を選択すると、データベース サーバーに許可されているように、現在の IP アドレスを追加するように求められます。</span><span class="sxs-lookup"><span data-stu-id="0e904-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="0e904-162">[はい] を選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-162">Select Yes.</span></span>

![ip アドレスを追加します。](publish-to-azure/_static/image11.png)

<span data-ttu-id="0e904-164">前の手順で追加した IP アドレスは、接続用に構成する必要がある唯一の IP アドレスではない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="0e904-165">接続を正しく設定されているかどうかをデータベースにログインしようとすることができます。</span><span class="sxs-lookup"><span data-stu-id="0e904-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="0e904-166">ユーザーと以前に作成したパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="0e904-166">Provide the user and password you created earlier.</span></span>

![ログイン](publish-to-azure/_static/image12.png)

<span data-ttu-id="0e904-168">エラー メッセージを受信する場合は、別の IP アドレスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="0e904-169">エラーに関する詳細を表示するエラー メッセージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e904-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="0e904-170">詳細を追加する必要のある IP アドレスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="0e904-171">この IP アドレスに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-171">Note this IP address.</span></span>

![許可されていません](publish-to-azure/_static/image13.png)

<span data-ttu-id="0e904-173">このログイン ウィンドウを閉じるし、Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="0e904-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="0e904-174">データベースのダッシュ ボードに移動します。</span><span class="sxs-lookup"><span data-stu-id="0e904-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="0e904-175">クリックして**使用できる IP アドレス管理**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-175">Click **Manage allowed IP addresses**.</span></span>

![ip アドレスを管理します。](publish-to-azure/_static/image14.png)

<span data-ttu-id="0e904-177">エラー メッセージから IP アドレスを追加する必要がありますようになりました。</span><span class="sxs-lookup"><span data-stu-id="0e904-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="0e904-178">含める 1 つのエラー メッセージから許可された IP アドレスの範囲を変更するか、個別のエントリとしてその IP アドレスを追加します。</span><span class="sxs-lookup"><span data-stu-id="0e904-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![新しいアドレスを追加します。](publish-to-azure/_static/image15.png)

<span data-ttu-id="0e904-180">許可された IP アドレスに変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="0e904-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="0e904-181">管理、 をクリックして、データベースにログインを再試行してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="0e904-182">許可された IP アドレスが正しく構成されて、ファイアウォールの前に数分待機する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="0e904-183">データベースに正常にログインすることができます、ときに、データベースへの接続の設定が完了しました。</span><span class="sxs-lookup"><span data-stu-id="0e904-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="0e904-184">おくことができます 管理ウィンドウでこの開いてすぐデータベース デプロイの結果は確認のためです。</span><span class="sxs-lookup"><span data-stu-id="0e904-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="0e904-185">データベース プロジェクトに戻ります。</span><span class="sxs-lookup"><span data-stu-id="0e904-185">Return to your database project.</span></span> <span data-ttu-id="0e904-186">プロジェクトを右クリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-186">Right-click the project and select **Publish**.</span></span>

![データベースを発行します。](publish-to-azure/_static/image16.png)

<span data-ttu-id="0e904-188">データベースの公開 ウィンドウで次のように選択します。**編集**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-188">In the Publish Database window, select **Edit**.</span></span>

![編集](publish-to-azure/_static/image17.png)

<span data-ttu-id="0e904-190">サーバーのデータベース サーバーと、認証資格情報の名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="0e904-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="0e904-191">資格情報を提供するには、利用可能なデータベースの一覧から作成したデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="0e904-192">既定では、Visual Studio は、作成したデータベースと同じである可能性がありますいないプロジェクトの名前をデータベース フィールドの名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e904-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="0e904-193">[OK] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0e904-193">Click OK.</span></span>

<span data-ttu-id="0e904-194">すべての接続情報を再入力しなくても、今後の更新プログラムを公開できるように、このプロファイルを保存するが可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="0e904-195">**[プロファイルの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0e904-195">Select **Create Profile**.</span></span>

![プロファイルを保存します。](publish-to-azure/_static/image19.png)

<span data-ttu-id="0e904-197">同じ名前のプロジェクトにファイルが作成されます**ContosoUniversityData.publish.xml**します。</span><span class="sxs-lookup"><span data-stu-id="0e904-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="0e904-198">次回データベースを Azure に発行するは、単にそのプロファイルを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="0e904-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="0e904-199">をクリックして**発行**を Azure にデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e904-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="0e904-201">実行後、しばらくの間の実行結果の発行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="0e904-203">ここで、することができますに戻り、管理ポータル、データベースの。</span><span class="sxs-lookup"><span data-stu-id="0e904-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="0e904-204">デザイン ビューを更新し、3 つのテーブルに自動的に入力データが配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0e904-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![新しいテーブル](publish-to-azure/_static/image22.png)

<span data-ttu-id="0e904-206">Azure にデプロイされている web アプリをテストする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="0e904-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="0e904-207">Azure で web アプリに移動します (など http://contosositeexample.azurewebsites.net/)します。</span><span class="sxs-lookup"><span data-stu-id="0e904-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="0e904-208">受講者の一覧のリンクをクリックし、受講者の index ビューを表示する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-208">Click the link for List of students and you should see the index view for students.</span></span>

![ビュー](publish-to-azure/_static/image23.png)

<span data-ttu-id="0e904-210">場合によっては、データベースと接続は、適切に構成するのには、少し時間を必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e904-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="0e904-211">エラーが発生した場合数分待ってから、もう一度やり直してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="0e904-212">まとめ</span><span class="sxs-lookup"><span data-stu-id="0e904-212">Conclusion</span></span>

<span data-ttu-id="0e904-213">このシリーズでは、ユーザーを編集、更新、作成、およびデータを削除できるように既存のデータベースからコードを生成する方法の簡単な例が用意されています。</span><span class="sxs-lookup"><span data-stu-id="0e904-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="0e904-214">ASP.NET MVC 5、Entity Framework および ASP.NET スキャフォールディングは、プロジェクトの作成に使用されます。</span><span class="sxs-lookup"><span data-stu-id="0e904-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="0e904-215">Code First の開発の基本的な例を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)します。</span><span class="sxs-lookup"><span data-stu-id="0e904-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="0e904-216">高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションの Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。</span><span class="sxs-lookup"><span data-stu-id="0e904-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="0e904-217">Database First のデータ処理に使用する DbContext API は Code First のデータを操作するために使用する API と同じことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0e904-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="0e904-218">Database First を使用する場合でも、コードの最初のチュートリアルからなど、同時実行の競合を処理、読み取りと、関連するデータの更新などのより複雑なシナリオを処理する方法を学習できます。</span><span class="sxs-lookup"><span data-stu-id="0e904-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="0e904-219">唯一の違いは、データベース、コンテキストのクラスおよびエンティティ クラスを作成する方法には。</span><span class="sxs-lookup"><span data-stu-id="0e904-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0e904-220">前へ</span><span class="sxs-lookup"><span data-stu-id="0e904-220">Previous</span></span>](enhancing-data-validation.md)
