---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure の Azure App Service へのアプリの発行 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 0290b392c1b292d0f3cc080dbfa25ec6103b2751
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400805"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="2ac17-102">Azure の Azure App Service にアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="2ac17-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2ac17-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2ac17-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="2ac17-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2ac17-105">最後の手順としては、Azure にアプリケーションを公開します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="2ac17-106">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="2ac17-107">クリックすると**発行**呼び出す、 **Web の発行**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="2ac17-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="2ac17-108">オンにした場合**クラウド内のホスト**ときに接続し、プロジェクトでは、最初に作成して、設定が既に構成されています。</span><span class="sxs-lookup"><span data-stu-id="2ac17-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="2ac17-109">その場合は、クリックするだけです、**設定** タブで確認し、 &quot;Code First Migrations の実行&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="2ac17-110">(確認しなかった場合**クラウド内のホスト**を先頭に次の手順では、[次のセクション](#new-website))。</span><span class="sxs-lookup"><span data-stu-id="2ac17-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="2ac17-111">アプリを展開する をクリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="2ac17-112">発行の進行状況を表示することができます、 **Web 発行アクティビティ**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="2ac17-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="2ac17-113">(から、**ビュー**メニューの **その他の Windows**を選択し、 **Web 発行アクティビティ**)。</span><span class="sxs-lookup"><span data-stu-id="2ac17-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="2ac17-114">Visual Studio では、アプリのデプロイが完了したら、デプロイされた web サイトの URL に既定のブラウザーが自動的に開き、作成したアプリケーションが、クラウドで実行されるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2ac17-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="2ac17-115">ブラウザーのアドレス バーに URL では、サイトがインターネットから読み込まれていることを示します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="2ac17-116">新しい web サイトに展開します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-116">Deploying to a New Website</span></span>

<span data-ttu-id="2ac17-117">選択しなかった場合**クラウド内のホスト**最初にプロジェクトを作成したときに、新しい web アプリを今すぐ構成できます。</span><span class="sxs-lookup"><span data-stu-id="2ac17-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="2ac17-118">ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="2ac17-119">選択、**プロファイル** タブでをクリックし、 **Microsoft Azure Websites**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="2ac17-120">現在 Azure にサインインしていない場合は、サインインする促されます。</span><span class="sxs-lookup"><span data-stu-id="2ac17-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="2ac17-121">**既存の web サイト**ダイアログ ボックスで、をクリックして**新規**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="2ac17-122">サイト名を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-122">Enter a site name.</span></span> <span data-ttu-id="2ac17-123">Azure サブスクリプションとリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="2ac17-124">**データベース サーバー**を選択します**新しいサーバーの作成**、または既存のサーバーを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="2ac17-125">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ac17-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="2ac17-126">をクリックして、**設定** タブで確認し、 &quot;Code First Migrations の実行&quot;します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="2ac17-127">クリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="2ac17-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2ac17-128">前へ</span><span class="sxs-lookup"><span data-stu-id="2ac17-128">Previous</span></span>](part-9.md)
