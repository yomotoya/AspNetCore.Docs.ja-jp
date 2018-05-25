---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: ASP.NET Web Pages の概要 - WebMatrix を使用して、サイトの発行 |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルは、Microsoft WebMatrix と ASP.NET Web Pages を紹介するチュートリアルのセットの最終回です。 T サイトを発行する方法についても説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a><span data-ttu-id="2545b-104">WebMatrix を使用してサイトを発行する-ASP.NET Web Pages の概要</span><span class="sxs-lookup"><span data-stu-id="2545b-104">Introducing ASP.NET Web Pages - Publishing a Site by Using WebMatrix</span></span>
====================
<span data-ttu-id="2545b-105">によって[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2545b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2545b-106">このチュートリアルは、Microsoft WebMatrix と ASP.NET Web Pages を紹介するチュートリアルのセットの最終回です。</span><span class="sxs-lookup"><span data-stu-id="2545b-106">This tutorial is the final installment in the tutorial set that introduces ASP.NET Web Pages and Microsoft WebMatrix.</span></span> <span data-ttu-id="2545b-107">これには、他のユーザーが操作できるように、サイトをインターネットに公開する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2545b-107">It discusses how to publish your site to the Internet so that others can work with it.</span></span> <span data-ttu-id="2545b-108">を通じてシリーズを完了すると想定[一貫した表示を作成する ASP.NET Web Pages サイトの](https://go.microsoft.com/fwlink/?LinkId=251585)します。</span><span class="sxs-lookup"><span data-stu-id="2545b-108">It assumes you have completed the series through [Creating a Consistent Look for ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=251585).</span></span>
> 
> <span data-ttu-id="2545b-109">使用して、サイトを発行する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2545b-109">You'll learn how to publish your site using:</span></span>
> 
> - <span data-ttu-id="2545b-110">Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="2545b-110">Microsoft Azure</span></span>
> - <span data-ttu-id="2545b-111">Web ホスティング企業</span><span class="sxs-lookup"><span data-stu-id="2545b-111">Web Hosting Company</span></span>


## <a name="about-publishing-your-site"></a><span data-ttu-id="2545b-112">サイトを発行する方法</span><span class="sxs-lookup"><span data-stu-id="2545b-112">About Publishing Your Site</span></span>

<span data-ttu-id="2545b-113">ここでは、最大では、テスト、ページを含む、ローカル コンピューター上のすべての作業を行っています。</span><span class="sxs-lookup"><span data-stu-id="2545b-113">Up to now, you've done all your work on a local computer, including testing your pages.</span></span> <span data-ttu-id="2545b-114">実行する、<em>.cshtml</em>ページ、WebMatrix、つまり IIS Express に組み込まれている web サーバーを使用しています。</span><span class="sxs-lookup"><span data-stu-id="2545b-114">To run your<em>.cshtml</em> pages, you've used the web server that's built into WebMatrix, namely IIS Express.</span></span> <span data-ttu-id="2545b-115">もちろんを除き、作成したサイトを参照していないことができます。</span><span class="sxs-lookup"><span data-stu-id="2545b-115">But of course no one can see the site you've created except you.</span></span> <span data-ttu-id="2545b-116">他のユーザーが、サイトを使用するにはには、インターネットに公開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2545b-116">To let others work with your site, you have to publish it to the Internet.</span></span>

<span data-ttu-id="2545b-117">既に公開 web サーバーへのアクセスがない限り、発行はことを意味を持つアカウントがあること、*クラウド プラットフォーム*または*ホスティング プロバイダー*です。</span><span class="sxs-lookup"><span data-stu-id="2545b-117">Unless you have access to a public web server already, publishing means that you have to have an account with a *cloud platform* or a *hosting provider*.</span></span> <span data-ttu-id="2545b-118">Microsoft Azure などのクラウド プラットフォームでは、アプリケーションのオンデマンドのインフラストラクチャを提供します。</span><span class="sxs-lookup"><span data-stu-id="2545b-118">A cloud platform, such as Microsoft Azure, provides on-demand infrastructure for your applications.</span></span> <span data-ttu-id="2545b-119">ホスティング プロバイダーは、パブリックにアクセスできる web サーバーを所有しているとをレンタルするは、会社のサイトの領域です。</span><span class="sxs-lookup"><span data-stu-id="2545b-119">A hosting provider is a company that owns publicly accessible web servers and that will rent you space for your site.</span></span> <span data-ttu-id="2545b-120">ホスティング プランの 1 か月の数ドルから実行 (無料) 小規模オフィスの何百もの大規模な商用 web サイトの毎月のドルにします。</span><span class="sxs-lookup"><span data-stu-id="2545b-120">Hosting plans run from a few dollars a month (or even free) for small sites to many hundreds of dollars a month for high-volume commercial websites.</span></span>

> [!NOTE]
> <span data-ttu-id="2545b-121">インターネット サービスを自宅で使用するインターネット サービス プロバイダー (ISP) 経由で公開 web サーバーへのアクセスがあります。</span><span class="sxs-lookup"><span data-stu-id="2545b-121">You might have access to a public web server via the internet service provider (ISP) that you use to get internet service at home.</span></span> <span data-ttu-id="2545b-122">ただし、ホスティング プロバイダーは、ASP.NET Web Pages をサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2545b-122">However, your hosting provider must support ASP.NET Web Pages.</span></span> <span data-ttu-id="2545b-123">多くの Isp しないが常に確認します。</span><span class="sxs-lookup"><span data-stu-id="2545b-123">Many ISPs don't, but it's always worth checking.</span></span>


<span data-ttu-id="2545b-124">このチュートリアルでは伝えを公開する方法の概要です。</span><span class="sxs-lookup"><span data-stu-id="2545b-124">In this tutorial, we'll give you an overview of how to publish.</span></span> <span data-ttu-id="2545b-125">すべてのホスティング プロバイダーのプロセスが少し異なるため、すべての正確な情報を提供する実用的ではありません。</span><span class="sxs-lookup"><span data-stu-id="2545b-125">It's not practical to provide exact details for everything, because the process differs a bit for every hosting provider.</span></span> <span data-ttu-id="2545b-126">プロセスのしくみの使用感を得られます。</span><span class="sxs-lookup"><span data-stu-id="2545b-126">But you'll get a good idea of how the process works.</span></span>

<span data-ttu-id="2545b-127">このチュートリアルには、4 つのセクションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2545b-127">This tutorial contains four sections:</span></span>

1. [<span data-ttu-id="2545b-128">既定のページを設定します。</span><span class="sxs-lookup"><span data-stu-id="2545b-128">Setting up the default page</span></span>](#defaultpage)
2. <span data-ttu-id="2545b-129">発行 (次のいずれかを選択)</span><span class="sxs-lookup"><span data-stu-id="2545b-129">Publishing (choose one of the following)</span></span>  
 <span data-ttu-id="2545b-130">a. </span><span class="sxs-lookup"><span data-stu-id="2545b-130">a.</span></span> [<span data-ttu-id="2545b-131">Microsoft azure サイトの発行</span><span class="sxs-lookup"><span data-stu-id="2545b-131">Publishing Your Site to Microsoft Azure</span></span>](#azure)  
 <span data-ttu-id="2545b-132">b. </span><span class="sxs-lookup"><span data-stu-id="2545b-132">b.</span></span> [<span data-ttu-id="2545b-133">Web ポータルをホストするサイトの発行</span><span class="sxs-lookup"><span data-stu-id="2545b-133">Publishing Your Site to a Web Hosting Company</span></span>](#host)
3. [<span data-ttu-id="2545b-134">実際のサイトを更新します再パブリッシュ。</span><span class="sxs-lookup"><span data-stu-id="2545b-134">Updating the Live Site: Republishing</span></span>](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a><span data-ttu-id="2545b-135">既定のページを設定します。</span><span class="sxs-lookup"><span data-stu-id="2545b-135">Setting up the default page</span></span>

<span data-ttu-id="2545b-136">ユーザーが web サイトのベース アドレスに移動する、ユーザーに、サイトの既定のページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-136">When a user navigates to the base address for your web site, the default page for your site is displayed to the user.</span></span> <span data-ttu-id="2545b-137">たとえば、Default.htm は www.contoso.com に、サイトの既定のページとして設定されている場合に移動する<strong>www.contoso.com</strong>に移動することと同じ<strong>www.contoso.com/Default.htm</strong>です。</span><span class="sxs-lookup"><span data-stu-id="2545b-137">For example, when Default.htm is set as the default page for the site at www.contoso.com, then navigating to <strong>www.contoso.com</strong> is the same as navigating to <strong>www.contoso.com/Default.htm</strong>.</span></span>

<span data-ttu-id="2545b-138">サイトを使用して現在のところ、 **Default.cshtml**既定のページとして。</span><span class="sxs-lookup"><span data-stu-id="2545b-138">Currently, your site uses **Default.cshtml** as the default page.</span></span> <span data-ttu-id="2545b-139">このページは、既定のページに適していますが、このチュートリアルで追加していないコンテンツはそのページに、空白のページを表示するようにします。</span><span class="sxs-lookup"><span data-stu-id="2545b-139">This page is fine for your default page, but in this tutorial you have not added any content to that page so it would display a blank page.</span></span> <span data-ttu-id="2545b-140">Default.cshtml を開き、次のコードにコンテンツを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2545b-140">Open Default.cshtml and replace the content with the following code.</span></span>

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

<span data-ttu-id="2545b-141">これで、サイトは、パブリケーションの準備がします。</span><span class="sxs-lookup"><span data-stu-id="2545b-141">Now your site is ready for publication.</span></span> <span data-ttu-id="2545b-142">最初に、サイトを Azure に配置する方法と、web ポータルをホストに展開する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-142">First, you will see how to deploy the site to Azure, and then how to deploy it to a web hosting company.</span></span> <span data-ttu-id="2545b-143">いずれか オプションは、web サイトのだけ従う必要が配置オプションのいずれか。 ある</span><span class="sxs-lookup"><span data-stu-id="2545b-143">Either option works for your web site, and you only need to follow one of the deployment options.</span></span>

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a><span data-ttu-id="2545b-144">Microsoft azure サイトの発行</span><span class="sxs-lookup"><span data-stu-id="2545b-144">Publishing Your Site to Microsoft Azure</span></span>

<span data-ttu-id="2545b-145">このチュートリアルは最初に表示する Microsoft Azure には、サイトを展開する方法です。</span><span class="sxs-lookup"><span data-stu-id="2545b-145">This tutorial will first show you how to deploy your site to Microsoft Azure.</span></span> <span data-ttu-id="2545b-146">Microsoft アカウントでサインイン、して、Azure で最大 10 個の無料サイトを作成できます。</span><span class="sxs-lookup"><span data-stu-id="2545b-146">By signing in with a Microsoft account, you can create up to 10 free sites on Azure.</span></span> <span data-ttu-id="2545b-147">これら無料サイトでは、サイトをテストする便利な手段を提供します。</span><span class="sxs-lookup"><span data-stu-id="2545b-147">These free sites provide a convenient way to test your sites.</span></span> <span data-ttu-id="2545b-148">このサイトの例はすべて、無料のサイトの使用を回避するには、後でいつでも削除できます。</span><span class="sxs-lookup"><span data-stu-id="2545b-148">You can always delete this example site later to avoid using all of your free sites.</span></span> <span data-ttu-id="2545b-149">ほんの数分で無料の試用アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="2545b-149">You can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2545b-150">詳細については、「 [Azure 無料評価版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。</span><span class="sxs-lookup"><span data-stu-id="2545b-150">For details, see [Azure Free Trial](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

<span data-ttu-id="2545b-151">[WebMatrix] リボンをクリックして、**発行**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-151">In the WebMatrix ribbon, click the **Publish** button.</span></span>

![[WebMatrix] リボンのボタンの 'publish'](publishing/_static/image1.png)

<span data-ttu-id="2545b-153">**サイトの発行** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-153">The **Publish Your Site** dialog box is displayed.</span></span> <span data-ttu-id="2545b-154">ダイアログ ボックスにが含まれていない、Microsoft アカウントにサインインしている場合、 **azure**リンクします。</span><span class="sxs-lookup"><span data-stu-id="2545b-154">If you have not signed in to your Microsoft account, the dialog box will contain a **Get started with Azure** link.</span></span> <span data-ttu-id="2545b-155">このリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-155">Click this link.</span></span>

![サイトを発行します。](publishing/_static/image2.png)

<span data-ttu-id="2545b-157">いないでは、サインイン、Microsoft アカウントに場合、は、もう一度サインインする機会を表示します。</span><span class="sxs-lookup"><span data-stu-id="2545b-157">If you have not signed in to a Microsoft account, you are again given the opportunity to sign in.</span></span> <span data-ttu-id="2545b-158">Azure でサイトを発行する Microsoft アカウントにサインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2545b-158">You must sign in to a Microsoft account to publish your site on Azure.</span></span>

![サインイン](publishing/_static/image3.png)

<span data-ttu-id="2545b-160">にサインインした後、Microsoft アカウントには、ダイアログ ボックスには、Azure で新しいサイトを作成するか、Azure で既存のサイトのいずれかに接続へのリンクが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2545b-160">After signing in to your Microsoft account, the dialog box contains links to create a new site on Azure or connect to one of your existing sites on Azure.</span></span>

![新しいサイトを作成します。](publishing/_static/image4.png)

<span data-ttu-id="2545b-162">選択**新しいサイトを作成**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-162">Select **Create a new site**.</span></span>

<span data-ttu-id="2545b-163">プロジェクトの名前を付けた場合**WebPagesMovies**、サイトの既定の名前になります**webpagesmovies.azurewebsites.net**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-163">If you named your project **WebPagesMovies**, the default name for your site will be **webpagesmovies.azurewebsites.net**.</span></span> <span data-ttu-id="2545b-164">この既定の名前は最も可能性の高い使用できない、赤色の感嘆符で示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-164">This default name is most likely not available, as indicated by the red exclamation mark.</span></span>

![既定の Web サイト名](publishing/_static/image5.png)

<span data-ttu-id="2545b-166">利用可能なものにサイトの名前を変更し、場所の近くにある場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="2545b-166">Change the site name to something that is available, and select a location that is close to your location.</span></span>

![変更されたサイト名](publishing/_static/image6.png)

<span data-ttu-id="2545b-168">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-168">Click **OK**.</span></span>

<span data-ttu-id="2545b-169">WebMatrix performss がかどうか、サーバー、サイトとの互換性をテストします。</span><span class="sxs-lookup"><span data-stu-id="2545b-169">WebMatrix performss a test to determine if the server is compatible with your site.</span></span>

![互換性テスト](publishing/_static/image7.png)

<span data-ttu-id="2545b-171">選択**続行**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-171">Select **Continue**.</span></span>

<span data-ttu-id="2545b-172">互換性テストの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-172">The results of the compatibility test are displayed.</span></span>

![互換性の結果](publishing/_static/image8.png)

<span data-ttu-id="2545b-174">選択**続行**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-174">Select **Continue**.</span></span>

<span data-ttu-id="2545b-175">WebMatrix には、ファイルおよびサイトにパブリッシュされるデータベースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-175">WebMatrix displays the files and databases that will be published to the site.</span></span> <span data-ttu-id="2545b-176">これは、サイトを発行する最初の時間であるため、すべてのファイルの一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-176">Since this is the first time you are publishing the site, all of the files are listed.</span></span> <span data-ttu-id="2545b-177">発行する準備ができていないファイルをオフにすることができます。</span><span class="sxs-lookup"><span data-stu-id="2545b-177">You can uncheck a file that is not ready to be published.</span></span> <span data-ttu-id="2545b-178">その後の印刷物では、変更されたファイルのみが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-178">In the subsequent publications, only the files that have changed will be displayed.</span></span> <span data-ttu-id="2545b-179">参照してください[実際のサイトを更新します。 再パブリッシュ](#update)です。</span><span class="sxs-lookup"><span data-stu-id="2545b-179">See [Updating the Live Site: Republishing](#update).</span></span>

![プレビューを公開します。](publishing/_static/image9.png)

<span data-ttu-id="2545b-181">選択**続行**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-181">Select **Continue**.</span></span>

<span data-ttu-id="2545b-182">サイトは、Azure に配置されている、展開が完了したかを示す、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-182">After the site has been deployed to Azure, a message is displayed that indicates the deployment has completed.</span></span>

![発行完了](publishing/_static/image10.png)

<span data-ttu-id="2545b-184">サイトとデータベース、Azure に発行されているし、はパブリックに使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2545b-184">Your site and database have been published to Azure, and are now available to the public.</span></span> <span data-ttu-id="2545b-185">発行が完了し、展開されたサイトが表示されますを示すメッセージ内のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-185">Click the link in the message indicating publishing has completed, and you will now see your deployed site.</span></span> <span data-ttu-id="2545b-186">またはインターネットにアクセスできるユーザーを追加したり、データベース内のレコードを変更します。</span><span class="sxs-lookup"><span data-stu-id="2545b-186">You or anyone with Internet access can add or modify records in the database.</span></span>

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a><span data-ttu-id="2545b-187">Web ポータルをホストするサイトの発行</span><span class="sxs-lookup"><span data-stu-id="2545b-187">Publishing Your Site to a Web Hosting Company</span></span>

<span data-ttu-id="2545b-188">Azure に発行しない場合は、web ホスティング企業に代わりにサイトを発行することができます。</span><span class="sxs-lookup"><span data-stu-id="2545b-188">If you decide to not publish to Azure, you can instead publish your site to a web hosting company.</span></span>

<span data-ttu-id="2545b-189">クリックして、 **web ホスティングの検索**リンクします。</span><span class="sxs-lookup"><span data-stu-id="2545b-189">Click the **Find web hosting** link.</span></span>

![発行の設定 ダイアログ ボックスで 'web ホスティングの検索' のボタン](publishing/_static/image12.png)

<span data-ttu-id="2545b-191">ページに移動するには、「ASP.NET をサポートするホスティング プロバイダー一覧を Microsoft サイト。</span><span class="sxs-lookup"><span data-stu-id="2545b-191">You go to a page on the Microsoft site that lists hosting providers that support ASP.NET.</span></span>

![ホスティング プロバイダーの一覧のマイクロソフト サイト上のページ](publishing/_static/image13.png)

<span data-ttu-id="2545b-193">当然ながら、長期的に必要なホスティング機能だけを今すぐがわかりにくいことができます。</span><span class="sxs-lookup"><span data-stu-id="2545b-193">Obviously, it can be difficult to know now exactly what hosting features you might require over the long term.</span></span> <span data-ttu-id="2545b-194">次に、いくつかの考慮事項を示します。</span><span class="sxs-lookup"><span data-stu-id="2545b-194">Here are a couple of things to consider:</span></span>

- <span data-ttu-id="2545b-195">WebPagesMovies サイト用に、SQL Server は、多くの場合、余分なコストの別のアドオンを持つ必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2545b-195">For purposes of the WebPagesMovies site, you don't have to have a separate add-on for SQL Server, which often costs extra.</span></span> <span data-ttu-id="2545b-196">サイト内には自己完結型である SQL Server Compact Edition、使用しています。</span><span class="sxs-lookup"><span data-stu-id="2545b-196">In your site, you're using SQL Server Compact Edition, which is self-contained.</span></span> <span data-ttu-id="2545b-197">ただし、将来の web サイトの作業を行ういくつかの SQL Server のアクセスを必要があります。</span><span class="sxs-lookup"><span data-stu-id="2545b-197">However, you might need SQL Server access for some future website work you do.</span></span> <span data-ttu-id="2545b-198">場合がありますと思われる場合は、SQL Server 機能を後で追加できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2545b-198">If you think you might, make sure that you can add SQL Server capability later.</span></span>
- <span data-ttu-id="2545b-199">ホスティング プロバイダーが、Web Deploy の発行プロトコルをサポートしているかどうかを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2545b-199">Check whether the hosting provider supports the Web Deploy publishing protocol.</span></span> <span data-ttu-id="2545b-200">FTP プロトコルを使用して発行することができますが、Web Deploy を使用する方が便利です。</span><span class="sxs-lookup"><span data-stu-id="2545b-200">You can publish by using FTP protocol, but it's more convenient to use Web Deploy.</span></span>

<span data-ttu-id="2545b-201">一部のサイトには、無料の試用期間が提供しています。</span><span class="sxs-lookup"><span data-stu-id="2545b-201">Some sites offer a free trial period.</span></span> <span data-ttu-id="2545b-202">無料の試用期間は発行を再試行することをお勧めして、WebMatrix と ASP.NET Web Pages をまだ実験中にホストします。</span><span class="sxs-lookup"><span data-stu-id="2545b-202">A free trial is a good way to try publishing and hosting while you're still experimenting with WebMatrix and ASP.NET Web Pages.</span></span>

<span data-ttu-id="2545b-203">1 つを選択します。</span><span class="sxs-lookup"><span data-stu-id="2545b-203">Pick one that you like.</span></span> <span data-ttu-id="2545b-204">このチュートリアルでは、その企業、チュートリアル、作成中に、数か月の無料のサイトをホストできるように、昇格されていたために DiscountASP.NET を選択します。</span><span class="sxs-lookup"><span data-stu-id="2545b-204">For this tutorial, we selected DiscountASP.NET, because while we were creating the tutorial, that company had a promotion that let people host a site free for a few months.</span></span>

> [!NOTE]
> <span data-ttu-id="2545b-205">このチュートリアルでは、ホスティング プロバイダーの選択肢はべきではありません、他にその会社を推薦として解釈されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-205">Our choice of a hosting provider for this tutorial shouldn't be interpreted as an endorsement of that company over any other.</span></span> <span data-ttu-id="2545b-206">図では、いずれかを選択する必要がありました DiscountASP.NET パブリッシング用の ASP.NET Web ページおよび Web Deploy プロトコルをサポートする多くの企業のいずれか。</span><span class="sxs-lookup"><span data-stu-id="2545b-206">But we had to pick one for illustration, and DiscountASP.NET is one of the many companies that supports ASP.NET Web Pages and the Web Deploy protocol for publishing.</span></span>


<span data-ttu-id="2545b-207">通常、ホスティング プロバイダーにサインアップしている会社に送信するユーザー名とパスワード、web サーバーの URL を含む電子メールをします。</span><span class="sxs-lookup"><span data-stu-id="2545b-207">Typically, after you've signed up with the hosting provider, the company sends you an email that contains a user name and password, the URL of the web server, and so on.</span></span> <span data-ttu-id="2545b-208">ホスティング プロバイダーは、Web Deploy プロトコルをサポートする場合がありますの送信の設定を発行するを含むファイルか、またはいずれかをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="2545b-208">If the hosting company supports Web Deploy protocol, they might send you a file that contains publish settings, or let you download one.</span></span> <span data-ttu-id="2545b-209">発行設定ファイルには、プロセスが簡略化します。</span><span class="sxs-lookup"><span data-stu-id="2545b-209">A publish settings file simplifies the process for you.</span></span>

<span data-ttu-id="2545b-210">サインアップが完了し、発行する準備が整いましたをクリックして、**発行**WebMatrix リボンのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-210">When you've signed up and are ready to publish, click the **Publish** button in the WebMatrix ribbon.</span></span> <span data-ttu-id="2545b-211">**発行設定** ダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-211">The **Publish Settings** dialog box is displayed.</span></span>

<span data-ttu-id="2545b-212">場合は、ホスティング プロバイダーでは、発行設定ファイルを送信をクリックして、**発行の設定をインポート**リンクし、ファイルをインポートします。</span><span class="sxs-lookup"><span data-stu-id="2545b-212">If the hosting provider sent you a publish settings file, click the **Import publish settings** link and import the file.</span></span> <span data-ttu-id="2545b-213">発行設定ファイルを持っていない場合は、ホスティング企業に電子メールを送信します。 値を使用して、フィールドに入力します。</span><span class="sxs-lookup"><span data-stu-id="2545b-213">If you don't have a publish settings file, fill in the fields by using the values that the hosting company sent you in email.</span></span> <span data-ttu-id="2545b-214">ここでは、どのような**発行設定** ダイアログ ボックスが終了するときのようになります。</span><span class="sxs-lookup"><span data-stu-id="2545b-214">Here's what the **Publish Settings** dialog box might look like when you're done:</span></span>

![発行設定が [公開設定] ダイアログ ボックスに入力](publishing/_static/image14.png)

<span data-ttu-id="2545b-216">をクリックして**への接続検証**です。</span><span class="sxs-lookup"><span data-stu-id="2545b-216">Click **Validate Connection**.</span></span> <span data-ttu-id="2545b-217">ダイアログ ボックスのレポートのかどうかは、[ok] のすべては、**が正常に接続されている**、ホスティング プロバイダーのサーバーと通信できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="2545b-217">If everything is ok, the dialog box reports **Connected successfully**, which means it can communicate with the hosting provider's server.</span></span>

![成功した場合のメッセージを発行の設定が正しい](publishing/_static/image15.png)

<span data-ttu-id="2545b-219">問題がある場合、WebMatrix は問題が何を確認する最善の状態は。</span><span class="sxs-lookup"><span data-stu-id="2545b-219">If there's a problem, WebMatrix does its best to tell you what the problem is:</span></span>

![発行設定のエラー メッセージに問題がある場合](publishing/_static/image16.png)

<span data-ttu-id="2545b-221">をクリックして**保存**設定を保存します。</span><span class="sxs-lookup"><span data-stu-id="2545b-221">Click **Save** to save your settings.</span></span> <span data-ttu-id="2545b-222">WebMatrix は、ホスティング サイトと正しく通信できることを確認するテストの実行に提供します。</span><span class="sxs-lookup"><span data-stu-id="2545b-222">WebMatrix offers to perform a test to make sure that it can communicate correctly with the hosting site:</span></span>

![メッセージの公開プロセスのテストを実行する内容](publishing/_static/image17.png)

<span data-ttu-id="2545b-224">**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-224">Click **Yes**.</span></span> <span data-ttu-id="2545b-225">WebMatrix は、ホスティング プロバイダーにいくつかのサンプル ファイルをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2545b-225">WebMatrix uploads some sample files to the hosting provider.</span></span> <span data-ttu-id="2545b-226">互換性テストを完了すると、WebMatrix は、結果をレポートします。</span><span class="sxs-lookup"><span data-stu-id="2545b-226">When the compatibility test is done, WebMatrix reports the results:</span></span>

![公開のテストの結果](publishing/_static/image18.png)

<span data-ttu-id="2545b-228">さあをクリックして移動する準備ができたら場合、**続行**を実際の発行プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="2545b-228">If you're ready to go, go ahead and click **Continue** to start the publish process for real.</span></span> <span data-ttu-id="2545b-229">WebMatrix な判別どのようなファイル、サイト内および (今すぐなし)、ホスト サーバー上に既に存在され、発行プロセスのプレビューを示します。</span><span class="sxs-lookup"><span data-stu-id="2545b-229">WebMatrix figures out what files are in your site and are already on the host server (right now, none) and gives you a preview of the publish process:</span></span>

![発行プロセスをアップロードするファイルのプレビュー](publishing/_static/image19.png)

<span data-ttu-id="2545b-231">発行するファイルの一覧と同様に作成した web ページが含まれています。 *Movies.cshtml*です。</span><span class="sxs-lookup"><span data-stu-id="2545b-231">The list of files to publish includes the web pages that you've created like *Movies.cshtml*.</span></span> <span data-ttu-id="2545b-232">一覧には、ヘルパーをインストールしたが、データベースの SQL Server Compact Edition を実行するために、ファイルのファイルも含まれています。</span><span class="sxs-lookup"><span data-stu-id="2545b-232">The list also includes files for helpers that you've installed, the files to run SQL Server Compact Edition for your database, and so on.</span></span> <span data-ttu-id="2545b-233">初期公開のプロセスの結果として、大幅に節約できます。</span><span class="sxs-lookup"><span data-stu-id="2545b-233">As a result, the initial publish process can be substantial.</span></span>

<span data-ttu-id="2545b-234">**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-234">Click **Continue**.</span></span> <span data-ttu-id="2545b-235">WebMatrix は、ホスティング プロバイダーのサーバーに、ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="2545b-235">WebMatrix copies your files to the hosting provider's server.</span></span> <span data-ttu-id="2545b-236">実行されると、結果は、ステータス バーに報告されます。</span><span class="sxs-lookup"><span data-stu-id="2545b-236">When it's done, the results are reported in the status bar:</span></span>

![発行プロセスが正常に完了すると、ステータス バー メッセージ](publishing/_static/image20.png)

<span data-ttu-id="2545b-238">ライブ サイトを表示するには、ステータス バーのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-238">To see your live site, click the link in the status bar.</span></span> <span data-ttu-id="2545b-239">追加*映画*URL にして表示されます、 *Movies.cshtml*作成したファイル。</span><span class="sxs-lookup"><span data-stu-id="2545b-239">Add *Movies* to the URL, and you'll see the *Movies.cshtml* file that you created:</span></span>

![映画 ページを示すライブ サイト](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a><span data-ttu-id="2545b-241">実際のサイトを更新します再パブリッシュ。</span><span class="sxs-lookup"><span data-stu-id="2545b-241">Updating the Live Site: Republishing</span></span>

<span data-ttu-id="2545b-242">Azure または web ポータルのホスト) は、サイトを公開した後の 2 つのコピーがある&mdash;お使いのコンピューターと、サービス プロバイダーのバージョンのバージョン。</span><span class="sxs-lookup"><span data-stu-id="2545b-242">Once you've published your site (to either Azure or a web hosting company), there are two copies of it &mdash; the version on your computer and the version on the service provider.</span></span> <span data-ttu-id="2545b-243">サイトの開発を続行したい可能性があります (ただ、次のチュートリアルのセットの一部として)。</span><span class="sxs-lookup"><span data-stu-id="2545b-243">You'll probably want to continue developing the site (if nothing else, as part of the next tutorial set).</span></span> <span data-ttu-id="2545b-244">実行すると、サービス プロバイダーにお使いのコンピューターからの変更をコピーするのには、サイトを再パブリッシュする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2545b-244">When you do, you have to republish your site in order to copy changes from your computer to the service provider.</span></span> <span data-ttu-id="2545b-245">WebMatrix で、発行プロセスでは、ファイルがサイトに変更を判断でき、それらのファイルだけを発行することができます。</span><span class="sxs-lookup"><span data-stu-id="2545b-245">The publish process in WebMatrix can determine what files have changed on your site and publish just those files.</span></span>

<span data-ttu-id="2545b-246">再パブリッシュの動作を確認するを開き、 *Movies.cshtml*サイト、いくつかの小さな変更を行い、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="2545b-246">To see how republishing works, open the *Movies.cshtml* site, make some small change, and then save the file.</span></span> <span data-ttu-id="2545b-247">たとえば、タイトルに変更`Movies - Updated`です。</span><span class="sxs-lookup"><span data-stu-id="2545b-247">For example, change the title to `Movies - Updated`.</span></span>

<span data-ttu-id="2545b-248">クリックして、**発行**リボンのボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-248">Click the **Publish** button in the ribbon.</span></span> <span data-ttu-id="2545b-249">WebMatrix は、何が変更され、発行するファイルのプレビューを表示を決定します。</span><span class="sxs-lookup"><span data-stu-id="2545b-249">WebMatrix determines what's changed and shows you a preview of the files it will publish.</span></span>

![変更を示す [公開] ダイアログ ボックスを再パブリッシュするファイルが準備完了します。](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> <span data-ttu-id="2545b-251">既定では、WebMatrix は、データベースを発行 (*.sdf*ファイル) だけで最初にサイトを発行します。</span><span class="sxs-lookup"><span data-stu-id="2545b-251">By default, WebMatrix publishes your database (*.sdf* file) only the first time you publish the site.</span></span> <span data-ttu-id="2545b-252">サイトが公開され、ユーザーが、web サイトと対話する、実際のサイトのデータベースは通常、サイトの実際のデータを持っています。</span><span class="sxs-lookup"><span data-stu-id="2545b-252">Once your site is published and people are interacting with the website, the database on the live site typically has the site's real data.</span></span> <span data-ttu-id="2545b-253">ライブ データベースを上書きしないように十分に注意する必要がある、 *.sdf*通常テスト データだけが含まれますコンピューターにインストールされているファイル。</span><span class="sxs-lookup"><span data-stu-id="2545b-253">You have to be very careful not to overwrite the live database with the *.sdf* file that's on your computer, which usually contains only test data.</span></span> <span data-ttu-id="2545b-254">理由です、警告が表示**発行は、リモート データベースを上書き**と理由のチェック ボックスを*WebPagesMovies.sdf*既定ではオフです。</span><span class="sxs-lookup"><span data-stu-id="2545b-254">That's why you see the warning **Publishing will overwrite any remote databases**, and why the check box for *WebPagesMovies.sdf* is cleared by default.</span></span>


<span data-ttu-id="2545b-255">**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2545b-255">Click **Continue**.</span></span> <span data-ttu-id="2545b-256">WebMatrix を使用して、変更されたファイルを発行、発行した最初の時間と同じように、成功メッセージを示します。</span><span class="sxs-lookup"><span data-stu-id="2545b-256">WebMatrix publishes the changed files and shows you a success message, like it did the first time you published.</span></span>

<span data-ttu-id="2545b-257">実際のサイト (リンクをクリック、成功メッセージがまだ表示されている場合) に移動し、変更が発行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2545b-257">Go to the live site (you can click the link in the success message if it's still showing) and verify that your change has been published.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="2545b-258">**リモート ファイルの編集**</span><span class="sxs-lookup"><span data-stu-id="2545b-258">**Editing Files Remotely**</span></span>
> 
> <span data-ttu-id="2545b-259">サイトを変更して、再パブリッシュする代わりに、WebMatrix で直接、リモート ファイルを編集できます。</span><span class="sxs-lookup"><span data-stu-id="2545b-259">As an alternative to changing your site and then republishing, you can edit remote files directly in WebMatrix.</span></span> <span data-ttu-id="2545b-260">このシナリオで、サービス プロバイダーが上にあるファイルを開くし、WebMatrix は、編集するためのコピーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="2545b-260">In this scenario, you open a file that's on the service provider, and WebMatrix downloads a copy of it for you to edit.</span></span> <span data-ttu-id="2545b-261">ファイルを保存するたびに、WebMatrix は、変更をサイトに送信します。</span><span class="sxs-lookup"><span data-stu-id="2545b-261">Every time you save the file, WebMatrix sends the changes to the site.</span></span>
> 
> <span data-ttu-id="2545b-262">簡単に、実際のサイトを変更するのには、リモート編集します。</span><span class="sxs-lookup"><span data-stu-id="2545b-262">Remote editing is an easy way to make changes to your live site.</span></span> <span data-ttu-id="2545b-263">ただし、変更を加えて、この方法は、ローカル サイト内のファイルと同期されていません。</span><span class="sxs-lookup"><span data-stu-id="2545b-263">However, the changes you make this way aren't synchronized with the files in your local site.</span></span> <span data-ttu-id="2545b-264">リモート サイトと、ローカル ファイルを同期するためには、リモート ファイルをダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="2545b-264">To synchronize the local files with the remote site, you can download the remote files.</span></span> <span data-ttu-id="2545b-265">このプロセスは同様に機能の公開、除く逆の順序で。</span><span class="sxs-lookup"><span data-stu-id="2545b-265">This process works much like publishing, except in reverse.</span></span>
> 
> <span data-ttu-id="2545b-266">説明はしませんより、リモート編集し、リモート ダウンロード機能についての WebMatrix は、ここです。</span><span class="sxs-lookup"><span data-stu-id="2545b-266">We won't describe more about the remote-editing and remote-download facilities of WebMatrix here.</span></span> <span data-ttu-id="2545b-267">複数のユーザーが同じサイトの異なるコンピューター上で動作する必要があると非常に便利です。</span><span class="sxs-lookup"><span data-stu-id="2545b-267">They're quite useful if multiple people have to work on the same site on different computers.</span></span> <span data-ttu-id="2545b-268">詳細については、次を参照してください。[発行し、WebMatrix 2 Beta でのリモート サイトを編集する](https://go.microsoft.com/fwlink/?LinkId=251591)です。</span><span class="sxs-lookup"><span data-stu-id="2545b-268">For more information, see [Publish and Edit a Remote Site with WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2545b-269">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="2545b-269">Additional Resources</span></span>

- <span data-ttu-id="2545b-270">[ASP.NET WebMatrix ASP.NET Web Pages のフォーラム](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)の投稿に、最適な場所の質問し、回答を得ます。</span><span class="sxs-lookup"><span data-stu-id="2545b-270">[ASP.NET WebMatrix ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), a great place to post questions and get answers.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2545b-271">前へ</span><span class="sxs-lookup"><span data-stu-id="2545b-271">Previous</span></span>](layouts.md)
