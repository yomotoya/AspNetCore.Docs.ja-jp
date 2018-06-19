---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Azure App Service で Web アプリを使用して SignalR を使用して |Microsoft ドキュメント
author: pfletcher
description: このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。 ソフトウェアのバージョンは、Visual Studio 2013 または Vis. に、このチュートリアルで使用される.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/01/2015
ms.topic: article
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 8386441690a3fb479ffb941ebd7c0b2f83870781
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043208"
---
<a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="f2a29-104">Azure App Service で Web アプリを使用して SignalR を使用します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-104">Using SignalR with Web Apps in Azure App Service</span></span>
====================
<span data-ttu-id="f2a29-105">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="f2a29-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="f2a29-106">このドキュメントでは、Microsoft Azure で実行されている SignalR アプリケーションを構成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f2a29-107">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="f2a29-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f2a29-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)または Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f2a29-108">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) or Visual Studio 2012</span></span>
> - <span data-ttu-id="f2a29-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="f2a29-109">.NET 4.5</span></span>
> - <span data-ttu-id="f2a29-110">SignalR バージョン 2</span><span class="sxs-lookup"><span data-stu-id="f2a29-110">SignalR version 2</span></span>
> - <span data-ttu-id="f2a29-111">Visual Studio 2013 または 2012 用の azure SDK 2.3</span><span class="sxs-lookup"><span data-stu-id="f2a29-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f2a29-112">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="f2a29-112">Questions and comments</span></span>
> 
> <span data-ttu-id="f2a29-113">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="f2a29-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f2a29-114">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)、 [StackOverflow.com](http://stackoverflow.com/)、または[Microsoft Azure フォーラム](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="f2a29-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>


## <a name="table-of-contents"></a><span data-ttu-id="f2a29-115">目次</span><span class="sxs-lookup"><span data-stu-id="f2a29-115">Table of Contents</span></span>

- [<span data-ttu-id="f2a29-116">はじめに</span><span class="sxs-lookup"><span data-stu-id="f2a29-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="f2a29-117">Azure App Service に SignalR Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="f2a29-118">Azure App Service で Websocket を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="f2a29-119">Azure Redis キャッシュのバック プレーンの使用</span><span class="sxs-lookup"><span data-stu-id="f2a29-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="f2a29-120">次の手順</span><span class="sxs-lookup"><span data-stu-id="f2a29-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="f2a29-121">はじめに</span><span class="sxs-lookup"><span data-stu-id="f2a29-121">Introduction</span></span>

<span data-ttu-id="f2a29-122">ASP.NET SignalR は、サーバーと web または .NET のクライアントの間の対話機能の新しいレベルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f2a29-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="f2a29-123">Azure でホストされるときに SignalR アプリケーションを活用すること、高可用性、拡張性が高く、し、パフォーマンスの高い環境、クラウドで実行されている提供します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="f2a29-124">Azure App Service に SignalR Web アプリを展開します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="f2a29-125">SignalR はいない社内設置のサーバーへの展開とアプリケーションを Azure の配置に特定の混乱を追加します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="f2a29-126">構成またはその他の設定は変更せず、SignalR を使用するアプリケーションを Azure でホストすることができます (ただし、Websocket のサポートを参照してください[Azure App Service で Websocket を有効にすると](#websocket)下です)。このチュートリアルで作成したアプリケーションを展開、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を Azure にします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="f2a29-127">**必須コンポーネント**</span><span class="sxs-lookup"><span data-stu-id="f2a29-127">**Prerequisites**</span></span>

- <span data-ttu-id="f2a29-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f2a29-128">Visual Studio 2013.</span></span> <span data-ttu-id="f2a29-129">Visual Studio を持っていない場合、Azure SDK のインストールでは Visual Studio 2013 の Express for Web が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f2a29-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="f2a29-130">[Visual Studio 2013 用の azure SDK 2.3](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)または[Visual Studio 2012 用の Azure SDK 2.3](https://go.microsoft.com/fwlink/p/?linkid=323511)です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="f2a29-131">このチュートリアルを完了するには、Azure サブスクリプションを必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2a29-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="f2a29-132">実行できます[の MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)、または[試用版サブスクリプションにサインアップする](https://azure.microsoft.com/pricing/free-trial/)です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="f2a29-133">SignalR web アプリを Azure に展開します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="f2a29-134">完了、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したプロジェクトをダウンロードまたは[コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="f2a29-135">Visual Studio で、次のように選択します。**ビルド**、**発行 SignalR チャット**です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="f2a29-136">"Web の発行 ダイアログ ボックスで、「Windows Azure Web サイト」を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Azure の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="f2a29-138">Microsoft アカウントにサインインしていない場合にをクリックして**サインインしています.** 「既存の Web サイトの選択」ダイアログでサインインします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![既存の Web サイトを選択します。](using-signalr-with-azure-web-sites/_static/image2.png)    ![Azure へのサインイン](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="f2a29-141">"既存の Web サイトの選択 ダイアログ ボックスで、をクリックして**新規**です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![[新しい Web サイト]](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="f2a29-143">「Windows Azure でサイトを作成する」ダイアログ ボックスで、一意のアプリ名を入力します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="f2a29-144">領域のドロップダウン内で自分に最も近い地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="f2a29-145">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-145">Click **Create**.</span></span>

    ![Azure でサイトを作成します。](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="f2a29-147">"Web の発行 ダイアログ ボックスで、をクリックして**発行**です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![サイトを発行します。](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="f2a29-149">アプリには、発行が完了したら、Azure App Service Web Apps でホストされている SignalR チャット アプリケーションはブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="f2a29-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![サイト、ブラウザーで開く](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="f2a29-151">Azure App Service Web Apps で Websocket を有効にします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="f2a29-152">Websocket は、SignalR アプリケーションで使用する web アプリで明示的に有効にする必要があります。それ以外の場合、その他のプロトコルを使用する (を参照してください[トランスポートとフォールバック](../getting-started/introduction-to-signalr.md#transports)詳細)。</span><span class="sxs-lookup"><span data-stu-id="f2a29-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="f2a29-153">Azure App Service Web Apps で Websocket を使用するためには、web アプリの構成セクションで有効にします。</span><span class="sxs-lookup"><span data-stu-id="f2a29-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="f2a29-154">これを行うには、web アプリを開く、 [Azure 管理ポータル](https://manage.windowsazure.com/)と構成 を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![[Configure (構成)] タブ](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="f2a29-156">[構成] ページの上部には、web アプリ用 .NET 4.5 が使用されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![.NET framework バージョン 4.5 の設定](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="f2a29-158">[構成] ページで、 **Websocket**選択を設定する**で**です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Websocket の設定: で](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="f2a29-160">[構成] ページの下部には、次のように選択します。**保存**して変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="f2a29-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![設定を保存します。](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="f2a29-162">Azure Redis キャッシュのバック プレーンの使用</span><span class="sxs-lookup"><span data-stu-id="f2a29-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="f2a29-163">かどうか、web アプリの複数のインスタンスを使用して、それらのインスタンスのユーザーは、(、たとえば、1 つのインスタンスに作成されたチャット メッセージ到達できるように、他のインスタンスに接続しているユーザー) に、相互に対話する必要があります、 [Azure Redis Cacheバック プレーン](../performance/scaleout-with-redis.md)アプリケーションに実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2a29-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="f2a29-164">次の手順</span><span class="sxs-lookup"><span data-stu-id="f2a29-164">Next Steps</span></span>

<span data-ttu-id="f2a29-165">Azure App service Web Apps での詳細については、次を参照してください。 [Web アプリの概要](https://azure.microsoft.com/documentation/articles/app-service-web-overview/)です。</span><span class="sxs-lookup"><span data-stu-id="f2a29-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
