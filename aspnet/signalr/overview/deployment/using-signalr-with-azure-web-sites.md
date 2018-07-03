---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: SignalR を使用して Azure App Service で Web アプリの使用 |Microsoft Docs
author: pfletcher
description: このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。 ソフトウェアのバージョンは、Visual Studio 2013 または Vis. チュートリアルで使用.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: dabf0f6cfed401e10d2c1134c260022d94c3ab92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389576"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="21648-104">Azure App Service で Web アプリで SignalR を使用</span><span class="sxs-lookup"><span data-stu-id="21648-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="21648-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="21648-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="21648-106">このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="21648-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="21648-107">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="21648-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="21648-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)または Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="21648-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="21648-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="21648-109">.NET 4.5</span></span>
> - <span data-ttu-id="21648-110">SignalR 2 のバージョン</span><span class="sxs-lookup"><span data-stu-id="21648-110">SignalR version 2</span></span>
> - <span data-ttu-id="21648-111">Visual Studio 2013 または 2012 用の azure SDK 2.3</span><span class="sxs-lookup"><span data-stu-id="21648-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="21648-112">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="21648-112">Questions and comments</span></span>
> 
> <span data-ttu-id="21648-113">このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="21648-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="21648-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)、 [StackOverflow.com](http://stackoverflow.com/)、または[Microsoft Azure フォーラム](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="21648-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="21648-115">目次</span><span class="sxs-lookup"><span data-stu-id="21648-115">Table of Contents</span></span>

- [<span data-ttu-id="21648-116">はじめに</span><span class="sxs-lookup"><span data-stu-id="21648-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="21648-117">Azure App Service に SignalR Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="21648-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="21648-118">Azure App Service で Websocket を有効にします。</span><span class="sxs-lookup"><span data-stu-id="21648-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="21648-119">Azure Redis Cache のバック プレーンを使用します。</span><span class="sxs-lookup"><span data-stu-id="21648-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="21648-120">次の手順</span><span class="sxs-lookup"><span data-stu-id="21648-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="21648-121">はじめに</span><span class="sxs-lookup"><span data-stu-id="21648-121">Introduction</span></span>

<span data-ttu-id="21648-122">ASP.NET SignalR は、新しいレベルのサーバーと web または .NET クライアント間の対話機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="21648-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="21648-123">Azure でホストされているときに SignalR アプリケーションを利用して高可用性のスケーラブルなおよびパフォーマンスの高い環境を提供する、クラウドで実行されています。</span><span class="sxs-lookup"><span data-stu-id="21648-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="21648-124">Azure App Service に SignalR Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="21648-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="21648-125">SignalR を Azure にアプリケーションを展開すると、オンプレミス サーバーにデプロイする特定の混乱を追加しません。</span><span class="sxs-lookup"><span data-stu-id="21648-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="21648-126">構成またはその他の設定に変更を行わず、SignalR を使用するアプリケーションを Azure でホストされることができます (ただし Websocket サポートについてを参照してください[Azure App Service で Websocket を有効にする](#websocket)以下です)。このチュートリアルで作成したアプリケーションをデプロイします、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を Azure にします。</span><span class="sxs-lookup"><span data-stu-id="21648-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="21648-127">**必須コンポーネント**</span><span class="sxs-lookup"><span data-stu-id="21648-127">**Prerequisites**</span></span>

- <span data-ttu-id="21648-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="21648-128">Visual Studio 2013.</span></span> <span data-ttu-id="21648-129">Visual Studio を持っていない場合、Azure SDK のインストールでは Visual Studio 2013 の Express for Web が含まれます。</span><span class="sxs-lookup"><span data-stu-id="21648-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="21648-130">[Visual Studio 2013 向け azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)または[Visual Studio 2012 用の Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)します。</span><span class="sxs-lookup"><span data-stu-id="21648-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="21648-131">このチュートリアルを完了するには、Azure サブスクリプションを必要があります。</span><span class="sxs-lookup"><span data-stu-id="21648-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="21648-132">できます[MSDN サブスクリプション会員の特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)、または[試用版サブスクリプションにサインアップ](https://azure.microsoft.com/pricing/free-trial/)します。</span><span class="sxs-lookup"><span data-stu-id="21648-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="21648-133">SignalR の web アプリを Azure に展開します。</span><span class="sxs-lookup"><span data-stu-id="21648-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="21648-134">完了、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したプロジェクトをダウンロードまたは[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)します。</span><span class="sxs-lookup"><span data-stu-id="21648-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="21648-135">Visual Studio で、次のように選択します。**ビルド**、 **SignalR チャットの発行**します。</span><span class="sxs-lookup"><span data-stu-id="21648-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="21648-136">"Web の発行 ダイアログ ボックスで、「Windows Azure の Web サイト」を選択します。</span><span class="sxs-lookup"><span data-stu-id="21648-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Azure の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="21648-138">Microsoft アカウントにサインインしていない場合はクリックして**サインインしています.** で [既存の Web サイトの選択] ダイアログ ボックスで、サインインします。</span><span class="sxs-lookup"><span data-stu-id="21648-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![既存の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure にサインインする](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="21648-141">[既存の Web サイトの選択] ダイアログ ボックスで、次のようにクリックします。**新規**します。</span><span class="sxs-lookup"><span data-stu-id="21648-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![[新しい Web サイト]](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="21648-143">"Windows Azure でサイトの作成 ダイアログ ボックスで、一意のアプリ名を入力します。</span><span class="sxs-lookup"><span data-stu-id="21648-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="21648-144">領域のドロップダウン リストでユーザーに最も近いリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="21648-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="21648-145">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="21648-145">Click **Create**.</span></span>

    ![Azure でサイトを作成します。](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="21648-147">"Web の発行 ダイアログ ボックスで、次のようにクリックします。**発行**します。</span><span class="sxs-lookup"><span data-stu-id="21648-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![サイトを発行します。](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="21648-149">アプリの発行が完了したら、SignalR チャット アプリケーションを Azure App Service Web Apps でホストされているが、ブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="21648-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![サイトがブラウザーで開く](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="21648-151">Azure App Service Web Apps で Websocket を有効にします。</span><span class="sxs-lookup"><span data-stu-id="21648-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="21648-152">Websocket は、SignalR アプリケーションで使用する web アプリで明示的に有効にする必要があります。それ以外の場合、他のプロトコルを使用する (を参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)詳細については)。</span><span class="sxs-lookup"><span data-stu-id="21648-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="21648-153">Azure App Service Web Apps で Websocket を使用するためには、web アプリの構成セクションで有効にします。</span><span class="sxs-lookup"><span data-stu-id="21648-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="21648-154">これを行うには、web アプリを開き、 [Azure 管理ポータル](https://manage.windowsazure.com/)構成を選択します。</span><span class="sxs-lookup"><span data-stu-id="21648-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![[Configure (構成)] タブ](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="21648-156">構成ページの上部にある、web アプリ用 .NET 4.5 が使用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="21648-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework バージョン 4.5 の設定](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="21648-158">[構成] ページで、 **Websocket**設定で選択**で**します。</span><span class="sxs-lookup"><span data-stu-id="21648-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Websocket の設定: 上](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="21648-160">[構成] ページの下部には、次のように選択します。**保存**、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="21648-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![設定を保存します。](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="21648-162">Azure Redis Cache のバック プレーンを使用します。</span><span class="sxs-lookup"><span data-stu-id="21648-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="21648-163">Web アプリの複数のインスタンスを使用して、それらのインスタンスのユーザー間の対話 (つまり、たとえば、1 つのインスタンスに作成されたチャット メッセージは、他のインスタンスに接続しているユーザーにアクセスできる) する必要があるかどうか、 [Azure Redis Cacheバック プレーン](../performance/scaleout-with-redis.md)アプリケーションで実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="21648-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="21648-164">次の手順</span><span class="sxs-lookup"><span data-stu-id="21648-164">Next Steps</span></span>

<span data-ttu-id="21648-165">Azure App Service で Web アプリの詳細については、次を参照してください。 [Web Apps の概要](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)します。</span><span class="sxs-lookup"><span data-stu-id="21648-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
