---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開する |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="a5e2f-103">Visual Studio を使用した ASP.NET Web 展開: コードの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="a5e2f-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a5e2f-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a5e2f-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="a5e2f-106">この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="a5e2f-107">系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a5e2f-108">概要</span><span class="sxs-lookup"><span data-stu-id="a5e2f-108">Overview</span></span>

<span data-ttu-id="a5e2f-109">初期展開後の維持と、web サイトの開発作業を続行、および時間の長い前にする更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="a5e2f-110">このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセスです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="a5e2f-111">実装し、このチュートリアルで展開する更新プログラムがデータベースの変更を含まない次のチュートリアルではデータベースの変更の展開の違いは何が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="a5e2f-112">アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="a5e2f-113">コードの変更</span><span class="sxs-lookup"><span data-stu-id="a5e2f-113">Make a code change</span></span>

<span data-ttu-id="a5e2f-114">単純な例として、更新プログラムをアプリケーションに、追加、**講習においてインストラクター**  ページで選択した講師コースの一覧です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="a5e2f-115">実行する場合、**講習においてインストラクター**  ページで、かがわかりますがある**選択**グリッド内のリンクを行わない make 以外の何か、行の背景が灰色に変わりますが、します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![講習においてインストラクターの選択 ページ](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="a5e2f-117">これで、ときに実行されるコードを追加、**選択**リンクがクリックされ、選択した講師でコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="a5e2f-118">*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="a5e2f-119">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-119">Run the page and select an instructor.</span></span> <span data-ttu-id="a5e2f-120">そのインストラクターでコースの一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-120">You see a list of courses taught by that instructor.</span></span>

    ![講習においてインストラクター学期コース ページ](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="a5e2f-122">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="a5e2f-123">コードの更新をテスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="a5e2f-124">テスト、ステージング、および実稼働環境に展開する、発行プロファイルを使用することができます、前に、データベースの公開オプションを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="a5e2f-125">メンバーシップ データベースの grant およびデータ配置スクリプトを実行する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="a5e2f-126">開く、 **Web の発行**ContosoUniversity プロジェクトを右クリックしをクリックしてウィザード**発行**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="a5e2f-127">クリックして、**テスト**のプロファイルの**プロファイル**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="a5e2f-128">クリックして、**設定**タブです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="a5e2f-129">[ **DefaultConnection**で、**データベース**] セクションで、クリア、**データベースの更新**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="a5e2f-130">クリックして、**プロファイル** タブをクリックして、**ステージング**のプロファイルの**プロファイル**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="a5e2f-131">変更を保存するように求めは、**テスト**プロファイルで、**はい**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="a5e2f-132">ステージングのプロファイルに同じ変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="a5e2f-133">実稼働プロファイルで、同じ変更を行う手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="a5e2f-134">閉じる、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="a5e2f-135">これで、実行中の 1 回のクリックの単純な処理をもう一度発行は、テスト環境に展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="a5e2f-136">このプロセスを短縮するためには、使用することができます、 **Web 1 つをクリックして 発行**ツールバー。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="a5e2f-137">**ビュー**  メニューの 選択**ツールバー**し、 **Web 1 つをクリックして 発行**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="a5e2f-139">**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="a5e2f-140">**Web 1 つをクリックして 発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印のアイコン)。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="a5e2f-142">Visual Studio 更新済みのアプリケーションを配置して、ブラウザーは、ホーム ページに自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="a5e2f-143">講習においてインストラクター ページを実行し、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="a5e2f-144">通常どおりも回帰テストを行う (つまり、新しい変更で、既存の機能が損なわれていないことを確認するサイトの残りの部分をテストする)。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="a5e2f-145">このチュートリアルでをその手順をスキップし、ステージング環境と運用環境の更新プログラムの展開に進みます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="a5e2f-146">再展開するときにどのファイルが変更を自動的に決定 Web 配置し、コピーのみがサーバーにファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="a5e2f-147">既定では、Web Deploy で最終変更日に基づいてファイル判別どれが変更されました。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="a5e2f-148">いくつかのソース管理システムでは、ファイルの内容を変更しない場合、ファイルがあっても日付に変更します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="a5e2f-149">その場合は、Web デプロイを決定するファイルが変更されたファイルのチェックサムを使用して構成することができます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="a5e2f-150">詳細については、次を参照してください。[理由はすべてのファイルを取得を再展開が、それらを変更していないか。](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 展開 FAQ にします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="a5e2f-151">配置時にオフライン アプリケーションします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-151">Take the application offline during deployment</span></span>

<span data-ttu-id="a5e2f-152">配置する変更は、単純な変更を単一のページを今すぐです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="a5e2f-153">規模の大きな変更を展開することがありますが、またはコードとデータベースの両方の変更を配置するし、サイトが正しく動作しない場合は、ユーザーが展開を完了する前に、ページを要求します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="a5e2f-154">ユーザーが、展開が進行中はサイトへのアクセスをするを防ぐために使用することができます、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="a5e2f-155">という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーに IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="a5e2f-156">アクセスを防ぐため、配置時に、配置するように*アプリ\_offline.htm*ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*後に正常に実行展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="a5e2f-157">Web は、既定値を自動的に展開を構成する*アプリ\_offline.htm*の展開開始時に、サーバー上のファイルし、完了時に削除します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="a5e2f-158">すべてあることを行うには、発行プロファイル (.pubxml) ファイルに次の XML 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="a5e2f-159">このチュートリアルで作成して、ユーザー設定を使用する方法が表示されます*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="a5e2f-160">使用して*アプリ\_offline.htm*ステージング サイトでは必要ありません、ステージング サイトにアクセスする必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="a5e2f-161">すべての実稼働環境で展開する方法をテストするステージングを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="a5e2f-162">アプリを作成する\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="a5e2f-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="a5e2f-163">**ソリューション エクスプ ローラー**ソリューションを右クリックしをクリックして、**追加**、順にクリック**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="a5e2f-164">作成、 **HTML ページ**という*アプリ\_offline.htm* (で最後の"l"を削除、 *.html* Visual Studio を既定で作成する拡張機能)。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="a5e2f-165">テンプレートのマークアップを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="a5e2f-166">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="a5e2f-167">コピー アプリ\_web サイトのルート フォルダーに offline.htm</span><span class="sxs-lookup"><span data-stu-id="a5e2f-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="a5e2f-168">任意の FTP ツールを使用すると、web サイトにファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="a5e2f-169">[FileZilla](http://filezilla-project.org/)は人気のある FTP およびツールのスクリーン ショットに表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="a5e2f-170">FTP ツールを使用する次の 3 つ必要があります: FTP の URL、ユーザー名とパスワード。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="a5e2f-171">URL は、Azure 管理ポータルで、web サイトのダッシュ ボード ページに表示され、ユーザー名と FTP のパスワードは含まれて、 *.publishsettings*以前ダウンロードしたファイルです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="a5e2f-172">次の手順では、これらの値を取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="a5e2f-173">Azure 管理ポータルでをクリックして**Web サイト**タブし、[ステージングの web サイト] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="a5e2f-174">**ダッシュ ボード** ページで、検索での FTP ホストの名前まで下にスクロール、 **Quick Glance**セクションです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP ホスト名](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="a5e2f-176">ステージングを開く*.publishsettings*メモ帳または別のテキスト エディターでファイル。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="a5e2f-177">検索、 `publishProfile` FTP プロファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="a5e2f-178">コピー、`userName`と`userPWD`値。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP の資格情報](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="a5e2f-180">FTP の URL にログインして、FTP ツールを開きます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="a5e2f-181">コピー*アプリ\_offline.htm*をソリューション フォルダーから、 */サイト/wwwroot*ステージング サイト内のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![コピー app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="a5e2f-183">ステージング サイトの URL を参照します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="a5e2f-184">参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![ブラウザーのウィンドウで app_offline.htm](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="a5e2f-186">ステージングを展開する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="a5e2f-187">ステージングと運用環境にコードの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="a5e2f-188">**Web 1 つをクリックして 発行**ツールバーで、選択、**ステージング**発行プロファイルをクリックして**Web の発行**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="a5e2f-189">Visual Studio では、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーが開きます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="a5e2f-190">*アプリ\_offline.htm*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="a5e2f-191">展開の成功を確認するテストすることができます、前に削除する必要があります、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="a5e2f-192">FTP ツールに戻るし、削除**アプリ\_offline.htm**ステージング サイトからです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="a5e2f-193">ブラウザーには、ステージング サイトで講習においてインストラクター ページを開き、更新プログラムが正常に展開されたことを確認するには、あるインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="a5e2f-194">場合と同様ステージング、実稼働の同じ手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="a5e2f-195">変更を確認し、特定のファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="a5e2f-196">Visual Studio 2012 は個々 のファイルを展開することも提供します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="a5e2f-197">選択したファイルのローカル バージョンと、展開されたバージョンの違いを表示、ファイルを送信先の環境に展開したり、送信先の環境からローカルのプロジェクトにファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="a5e2f-198">チュートリアルのこのセクションには、これらの機能を使用する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="a5e2f-199">展開する変更を行う</span><span class="sxs-lookup"><span data-stu-id="a5e2f-199">Make a change to deploy</span></span>

1. <span data-ttu-id="a5e2f-200">開いている*Content/Site.css*のブロックを見つけて、`body`タグ。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="a5e2f-201">値を変更`background-color`から`#fff`に`darkblue`です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="a5e2f-202">公開プレビュー ウィンドウで、変更を表示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="a5e2f-203">使用すると、 **Web の発行**、プロジェクトを発行するウィザードでファイルをダブルクリックして公開されることは、変更を参照してください、**プレビュー**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="a5e2f-204">ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="a5e2f-205">**プロファイル**ドロップダウン リストで、**テスト**プロファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="a5e2f-206">をクリックして**プレビュー**、順にクリック**開始プレビュー**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="a5e2f-207">**プレビュー**  ウィンドウをダブルクリックして**Site.css**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Site.css をダブルクリックします。](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="a5e2f-209">**変更のプレビュー**ダイアログが展開される変更のプレビューを表示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Site.css に加えた変更のプレビュー](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="a5e2f-211">ダブルクリックする場合、 *Web.config*ファイル、**変更のプレビュー**ダイアログ変換の構成、ビルドの効果を示していて、発行プロファイルの変換。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="a5e2f-212">この時点で実行していない原因は、 *Web.config*を変更すると、変更が表示されないため、サーバー上のファイルです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="a5e2f-213">ただし、**変更のプレビュー**ウィンドウが 2 つの変更を正しく表示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="a5e2f-214">2 つの XML 要素が削除するようです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="a5e2f-215">選択すると、発行プロセスでこれらの要素が追加された**実行 Code First Migrations アプリケーション開始時に**Code First コンテキスト クラスです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="a5e2f-216">発行プロセスが削除されませんが、取り消されるように見えるために、それらの要素を追加する前に、比較が行われます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="a5e2f-217">このエラーは、将来のリリースで修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="a5e2f-218">**[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-218">Click **Close**.</span></span>
6. <span data-ttu-id="a5e2f-219">**[発行]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-219">Click **Publish**.</span></span>
7. <span data-ttu-id="a5e2f-220">テスト サイトのホーム ページを開くと、ブラウザー、CTRL + f5 キーを押して CSS の変更の結果を確認するためにハード更新が発生します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![CSS の変更の結果](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="a5e2f-222">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="a5e2f-223">ソリューション エクスプ ローラーからの特定のファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="a5e2f-224">などの背景色が青おらず、元の色に戻したいとします。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="a5e2f-225">このセクションの内容から直接特定のファイルを発行することによって、元の設定を復元**ソリューション エクスプ ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="a5e2f-226">開いている*Content/Site.css*と復元、`background-color`設定`#fff`です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="a5e2f-227">**ソリューション エクスプ ローラー**を右クリックし、 *Content/Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="a5e2f-228">コンテキスト メニューでは、3 つのパブリッシュ オプションを示します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-228">The context menu shows three publish options.</span></span>

    ![ソリューション エクスプ ローラーからのオプションのパブリッシュ](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="a5e2f-230">をクリックして**Site.css に加えた変更のプレビュー**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="a5e2f-231">送信先の環境に、ローカル ファイルとそのバージョンの違いを表示するウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="a5e2f-233">**ソリューション エクスプ ローラー**を右クリックして**Site.css**再度 をクリック**発行 Site.css**です。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="a5e2f-234">**Web 公開アクティビティ**ファイルが公開されているウィンドウを示しています。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web 公開活動ウィンドウ](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="a5e2f-236">ブラウザーを開いて、 `http://localhost/contosouniversity` URL、およびし、CTRL + f5 キーを押してハードが発生する変更、CSS の効果を確認するために更新します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![通常の CSS のホーム ページ](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="a5e2f-238">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="a5e2f-239">まとめ</span><span class="sxs-lookup"><span data-stu-id="a5e2f-239">Summary</span></span>

<span data-ttu-id="a5e2f-240">データベースの変更を含まないアプリケーションの更新プログラムを展開するいくつかの方法を確認したようになりましたし、新機能が更新されますが期待どおりに表示を確認する変更をプレビューする方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="a5e2f-241">講習においてインストラクター ページのようになりましたが、**コース学期**セクションです。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-241">The Instructors page now has a **Courses Taught** section.</span></span>

![講習においてインストラクター学期コース ページ](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="a5e2f-243">次のチュートリアルは、データベースの変更を展開する方法を示します: データベースと講習においてインストラクター ページには、生年月日フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="a5e2f-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a5e2f-244">[前へ](deploying-to-production.md)
> [次へ](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="a5e2f-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
