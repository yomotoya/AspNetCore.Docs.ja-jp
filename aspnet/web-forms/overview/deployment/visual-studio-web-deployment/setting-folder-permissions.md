---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Visual Studio を使用して ASP.NET Web 展開: フォルダーのアクセス許可の設定 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: a0c4f9f7cf30c1fc6a06c2cf798dc7ed04585504
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383650"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a><span data-ttu-id="131ae-103">Visual Studio を使用して ASP.NET Web 展開: フォルダーのアクセス許可の設定</span><span class="sxs-lookup"><span data-stu-id="131ae-103">ASP.NET Web Deployment using Visual Studio: Setting Folder Permissions</span></span>
====================
<span data-ttu-id="131ae-104">によって[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="131ae-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="131ae-105">スタート プロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="131ae-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="131ae-106">このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="131ae-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="131ae-107">系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="131ae-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="131ae-108">概要</span><span class="sxs-lookup"><span data-stu-id="131ae-108">Overview</span></span>

<span data-ttu-id="131ae-109">このチュートリアルでは、フォルダーのアクセス許可の設定、 *Elmah*フォルダーにデプロイされた web サイト、アプリケーションは、そのフォルダー内のログ ファイルを作成できるようにします。</span><span class="sxs-lookup"><span data-stu-id="131ae-109">In this tutorial, you set folder permissions for the *Elmah* folder in the deployed web site so that the application can create log files in that folder.</span></span>

<span data-ttu-id="131ae-110">Visual Studio 開発サーバー (Cassini) または IIS Express を使用して Visual Studio の web アプリケーションをテストする場合は、自分のユーザー名、アプリケーションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="131ae-110">When you test a web application in Visual Studio using the Visual Studio Development Server (Cassini) or IIS Express, the application runs under your identity.</span></span> <span data-ttu-id="131ae-111">開発用コンピューターの管理者であるほとんどの場合と、任意のフォルダー内の任意のファイルに何もする完全な権限を持ちます。</span><span class="sxs-lookup"><span data-stu-id="131ae-111">You are most likely an administrator on your development computer and have full authority to do anything to any file in any folder.</span></span> <span data-ttu-id="131ae-112">アプリケーション プールに割り当てられているサイトに対して定義されている id の下で実行されます IIS 下でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="131ae-112">But when an application runs under IIS, it runs under the identity defined for the application pool that the site is assigned to.</span></span> <span data-ttu-id="131ae-113">これは、通常、アクセス許可が制限されているシステム定義のアカウントです。</span><span class="sxs-lookup"><span data-stu-id="131ae-113">This is typically a system-defined account that has limited permissions.</span></span> <span data-ttu-id="131ae-114">既定でに読み取りし、web アプリケーションのファイルおよびフォルダーに対する execute 権限が、書き込みアクセス権がありません。</span><span class="sxs-lookup"><span data-stu-id="131ae-114">By default it has read and execute permissions on your web application's files and folders, but it doesn't have write access.</span></span>

<span data-ttu-id="131ae-115">これは、場合は、アプリケーションを作成するか、web アプリケーションで必要な更新プログラムのファイルは、共通が問題になります。</span><span class="sxs-lookup"><span data-stu-id="131ae-115">This becomes an issue if your application creates or updates files, which is a common need in web applications.</span></span> <span data-ttu-id="131ae-116">Elmah で XML ファイルの作成、Contoso University アプリケーションで、 *Elmah*エラーの詳細を保存するにはフォルダー。</span><span class="sxs-lookup"><span data-stu-id="131ae-116">In the Contoso University application, Elmah creates XML files in the *Elmah* folder in order to save details about errors.</span></span> <span data-ttu-id="131ae-117">Elmah のようなものを使用しない場合でも、サイトは、ユーザーがファイルをアップロードまたはサイトのフォルダーにデータを書き込むことが他のタスクを実行できるように可能性があります。</span><span class="sxs-lookup"><span data-stu-id="131ae-117">Even if you don't use something like Elmah, your site might let users upload files or perform other tasks that write data to a folder in your site.</span></span>

<span data-ttu-id="131ae-118">リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。</span><span class="sxs-lookup"><span data-stu-id="131ae-118">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="test-error-logging-and-reporting"></a><span data-ttu-id="131ae-119">テスト エラーのログ記録とレポート</span><span class="sxs-lookup"><span data-stu-id="131ae-119">Test error logging and reporting</span></span>

<span data-ttu-id="131ae-120">どのアプリケーションが動作しない正しく IIS で (ただし、Visual Studio でテストされたときに行った) を表示する、Elmah は、によってログが記録される通常を開き、Elmah のエラー ログの詳細を表示するエラーが発生することができます。</span><span class="sxs-lookup"><span data-stu-id="131ae-120">To see how the application doesn't work correctly in IIS (although it did when you tested it in Visual Studio), you can cause an error that would normally be logged by Elmah, and then open the Elmah error log to see the details.</span></span> <span data-ttu-id="131ae-121">Elmah が XML ファイルを作成し、エラーの詳細を格納することでない場合は、空のエラー レポートを参照してください。</span><span class="sxs-lookup"><span data-stu-id="131ae-121">If Elmah was unable to create an XML file and store the error details, you see an empty error report.</span></span>

<span data-ttu-id="131ae-122">ブラウザーを開きに移動`http://localhost/ContosoUniversity`のように、無効な URL を要求し、 *Studentsxxx.aspx*します。</span><span class="sxs-lookup"><span data-stu-id="131ae-122">Open a browser and go to `http://localhost/ContosoUniversity`, and then request an invalid URL like *Studentsxxx.aspx*.</span></span> <span data-ttu-id="131ae-123">代わりにシステムが生成したエラー ページを参照してください、 *GenericErrorPage.aspx*ため、ページ、 `customErrors` Web.config ファイルの設定は"RemoteOnly"と、ローカル IIS を実行しています。</span><span class="sxs-lookup"><span data-stu-id="131ae-123">You see a system-generated error page instead of the *GenericErrorPage.aspx* page because the `customErrors` setting in the Web.config file is "RemoteOnly" and you are running IIS locally:</span></span>

![HTTP 404 エラー ページ](setting-folder-permissions/_static/image1.png)

<span data-ttu-id="131ae-125">今すぐ実行*Elmah.axd*エラー レポートを表示します。</span><span class="sxs-lookup"><span data-stu-id="131ae-125">Now run *Elmah.axd* to see the error report.</span></span> <span data-ttu-id="131ae-126">管理者アカウントの資格情報でログインした後 (&quot;管理者&quot;と&quot;devpwd&quot;)、Elmah での XML ファイルを作成できなかったために、空のエラー ログのページを参照してください、 *Elmah*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="131ae-126">After you log in with the administrator account credentials (&quot;admin&quot; and &quot;devpwd&quot;), you see an empty error log page because Elmah was unable to create an XML file in the *Elmah* folder:</span></span>

![エラー ログ空](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a><span data-ttu-id="131ae-128">Elmah フォルダーに対する書き込みアクセス許可の設定</span><span class="sxs-lookup"><span data-stu-id="131ae-128">Set write permission on the Elmah folder</span></span>

<span data-ttu-id="131ae-129">フォルダーのアクセス許可を手動で設定することができますか、自動展開プロセスの部分を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="131ae-129">You can set folder permissions manually or you can make it an automatic part of the deployment process.</span></span> <span data-ttu-id="131ae-130">複雑な MSBuild コードでは、自動を行う必要があり、次の手順を手動で行う方法、最初にデプロイするときに行うだけであるためです。</span><span class="sxs-lookup"><span data-stu-id="131ae-130">Making it automatic requires complex MSBuild code, and since you only have to do this the first time you deploy, the following steps how to do it manually.</span></span> <span data-ttu-id="131ae-131">(展開プロセスのこの部分を作成する方法については、次を参照してください[で Web の発行フォルダーのアクセス許可の設定](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx)Sayed Hashimi's ブログ。)。</span><span class="sxs-lookup"><span data-stu-id="131ae-131">(For information about how to make this part of the deployment process, see [Setting Folder Permissions on Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) on Sayed Hashimi's blog.)</span></span>

1. <span data-ttu-id="131ae-132">**ファイル エクスプ ローラー**に移動します*C:\inetpub\wwwroot\ContosoUniversity*します。</span><span class="sxs-lookup"><span data-stu-id="131ae-132">In **File Explorer**, navigate to *C:\inetpub\wwwroot\ContosoUniversity*.</span></span> <span data-ttu-id="131ae-133">右クリックし、 *Elmah*フォルダーで、**プロパティ**を選び、**セキュリティ**タブ。</span><span class="sxs-lookup"><span data-stu-id="131ae-133">Right-click the *Elmah* folder, select **Properties**, and then select the **Security** tab.</span></span>
2. <span data-ttu-id="131ae-134">**[編集]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="131ae-134">Click **Edit**.</span></span>
3. <span data-ttu-id="131ae-135">**Elmah のアクセス許可**ダイアログ ボックスで、 **DefaultAppPool**を選び、**書き込み** チェック ボックス、**許可**列。</span><span class="sxs-lookup"><span data-stu-id="131ae-135">In the **Permissions for Elmah** dialog box, select **DefaultAppPool**, and then select the **Write** check box in the **Allow** column.</span></span>

    ![ELMAH フォルダーのアクセス許可](setting-folder-permissions/_static/image3.png)

    <span data-ttu-id="131ae-137">(表示されない場合**DefaultAppPool**で、**グループまたはユーザー名**一覧で、おそらくメソッドを使用していくつかその他のこのチュートリアルでは指定されているコンピューターに IIS および ASP.NET 4 を設定します。</span><span class="sxs-lookup"><span data-stu-id="131ae-137">(If you don't see **DefaultAppPool** in the **Group or user names** list, you probably used some other method than the one specified in this tutorial to set up IIS and ASP.NET 4 on your computer.</span></span> <span data-ttu-id="131ae-138">その場合は、どのような id は、Contoso University アプリケーションに割り当てられているアプリケーション プールに使用されます。 確認し、その id に書き込みアクセス権を付与します。</span><span class="sxs-lookup"><span data-stu-id="131ae-138">In that case, find out what identity is used by the application pool assigned to the Contoso University application, and grant write permission to that identity.</span></span> <span data-ttu-id="131ae-139">このチュートリアルの最後にアプリケーション プールの id に関するリンクを表示します。)クリックして**OK**両方のダイアログ ボックス。</span><span class="sxs-lookup"><span data-stu-id="131ae-139">See the links about application pool identities at the end of this tutorial.) Click **OK** in both dialog boxes.</span></span>

## <a name="retest-error-logging-and-reporting"></a><span data-ttu-id="131ae-140">エラーのログ記録とレポートを再テストします。</span><span class="sxs-lookup"><span data-stu-id="131ae-140">Retest error logging and reporting</span></span>

<span data-ttu-id="131ae-141">もう一度 (無効な URL を要求する) と同様に、エラーを発生させることによってテストし、実行、**エラー ログ**ページ。</span><span class="sxs-lookup"><span data-stu-id="131ae-141">Test by causing an error again in the same way (request a bad URL) and run the **Error Log** page.</span></span> <span data-ttu-id="131ae-142">この時間、ページで、エラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="131ae-142">This time the error appears on the page.</span></span>

![ELMAH エラー ログ ページ](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a><span data-ttu-id="131ae-144">まとめ</span><span class="sxs-lookup"><span data-stu-id="131ae-144">Summary</span></span>

<span data-ttu-id="131ae-145">完了しましたの Contoso University のために必要なタスクをすべてローカル コンピューターに IIS で正常に動作します。</span><span class="sxs-lookup"><span data-stu-id="131ae-145">You have now completed all of the tasks necessary to get Contoso University working correctly in IIS on your local computer.</span></span> <span data-ttu-id="131ae-146">次のチュートリアルでは、サイト パブリックに利用できるようにする Azure にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="131ae-146">In the next tutorial, you will make the site publicly available by deploying it to Azure.</span></span>

## <a name="more-information"></a><span data-ttu-id="131ae-147">詳細情報</span><span class="sxs-lookup"><span data-stu-id="131ae-147">More information</span></span>

<span data-ttu-id="131ae-148">この例では、Elmah されたログ ファイルを保存できない理由は明らかでした。</span><span class="sxs-lookup"><span data-stu-id="131ae-148">In this example, the reason why Elmah was unable to save log files was fairly obvious.</span></span> <span data-ttu-id="131ae-149">問題の原因が; ほど明白ではない場合に IIS のトレースを使用することができます。参照してください[トラブルシューティング失敗した要求を使用してトレース IIS 7 で](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net サイト。</span><span class="sxs-lookup"><span data-stu-id="131ae-149">You can use IIS tracing in cases where the cause of the problem is not so obvious; see [Troubleshooting Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) on the IIS.net site.</span></span>

<span data-ttu-id="131ae-150">アプリケーション プール id へのアクセス許可を付与する方法についての詳細については、次を参照してください。[アプリケーション プール Id](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)と[ファイル システム Acl を IIS 内のコンテンツをセキュリティで保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net サイト。</span><span class="sxs-lookup"><span data-stu-id="131ae-150">For more information about how to grant permissions to application pool identities, see [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) and [Secure Content in IIS Through File System ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) on the IIS.net site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="131ae-151">[前へ](deploying-to-iis.md)
> [次へ](deploying-to-production.md)</span><span class="sxs-lookup"><span data-stu-id="131ae-151">[Previous](deploying-to-iis.md)
[Next](deploying-to-production.md)</span></span>
