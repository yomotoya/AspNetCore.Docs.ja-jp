---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: ソース管理へのコンテンツの追加 |Microsoft Docs
author: jrjlee
description: このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。 ソリューションとプロジェクトをチーム プロジェクトに追加する方法を説明しています.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827280"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="f0c65-104">ソース管理へのコンテンツの追加</span><span class="sxs-lookup"><span data-stu-id="f0c65-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="f0c65-105">によって[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="f0c65-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="f0c65-106">PDF のダウンロード</span><span class="sxs-lookup"><span data-stu-id="f0c65-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="f0c65-107">このトピックでは、ソース管理では、Team Foundation Server (TFS) 2010 にコンテンツを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="f0c65-108">ソリューションとプロジェクトを TFS でチーム プロジェクトに追加する方法について説明し、フレームワーク、またはアセンブリなどの外部の依存関係をソース管理に追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="f0c65-109">このトピックでは、一連の Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に基づいているチュートリアルの一部を形成します。このチュートリアル シリーズは、サンプル ソリューションを使用して&#x2014;、[連絡先マネージャー ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的なレベルの ASP.NET MVC 3 アプリケーション、Windows の通信など、複雑な web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="f0c65-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="f0c65-110">タスクの概要</span><span class="sxs-lookup"><span data-stu-id="f0c65-110">Task Overview</span></span>

<span data-ttu-id="f0c65-111">開発者チームのすべてのメンバーは、ほとんどの場合、ソース管理にコンテンツを追加できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="f0c65-112">TFS でソース管理にソリューションを追加するには、これらの大まかな手順を完了する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="f0c65-113">チーム プロジェクトに接続します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-113">Connect to a team project.</span></span>
- <span data-ttu-id="f0c65-114">サーバー上のチーム プロジェクトのフォルダー構造をローカル コンピューター上のフォルダー構造にマップします。</span><span class="sxs-lookup"><span data-stu-id="f0c65-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="f0c65-115">ソリューションとその内容をソース コントロールに追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="f0c65-116">ソース管理には、外部依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="f0c65-117">このトピックでは、これらの手順を実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="f0c65-118">タスクとチュートリアルでは、このトピックでは、コンテンツを管理する新しい TFS チーム プロジェクトを既に作成したことを想定しています。</span><span class="sxs-lookup"><span data-stu-id="f0c65-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="f0c65-119">新しいチーム プロジェクトを作成する方法の詳細については、次を参照してください。 [TFS でチーム プロジェクトを作成する](creating-a-team-project-in-tfs.md)します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="f0c65-120">これらの手順を実行しますか。</span><span class="sxs-lookup"><span data-stu-id="f0c65-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="f0c65-121">ほとんどの場合、開発者チームのすべてのメンバーを追加し、特定のチーム プロジェクト内のコンテンツを変更する必要がありますできます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="f0c65-122">チーム プロジェクトに接続し、フォルダー マッピングの作成</span><span class="sxs-lookup"><span data-stu-id="f0c65-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="f0c65-123">任意のコンテンツをソース管理に追加する前に、チーム プロジェクトに接続し、ローカル コンピューターで、サーバー上のフォルダー構造とファイル システム間のマッピングを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="f0c65-124">**チーム プロジェクトに接続し、ローカル パスをマップするには**</span><span class="sxs-lookup"><span data-stu-id="f0c65-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="f0c65-125">開発者のワークステーションでは、Visual Studio 2010 を開きます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="f0c65-126">Visual Studio での**チーム** メニューのをクリックして**Team Foundation Server への接続**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f0c65-127">TFS サーバーへの接続を既に構成した場合は、手順 3. ~ 6. を省略できます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="f0c65-128">**チーム プロジェクトへの接続を**ダイアログ ボックスで、をクリックして**サーバー**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="f0c65-129">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**追加**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="f0c65-130">**Team Foundation Server の追加** ダイアログ ボックスでは、TFS インスタンスの詳細を入力し、順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="f0c65-131">**Team Foundation Server の追加と削除**ダイアログ ボックスで、をクリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="f0c65-132">**チーム プロジェクトへの接続**ダイアログ ボックスで、接続するチームを選択する TFS インスタンスを選択はプロジェクト コレクション、選択、追加するチーム プロジェクトと順にクリックします**Connect**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="f0c65-133">**チーム エクスプ ローラー**ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="f0c65-134">**ソース管理エクスプ ローラー** ] タブで [**マップされていない**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="f0c65-135">**マップ** ダイアログ ボックスで、**ローカル フォルダー**クリックして、チーム プロジェクトのルート フォルダーとして機能するローカル フォルダーのボックスを参照 (または作成)**マップ**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="f0c65-136">ソース ファイルをダウンロードするメッセージが表示されたら、クリックして**はい**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="f0c65-137">この時点では、開発者のワークステーション上のローカル フォルダーをチーム プロジェクトのサーバー側のフォルダーにマップします。</span><span class="sxs-lookup"><span data-stu-id="f0c65-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="f0c65-138">既存の内容は、ローカル フォルダー構造に、チーム プロジェクトからもダウンロードしました。</span><span class="sxs-lookup"><span data-stu-id="f0c65-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="f0c65-139">ソース管理に独自のコンテンツを追加することができますようになりました。</span><span class="sxs-lookup"><span data-stu-id="f0c65-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="f0c65-140">プロジェクトとソリューションをソース管理に追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="f0c65-141">ソース管理にプロジェクトとソリューションを追加するには、まず、ローカル コンピューターに、チーム プロジェクトのマップされたフォルダーに移動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="f0c65-142">サーバーと、追加機能を同期するコンテンツで確認できます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="f0c65-143">**ソース管理にプロジェクトを追加するには**</span><span class="sxs-lookup"><span data-stu-id="f0c65-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="f0c65-144">開発者のワークステーションで、プロジェクトやソリューションをチーム プロジェクトのマップされたフォルダー構造内の適切な場所に移動します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f0c65-145">多くの組織では、プロジェクトおよびソリューションはソース管理して整理する方法の推奨されるアプローチがあります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="f0c65-146">フォルダーを構造にする方法のガイダンスについては、次を参照してください。 [How To: 構造体のソースの管理フォルダーで Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="f0c65-147">Visual Studio 2010 でのソリューションを開きます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="f0c65-148">**ソリューション エクスプ ローラー**ウィンドウで、ソリューションを右クリックし、順にクリックします**ソリューションをソース管理に追加**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="f0c65-149">場合によっては、組織が TFS では、構造のコンテンツを好きな方法によっては、ソース コードの編成方法きめの細かい制御を提供するには、個別にソース管理にプロジェクトを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="f0c65-150">いることを確認、**ソース管理エクスプ ローラー**  タブには、チーム プロジェクトのサーバーのフォルダー構造内に追加したコンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="f0c65-151">**ソース管理エクスプ ローラー**  タブには、ローカル ファイル システム上のマップされたフォルダーにソリューションを追加するためにそれ以上入力を求めるでコンテンツが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="f0c65-152">マップされていない場所に、ソリューションであった場合は、TFS とローカル ファイル システムの両方でフォルダーの場所を指定する求めるは。</span><span class="sxs-lookup"><span data-stu-id="f0c65-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="f0c65-153">**ソース管理エクスプ ローラー**  タブで、**フォルダー**ウィンドウで、チーム プロジェクトを右クリックして (たとえば、 **ContactManager**)、順にクリックします**チェックイン保留中の変更**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="f0c65-154">**チェックイン – ソース ファイル**ダイアログ ボックスで、コメントを入力し、順にクリックします**チェックイン**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="f0c65-155">この時点で、ソリューションを TFS でソース管理に追加しました。</span><span class="sxs-lookup"><span data-stu-id="f0c65-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="f0c65-156">ソース管理への外部の依存関係を追加します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="f0c65-157">プロジェクトまたはソリューションをソース管理に追加すると、ファイルやプロジェクトまたはソリューション内のフォルダーも追加されます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="f0c65-158">ただし、多くの場合に、プロジェクトおよびソリューションも使用して正常に機能する、ローカル アセンブリのように、外部の依存関係します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="f0c65-159">コードは正常にビルドをソース管理により、チーム ビルドその開発者チームの他のメンバーにこのようなすべてのリソースを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="f0c65-160">たとえば、連絡先マネージャーのサンプル ソリューションのフォルダー構造には、パッケージをという名前のフォルダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="f0c65-161">ADO.NET の Entity Framework 4.1 のアセンブリとサポートするさまざまなリソースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f0c65-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="f0c65-162">パッケージ フォルダーは、連絡先マネージャー ソリューションの一部ではありませんが、しなくても、ソリューションが正常にビルドされません。</span><span class="sxs-lookup"><span data-stu-id="f0c65-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="f0c65-163">ソリューションをビルドするチームがビルドを有効にするには、パッケージ フォルダーをソース管理に追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="f0c65-164">パッケージ フォルダーを含めることは、Visual Studio 2010 用の NuGet 拡張機能を使用して、ソリューションに、Entity Framework、または同様のリソースを追加するときの動作の一般的な例です。</span><span class="sxs-lookup"><span data-stu-id="f0c65-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="f0c65-165">**ソース管理にプロジェクト以外のコンテンツを追加するには**</span><span class="sxs-lookup"><span data-stu-id="f0c65-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="f0c65-166">(たとえば、パッケージ フォルダーなど) を追加する項目が、ローカル ファイル システム上のマップされたフォルダー内の適切な場所であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="f0c65-167">Visual Studio 2010 での**チーム エクスプ ローラー**ウィンドウでは、チーム プロジェクトを展開し、ダブルクリック**ソース管理**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="f0c65-168">**ソース管理エクスプ ローラー**  タブで、**フォルダー**ウィンドウで、追加する項目を格納またはアイテムをフォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="f0c65-169">をクリックして、**フォルダーに項目の追加**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0c65-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="f0c65-170">**ソース管理に追加** ダイアログ ボックスで、フォルダーまたは追加するをクリックし項目を選択**次**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="f0c65-171">**項目を除外** タブで、除外されている自動的に (たとえば、アセンブリ) をクリックして必要な項目を選択**項目の追加**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="f0c65-172">**追加する項目** タブで、追加するすべてのファイルが一覧表示されます。 順にクリックしますであることを確認**完了**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="f0c65-173">**ソース管理エクスプ ローラー**ウィンドウで、をクリックして、**チェックイン**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f0c65-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="f0c65-174">**チェックイン – ソース ファイル**ダイアログ ボックスで、コメントを入力し、順にクリックします**チェックイン**します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="f0c65-175">この時点では、ソース管理にソリューションの外部の依存関係を追加があります。</span><span class="sxs-lookup"><span data-stu-id="f0c65-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f0c65-176">まとめ</span><span class="sxs-lookup"><span data-stu-id="f0c65-176">Conclusion</span></span>

<span data-ttu-id="f0c65-177">このトピックでは、チーム プロジェクトに接続し、フォルダー構造をマップし、ソース管理にコンテンツを追加する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="f0c65-178">ソース管理下にある項目を操作する方法の詳細については、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="f0c65-179">次のトピックでは、 [Web 配置の TFS ビルド サーバーを構成する](configuring-a-tfs-build-server-for-web-deployment.md)、構築して、ソリューションをデプロイする TFS チーム ビルド サーバーを準備する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="f0c65-180">関連項目</span><span class="sxs-lookup"><span data-stu-id="f0c65-180">Further Reading</span></span>

<span data-ttu-id="f0c65-181">TFS でソース管理の操作方法の詳細については、次を参照してください。[を使用してバージョン管理](https://msdn.microsoft.com/library/ms181368.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="f0c65-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0c65-182">[前へ](creating-a-team-project-in-tfs.md)
> [次へ](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="f0c65-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
