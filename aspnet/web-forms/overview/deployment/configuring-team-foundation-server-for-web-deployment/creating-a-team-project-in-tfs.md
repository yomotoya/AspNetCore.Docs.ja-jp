---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: TFS でチーム プロジェクトの作成 |Microsoft Docs
author: jrjlee
description: このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397562"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="2ccc8-103">TFS でチーム プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="2ccc8-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="2ccc8-104">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2ccc8-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2ccc8-105">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="2ccc8-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2ccc8-106">このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="2ccc8-107">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="2ccc8-108">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="2ccc8-108">Task Overview</span></span>

<span data-ttu-id="2ccc8-109">プロビジョニングして、TFS で新しいチーム プロジェクトを使用するには、これらの大まかな手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="2ccc8-110">新しいチーム プロジェクトを作成したユーザーにアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="2ccc8-111">チーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-111">Create the team project.</span></span>
- <span data-ttu-id="2ccc8-112">プロジェクトで作業するチーム メンバーにアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="2ccc8-113">一部のコンテンツで確認します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-113">Check in some content.</span></span>

<span data-ttu-id="2ccc8-114">このトピックがこれらの手順を実行する方法を示し、ユーザーと各プロシージャを担当する可能性のあるジョブの役割は識別します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="2ccc8-115">によって、組織の構造、これらの各タスクがありますを他のユーザーの責任を認識します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="2ccc8-116">タスクとチュートリアルでは、このトピックでは、インストールされているし、TFS が構成されているし、構成プロセスの一部としてチーム プロジェクト コレクションを作成したことを想定しています。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="2ccc8-117">詳細については、このような想定とシナリオの一般的な背景情報をご覧ください[Web 配置の TFS のビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="2ccc8-118">チーム プロジェクトの作成者のアクセス許可します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="2ccc8-119">新しいチーム プロジェクトを作成するためには、これらのアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="2ccc8-120">必要があります、**新しいプロジェクトを作成**TFS アプリケーション層に対する権限。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="2ccc8-121">一般にユーザーを追加してこのアクセス許可を付与する、**プロジェクト コレクション管理者**TFS グループ。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="2ccc8-122">**Team Foundation 管理者**グローバル グループにもこのアクセス許可が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="2ccc8-123">TFS のチーム プロジェクト コレクションに対応する SharePoint サイト コレクション内の新しいチーム サイトを作成するアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="2ccc8-124">通常の SharePoint グループにユーザーを追加することでこのアクセス許可を付与する**フルコントロール**権限を SharePoint サイト コレクション。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="2ccc8-125">メンバーである必要がある SQL Server Reporting Services の機能を使用している場合、 **Team Foundation Content Manager** Reporting services のロール。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2ccc8-126">これらの手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="2ccc8-127">通常、ユーザーまたは TFS の配置を管理グループはまた、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="2ccc8-128">これは、高い特権を持つ権限のセットであるため、新しいチーム プロジェクトはごく一部のユーザーによって TFS の配置を管理するための責任で通常作成されます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="2ccc8-129">開発者が通常は付与されない新しいチーム プロジェクトを作成するために必要なアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="2ccc8-130">TFS のアクセス許可の付与</span><span class="sxs-lookup"><span data-stu-id="2ccc8-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="2ccc8-131">ユーザーを追加する最初の高レベルのタスクは、新しいチーム プロジェクトを作成するユーザーを有効にする場合、**プロジェクト コレクション管理者**チーム プロジェクト コレクションのグループ化します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="2ccc8-132">**プロジェクト コレクション管理者グループにユーザーを追加するには**</span><span class="sxs-lookup"><span data-stu-id="2ccc8-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="2ccc8-133">TFS サーバー上で、**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Team Foundation Server 2010**、順にクリックします**Team Foundation管理コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="2ccc8-134">ナビゲーション ツリー ビューで、展開**アプリケーション層**、 をクリックし、**チーム プロジェクト コレクション**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="2ccc8-135">**チーム プロジェクト コレクション**ウィンドウで、チーム プロジェクトを管理するコレクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="2ccc8-136">**全般**] タブで [**グループ メンバーシップ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="2ccc8-137">**グローバル グループ**ダイアログ ボックスで、**プロジェクト コレクション管理者**グループ化、およびクリックして**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="2ccc8-138">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="2ccc8-139">**ユーザー、コンピューター、またはグループ**新しいチーム プロジェクトを作成しユーザーのユーザー名 をクリックして ダイアログ ボックスで、「**名前の確認**、順にクリックします**OK**.</span><span class="sxs-lookup"><span data-stu-id="2ccc8-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="2ccc8-140">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="2ccc8-141">**グローバル グループ**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="2ccc8-142">SharePoint Services のアクセス許可の付与</span><span class="sxs-lookup"><span data-stu-id="2ccc8-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="2ccc8-143">次に、TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクションで新しいチーム サイトを作成するユーザーのアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="2ccc8-144">**SharePoint サイト コレクションに対するフル コントロール権限を付与**</span><span class="sxs-lookup"><span data-stu-id="2ccc8-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="2ccc8-145">Team Foundation Server 管理コンソールで、**チーム プロジェクト コレクション** ページで、管理するチーム プロジェクト コレクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="2ccc8-146">**SharePoint サイト** タブの値に注意してください、**現在既定サイトの場所**URL。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="2ccc8-147">Internet Explorer を開きし、手順 2 で書き留めた URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ccc8-148">しないにログオンしている Windows チーム プロジェクト コレクションを作成したユーザーとしての場合は、このユーザーとして続行するには SharePoint にサインインします。 必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="2ccc8-149">**サイトの操作** メニューのをクリックして**サイト設定**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="2ccc8-150">**サイト設定** ページ **ユーザーおよびアクセス許可**、 をクリックして**ユーザーとグループ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="2ccc8-151">左側のナビゲーション パネル で、**グループ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="2ccc8-152">**ユーザーとグループ: すべてのグループ**] ページで [**このサイトのグループの設定**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="2ccc8-153">表示される、 <strong>HTTP 404 Not Found</strong>エラー二重 HTTP エンコード バグが発生しました。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="2ccc8-154">この場合、この URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="2ccc8-155">`[site_collection_URL]/_layouts/permsetup.aspx` 例えば：</span><span class="sxs-lookup"><span data-stu-id="2ccc8-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="2ccc8-156">**このサイトのグループの設定**ページで、チーム プロジェクトを作成するユーザーを追加、**所有者**グループ化、およびクリックして **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="2ccc8-157">チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するための詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可の設定](https://msdn.microsoft.com/library/dd547204.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="2ccc8-158">新しいチーム プロジェクトを作成し、ユーザーの追加</span><span class="sxs-lookup"><span data-stu-id="2ccc8-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="2ccc8-159">必要なアクセス許可がある場合とを使用できます、**チーム エクスプ ローラー**新しいチーム プロジェクトを作成する Visual Studio 2010 でのウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="2ccc8-160">このアプローチでは、必要なすべての情報を収集し、TFS、SharePoint、および SQL Server Reporting Services で必要なタスクを実行するウィザードを提供します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="2ccc8-161">また、追加し、コンテンツを変更できるように、開発者チームのメンバーに、新しいチーム プロジェクトに対するアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2ccc8-162">これらの手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="2ccc8-163">通常、TFS 管理者または開発者のチーム リーダーは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="2ccc8-164">新しいチーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-164">Create a New Team Project</span></span>

<span data-ttu-id="2ccc8-165">次の手順では、TFS 2010 で、新しいチーム プロジェクトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="2ccc8-166">**新しいチーム プロジェクトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="2ccc8-166">**To create a new team project**</span></span>

1. <span data-ttu-id="2ccc8-167">**開始**メニューで、**すべてのプログラム**、 をクリックして**Microsoft Visual Studio 2010**、右クリックして**Microsoft Visual Studio 2010**、クリックして**管理者として実行**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ccc8-168">Visual Studio 2010 を管理者として実行しない、最後の手順で、新しいチーム プロジェクト ウィザードは失敗します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="2ccc8-169">場合、**ユーザー アカウント制御** ダイアログ ボックスが表示されたら、をクリックして**はい**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="2ccc8-170">Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ccc8-171">TFS サーバーへの接続を既に構成した場合は、手順 4 ~ 7 を省略できます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="2ccc8-172">**チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="2ccc8-173">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="2ccc8-174">**Team Foundation Server の追加** ダイアログ ボックスでは、TFS インスタンスの詳細を入力し、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="2ccc8-175">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="2ccc8-176">**チーム プロジェクトへの接続**ダイアログ ボックスで、TFS インスタンスを接続するチームを選択するプロジェクト コレクションに追加し、クリックする**Connect**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="2ccc8-177">**チーム エクスプ ローラー**ウィンドウで、右クリックし、チーム プロジェクト コレクション をクリックして**新しいチーム プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="2ccc8-178">**新しいチーム プロジェクト**ダイアログ ボックスで、名前と、チーム プロジェクトの説明を指定し、順にクリックします**次**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ccc8-179">チーム プロジェクトには、スペースが含まれている場合は、するように、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して出力パスからパッケージをデプロイするときに、いくつかの問題を発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="2ccc8-180">パスにスペースによりするよりずっと困難で Web Deploy コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="2ccc8-181">**プロセス テンプレートを選択**をクリックして、開発プロセスの管理を使用するプロセス テンプレートの選択 ページで、**次**。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ccc8-182">Tfs プロセス テンプレートの詳細については、次を参照してください。[プロセス テンプレートとツール](https://msdn.microsoft.com/vstudio/aa718795)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="2ccc8-183">**チーム サイトの設定** ページで変更せずに、既定の設定のままにしてをクリックし、**次**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="2ccc8-184">この設定を作成、または TFS のチーム プロジェクトに関連付けられている SharePoint チーム サイトを識別します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="2ccc8-185">開発チームは、ドキュメントの管理、スレッドのディスカッションに参加、wiki ページを作成、およびコードに関連していないその他の各種のタスクを実行する、このサイトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="2ccc8-186">詳細については、次を参照してください。 [SharePoint 製品との間の相互作用と Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="2ccc8-187">**ソース管理設定の指定** ページで変更せずに、既定の設定のままにしてをクリックし、**次**。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="2ccc8-188">この設定は、識別またはコンテンツのルート フォルダーとして動作する TFS フォルダー階層内の場所を作成します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="2ccc8-189">**チーム プロジェクト設定の確認**] ページで [**完了**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="2ccc8-190">ときに、新しいチーム プロジェクトが正常に作成で、**チーム プロジェクト作成**] ページで [**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="2ccc8-191">チーム プロジェクトにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-191">Add Users to a Team Project</span></span>

<span data-ttu-id="2ccc8-192">新しいチーム プロジェクトを作成するので、追加して、コンテンツの共同作業を開始することをユーザーに権限を付与できます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="2ccc8-193">**チーム プロジェクトにユーザーを追加するには**</span><span class="sxs-lookup"><span data-stu-id="2ccc8-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="2ccc8-194">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクトを右クリックし、 をポイント**チーム プロジェクトの設定**、順にクリックします**グループ メンバーシップ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="2ccc8-195">追加、変更、およびソース管理下にあるコードを削除するユーザーを有効にするのに自分を追加する必要があります、**共同作成者**グループ。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="2ccc8-196">**プロジェクト グループ**ダイアログ ボックスで、**共同作成者**グループ化、およびクリックして**プロパティ**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="2ccc8-197">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="2ccc8-198">**ユーザー、コンピューター、またはグループ**、チーム プロジェクトに追加するユーザーのユーザー名 をクリックして ダイアログ ボックスで、「**名前の確認**、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="2ccc8-199">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="2ccc8-200">**プロジェクト グループ**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2ccc8-201">まとめ</span><span class="sxs-lookup"><span data-stu-id="2ccc8-201">Conclusion</span></span>

<span data-ttu-id="2ccc8-202">この時点で、新しいチーム プロジェクトを使用する準備が、開発者チームは、コンテンツを追加して、開発プロセスでの共同作業を開始できます。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="2ccc8-203">次のトピックでは、[をソース管理に追加するコンテンツ](adding-content-to-source-control.md)、コンテンツ ソースのコントロールを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="2ccc8-204">関連項目</span><span class="sxs-lookup"><span data-stu-id="2ccc8-204">Further Reading</span></span>

<span data-ttu-id="2ccc8-205">TFS でチーム プロジェクトを作成する方法より広範なガイダンスについては、次を参照してください。[チーム プロジェクト作成](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="2ccc8-206">チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するための詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可の設定](https://msdn.microsoft.com/library/dd547204.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="2ccc8-207">チーム プロジェクトにユーザーを追加する方法の詳細については、次を参照してください。[チーム プロジェクトへのユーザーの追加](https://msdn.microsoft.com/library/bb558971.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2ccc8-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ccc8-208">[前へ](configuring-team-foundation-server-for-web-deployment.md)
> [次へ](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="2ccc8-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
