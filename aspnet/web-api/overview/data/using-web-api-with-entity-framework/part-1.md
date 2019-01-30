---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Entity Framework 6 で Web API 2 を使用して |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは説明する ASP.NET Web API を使用して web アプリケーションの作成の基本のバック エンドです。 チュートリアルでは、データ レイアウトの Entity Framework 6 を使用しています.
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236524"
---
<a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="e4671-104">Entity Framework 6 で Web API 2 を使用</span><span class="sxs-lookup"><span data-stu-id="e4671-104">Using Web API 2 with Entity Framework 6</span></span>
====================

[<span data-ttu-id="e4671-105">完成したプロジェクトのダウンロード</span><span class="sxs-lookup"><span data-stu-id="e4671-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="e4671-106">このチュートリアルは、バック エンドを ASP.NET Web API を使用して web アプリケーションの作成の基本を説明します。</span><span class="sxs-lookup"><span data-stu-id="e4671-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="e4671-107">チュートリアルでは、クライアント側の JavaScript アプリケーションのデータ層、および Knockout.js Entity Framework 6 を使用します。</span><span class="sxs-lookup"><span data-stu-id="e4671-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="e4671-108">このチュートリアルでは、アプリを Azure App Service Web Apps にデプロイする方法も示します。</span><span class="sxs-lookup"><span data-stu-id="e4671-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e4671-109">このチュートリアルで使用されるソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="e4671-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="e4671-110">Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="e4671-110">Web API 2.1</span></span>
> - <span data-ttu-id="e4671-111">Visual Studio 2017 (Visual Studio 2017 ダウンロード[ここ](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="e4671-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="e4671-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e4671-112">Entity Framework 6</span></span>
> - <span data-ttu-id="e4671-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="e4671-113">.NET 4.7</span></span>
> - <span data-ttu-id="e4671-114">[Knockout.js](http://knockoutjs.com/) 3.1</span><span class="sxs-lookup"><span data-stu-id="e4671-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="e4671-115">このチュートリアルでは、Entity Framework 6 で ASP.NET Web API 2 を使用して、バックエンド データベースを操作する web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4671-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="e4671-116">作成するアプリケーションのスクリーン ショットを次に示します。</span><span class="sxs-lookup"><span data-stu-id="e4671-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="e4671-117">アプリでは、シングル ページ アプリケーション (SPA) のデザインを使用します。</span><span class="sxs-lookup"><span data-stu-id="e4671-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="e4671-118">「シングル ページ アプリケーション」は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。</span><span class="sxs-lookup"><span data-stu-id="e4671-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="e4671-119">最初のページの読み込み後に、アプリは、AJAX 要求を介してサーバーとで説明します。</span><span class="sxs-lookup"><span data-stu-id="e4671-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="e4671-120">AJAX では、アプリを使用して UI を更新する戻り値の JSON データを要求します。</span><span class="sxs-lookup"><span data-stu-id="e4671-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="e4671-121">AJAX は、新しいはありませんが、今日は JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理しやすくします。</span><span class="sxs-lookup"><span data-stu-id="e4671-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="e4671-122">このチュートリアルでは[Knockout.js](http://knockoutjs.com/)、任意の JavaScript クライアント フレームワークを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e4671-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="e4671-123">このアプリの主な構成要素を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e4671-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="e4671-124">ASP.NET MVC では、HTML ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4671-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="e4671-125">ASP.NET Web API は、AJAX 要求を処理し、JSON データを返します。</span><span class="sxs-lookup"><span data-stu-id="e4671-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="e4671-126">Knockout.js データ、HTML 要素をバインド JSON データ。</span><span class="sxs-lookup"><span data-stu-id="e4671-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="e4671-127">Entity Framework では、データベースに説明します。</span><span class="sxs-lookup"><span data-stu-id="e4671-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="e4671-128">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e4671-128">See this app running on Azure</span></span>

<span data-ttu-id="e4671-129">ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="e4671-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="e4671-130">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="e4671-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="e4671-131">このソリューションを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="e4671-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="e4671-132">アカウントがいない場合は、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="e4671-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="e4671-133">[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="e4671-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="e4671-134">[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="e4671-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e4671-135">プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="e4671-135">Create the project</span></span>

<span data-ttu-id="e4671-136">Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="e4671-136">Open Visual Studio.</span></span> <span data-ttu-id="e4671-137">**ファイル**メニューの **新規**を選択し、**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="e4671-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="e4671-138">(または選択**新しいプロジェクト**スタート ページです)。</span><span class="sxs-lookup"><span data-stu-id="e4671-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="e4671-139">**新しいプロジェクト**ダイアログ ボックスで、 **Web**左側のウィンドウで、 **ASP.NET Web アプリケーション (.NET Framework)** 中央のペインでします。</span><span class="sxs-lookup"><span data-stu-id="e4671-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="e4671-140">プロジェクトに名前を**BookService**選択**OK**。</span><span class="sxs-lookup"><span data-stu-id="e4671-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="e4671-141">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web API**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="e4671-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


<span data-ttu-id="e4671-142">**[OK]** をクリックして、プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4671-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="e4671-143">(省略可能) Azure の設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="e4671-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="e4671-144">プロジェクトを作成した後はいつでも Azure App Service Web Apps にデプロイすることもできます。</span><span class="sxs-lookup"><span data-stu-id="e4671-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="e4671-145">ソリューション エクスプ ローラーでクリックし、プロジェクトを右クリックして**発行**します。</span><span class="sxs-lookup"><span data-stu-id="e4671-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="e4671-146">表示されるウィンドウで、選択**開始**します。</span><span class="sxs-lookup"><span data-stu-id="e4671-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="e4671-147">**発行先を選択** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="e4671-148">**[プロファイルの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e4671-148">Select **Create Profile**.</span></span> <span data-ttu-id="e4671-149">**[App Service の作成]** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="e4671-150">既定値をまたはアプリケーション名、リソース グループ、ホスティング プラン、Azure サブスクリプションと地理的リージョンの別の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="e4671-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="e4671-151">選択**SQL データベースを作成**です。</span><span class="sxs-lookup"><span data-stu-id="e4671-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="e4671-152">**SQL Server の構成** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="e4671-153">既定値を受け入れるか、別の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="e4671-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="e4671-154">入力、**管理者ユーザー名**と**管理者パスワード**新しいデータベースの。</span><span class="sxs-lookup"><span data-stu-id="e4671-154">Enter an **Adminstrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="e4671-155">選択**OK**完了したら。</span><span class="sxs-lookup"><span data-stu-id="e4671-155">Select **OK** when you're done.</span></span> <span data-ttu-id="e4671-156">**App Service の作成**ページが再表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="e4671-157">選択**作成**プロファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="e4671-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="e4671-158">右上隅にある展開が進行中であることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="e4671-159">少し時間が、後に、**発行**ウィンドウが再び表示されます。</span><span class="sxs-lookup"><span data-stu-id="e4671-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="e4671-160">アプリを展開するために作成したプロファイルは、ご利用いただけます。</span><span class="sxs-lookup"><span data-stu-id="e4671-160">The profile you created to deploy the app is now available.</span></span> 


> [!div class="step-by-step"]
> [<span data-ttu-id="e4671-161">次へ</span><span class="sxs-lookup"><span data-stu-id="e4671-161">Next</span></span>](part-2.md)
