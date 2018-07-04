---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio を使用して ASP.NET Web 展開: コードの更新の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3649f0250e830b76c42d832e51038c2c52ed4561
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368215"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="67905-103">Visual Studio を使用して ASP.NET Web 展開: コードの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="67905-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="67905-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="67905-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="67905-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="67905-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="67905-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="67905-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="67905-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="67905-108">概要</span><span class="sxs-lookup"><span data-stu-id="67905-108">Overview</span></span>

<span data-ttu-id="67905-109">初期のデプロイ後の保守および web サイトの開発作業が引き続き発生してまで長い間は、更新プログラムを展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="67905-110">このチュートリアルでは、アプリケーション コードへの更新プログラムの展開のプロセス説明。</span><span class="sxs-lookup"><span data-stu-id="67905-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="67905-111">データベースの変更を伴わない更新プログラムを実装し、このチュートリアルでデプロイします。次のチュートリアルでデータベースの変更の展開に関する相違点を確認します。</span><span class="sxs-lookup"><span data-stu-id="67905-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="67905-112">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。</span><span class="sxs-lookup"><span data-stu-id="67905-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="67905-113">コードの変更</span><span class="sxs-lookup"><span data-stu-id="67905-113">Make a code change</span></span>

<span data-ttu-id="67905-114">アプリケーションへの更新プログラムの簡単な例とに追加します、 **Instructors**ページ、選択したインストラクターが指導するコースのリスト。</span><span class="sxs-lookup"><span data-stu-id="67905-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="67905-115">実行する場合、 **Instructors**ページが表示されます**選択**グリッド内のリンクが何もしないこと以外、行の背景が灰色に変わりますが、します。</span><span class="sxs-lookup"><span data-stu-id="67905-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Instructors ページを選択した場合](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="67905-117">追加するときに実行するコード、**選択**リンクがクリックされ、選択したインストラクター指導するコースの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="67905-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="67905-118">*Instructors.aspx*、次のマークアップを追加した直後に、 **ErrorMessageLabel** `Label`コントロール。</span><span class="sxs-lookup"><span data-stu-id="67905-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="67905-119">ページを実行し、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="67905-119">Run the page and select an instructor.</span></span> <span data-ttu-id="67905-120">そのインストラクターが指導するコースのリストを参照してください。</span><span class="sxs-lookup"><span data-stu-id="67905-120">You see a list of courses taught by that instructor.</span></span>

    ![Instructors ページに指導するコース](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="67905-122">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="67905-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="67905-123">テスト環境にコードの更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="67905-124">発行プロファイルを使用して、テスト、ステージング、および運用環境にデプロイすることができます、前に、データベースの公開オプションを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="67905-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="67905-125">メンバーシップ データベースの grant およびデータの配置スクリプトを実行する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="67905-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="67905-126">開く、 **Web の発行**を ContosoUniversity プロジェクトを右クリックしてウィザード**発行**します。</span><span class="sxs-lookup"><span data-stu-id="67905-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="67905-127">をクリックして、**テスト**のプロファイルの**プロファイル**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="67905-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="67905-128">をクリックして、**設定**タブ。</span><span class="sxs-lookup"><span data-stu-id="67905-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="67905-129">[ **DefaultConnection**で、**データベース**] セクションで、クリア、 **Update database**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="67905-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="67905-130">をクリックして、**プロファイル**タブをクリックし、をクリックし、**ステージング**のプロファイルの**プロファイル**ドロップダウン リスト。</span><span class="sxs-lookup"><span data-stu-id="67905-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="67905-131">加えられた変更を保存するように求め、**テスト**プロファイルで、**はい**します。</span><span class="sxs-lookup"><span data-stu-id="67905-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="67905-132">ステージングのプロファイルに同じ変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="67905-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="67905-133">運用プロファイルで同じ変更を行うプロセスを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="67905-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="67905-134">閉じる、 **Web の発行**ウィザード。</span><span class="sxs-lookup"><span data-stu-id="67905-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="67905-135">テスト環境へのデプロイが実行 1 回のクリックの簡単な処理をもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="67905-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="67905-136">このプロセスを短縮するためには、使用することができます、 **Web の 1 クリックして発行**ツールバー。</span><span class="sxs-lookup"><span data-stu-id="67905-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="67905-137">**ビュー** ] メニューの [選択**ツールバー**選び**Web の 1 クリックして発行**します。</span><span class="sxs-lookup"><span data-stu-id="67905-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="67905-139">**ソリューション エクスプ ローラー**、ContosoUniversity プロジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="67905-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="67905-140">**Web の 1 クリックして発行**ツールバーで、選択、**テスト**発行プロファイルをクリックして**Web の発行**(左向きおよび右向きの矢印の付いたアイコン)。</span><span class="sxs-lookup"><span data-stu-id="67905-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="67905-142">Visual Studio が更新されたアプリケーションを配置し、ブラウザーはホーム ページに自動的に開きます。</span><span class="sxs-lookup"><span data-stu-id="67905-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="67905-143">Instructors ページを実行し、更新プログラムが正常にデプロイされたことを確認するインストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="67905-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="67905-144">通常どおり回帰テストを行うことも (つまり、新しい変更が、既存の機能を中断していないことを確認するには、サイトの残りの部分をテストする)。</span><span class="sxs-lookup"><span data-stu-id="67905-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="67905-145">このチュートリアルでをその手順をスキップし、ステージングと運用環境に更新を展開に進みます。</span><span class="sxs-lookup"><span data-stu-id="67905-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="67905-146">を再デプロイするときに Web デプロイ、変更されたファイルを自動的に決定し、コピーのみがサーバーにファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="67905-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="67905-147">既定では、Web Deploy は、最終変更日をファイルに変更されたものを判断使用します。</span><span class="sxs-lookup"><span data-stu-id="67905-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="67905-148">いくつかのソース管理システムでは、ファイルの内容を変更しないと、ファイルが偶数の日付に変更します。</span><span class="sxs-lookup"><span data-stu-id="67905-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="67905-149">その場合は、Web 配置ファイルのチェックサムを使用して、変更されたファイルを確認するを構成する場合があります。</span><span class="sxs-lookup"><span data-stu-id="67905-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="67905-150">詳細については、次を参照してください。[理由はすべてのファイル再デプロイが、それらを変更していないか。](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET 配置のよく寄せられる質問。</span><span class="sxs-lookup"><span data-stu-id="67905-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="67905-151">デプロイ時にアプリケーションをオフライン</span><span class="sxs-lookup"><span data-stu-id="67905-151">Take the application offline during deployment</span></span>

<span data-ttu-id="67905-152">デプロイする変更は、1 つのページを簡単に変更を今すぐです。</span><span class="sxs-lookup"><span data-stu-id="67905-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="67905-153">大規模な変更を展開することがありますまたはコードとデータベースの変更を配置して、サイトが正しく動作しない場合、デプロイが完了する前に、ユーザーがページを要求します。</span><span class="sxs-lookup"><span data-stu-id="67905-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="67905-154">ユーザーが、展開が進行中はサイトへのアクセスをするを防ぐために使用することができます、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="67905-155">という名前のファイルを配置すると*アプリ\_offline.htm*アプリケーションのルート フォルダーで IIS に自動的にファイルが表示されますそのアプリケーションを実行する代わりにします。</span><span class="sxs-lookup"><span data-stu-id="67905-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="67905-156">アクセスを防ぐため、デプロイ時に、配置するように*アプリ\_offline.htm* 、ルート フォルダーで、展開プロセスを実行し、削除*アプリ\_offline.htm*後成功展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="67905-157">Web は、既定値を自動的に展開を構成する*アプリ\_offline.htm*展開を開始した場合に、サーバー上のファイルし、完了すると、それを削除します。</span><span class="sxs-lookup"><span data-stu-id="67905-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="67905-158">行う必要があることを行うには、発行プロファイル (.pubxml) ファイルに次の XML 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="67905-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="67905-159">このチュートリアルで作成し、カスタムを使用する方法が紹介*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="67905-160">使用して*アプリ\_offline.htm*ステージング サイトでは必要ありません、ステージング サイトにアクセスする必要がないためです。</span><span class="sxs-lookup"><span data-stu-id="67905-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="67905-161">すべての実稼働環境で展開する方法をテストするステージングを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="67905-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="67905-162">アプリの作成\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="67905-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="67905-163">**ソリューション エクスプ ローラー**ソリューションを右クリックし、クリックして**追加**、 をクリックし、**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="67905-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="67905-164">作成、 **HTML ページ**という*アプリ\_offline.htm* (で最後の"l"を削除、 *.html* Visual Studio は既定で作成する拡張機能)。</span><span class="sxs-lookup"><span data-stu-id="67905-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="67905-165">テンプレート マークアップを次のマークアップに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="67905-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="67905-166">ファイルを保存して閉じます。</span><span class="sxs-lookup"><span data-stu-id="67905-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="67905-167">アプリのコピー\_offline.htm web サイトのルート フォルダーに</span><span class="sxs-lookup"><span data-stu-id="67905-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="67905-168">任意の FTP ツールを使用して、web サイトにファイルをコピーすることができます。</span><span class="sxs-lookup"><span data-stu-id="67905-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="67905-169">[FileZilla](http://filezilla-project.org/)一般的な FTP ツールであり、スクリーン ショットに表示されます。</span><span class="sxs-lookup"><span data-stu-id="67905-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="67905-170">FTP ツールを使用するには、するには、次の 3 つが必要です。: FTP の URL、ユーザー名、およびパスワード。</span><span class="sxs-lookup"><span data-stu-id="67905-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="67905-171">URL は、Azure 管理ポータルでの web サイトのダッシュ ボード ページに表示され、ユーザー名と FTP のパスワードで見つかんだことができます、 *.publishsettings*以前ダウンロードしたファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="67905-172">次の手順では、これらの値を取得する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="67905-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="67905-173">Azure 管理ポータルで次のようにクリックします。 **Websites**タブと、ステージング web サイトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="67905-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="67905-174">**ダッシュ ボード** ページで、FTP ホスト名が見つかるまでスクロール、**概要**セクション。</span><span class="sxs-lookup"><span data-stu-id="67905-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![FTP ホスト名](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="67905-176">ステージングを開く *.publishsettings*メモ帳または別のテキスト エディターでファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="67905-177">検索、 `publishProfile` FTP プロファイルの要素。</span><span class="sxs-lookup"><span data-stu-id="67905-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="67905-178">コピー、`userName`と`userPWD`値。</span><span class="sxs-lookup"><span data-stu-id="67905-178">Copy the `userName` and `userPWD` values.</span></span>

    ![FTP の資格情報](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="67905-180">FTP ツールと FTP の URL にログオンを開きます。</span><span class="sxs-lookup"><span data-stu-id="67905-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="67905-181">コピー*アプリ\_offline.htm*をソリューション フォルダーから、 *wwwroot/サイト/* ステージング サイト内のフォルダー。</span><span class="sxs-lookup"><span data-stu-id="67905-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![App_offline をコピーします。](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="67905-183">ステージング サイトの URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="67905-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="67905-184">参照してください、*アプリ\_offline.htm*ホーム ページではなくページが表示されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="67905-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![ブラウザーのウィンドウで app_offline.htm](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="67905-186">ステージング環境にデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="67905-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="67905-187">ステージングと運用環境にコード更新を展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="67905-188">**Web の 1 クリックして発行**ツールバーで、選択、**ステージング**発行プロファイルをクリックして**Web の発行**します。</span><span class="sxs-lookup"><span data-stu-id="67905-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="67905-189">Visual Studio は、更新されたアプリケーションを展開し、サイトのホーム ページをブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="67905-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="67905-190">*アプリ\_offline.htm*ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67905-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="67905-191">削除する必要がありますを正常にデプロイを確認するテストする前に、*アプリ\_offline.htm*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="67905-192">FTP ツールに戻るし、削除**アプリ\_offline.htm**ステージング サイトからします。</span><span class="sxs-lookup"><span data-stu-id="67905-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="67905-193">ブラウザーで、ステージング サイトで、Instructors ページを開くし、更新プログラムが正常にデプロイされたことを確認する、インストラクターを選択します。</span><span class="sxs-lookup"><span data-stu-id="67905-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="67905-194">ステージングしたとき、運用環境と同じ手順に従います。</span><span class="sxs-lookup"><span data-stu-id="67905-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="67905-195">変更内容を確認し、特定のファイルを展開します。</span><span class="sxs-lookup"><span data-stu-id="67905-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="67905-196">Visual Studio 2012 は個々 のファイルを展開する機能も提供します。</span><span class="sxs-lookup"><span data-stu-id="67905-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="67905-197">選択したファイルには、ローカル バージョンと、展開されたバージョンの違いを表示し、ファイルを展開先の環境に展開または移行先の環境からローカルのプロジェクトにファイルをコピーすることができます。</span><span class="sxs-lookup"><span data-stu-id="67905-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="67905-198">チュートリアルのこのセクションでこれらの機能を使用する方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="67905-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="67905-199">変更をデプロイするには</span><span class="sxs-lookup"><span data-stu-id="67905-199">Make a change to deploy</span></span>

1. <span data-ttu-id="67905-200">開いている*Content/Site.css*のブロックを見つけて、`body`タグ。</span><span class="sxs-lookup"><span data-stu-id="67905-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="67905-201">値を変更`background-color`から`#fff`に`darkblue`します。</span><span class="sxs-lookup"><span data-stu-id="67905-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="67905-202">発行のプレビュー ウィンドウで、変更を表示します。</span><span class="sxs-lookup"><span data-stu-id="67905-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="67905-203">使用すると、 **Web の発行**、プロジェクトを発行するウィザードでファイルをダブルクリックして発行すると、変更を参照してください、**プレビュー**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="67905-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="67905-204">ContosoUniversity プロジェクトを右クリックし、をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="67905-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="67905-205">**プロファイル**ドロップダウン リストで、**テスト**プロファイルを公開します。</span><span class="sxs-lookup"><span data-stu-id="67905-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="67905-206">クリックして**プレビュー**、 をクリックし、**プレビューの開始**します。</span><span class="sxs-lookup"><span data-stu-id="67905-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="67905-207">**プレビュー**ウィンドウで、ダブルクリックして**Site.css**します。</span><span class="sxs-lookup"><span data-stu-id="67905-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Site.css をダブルクリックします。](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="67905-209">**変更のプレビュー**ダイアログが展開される変更のプレビューが表示されます。</span><span class="sxs-lookup"><span data-stu-id="67905-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Site.css に変更のプレビュー](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="67905-211">ダブルクリックする場合、 *Web.config*ファイル、**変更のプレビュー**ダイアログは、構成の変換、ビルドの効果を示していて、発行プロファイルの変換。</span><span class="sxs-lookup"><span data-stu-id="67905-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="67905-212">この時点で完了していないことになるもの、 *Web.config*その変更が表示されないことを想定して、変更するサーバー上のファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="67905-213">ただし、**変更のプレビュー**ウィンドウで 2 つの変更が正しく表示されます。</span><span class="sxs-lookup"><span data-stu-id="67905-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="67905-214">2 つの XML 要素が削除されるように見えます。</span><span class="sxs-lookup"><span data-stu-id="67905-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="67905-215">選択すると、発行プロセスでこれらの要素が追加された**実行 Code First Migrations アプリケーションの起動時に**Code First のコンテキスト クラス。</span><span class="sxs-lookup"><span data-stu-id="67905-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="67905-216">発行プロセスが削除されませんが削除されているように表示されます、それらの要素を追加する前に、比較が行われます。</span><span class="sxs-lookup"><span data-stu-id="67905-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="67905-217">このエラーは、将来のリリースで修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="67905-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="67905-218">**[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="67905-218">Click **Close**.</span></span>
6. <span data-ttu-id="67905-219">**[発行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="67905-219">Click **Publish**.</span></span>
7. <span data-ttu-id="67905-220">テスト サイトのホーム ページを開くと、ブラウザー、CSS の変更の影響を確認するためにハードの更新が発生する CTRL + f5 キーを押します。</span><span class="sxs-lookup"><span data-stu-id="67905-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![CSS の変更の影響](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="67905-222">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="67905-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="67905-223">ソリューション エクスプ ローラーの特定のファイルを発行します。</span><span class="sxs-lookup"><span data-stu-id="67905-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="67905-224">背景色が青し元の色に戻す対象にしないとします。</span><span class="sxs-lookup"><span data-stu-id="67905-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="67905-225">このセクションでは、特定のファイルから直接発行することによって、元の設定を復元します**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="67905-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="67905-226">開いている*Content/Site.css*と復元、`background-color`設定`#fff`します。</span><span class="sxs-lookup"><span data-stu-id="67905-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="67905-227">**ソリューション エクスプ ローラー**を右クリックし、 *Content/Site.css*ファイル。</span><span class="sxs-lookup"><span data-stu-id="67905-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="67905-228">コンテキスト メニューでは、3 つの発行オプションを示しています。</span><span class="sxs-lookup"><span data-stu-id="67905-228">The context menu shows three publish options.</span></span>

    ![ソリューション エクスプ ローラーからオプションのパブリッシュ](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="67905-230">クリックして**Site.css に加えた変更のプレビュー**します。</span><span class="sxs-lookup"><span data-stu-id="67905-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="67905-231">移行先の環境でローカル ファイルとそのバージョンの違いを表示するウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="67905-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![差分-コンテンツ/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="67905-233">**ソリューション エクスプ ローラー**を右クリックして**Site.css**もう一度クリックします**発行 Site.css**。</span><span class="sxs-lookup"><span data-stu-id="67905-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="67905-234">**Web 発行アクティビティ**ファイルが公開されているウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="67905-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Web 発行アクティビティ ウィンドウ](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="67905-236">ブラウザーを開いて、 `http://localhost/contosouniversity` URL とキーを押しますがハードに CTRL + f5 キーを変更、CSS の効果を確認するために更新します。</span><span class="sxs-lookup"><span data-stu-id="67905-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![通常の CSS を使用してホーム ページ](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="67905-238">ブラウザーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="67905-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="67905-239">まとめ</span><span class="sxs-lookup"><span data-stu-id="67905-239">Summary</span></span>

<span data-ttu-id="67905-240">データベースの変更が関係しないアプリケーションの更新プログラムを展開するいくつかの方法が表示されました、ことを確認するどのような更新が期待どおりの変更をプレビューする方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="67905-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="67905-241">Instructors ページのようになりましたが、**指導するコース**セクション。</span><span class="sxs-lookup"><span data-stu-id="67905-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Instructors ページに指導するコース](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="67905-243">次のチュートリアルでは、データベースの変更をデプロイする方法を示します。 データベースと、Instructors ページには、birthdate フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="67905-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="67905-244">[前へ](deploying-to-production.md)
> [次へ](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="67905-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
