---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: TFS でチーム プロジェクトの作成 |Microsoft ドキュメント
author: jrjlee
description: このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880428"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="2933c-103">TFS でチーム プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="2933c-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="2933c-104">によって[Jason lee 著](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="2933c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="2933c-105">PDF をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2933c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="2933c-106">このトピックでは、Team Foundation Server (TFS) 2010 で、新しいチーム プロジェクトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2933c-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="2933c-107">このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="2933c-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="2933c-108">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="2933c-108">Task Overview</span></span>

<span data-ttu-id="2933c-109">プロビジョニングし、TFS で、新しいチーム プロジェクトを使用するには、これらの大まかな手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2933c-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="2933c-110">新しいチーム プロジェクトを作成したユーザーに権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="2933c-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="2933c-111">チーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2933c-111">Create the team project.</span></span>
- <span data-ttu-id="2933c-112">プロジェクトで作業するチーム メンバーに権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="2933c-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="2933c-113">一部のコンテンツで確認してください。</span><span class="sxs-lookup"><span data-stu-id="2933c-113">Check in some content.</span></span>

<span data-ttu-id="2933c-114">このトピックの内容が、次の手順を実行する方法を示すし、ユーザーおよび各プロシージャを担当する可能性が高いジョブ ロールが識別します。</span><span class="sxs-lookup"><span data-stu-id="2933c-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="2933c-115">、、組織の構造に応じてこれらの各タスク場合がある別のユーザーの責任において、注意してください。</span><span class="sxs-lookup"><span data-stu-id="2933c-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="2933c-116">タスクとここでチュートリアルで作成した構成プロセスの一部としてチーム プロジェクト コレクションおよびインストールし、TFS を構成したことを想定しています。</span><span class="sxs-lookup"><span data-stu-id="2933c-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="2933c-117">これらの前提条件の詳細については、シナリオでは、全般的な背景情報については、次を参照してください。 [Web 配置を TFS のビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="2933c-118">チーム プロジェクト作成者に権限を付与します。</span><span class="sxs-lookup"><span data-stu-id="2933c-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="2933c-119">新しいチーム プロジェクトを作成するのには、これらのアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="2933c-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="2933c-120">必要があります、**プロジェクトを新規作成**が TFS アプリケーション層にアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="2933c-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="2933c-121">一般にユーザーを追加することでこのアクセス許可を付与する、**プロジェクト コレクション管理者**TFS グループ。</span><span class="sxs-lookup"><span data-stu-id="2933c-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="2933c-122">**Team Foundation 管理者**グローバル グループにもこのアクセス許可が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2933c-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="2933c-123">TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクション内の新しいチーム サイトを作成する権限が必要です。</span><span class="sxs-lookup"><span data-stu-id="2933c-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="2933c-124">SharePoint グループにユーザーを追加することでこのアクセス許可を付与する通常**フル コントロール**サイト コレクションを SharePoint の権限です。</span><span class="sxs-lookup"><span data-stu-id="2933c-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="2933c-125">SQL Server Reporting Services 機能を使用している場合は、メンバーをする必要があります、 **Team Foundation Content Manager** Reporting Services のロール。</span><span class="sxs-lookup"><span data-stu-id="2933c-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2933c-126">ユーザーには、これらの手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="2933c-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="2933c-127">通常、人や、TFS の配置を管理するグループはまた、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2933c-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="2933c-128">これは権限の高い特権を持つセットであるため、新しいチーム プロジェクトはユーザーの小さなサブセットによって TFS の配置の管理を行う通常作成されます。</span><span class="sxs-lookup"><span data-stu-id="2933c-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="2933c-129">開発者は通常与えてはいけません新しいチーム プロジェクトの作成に必要なアクセス許可。</span><span class="sxs-lookup"><span data-stu-id="2933c-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="2933c-130">TFS のアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="2933c-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="2933c-131">新しいチーム プロジェクトを作成するユーザーを有効にするには、最初の高レベルのタスクがユーザーを追加する、**プロジェクト コレクション管理者**チーム プロジェクト コレクションにグループ化します。</span><span class="sxs-lookup"><span data-stu-id="2933c-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="2933c-132">**プロジェクト コレクション管理者グループにユーザーを追加するには**</span><span class="sxs-lookup"><span data-stu-id="2933c-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="2933c-133">TFS サーバー上で、**開始** メニューのをポイント**すべてのプログラム**、 をクリックして**Microsoft Team Foundation Server 2010**、クリックして**Team Foundation管理コンソール**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="2933c-134">ナビゲーション ツリー ビューで、展開**アプリケーション層**、クリックして**チーム プロジェクト コレクション**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="2933c-135">**チーム プロジェクト コレクション**ペインで、チーム プロジェクトを管理するコレクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2933c-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="2933c-136">**全般** タブで、をクリックして**グループ メンバーシップ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="2933c-137">**グローバル グループ**ダイアログ ボックスで、**プロジェクト コレクション管理者**グループ化、およびクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="2933c-138">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="2933c-139">**ユーザー、コンピューター、またはグループ**新しいチーム プロジェクトを作成できるようにユーザーのユーザー名をクリックしてダイアログ ボックスで、「**名前の確認**、順にクリック**OK**.</span><span class="sxs-lookup"><span data-stu-id="2933c-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="2933c-140">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="2933c-141">**グローバル グループ**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="2933c-142">SharePoint サービスにアクセス許可の付与</span><span class="sxs-lookup"><span data-stu-id="2933c-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="2933c-143">次に、TFS チーム プロジェクト コレクションに対応する SharePoint サイト コレクションで新しいチーム サイトを作成するユーザー権限を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2933c-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="2933c-144">**SharePoint サイト コレクションに対するフル コントロール権限を付与**</span><span class="sxs-lookup"><span data-stu-id="2933c-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="2933c-145">Team Foundation Server 管理コンソールでの**チーム プロジェクト コレクション** ページで、管理するチーム プロジェクト コレクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2933c-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="2933c-146">**SharePoint サイト** タブの値に注意してください、**既定のサイトは、常に現在**URL。</span><span class="sxs-lookup"><span data-stu-id="2933c-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="2933c-147">Internet Explorer を開き、手順 2 で書き留めた URL に進みます。</span><span class="sxs-lookup"><span data-stu-id="2933c-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2933c-148">しているログオンしていない場合 Windows をチーム プロジェクト コレクションを作成したユーザーとして、このユーザーに続行するのには SharePoint にサインインします。 する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2933c-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="2933c-149">**サイトの操作** メニューのをクリックして**サイト設定**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="2933c-150">**サイト設定** ページの **ユーザーと権限**、 をクリックして**ユーザーとグループ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="2933c-151">左側のナビゲーション パネル で、**グループ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="2933c-152">**ユーザーとグループ: すべてのグループ**] ページで [**このサイトのグループの設定**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="2933c-153">表示される、 <strong>HTTP 404 Not Found</strong>二重 HTTP エンコード バグによるエラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="2933c-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="2933c-154">この場合、この URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2933c-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="2933c-155">[<em>サイト コレクション URL</em>]/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="2933c-155">[<em>site collection URL</em>]/\_layouts/permsetup.aspx</span></span>  
   > <span data-ttu-id="2933c-156">例えば:</span><span class="sxs-lookup"><span data-stu-id="2933c-156">For example:</span></span>  
   > <span data-ttu-id="2933c-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span><span class="sxs-lookup"><span data-stu-id="2933c-157">http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx</span></span>
8. <span data-ttu-id="2933c-158">**このサイトのグループの設定**ページで、チーム プロジェクトを作成したユーザーを追加、**所有者**グループ化、およびをクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-158">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="2933c-159">チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するユーザーを有効にする方法の詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可を設定](https://msdn.microsoft.com/library/dd547204.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-159">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="2933c-160">新しいチーム プロジェクトを作成し、ユーザーの追加</span><span class="sxs-lookup"><span data-stu-id="2933c-160">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="2933c-161">必要なアクセス許可がある場合を使用して、**チーム エクスプ ローラー**新しいチーム プロジェクトを作成する Visual Studio 2010 でのウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="2933c-161">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="2933c-162">この方法は、必要なすべての情報を収集し、TFS、SharePoint、および SQL Server Reporting Services で必要なタスクを実行するウィザードを提供します。</span><span class="sxs-lookup"><span data-stu-id="2933c-162">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="2933c-163">またを追加し、内容を変更するようにする、開発者チームのメンバーに、新しいチーム プロジェクトに対するアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2933c-163">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="2933c-164">ユーザーには、これらの手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="2933c-164">Who Performs These Procedures?</span></span>

<span data-ttu-id="2933c-165">通常、TFS 管理者または開発者のチーム リーダーは、これらの手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="2933c-165">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="2933c-166">新しいチーム プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2933c-166">Create a New Team Project</span></span>

<span data-ttu-id="2933c-167">次の手順では、TFS 2010 で、新しいチーム プロジェクトを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2933c-167">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="2933c-168">**新しいチーム プロジェクトを作成するには**</span><span class="sxs-lookup"><span data-stu-id="2933c-168">**To create a new team project**</span></span>

1. <span data-ttu-id="2933c-169">**開始** メニューのをポイント**すべてのプログラム**をクリックして**Microsoft Visual Studio 2010**を右クリックして**Microsoft Visual Studio 2010**、クリックして**管理者として実行**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-169">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2933c-170">管理者として Visual Studio 2010 を実行しない、最後の手順で、新しいチーム プロジェクト ウィザードは失敗します。</span><span class="sxs-lookup"><span data-stu-id="2933c-170">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="2933c-171">場合、**ユーザー アカウント制御** ダイアログ ボックスが表示されたら、をクリックして**はい**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-171">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="2933c-172">Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-172">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2933c-173">既に TFS サーバーへの接続を構成した場合は、手順 4. ~ 7. を省略できます。</span><span class="sxs-lookup"><span data-stu-id="2933c-173">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="2933c-174">**チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-174">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="2933c-175">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-175">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="2933c-176">**Team Foundation Server の追加**ダイアログ ボックスでは、TFS インスタンスの詳細を提供し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-176">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="2933c-177">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-177">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="2933c-178">**チーム プロジェクトへ接続** ダイアログ ボックスで、TFS インスタンスが接続するチームを選択するプロジェクト コレクションに追加し、をクリックする**接続**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-178">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="2933c-179">**チーム エクスプ ローラー**ウィンドウで、右クリックし、チーム プロジェクト コレクション、をクリックして**新しいチーム プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-179">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="2933c-180">**新しいチーム プロジェクト**ダイアログ ボックスでは、名前と、チーム プロジェクトの説明を入力し、をクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-180">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2933c-181">チーム プロジェクトには、スペースが含まれている場合 (Web 配置) インターネット インフォメーション サービス (IIS) Web 展開ツールを使用して出力パスからパッケージを配置に到達するといくつかの問題に直面可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2933c-181">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="2933c-182">パスにスペースが困難よりずっと Web Deploy コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="2933c-182">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="2933c-183">**プロセス テンプレートの選択** ページで、使用して、開発プロセスを管理し、をクリックするプロセス テンプレートを選択**次**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-183">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2933c-184">TFS のプロセス テンプレートの詳細については、次を参照してください。[プロセス テンプレートとツール](https://msdn.microsoft.com/vstudio/aa718795)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-184">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="2933c-185">**チーム サイトの設定** ページで変更せずに、既定の設定のままにしてをクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-185">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="2933c-186">この設定は、作成すると、または TFS チーム プロジェクトに関連付けられている SharePoint チーム サイトを識別します。</span><span class="sxs-lookup"><span data-stu-id="2933c-186">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="2933c-187">開発チームは、ドキュメントの管理、ディスカッション スレッドに参加、wiki ページを作成するコードに関連付けられていないその他の各種のタスクを実行し、このサイトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2933c-187">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="2933c-188">詳細については、次を参照してください。 [Team Foundation Server と SharePoint 製品との間の相互作用](https://msdn.microsoft.com/library/ms253177.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-188">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="2933c-189">**ソース管理の設定の指定** ページで変更せずに、既定の設定のままにしてをクリックして**次**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-189">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="2933c-190">この設定を識別または、TFS フォルダー階層内のあるコンテンツのルート フォルダーとして機能する場所を作成します。</span><span class="sxs-lookup"><span data-stu-id="2933c-190">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="2933c-191">**チーム プロジェクトの設定の確認**] ページで [**完了**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-191">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="2933c-192">ときに、新しいチーム プロジェクトが正常が作成した、**チーム プロジェクト作成** ページで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-192">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="2933c-193">チーム プロジェクトにユーザーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2933c-193">Add Users to a Team Project</span></span>

<span data-ttu-id="2933c-194">これで、新しいチーム プロジェクトを作成した、追加して、コンテンツに対する共同作業を開始するようにするユーザーに権限を付与できます。</span><span class="sxs-lookup"><span data-stu-id="2933c-194">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="2933c-195">**ユーザーをチーム プロジェクトに追加するには**</span><span class="sxs-lookup"><span data-stu-id="2933c-195">**To add users to a team project**</span></span>

1. <span data-ttu-id="2933c-196">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウで、チーム プロジェクトを右クリックし、順にポイント**チーム プロジェクトの設定**、順にクリック**グループ メンバーシップ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-196">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="2933c-197">追加、変更、およびソース管理下にあるコードを削除するユーザーを有効にするには、その人に追加する必要があります、**コントリビューター**グループ。</span><span class="sxs-lookup"><span data-stu-id="2933c-197">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="2933c-198">**プロジェクト グループ**ダイアログ ボックスで、**コントリビューター**グループ化、およびをクリックして**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-198">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="2933c-199">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、 **Windows ユーザーまたはグループ**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-199">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="2933c-200">**ユーザー、コンピューター、またはグループ**をチーム プロジェクトに追加するユーザーのユーザー名をクリックしてダイアログ ボックスで、型**名前の確認**、クリックして**ok**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-200">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="2933c-201">**Team Foundation Server グループのプロパティ**ダイアログ ボックスで、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-201">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="2933c-202">**プロジェクト グループ**ダイアログ ボックスで、をクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="2933c-202">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="2933c-203">まとめ</span><span class="sxs-lookup"><span data-stu-id="2933c-203">Conclusion</span></span>

<span data-ttu-id="2933c-204">この時点では、使用するには、新しいチーム プロジェクト準備が整うし、開発者チームは、コンテンツを追加して、開発プロセスで共同作業を開始できます。</span><span class="sxs-lookup"><span data-stu-id="2933c-204">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="2933c-205">次のトピックでは、[をソース管理に追加するコンテンツ](adding-content-to-source-control.md)、ソース管理にコンテンツを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2933c-205">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="2933c-206">関連項目</span><span class="sxs-lookup"><span data-stu-id="2933c-206">Further Reading</span></span>

<span data-ttu-id="2933c-207">TFS でチーム プロジェクトを作成する方法より広範なガイダンスについては、次を参照してください。[チーム プロジェクトの作成](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-207">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="2933c-208">チーム プロジェクト コレクション内の新しいチーム プロジェクトを作成するユーザーを有効にする方法の詳細については、次を参照してください。[チーム プロジェクト コレクションの管理者アクセス許可を設定](https://msdn.microsoft.com/library/dd547204.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-208">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="2933c-209">ユーザーをチーム プロジェクトに追加する方法の詳細については、次を参照してください。[チーム プロジェクトへのユーザーの追加](https://msdn.microsoft.com/library/bb558971.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="2933c-209">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2933c-210">[前へ](configuring-team-foundation-server-for-web-deployment.md)
> [次へ](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="2933c-210">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
