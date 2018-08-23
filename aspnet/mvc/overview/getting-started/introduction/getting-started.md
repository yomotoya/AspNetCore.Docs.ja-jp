---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 の概要 |Microsoft Docs
author: Rick-Anderson
description: '注: このチュートリアルの最新バージョンは Visual Studio 2015 を使用して、ここで使用できます。 新しいチュートリアルでは、多くの improvem を提供する ASP.NET Core MVC 6 を使用しています.'
ms.author: riande
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8417be945e16ed99e655f628134c9190429f1d67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836198"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="8e2db-104">ASP.NET MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="8e2db-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="8e2db-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8e2db-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 <span data-ttu-id="8e2db-106">このチュートリアルが ASP.NET MVC 5 web アプリを使用して、構築の基礎を講義[Visual Studio 2017](https://www.visualstudio.com/)します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="8e2db-107">チュートリアルの最後のソースの上にある[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="8e2db-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>


 <span data-ttu-id="8e2db-108">このチュートリアルの執筆者[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )、 [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )、および[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="8e2db-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

 <span data-ttu-id="8e2db-109">このアプリを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="8e2db-109">You need an Azure account to deploy this app to Azure:</span></span>

 - <span data-ttu-id="8e2db-110">できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="8e2db-111">できます[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="8e2db-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="8e2db-112">作業の開始</span><span class="sxs-lookup"><span data-stu-id="8e2db-112">Getting Started</span></span>

<span data-ttu-id="8e2db-113">インストールと実行によって開始[Visual Studio 2017](https://www.visualstudio.com/)します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="8e2db-114">Visual Studio は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="8e2db-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="8e2db-115">Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="8e2db-116">Visual Studio を使用できるさまざまなオプションを示す下部の一覧です。</span><span class="sxs-lookup"><span data-stu-id="8e2db-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="8e2db-117">また、IDE でタスクを実行する別の方法を提供するメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="8e2db-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="8e2db-118">(選択ではなく、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **の新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="8e2db-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a><span data-ttu-id="8e2db-119">最初のアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-119">Creating Your First Application</span></span>

<span data-ttu-id="8e2db-120">クリックして**新しいプロジェクト**、Visual c# を選択、左、 **Web**し、 **ASP.NET Web アプリケーション (.NET Framework)** します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="8e2db-121">クリックして、プロジェクトに"MvcMovie" **OK**します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="8e2db-122">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**順にクリックします**OK**します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="8e2db-123">Visual Studio は、何もせず、実用的なアプリケーションが現在あるため、作成した ASP.NET MVC プロジェクトの既定のテンプレートを使用!</span><span class="sxs-lookup"><span data-stu-id="8e2db-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="8e2db-124">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="8e2db-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="8e2db-125">プロジェクト、およびそのアプリケーションにお勧めです。</span><span class="sxs-lookup"><span data-stu-id="8e2db-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="8e2db-126">F5 キーを押してデバッグを開始する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8e2db-126">Click F5 to start debugging.</span></span> <span data-ttu-id="8e2db-127">F5 キーを押して Visual Studio を開始するが[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)し、web アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="8e2db-128">Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="8e2db-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="8e2db-129">ブラウザーのアドレス バーが表示されますが`localhost:port#`ようなものではありません`example.com`します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="8e2db-130">だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="8e2db-131">Visual Studio web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="8e2db-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="8e2db-132">次の図では、ポート番号は、1234 です。</span><span class="sxs-lookup"><span data-stu-id="8e2db-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="8e2db-133">アプリケーションを実行するときに、別のポート番号を確認します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="8e2db-134">すぐに使えるこの既定テンプレートを使用してホーム、連絡先についてのページ。</span><span class="sxs-lookup"><span data-stu-id="8e2db-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="8e2db-135">上の画像が表示されない、**ホーム**、**について**と**連絡先**リンク。</span><span class="sxs-lookup"><span data-stu-id="8e2db-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="8e2db-136">ブラウザー ウィンドウのサイズによっては、これらのリンクを参照するナビゲーション アイコンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e2db-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="8e2db-137">アプリケーションには、登録し、ログインのサポートも提供します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="8e2db-138">次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC についてもう少し説明します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="8e2db-139">ASP.NET MVC アプリケーションを終了し、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8e2db-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="8e2db-140">現在のチュートリアルの一覧は、次を参照してください。[の記事をお勧めします。 MVC](../mvc-learning-sequence.md)します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="8e2db-141">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e2db-141">See this App Running on Azure</span></span>

<span data-ttu-id="8e2db-142">ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="8e2db-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="8e2db-143">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。</span><span class="sxs-lookup"><span data-stu-id="8e2db-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="8e2db-144">このソリューションを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="8e2db-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="8e2db-145">アカウントがいない場合は、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="8e2db-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="8e2db-146">[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="8e2db-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="8e2db-147">[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="8e2db-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8e2db-148">次へ</span><span class="sxs-lookup"><span data-stu-id="8e2db-148">Next</span></span>](adding-a-controller.md)
