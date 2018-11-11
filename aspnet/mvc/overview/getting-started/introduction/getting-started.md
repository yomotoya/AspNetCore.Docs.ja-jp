---
uid: mvc/overview/getting-started/introduction/getting-started
title: ASP.NET MVC 5 の概要 |Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 462583a42f20126ef8f8b5927268c20ec1ceab89
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505805"
---
<a name="getting-started-with-aspnet-mvc-5"></a><span data-ttu-id="4bff7-102">ASP.NET MVC 5 の概要</span><span class="sxs-lookup"><span data-stu-id="4bff7-102">Getting started with ASP.NET MVC 5</span></span>
====================
<span data-ttu-id="4bff7-103">によって[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="4bff7-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [consider RP](../../../../includes/razor.md)]

<span data-ttu-id="4bff7-104">このチュートリアルは、ASP.NET MVC 5 web アプリを使用して、構築の基礎を説明[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-104">This tutorial teaches you the basics of building an ASP.NET MVC 5 web app using [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="4bff7-105">チュートリアルでは、最終的なソース コードが上にある[GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-105">The final source code for the tutorial is located on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).</span></span>

<span data-ttu-id="4bff7-106">このチュートリアルの執筆者[Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) )、 [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) )、および[Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )</span><span class="sxs-lookup"><span data-stu-id="4bff7-106">This tutorial was written by [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[@scottgu](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](https://twitter.com/shanselman) ), and [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )</span></span>

<span data-ttu-id="4bff7-107">このアプリを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="4bff7-107">You need an Azure account to deploy this app to Azure:</span></span>

- <span data-ttu-id="4bff7-108">できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-108">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="4bff7-109">できます[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="4bff7-109">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="get-started"></a><span data-ttu-id="4bff7-110">作業開始</span><span class="sxs-lookup"><span data-stu-id="4bff7-110">Get started</span></span>

<span data-ttu-id="4bff7-111">まず[Visual Studio 2017 をインストールする](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-111">Start by [installing Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="4bff7-112">次に、Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="4bff7-112">Then, open Visual Studio.</span></span>

<span data-ttu-id="4bff7-113">Visual Studio は、IDE、または統合開発環境です。</span><span class="sxs-lookup"><span data-stu-id="4bff7-113">Visual Studio is an IDE, or integrated development environment.</span></span> <span data-ttu-id="4bff7-114">Microsoft Word を使用してドキュメントを記述するのにのと同じようにアプリケーションを作成、IDE を使用します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-114">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="4bff7-115">Visual Studio を使用できるさまざまなオプションを示す下部の一覧です。</span><span class="sxs-lookup"><span data-stu-id="4bff7-115">In Visual Studio, there's a list along the bottom showing various options available to you.</span></span> <span data-ttu-id="4bff7-116">また、IDE でタスクを実行する別の方法を提供するメニューがあります。</span><span class="sxs-lookup"><span data-stu-id="4bff7-116">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="4bff7-117">選択する代わりに、**新しいプロジェクト**上、**スタート ページ**、メニュー バーを使用して選択**ファイル** > **の新しいプロジェクト**.</span><span class="sxs-lookup"><span data-stu-id="4bff7-117">For example, instead of selecting **New Project** on the **Start page**, you can use the menu bar and select **File** > **New Project**.</span></span>

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a><span data-ttu-id="4bff7-118">最初のアプリの作成</span><span class="sxs-lookup"><span data-stu-id="4bff7-118">Create your first app</span></span>

<span data-ttu-id="4bff7-119">**スタート ページ**、**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-119">On the **Start page**, select **New Project**.</span></span> <span data-ttu-id="4bff7-120">**新しいプロジェクト**ダイアログ ボックスで、 **Visual c#** し、左側のカテゴリ**Web**、クリックして、 **ASP.NET Web アプリケーション (.NET Framework)** プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="4bff7-120">In the **New project** dialog box, select the **Visual C#** category on the left, then **Web**, and then select the **ASP.NET Web Application (.NET Framework)** project template.</span></span> <span data-ttu-id="4bff7-121">プロジェクトに"MvcMovie"という名前にして**OK**します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-121">Name your project "MvcMovie" and then choose **OK**.</span></span>

![](getting-started/_static/image2.png)

<span data-ttu-id="4bff7-122">**新しい ASP.NET Web アプリケーション**ダイアログ ボックスで、選択**MVC**選び、 **OK**します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-122">In the **New ASP.NET Web Application** dialog, choose **MVC** and then choose **OK**.</span></span>

![](getting-started/_static/image3.png)

<span data-ttu-id="4bff7-123">Visual Studio は、何もせず、実用的なアプリケーションが現在あるため、作成した ASP.NET MVC プロジェクトの既定のテンプレートを使用!</span><span class="sxs-lookup"><span data-stu-id="4bff7-123">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="4bff7-124">これは、単純です"Hello World!"</span><span class="sxs-lookup"><span data-stu-id="4bff7-124">This is a simple "Hello World!"</span></span> <span data-ttu-id="4bff7-125">プロジェクト、およびそのアプリケーションにお勧めです。</span><span class="sxs-lookup"><span data-stu-id="4bff7-125">project, and it's a good place to start your application.</span></span>

![](getting-started/_static/image4.png)

<span data-ttu-id="4bff7-126">**F5** キーを押してデバッグを開始します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-126">Press **F5** to start debugging.</span></span> <span data-ttu-id="4bff7-127">押したとき**F5**、Visual Studio の起動時に[IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)し、web アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-127">When you press **F5**, Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your web app.</span></span> <span data-ttu-id="4bff7-128">Visual Studio は、ブラウザーを起動し、アプリケーションのホーム ページを開きます。</span><span class="sxs-lookup"><span data-stu-id="4bff7-128">Visual Studio then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="4bff7-129">ブラウザーのアドレス バーが表示されますが`localhost:port#`ようなものではありません`example.com`します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-129">Notice that the address bar of the browser says `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="4bff7-130">だ`localhost`常にこの例でビルドしたアプリケーションを実行して、自分のローカル コンピューターを指します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-130">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="4bff7-131">Visual Studio web プロジェクトを実行すると、web サーバーのランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="4bff7-131">When Visual Studio runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="4bff7-132">次の図では、ポート番号は、1234 です。</span><span class="sxs-lookup"><span data-stu-id="4bff7-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="4bff7-133">アプリケーションを実行するときに、別のポート番号を確認します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-133">When you run the application, you'll see a different port number.</span></span>

![](getting-started/_static/image5.png)

<span data-ttu-id="4bff7-134">すぐに使えるこの既定のテンプレートを使用する`Home`、 `Contact`、および`About`ページ。</span><span class="sxs-lookup"><span data-stu-id="4bff7-134">Right out of the box this default template gives you `Home`, `Contact`, and `About` pages.</span></span> <span data-ttu-id="4bff7-135">次の画像が表示されない、**ホーム**、**について**と**連絡先**リンク。</span><span class="sxs-lookup"><span data-stu-id="4bff7-135">The image below doesn't show the **Home**, **About**, and **Contact** links.</span></span> <span data-ttu-id="4bff7-136">ブラウザー ウィンドウのサイズによっては、これらのリンクを参照するナビゲーション アイコンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4bff7-136">Depending on the size of your browser window, you might need to click the navigation icon to see these links.</span></span>

![](getting-started/_static/image6.png)

<span data-ttu-id="4bff7-137">アプリケーションには、登録し、ログインのサポートも提供します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-137">The application also provides support to register and log in.</span></span> <span data-ttu-id="4bff7-138">次の手順では、このアプリケーションの動作を変更し、ASP.NET MVC についてもう少し説明します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-138">The next step is to change how this application works and learn a little bit about ASP.NET MVC.</span></span> <span data-ttu-id="4bff7-139">ASP.NET MVC アプリケーションを終了し、いくつかのコードを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="4bff7-139">Close the ASP.NET MVC application and let's change some code.</span></span>

<span data-ttu-id="4bff7-140">現在のチュートリアルの一覧は、次を参照してください。[の記事をお勧めします。 MVC](../mvc-learning-sequence.md)します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-140">For a list of current tutorials, see [MVC recommended articles](../mvc-learning-sequence.md).</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="4bff7-141">Azure で実行されているこのアプリを参照してください。</span><span class="sxs-lookup"><span data-stu-id="4bff7-141">See this app running on Azure</span></span>

<span data-ttu-id="4bff7-142">ライブ web アプリとして実行されている完成したサイトを参照してもよろしいですか。</span><span class="sxs-lookup"><span data-stu-id="4bff7-142">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="4bff7-143">Azure アカウントに、アプリの完全なバージョンを展開するには、次のボタンをクリックするだけです。</span><span class="sxs-lookup"><span data-stu-id="4bff7-143">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

<span data-ttu-id="4bff7-144">このソリューションを Azure にデプロイする Azure アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="4bff7-144">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="4bff7-145">アカウントがない場合は、次のオプションのいずれかを使用して、1 つを作成します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-145">If you don't already have an account, use one of the following options to create one:</span></span>

- <span data-ttu-id="4bff7-146">[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="4bff7-146">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="4bff7-147">[Visual Studio サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers)-Visual Studio サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。</span><span class="sxs-lookup"><span data-stu-id="4bff7-147">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4bff7-148">次へ</span><span class="sxs-lookup"><span data-stu-id="4bff7-148">Next</span></span>](adding-a-controller.md)
