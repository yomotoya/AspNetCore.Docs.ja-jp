---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 で Web API 2 を使用して |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。 チュートリアルでは、データ レイアウトの Entity Framework 6 を使用しています.
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 4abe0e06dfd927765efd8e566584e111cf4117d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826473"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="6f078-104">Entity Framework 6 で Web API 2 を使用</span><span class="sxs-lookup"><span data-stu-id="6f078-104">Using Web API 2 with Entity Framework 6</span></span>
====================
<span data-ttu-id="6f078-105">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6f078-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6f078-106">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="6f078-106">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="6f078-107">このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。</span><span class="sxs-lookup"><span data-stu-id="6f078-107">This tutorial will teach you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="6f078-108">チュートリアルでは、クライアント側の JavaScript アプリケーションのデータ層、および Knockout.js Entity Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="6f078-108">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="6f078-109">このチュートリアルでは、アプリを Azure App Service Web Apps にデプロイする方法も示します。</span><span class="sxs-lookup"><span data-stu-id="6f078-109">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6f078-110">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="6f078-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6f078-111">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="6f078-111">Web API 2.1</span></span>
> - [<span data-ttu-id="6f078-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="6f078-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6f078-113">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6f078-113">Entity Framework 6</span></span>
> - <span data-ttu-id="6f078-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6f078-114">.NET 4.5</span></span>
> - <span data-ttu-id="6f078-115">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="6f078-115">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>


<span data-ttu-id="6f078-116">このチュートリアルでは、Entity Framework 6 で ASP.NET Web API 2 を使用して、バックエンド データベースを操作する web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6f078-116">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="6f078-117">作成するアプリケーションのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="6f078-117">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="6f078-118">アプリでは、シングル ページ アプリケーション (SPA) のデザインを使用します。</span><span class="sxs-lookup"><span data-stu-id="6f078-118">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="6f078-119">「シングル ページ アプリケーション」は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。</span><span class="sxs-lookup"><span data-stu-id="6f078-119">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="6f078-120">最初のページの読み込み後に、アプリは、AJAX 要求を介してサーバーとで説明します。</span><span class="sxs-lookup"><span data-stu-id="6f078-120">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="6f078-121">AJAX では、アプリを使用して UI を更新する戻り値の JSON データを要求します。</span><span class="sxs-lookup"><span data-stu-id="6f078-121">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="6f078-122">AJAX は、新しいはありませんが、今日は JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理しやすくします。</span><span class="sxs-lookup"><span data-stu-id="6f078-122">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="6f078-123">このチュートリアルでは[Knockout.js](http://knockoutjs.com/)、任意の JavaScript クライアント フレームワークを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="6f078-123">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="6f078-124">このアプリの主な構成要素を次に示します。</span><span class="sxs-lookup"><span data-stu-id="6f078-124">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="6f078-125">ASP.NET MVC では、HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="6f078-125">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="6f078-126">ASP.NET Web API は、AJAX 要求を処理し、JSON データを返します。</span><span class="sxs-lookup"><span data-stu-id="6f078-126">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="6f078-127">Knockout.js データ、HTML 要素をバインド JSON データ。</span><span class="sxs-lookup"><span data-stu-id="6f078-127">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="6f078-128">Entity Framework では、データベースに説明します。</span><span class="sxs-lookup"><span data-stu-id="6f078-128">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="6f078-129">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="6f078-129">See this App Running on Azure</span></span>

<span data-ttu-id="6f078-130">ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="6f078-130">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="6f078-131">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。</span><span class="sxs-lookup"><span data-stu-id="6f078-131">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="6f078-132">このソリューションを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="6f078-132">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="6f078-133">アカウントがいない場合は、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="6f078-133">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="6f078-134">[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="6f078-134">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="6f078-135">[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="6f078-135">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="6f078-136">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="6f078-136">Create the Project</span></span>

<span data-ttu-id="6f078-137">Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="6f078-137">Open Visual Studio.</span></span> <span data-ttu-id="6f078-138">**ファイル**メニューの **新規**を選択し、**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="6f078-138">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="6f078-139">(をクリックしてまたは**新しいプロジェクト**スタート ページです)。</span><span class="sxs-lookup"><span data-stu-id="6f078-139">(Or click **New Project** on the Start page.)</span></span>

<span data-ttu-id="6f078-140">**新しいプロジェクト**ダイアログ ボックスで、をクリックして**Web**左側のウィンドウで、 **ASP.NET Web アプリケーション**中央のペインで。</span><span class="sxs-lookup"><span data-stu-id="6f078-140">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span> <span data-ttu-id="6f078-141">BookService プロジェクトの名前を指定し、をクリックして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="6f078-141">Name the project BookService and click **OK**.</span></span>

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

<span data-ttu-id="6f078-142">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web API**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="6f078-142">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

<span data-ttu-id="6f078-143">場合は、Azure App Service でプロジェクトをホストする、**クラウドでホスト**ボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="6f078-143">If you want to host the project in a Azure App Service, leave the **Host in the cloud** box checked.</span></span>

<span data-ttu-id="6f078-144">**[OK]** をクリックして、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="6f078-144">Click **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="6f078-145">(省略可能) Azure の設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="6f078-145">Configure Azure Settings (Optional)</span></span>

<span data-ttu-id="6f078-146">中断した場合、**クラウド内のホスト**オプションのチェック、Visual Studio には、Microsoft Azure にサインインするように求められます</span><span class="sxs-lookup"><span data-stu-id="6f078-146">If you left the **Host in Cloud** option checked, Visual Studio will prompt you to sign in to Microsoft Azure</span></span>

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

<span data-ttu-id="6f078-147">Azure にサインインした後に Visual Studio では、web アプリを構成するように求められます。</span><span class="sxs-lookup"><span data-stu-id="6f078-147">After you sign in to Azure, Visual Studio prompts you to configure the web app.</span></span> <span data-ttu-id="6f078-148">サイトの名前を入力、Azure サブスクリプションを選択し、地理的リージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6f078-148">Enter a name for the site, select your Azure subscription, and select a geographical region.</span></span> <span data-ttu-id="6f078-149">**データベース サーバー**、**新しいサーバーの作成**です。</span><span class="sxs-lookup"><span data-stu-id="6f078-149">Under **Database server**, select **Create new server**.</span></span> <span data-ttu-id="6f078-150">管理者のユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="6f078-150">Enter an administrator username and password.</span></span>

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="6f078-151">次へ</span><span class="sxs-lookup"><span data-stu-id="6f078-151">Next</span></span>](part-2.md)
