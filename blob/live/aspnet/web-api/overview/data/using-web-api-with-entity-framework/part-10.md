---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "Azure の Azure App Service にアプリを公開する |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="b07f6-102">Azure の Azure App Service にアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="b07f6-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b07f6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b07f6-104">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="b07f6-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b07f6-105">最後のステップとして Azure にアプリケーションを発行します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="b07f6-106">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="b07f6-107">クリックすると**発行**呼び出します、 **Web の発行**ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="b07f6-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="b07f6-108">オンにした場合**クラウドでホスト**ときに、プロジェクトでは、その接続に最初に作成して、設定が既に構成されています。</span><span class="sxs-lookup"><span data-stu-id="b07f6-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="b07f6-109">その場合をクリックするだけ、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="b07f6-110">(確認しなかった場合**クラウドでホスト**開始時から、手順に従って、[次のセクション](#new-website))。</span><span class="sxs-lookup"><span data-stu-id="b07f6-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="b07f6-111">アプリを展開する をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="b07f6-112">発行の進行状況を表示することができます、 **Web 公開アクティビティ**ウィンドウです。</span><span class="sxs-lookup"><span data-stu-id="b07f6-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="b07f6-113">(から、**ビュー**メニューの **その他のウィンドウ**選択してから、 **Web 公開アクティビティ**)。</span><span class="sxs-lookup"><span data-stu-id="b07f6-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="b07f6-114">Visual Studio では、アプリケーションの展開が完了すると、既定のブラウザーが配置された web サイトの URL を自動的に開き、作成したアプリケーションが、クラウドで実行するようになりました。</span><span class="sxs-lookup"><span data-stu-id="b07f6-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="b07f6-115">ブラウザーのアドレス バーに URL では、サイトがインターネットから読み込まれることを示します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="b07f6-116">新しい web サイトに展開します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-116">Deploying to a New Website</span></span>

<span data-ttu-id="b07f6-117">オンにしなかった場合**クラウドでホスト**最初にプロジェクトを作成したときに、新しい web アプリを今すぐ構成できます。</span><span class="sxs-lookup"><span data-stu-id="b07f6-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="b07f6-118">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**発行**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="b07f6-119">選択、**プロファイル** タブでをクリックし、 **Microsoft Azure Websites**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="b07f6-120">現在 Azure にサインインしていない場合は、サインインするように求められます。</span><span class="sxs-lookup"><span data-stu-id="b07f6-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="b07f6-121">**既存の web サイト**ダイアログ ボックスで、をクリックして**新規**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="b07f6-122">サイト名を入力します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-122">Enter a site name.</span></span> <span data-ttu-id="b07f6-123">Azure サブスクリプションと地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="b07f6-124">**データベース サーバー****新しいサーバーの作成**、または既存のサーバーを選択します。</span><span class="sxs-lookup"><span data-stu-id="b07f6-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="b07f6-125">
              **[作成]**をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b07f6-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="b07f6-126">クリックして、**設定** タブで確認し、 &quot;Code First Migrations を実行&quot;です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="b07f6-127">をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="b07f6-127">Then click **Publish**.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b07f6-128">前へ</span><span class="sxs-lookup"><span data-stu-id="b07f6-128">Previous</span></span>](part-9.md)
