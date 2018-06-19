---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Azure の Azure App Service にアプリを公開する |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867815"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="d5a89-102">Azure の Azure App Service にアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="d5a89-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d5a89-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d5a89-104">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="d5a89-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d5a89-105">最後のステップとして Azure にアプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="d5a89-106">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="d5a89-107">クリックすると**発行**呼び出します、 **Web の発行**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="d5a89-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="d5a89-108">オンにした場合**クラウドでホスト**ときに、プロジェクトでは、その接続に最初に作成して、設定が既に構成されています。</span><span class="sxs-lookup"><span data-stu-id="d5a89-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="d5a89-109">その場合をクリックするだけ、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="d5a89-110">(確認しなかった場合**クラウドでホスト**開始時から、手順に従って、[次のセクション](#new-website))。</span><span class="sxs-lookup"><span data-stu-id="d5a89-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="d5a89-111">アプリを展開する をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="d5a89-112">発行の進行状況を表示することができます、 **Web 公開アクティビティ**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="d5a89-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="d5a89-113">(から、**ビュー**メニューの **その他のウィンドウ**選択してから、 **Web 公開アクティビティ**)。</span><span class="sxs-lookup"><span data-stu-id="d5a89-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="d5a89-114">Visual Studio では、アプリケーションの展開が完了すると、既定のブラウザーが配置された web サイトの URL を自動的に開き、作成したアプリケーションが、クラウドで実行するようになりました。</span><span class="sxs-lookup"><span data-stu-id="d5a89-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="d5a89-115">ブラウザーのアドレス バーに URL では、サイトがインターネットから読み込まれることを示します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="d5a89-116">新しい web サイトに展開します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-116">Deploying to a New Website</span></span>

<span data-ttu-id="d5a89-117">オンにしなかった場合**クラウドでホスト**最初にプロジェクトを作成したときに、新しい web アプリを今すぐ構成できます。</span><span class="sxs-lookup"><span data-stu-id="d5a89-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="d5a89-118">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="d5a89-119">選択、**プロファイル** タブでをクリックし、 **Microsoft Azure Websites**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="d5a89-120">現在 Azure にサインインしていない場合は、サインインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="d5a89-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="d5a89-121">**既存の web サイト**ダイアログ ボックスで、をクリックして**新規**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="d5a89-122">サイト名を入力します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-122">Enter a site name.</span></span> <span data-ttu-id="d5a89-123">Azure サブスクリプションと地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="d5a89-124">**データベース サーバー****新しいサーバーの作成**、または既存のサーバーを選択します。</span><span class="sxs-lookup"><span data-stu-id="d5a89-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="d5a89-125">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d5a89-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="d5a89-126">クリックして、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="d5a89-127">をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="d5a89-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d5a89-128">前へ</span><span class="sxs-lookup"><span data-stu-id="d5a89-128">Previous</span></span>](part-9.md)
