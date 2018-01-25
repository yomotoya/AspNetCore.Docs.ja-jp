---
uid: mvc/overview/getting-started/introduction/getting-started
title: "ASP.NET MVC 5 の概要 |Microsoft ドキュメント"
author: Rick-Anderson
description: "メモ: このチュートリアルの最新バージョンは Visual Studio 2015 を使用して、ここで使用できます。 新しいチュートリアルでは、多くの improvem を提供する ASP.NET Core MVC 6 を使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 1616b238612fa9f519418f583c40a46b9d81d8ce
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="607c6-104">ASP.NET MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="607c6-104">Getting Started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="607c6-105">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="607c6-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 <span data-ttu-id="607c6-106">このチュートリアルが ASP.NET MVC 5 web アプリケーションを使用して、作成の基本を教える[Visual Studio 2017](https://www.visualstudio.com/)です。</span><span class="sxs-lookup"><span data-stu-id="607c6-106">This tutorial will teach you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://www.visualstudio.com/).</span></span> <span data-ttu-id="607c6-107">チュートリアルの最後のソースが上にある[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span><span class="sxs-lookup"><span data-stu-id="607c6-107">Final Source for tutorial located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)</span></span>
 
 
 <span data-ttu-id="607c6-108">このチュートリアルは、によって書き込まれました[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )、 [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )、および[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="607c6-108">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>
 
 <span data-ttu-id="607c6-109">Azure アカウントをこのアプリを Azure に展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="607c6-109">You need an Azure account to deploy this app to Azure:</span></span>
 
 - <span data-ttu-id="607c6-110">実行できます[無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。</span><span class="sxs-lookup"><span data-stu-id="607c6-110">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
 - <span data-ttu-id="607c6-111">実行できます[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-、MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。</span><span class="sxs-lookup"><span data-stu-id="607c6-111">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>


## <a name="getting-started"></a><span data-ttu-id="607c6-112">作業の開始</span><span class="sxs-lookup"><span data-stu-id="607c6-112">Getting Started</span></span>

<span data-ttu-id="607c6-113">インストールと実行によって開始[Visual Studio 2017](https://www.visualstudio.com/)です。</span><span class="sxs-lookup"><span data-stu-id="607c6-113">Start by installing and running [Visual Studio 2017](https://www.visualstudio.com/).</span></span>

<span data-ttu-id="607c6-114">Visual Studio は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="607c6-114">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="607c6-115">Microsoft Word を使用してドキュメントを作成するのにのと同じようにアプリケーションを作成するのに、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="607c6-115">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="607c6-116">Visual Studio では、下部で、使用可能なさまざまなオプションを表示するリストです。</span><span class="sxs-lookup"><span data-stu-id="607c6-116">In Visual Studio there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="607c6-117">IDE でのタスクを実行する別の方法を提供するメニューもあります。</span><span class="sxs-lookup"><span data-stu-id="607c6-117">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="607c6-118">(選択する代わりに、たとえば、**新しいプロジェクト**から、**開始** ページで、メニューを使用して選択**ファイル** &gt; **新しいプロジェクト**.)</span><span class="sxs-lookup"><span data-stu-id="607c6-118">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a><span data-ttu-id="607c6-119">初めてアプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="607c6-119">Creating Your First Application</span></span>

<span data-ttu-id="607c6-120">をクリックして**新しいプロジェクト**、クリックして Visual C# の場合、左側の**Web**し、 **ASP.NET Web アプリケーション (.NET Framework)**です。</span><span class="sxs-lookup"><span data-stu-id="607c6-120">Click **New Project**, then select Visual C# on the left, then **Web** and then select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="607c6-121">プロジェクト"MvcMovie"の名前を指定し、をクリックして**OK**です。</span><span class="sxs-lookup"><span data-stu-id="607c6-121">Name your project "MvcMovie" and then click **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="607c6-122">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**  をクリックし、 **ok**です。</span><span class="sxs-lookup"><span data-stu-id="607c6-122">In the **New ASP.NET Project** dialog, click **MVC** and then click **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="607c6-123">Visual Studio では、作成した ASP.NET MVC プロジェクトの既定のテンプレートが使用されるため、何もせず作業アプリケーションが現在ある!</span><span class="sxs-lookup"><span data-stu-id="607c6-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="607c6-124">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="607c6-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="607c6-125">プロジェクト、・ アプリケーションを起動するに適しています。</span><span class="sxs-lookup"><span data-stu-id="607c6-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="607c6-126">F5 キーを押してデバッグを開始する をクリックします。</span><span class="sxs-lookup"><span data-stu-id="607c6-126">Click F5 to start debugging.</span></span> <span data-ttu-id="607c6-127">F5 キーを押して Visual Studio を起動するが[IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) web アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="607c6-127">F5 causes Visual Studio to start [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) and run your web app.</span></span> <span data-ttu-id="607c6-128">Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページが開きます。</span><span class="sxs-lookup"><span data-stu-id="607c6-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="607c6-129">ブラウザーのアドレス バーに表示される`localhost:port#`などのメカニズムではありませんし`example.com`です。</span><span class="sxs-lookup"><span data-stu-id="607c6-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="607c6-130">これはため`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="607c6-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="607c6-131">Visual Studio web プロジェクトの実行時に、ランダムなポートが、web サーバーに使用されます。</span><span class="sxs-lookup"><span data-stu-id="607c6-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="607c6-132">次の図では、ポート番号は、1234 です。</span><span class="sxs-lookup"><span data-stu-id="607c6-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="607c6-133">アプリケーションを実行するときに、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="607c6-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="607c6-134">すぐこの既定テンプレートを使用して自宅、連絡先、についてのページです。</span><span class="sxs-lookup"><span data-stu-id="607c6-134">Right out of the box this default template gives you Home, Contact and About pages.</span></span> <span data-ttu-id="607c6-135">上記の図に表示されない、**ホーム**、**に関する**と**連絡先**リンクします。</span><span class="sxs-lookup"><span data-stu-id="607c6-135">The image above doesn't show the **Home**, **About** and **Contact** links.</span></span> <span data-ttu-id="607c6-136">ブラウザー ウィンドウのサイズによっては、これらのリンクを参照するナビゲーション アイコンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="607c6-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)  

<span data-ttu-id="607c6-137">アプリケーションでは、登録およびログ記録のサポートも提供します。</span><span class="sxs-lookup"><span data-stu-id="607c6-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="607c6-138">次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC について少し説明します。</span><span class="sxs-lookup"><span data-stu-id="607c6-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="607c6-139">ASP.NET MVC アプリケーションを終了し、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="607c6-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="607c6-140">現在のチュートリアルの一覧は、次を参照してください。[記事をお勧め MVC](../mvc-learning-sequence.md)です。</span><span class="sxs-lookup"><span data-stu-id="607c6-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="607c6-141">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="607c6-141">See this App Running on Azure</span></span>

<span data-ttu-id="607c6-142">ライブ web アプリとして実行している完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="607c6-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="607c6-143">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。</span><span class="sxs-lookup"><span data-stu-id="607c6-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="607c6-144">Azure アカウントを Azure にこのソリューションを展開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="607c6-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="607c6-145">アカウントがない場合は、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="607c6-145">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="607c6-146">[無料の Azure アカウントを開設](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。</span><span class="sxs-lookup"><span data-stu-id="607c6-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="607c6-147">[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-お客様の MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。</span><span class="sxs-lookup"><span data-stu-id="607c6-147">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="607c6-148">次へ</span><span class="sxs-lookup"><span data-stu-id="607c6-148">Next</span></span>](adding-a-controller.md)
