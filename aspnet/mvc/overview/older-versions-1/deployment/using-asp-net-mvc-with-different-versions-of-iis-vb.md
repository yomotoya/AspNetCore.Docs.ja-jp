---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: "ASP.NET MVC を使用して、さまざまなバージョンの IIS (VB) |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルでは、インターネット インフォメーション サービスの異なるバージョンの ASP.NET MVC と URL がルーティングを使用する方法を学習します。 さまざまな方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c9c3bf004b13677728c7c6bf2f5adf6a264dc49
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a><span data-ttu-id="7f03b-104">別のバージョンの IIS (VB) での ASP.NET MVC の使用</span><span class="sxs-lookup"><span data-stu-id="7f03b-104">Using ASP.NET MVC with Different Versions of IIS (VB)</span></span>
====================
<span data-ttu-id="7f03b-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7f03b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7f03b-106">このチュートリアルでは、インターネット インフォメーション サービスの異なるバージョンの ASP.NET MVC と URL がルーティングを使用する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-106">In this tutorial, you learn how to use ASP.NET MVC, and URL Routing, with different versions of Internet Information Services.</span></span> <span data-ttu-id="7f03b-107">IIS 7.0 (クラシック モード)、IIS 6.0、および以前のバージョンの IIS で ASP.NET MVC を使用するためのさまざまな方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-107">You learn different strategies for using ASP.NET MVC with IIS 7.0 (classic mode), IIS 6.0, and earlier versions of IIS.</span></span>


<span data-ttu-id="7f03b-108">ASP.NET MVC フレームワークは、コント ローラーのアクションをブラウザー要求をルーティングする ASP.NET ルーティングに依存します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-108">The ASP.NET MVC framework depends on ASP.NET Routing to route browser requests to controller actions.</span></span> <span data-ttu-id="7f03b-109">ASP.NET ルーティングの利点を活かすため、web サーバーで追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-109">In order to take advantage of ASP.NET Routing, you might have to perform additional configuration steps on your web server.</span></span> <span data-ttu-id="7f03b-110">すべては、インターネット インフォメーション サービス (IIS) と、アプリケーションのモードの処理要求のバージョンによって異なります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-110">It all depends on the version of Internet Information Services (IIS) and the request processing mode for your application.</span></span>

<span data-ttu-id="7f03b-111">別のバージョンの IIS の概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-111">Here's a summary of the different versions of IIS:</span></span>

- <span data-ttu-id="7f03b-112">IIS 7.0 (統合モード) - ASP.NET のルーティングを使用するために必要な特別な構成がありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-112">IIS 7.0 (integrated mode) - No special configuration necessary to use ASP.NET Routing.</span></span>
- <span data-ttu-id="7f03b-113">IIS 7.0 (クラシック モード) - ASP.NET のルーティングを使用する特別な構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-113">IIS 7.0 (classic mode) - You need to perform special configuration to use ASP.NET Routing.</span></span>
- <span data-ttu-id="7f03b-114">IIS 6.0 または以下の ASP.NET のルーティングを使用する特別な構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-114">IIS 6.0 or below - You need to perform special configuration to use ASP.NET Routing.</span></span>

<span data-ttu-id="7f03b-115">最新バージョンの IIS では、(Win7) バージョン 7.5 がします。</span><span class="sxs-lookup"><span data-stu-id="7f03b-115">The latest version of IIS is version 7.5 (on Win7).</span></span> <span data-ttu-id="7f03b-116">IIS の IIS 7 は、Windows Server 2008 および VISTA/SP1 に含まれる以降です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-116">IIS 7 of IIS is included with Windows Server 2008 AND VISTA/SP1 and higher.</span></span> <span data-ttu-id="7f03b-117">インストールすることもできる IIS 7.0、Vista オペレーティング システムの Home Basic 以外のすべてのバージョンで (を参照してください[https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。</span><span class="sxs-lookup"><span data-stu-id="7f03b-117">You also can install IIS 7.0 on any version of the Vista operating system except Home Basic (see [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).</span></span>

<span data-ttu-id="7f03b-118">IIS 7.0 では、要求を処理する 2 つのモードをサポートします。</span><span class="sxs-lookup"><span data-stu-id="7f03b-118">IIS 7.0 supports two modes for processing requests.</span></span> <span data-ttu-id="7f03b-119">統合モードまたはクラシック モードを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-119">You can use integrated mode or classic mode.</span></span> <span data-ttu-id="7f03b-120">統合モードで IIS 7.0 を使用する場合は、特別な構成手順を実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-120">You don't need to perform any special configuration steps when using IIS 7.0 in integrated mode.</span></span> <span data-ttu-id="7f03b-121">ただしはクラシック モードで IIS 7.0 を使用する場合は、追加の構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-121">However, you do need to perform additional configuration when using IIS 7.0 in classic mode.</span></span>

<span data-ttu-id="7f03b-122">Microsoft Windows Server 2003 には、IIS 6.0 が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7f03b-122">Microsoft Windows Server 2003 includes IIS 6.0.</span></span> <span data-ttu-id="7f03b-123">Windows Server 2003 オペレーティング システムを使用する場合は、IIS 6.0 を IIS 7.0 にアップグレードできません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-123">You cannot upgrade IIS 6.0 to IIS 7.0 when using the Windows Server 2003 operating system.</span></span> <span data-ttu-id="7f03b-124">IIS 6.0 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-124">You must perform additional configuration steps when using IIS 6.0.</span></span>

<span data-ttu-id="7f03b-125">Microsoft Windows XP Professional には、IIS 5.1 が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7f03b-125">Microsoft Windows XP Professional includes IIS 5.1.</span></span> <span data-ttu-id="7f03b-126">IIS 5.1 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-126">You must perform additional configuration steps when using IIS 5.1.</span></span>

<span data-ttu-id="7f03b-127">最後に、Microsoft Windows 2000 と Microsoft Windows 2000 Professional には、IIS 5.0 が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-127">Finally, Microsoft Windows 2000 and Microsoft Windows 2000 Professional includes IIS 5.0.</span></span> <span data-ttu-id="7f03b-128">IIS 5.0 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-128">You must perform additional configuration steps when using IIS 5.0.</span></span>

## <a name="integrated-versus-classic-mode"></a><span data-ttu-id="7f03b-129">クラシック モードとの統合</span><span class="sxs-lookup"><span data-stu-id="7f03b-129">Integrated versus Classic Mode</span></span>

<span data-ttu-id="7f03b-130">IIS 7.0 は、2 つの異なる要求の処理モードを使用して要求を処理できます。 統合とクラシックです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-130">IIS 7.0 can process requests using two different request processing modes: integrated and classic.</span></span> <span data-ttu-id="7f03b-131">統合モードでは、パフォーマンスが向上しより多くの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-131">Integrated mode provides better performance and more features.</span></span> <span data-ttu-id="7f03b-132">クラシック モードがの旧バージョンと含まれている IIS の以前のバージョンとの互換性。</span><span class="sxs-lookup"><span data-stu-id="7f03b-132">Classic mode is included for backwards compatibility with earlier versions of IIS.</span></span>

<span data-ttu-id="7f03b-133">要求の処理モードは、アプリケーション プールによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-133">The request processing mode is determined by the application pool.</span></span> <span data-ttu-id="7f03b-134">どちらの処理モードは、アプリケーションに関連付けられているアプリケーション プールを決定することにより、特定の web アプリケーションで使用されているを指定できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-134">You can determine which processing mode is being used by a particular web application by determining the application pool associated with the application.</span></span> <span data-ttu-id="7f03b-135">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-135">Follow these steps:</span></span>

1. <span data-ttu-id="7f03b-136">インターネット インフォメーション サービス マネージャーを起動します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-136">Launch the Internet Information Services Manager</span></span>
2. <span data-ttu-id="7f03b-137">[接続] ウィンドウで、アプリケーションを選択します</span><span class="sxs-lookup"><span data-stu-id="7f03b-137">In the Connections window, select an application</span></span>
3. <span data-ttu-id="7f03b-138">アクション ウィンドウ、**基本設定**アプリケーションの編集 ダイアログを開くためのリンク (図 1 を参照してください)</span><span class="sxs-lookup"><span data-stu-id="7f03b-138">In the Actions window, click the **Basic Settings** link to open the Edit Application dialog box (see Figure 1)</span></span>
4. <span data-ttu-id="7f03b-139">選択したアプリケーション プールを書き留めます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-139">Take note of the Application pool selected.</span></span>

<span data-ttu-id="7f03b-140">2 つのアプリケーション プールをサポートするために既定では、IIS が構成されている: **DefaultAppPool**と**Classic .NET AppPool**です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-140">By default, IIS is configured to support two application pools: **DefaultAppPool** and **Classic .NET AppPool**.</span></span> <span data-ttu-id="7f03b-141">DefaultAppPool が選択されている場合、アプリケーションは、統合された要求の処理モードで実行されています。</span><span class="sxs-lookup"><span data-stu-id="7f03b-141">If DefaultAppPool is selected, then your application is running in integrated request processing mode.</span></span> <span data-ttu-id="7f03b-142">Classic .NET AppPool が選択されている場合は、従来の要求の処理モードで、アプリケーションが実行されています。</span><span class="sxs-lookup"><span data-stu-id="7f03b-142">If Classic .NET AppPool is selected, your application is running in classic request processing mode.</span></span>


<span data-ttu-id="7f03b-143">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7f03b-143">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)</span></span>

<span data-ttu-id="7f03b-144">**図 1**: 要求の処理モードの検出 ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7f03b-144">**Figure 1**: Detecting the request processing mode([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))</span></span>


<span data-ttu-id="7f03b-145">アプリケーションの編集 ダイアログ ボックス内で要求の処理モードを変更できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-145">Notice that you can modify the request processing mode within the Edit Application dialog box.</span></span> <span data-ttu-id="7f03b-146">[選択] ボタンをクリックし、アプリケーションに関連付けられているアプリケーション プールを変更します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-146">Click the Select button and change the application pool associated with the application.</span></span> <span data-ttu-id="7f03b-147">ASP.NET アプリケーションをクラシックから統合モードに変更するときに互換性の問題が認識がある注意してください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-147">Realize that there are compatibility issues when changing an ASP.NET application from classic to integrated mode.</span></span> <span data-ttu-id="7f03b-148">詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-148">For more information, see the following articles:</span></span>

- <span data-ttu-id="7f03b-149">Windows Vista および Windows Server 2008--上の iis 7.0 ASP.NET 1.1 をアップグレードする[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)</span><span class="sxs-lookup"><span data-stu-id="7f03b-149">Upgrading ASP.NET 1.1 to IIS 7.0 on Windows Vista and Windows Server 2008 -- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)</span></span>

- <span data-ttu-id="7f03b-150">IIS 7.0 の ASP.NET 統合[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)</span><span class="sxs-lookup"><span data-stu-id="7f03b-150">ASP.NET Integration With IIS 7.0 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)</span></span>


<span data-ttu-id="7f03b-151">ASP.NET アプリケーションが、DefaultAppPool を使用している場合しない、ASP.NET ルーティングと ASP.NET MVC) 作業を取得する追加の手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-151">If an ASP.NET application is using the DefaultAppPool, then you don't need to perform any additional steps to get ASP.NET Routing (and therefore ASP.NET MVC) to work.</span></span> <span data-ttu-id="7f03b-152">ただし、ASP.NET アプリケーションを構成して、Classic .NET AppPool を使用し、読み取りを保持する場合により多くの作業を行うにを用意します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-152">However, if the ASP.NET application is configured to use the Classic .NET AppPool then keep reading, you have more work to do.</span></span>

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a><span data-ttu-id="7f03b-153">古いバージョンの IIS で ASP.NET MVC の使用</span><span class="sxs-lookup"><span data-stu-id="7f03b-153">Using ASP.NET MVC with Older Versions of IIS</span></span>

<span data-ttu-id="7f03b-154">IIS 7.0 より古いバージョンの IIS で ASP.NET MVC を使用する必要。 または、クラシック モードで IIS 7.0 を使用する必要があります、しする 2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-154">If you need to use ASP.NET MVC with an older version of IIS than IIS 7.0, or you need to use IIS 7.0 in classic mode, then you have two options.</span></span> <span data-ttu-id="7f03b-155">最初に、ファイル拡張子を使用するルート テーブルを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-155">First, you can modify the route table to use file extensions.</span></span> <span data-ttu-id="7f03b-156">たとえば、/Store/Details ような URL を要求するではなく/Store.aspx/Details ような URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-156">For example, instead of requesting a URL like /Store/Details, you would request a URL like /Store.aspx/Details.</span></span>

<span data-ttu-id="7f03b-157">2 番目のオプションと呼ばれるものを作成する、*ワイルドカード スクリプト マップ*です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-157">The second option is to create something called a *wildcard script map*.</span></span> <span data-ttu-id="7f03b-158">ワイルドカード スクリプト マップでは、ASP.NET フレームワークのすべての要求にマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-158">A wildcard script map enables you to map every request into the ASP.NET framework.</span></span>

<span data-ttu-id="7f03b-159">場合は、web サーバー (たとえば、ASP.NET MVC アプリケーションは、インターネット サービス プロバイダーによってホストされている) にアクセスできませんが、最初のオプションを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-159">If you don't have access to your web server (for example, your ASP.NET MVC application is being hosted by an Internet Service Provider) then you'll need to use the first option.</span></span> <span data-ttu-id="7f03b-160">Url の外観を変更したくない、web サーバーにアクセス権がある場合は、2 番目のオプションを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-160">If you don't want to modify the appearance of your URLs, and you have access to your web server, then you can use the second option.</span></span>

<span data-ttu-id="7f03b-161">次のセクションで詳しくは、各オプションをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-161">We explore each option in detail in the following sections.</span></span>

## <a name="adding-extensions-to-the-route-table"></a><span data-ttu-id="7f03b-162">ルート テーブルへの拡張機能の追加</span><span class="sxs-lookup"><span data-stu-id="7f03b-162">Adding Extensions to the Route Table</span></span>

<span data-ttu-id="7f03b-163">古いバージョンの IIS を使用する ASP.NET のルーティングを取得する最も簡単な方法は、Global.asax ファイルで、ルート テーブルを変更するのにです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-163">The easiest way to get ASP.NET Routing to work with older versions of IIS is to modify your route table in the Global.asax file.</span></span> <span data-ttu-id="7f03b-164">既定値をリスト 1 の未変更の Global.asax ファイルが既定のルートをという名前の 1 つのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-164">The default and unmodified Global.asax file in Listing 1 configures one route named the Default route.</span></span>

<span data-ttu-id="7f03b-165">**1 - Global.asax (未変更の状態) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7f03b-165">**Listing 1 - Global.asax (unmodified)**</span></span>

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

<span data-ttu-id="7f03b-166">リスト 1 で構成されている既定のルートでは、次のようにルート Url できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-166">The Default route configured in Listing 1 enables you to route URLs that look like this:</span></span>

<span data-ttu-id="7f03b-167">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="7f03b-167">/Home/Index</span></span>

<span data-ttu-id="7f03b-168">/Product/Details/3</span><span class="sxs-lookup"><span data-stu-id="7f03b-168">/Product/Details/3</span></span>

<span data-ttu-id="7f03b-169">/製品</span><span class="sxs-lookup"><span data-stu-id="7f03b-169">/Product</span></span>

<span data-ttu-id="7f03b-170">残念ながら、古いバージョンの IIS、ASP.NET フレームワークにこれらの要求が転送されません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-170">Unfortunately, older versions of IIS won't pass these requests to the ASP.NET framework.</span></span> <span data-ttu-id="7f03b-171">したがって、これらの要求、コント ローラーにルーティングされません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-171">Therefore, these requests won't get routed to a controller.</span></span> <span data-ttu-id="7f03b-172">たとえば、URL/Home/インデックスに対してブラウザー要求を行う場合は、図 2 にエラー ページを取得します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-172">For example, if you make a browser request for the URL /Home/Index then you'll get the error page in Figure 2.</span></span>


<span data-ttu-id="7f03b-173">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="7f03b-173">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)</span></span>

<span data-ttu-id="7f03b-174">**図 2**: 404 Not Found エラーを受け取る ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="7f03b-174">**Figure 2**: Receiving a 404 Not Found error([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))</span></span>


<span data-ttu-id="7f03b-175">古いバージョンの IIS は、ASP.NET フレームワークにのみ特定の要求をマップします。</span><span class="sxs-lookup"><span data-stu-id="7f03b-175">Older versions of IIS only map certain requests to the ASP.NET framework.</span></span> <span data-ttu-id="7f03b-176">要求は、適切なファイル拡張子を持つ URL になければなりません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-176">The request must be for a URL with the right file extension.</span></span> <span data-ttu-id="7f03b-177">たとえば、/SomePage.aspx の要求は、ASP.NET フレームワークにマップを取得します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-177">For example, a request for /SomePage.aspx gets mapped to the ASP.NET framework.</span></span> <span data-ttu-id="7f03b-178">ただし、/SomePage.htm の要求されていません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-178">However, a request for /SomePage.htm does not.</span></span>

<span data-ttu-id="7f03b-179">そのため、ASP.NET が動作するルーティングを取得する、ASP.NET フレームワークにマップされているファイル拡張子が含まれるように既定のルートを変更する必要がありますおです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-179">Therefore, to get ASP.NET Routing to work, we must modify the Default route so that it includes a file extension that is mapped to the ASP.NET framework.</span></span>

<span data-ttu-id="7f03b-180">これは、という名前のスクリプトを使用して`registermvc.wsf`です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-180">This is done using a script named `registermvc.wsf`.</span></span> <span data-ttu-id="7f03b-181">ASP.NET MVC 1 リリースには含まれて`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`、ASP.NET 2 の時点でこのスクリプトに移動されました ASP.NET フューチャで使用できますが、 [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-181">It was included with the ASP.NET MVC 1 release in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, but as of ASP.NET 2 this script has been moved to the ASP.NET Futures, available at [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).</span></span>

<span data-ttu-id="7f03b-182">このスクリプトを実行すると、新しい .mvc 拡張機能を IIS に登録します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-182">Executing this script registers a new .mvc extension with IIS.</span></span> <span data-ttu-id="7f03b-183">.Mvc 拡張機能を登録した後は、ルートは、.mvc 拡張機能を使用できるように、Global.asax ファイルで、ルートを変更できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-183">After you register the .mvc extension, you can modify your routes in the Global.asax file so that the routes use the .mvc extension.</span></span>

<span data-ttu-id="7f03b-184">リスト 2 での変更の Global.asax ファイルは、古いバージョンの IIS で使用できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-184">The modified Global.asax file in Listing 2 works with older versions of IIS.</span></span>

<span data-ttu-id="7f03b-185">**2 - Global.asax (拡張子を持つ変更済み) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7f03b-185">**Listing 2 - Global.asax (modified with extensions)**</span></span>

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


<span data-ttu-id="7f03b-186">重要: Global.asax ファイルを変更した後に ASP.NET MVC アプリケーションをビルドしてください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-186">Important: remember to build your ASP.NET MVC Application again after changing the Global.asax file.</span></span>


<span data-ttu-id="7f03b-187">2 つの重要な変更を一覧表示する 2 の Global.asax ファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-187">There are two important changes to the Global.asax file in Listing 2.</span></span> <span data-ttu-id="7f03b-188">Global.asax で定義されている 2 つのルートはようになりました。</span><span class="sxs-lookup"><span data-stu-id="7f03b-188">There are now two routes defined in the Global.asax.</span></span> <span data-ttu-id="7f03b-189">既定のルート、最初のルートの URL パターンは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-189">The URL pattern for the Default route, the first route, now looks like:</span></span>

<span data-ttu-id="7f03b-190">{controller}.mvc/{action}/{id}</span><span class="sxs-lookup"><span data-stu-id="7f03b-190">{controller}.mvc/{action}/{id}</span></span>

<span data-ttu-id="7f03b-191">.Mvc 拡張機能の追加は、ASP.NET ルーティング モジュールをインターセプトできるファイルの種類を変更します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-191">The addition of the .mvc extension changes the type of files that the ASP.NET Routing module intercepts.</span></span> <span data-ttu-id="7f03b-192">この変更によりは、ASP.NET MVC アプリケーションは、次のように要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="7f03b-192">With this change, the ASP.NET MVC application now routes requests like the following:</span></span>

<span data-ttu-id="7f03b-193">/Home.mvc/Index/</span><span class="sxs-lookup"><span data-stu-id="7f03b-193">/Home.mvc/Index/</span></span>

<span data-ttu-id="7f03b-194">/Product.mvc/Details/3</span><span class="sxs-lookup"><span data-stu-id="7f03b-194">/Product.mvc/Details/3</span></span>

<span data-ttu-id="7f03b-195">/Product.mvc/</span><span class="sxs-lookup"><span data-stu-id="7f03b-195">/Product.mvc/</span></span>

<span data-ttu-id="7f03b-196">2 番目のルートでは、ルートのルートは、新しいです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-196">The second route, the Root route, is new.</span></span> <span data-ttu-id="7f03b-197">ルートのルートの次の URL パターンは、空の文字列です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-197">This URL pattern for the Root route is an empty string.</span></span> <span data-ttu-id="7f03b-198">このルートは、アプリケーションのルートに対する要求に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-198">This route is necessary for matching requests made against the root of your application.</span></span> <span data-ttu-id="7f03b-199">たとえば、ルートのルートでは、次のような要求は一致します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-199">For example, the Root route will match a request that looks like this:</span></span>

[<span data-ttu-id="7f03b-200">http://www.YourApplication.com/</span><span class="sxs-lookup"><span data-stu-id="7f03b-200">http://www.YourApplication.com/</span></span>](http://www.YourApplication.com/)

<span data-ttu-id="7f03b-201">ルート テーブルに次の変更を行った後は、すべてのアプリケーションのリンクが、これらの新しい URL パターンと互換性があるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-201">After making these modifications to your route table, you'll need to make sure that all of the links in your application are compatible with these new URL patterns.</span></span> <span data-ttu-id="7f03b-202">つまり、すべてのリンクには、.mvc 拡張子を含めることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-202">In other words, make sure that all of your links include the .mvc extension.</span></span> <span data-ttu-id="7f03b-203">Html.ActionLink() ヘルパー メソッドを使用して、リンクを生成する場合は、何も変更する必要する必要がありますありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-203">If you use the Html.ActionLink() helper method to generate your links, then you should not need to make any changes.</span></span>


<span data-ttu-id="7f03b-204">Registermvc.wcf スクリプトを使用する代わりに、ASP.NET framework を手動でマップされている IIS に新しい拡張機能を追加できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-204">Instead of using the registermvc.wcf script, you can add a new extension to IIS that is mapped to the ASP.NET framework by hand.</span></span> <span data-ttu-id="7f03b-205">新しい拡張機能を自分で追加するとき、チェック ボックスがラベル確認**ファイルが存在することを確認**はチェックされません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-205">When adding a new extension yourself, make sure that the checkbox labeled **Verify that file exists** is not checked.</span></span>


## <a name="hosted-server"></a><span data-ttu-id="7f03b-206">ホストされているサーバー</span><span class="sxs-lookup"><span data-stu-id="7f03b-206">Hosted Server</span></span>

<span data-ttu-id="7f03b-207">Web サーバーへのアクセスを常がありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-207">You don't always have access to your web server.</span></span> <span data-ttu-id="7f03b-208">たとえば、インターネット ホスト プロバイダーを使用して、ASP.NET MVC アプリケーションをホストしている場合、必ずしもはありません IIS へのアクセス。</span><span class="sxs-lookup"><span data-stu-id="7f03b-208">For example, if you are hosting your ASP.NET MVC application using an Internet Hosting Provider, then you won't necessarily have access to IIS.</span></span>

<span data-ttu-id="7f03b-209">その場合は、ASP.NET フレームワークにマップされている既存のファイル拡張子のいずれかを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-209">In that case, you should use one of the existing file extensions that are mapped to the ASP.NET framework.</span></span> <span data-ttu-id="7f03b-210">ASP.NET にマップされているファイル拡張機能の例には、.aspx には、.axd、.ashx 拡張機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-210">Examples of file extensions mapped to ASP.NET include the .aspx, .axd, and .ashx extensions.</span></span>

<span data-ttu-id="7f03b-211">たとえば、リスト 3 の変更の Global.asax ファイルでは、.mvc 拡張機能ではなく、拡張子が .aspx を使用します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-211">For example, the modified Global.asax file in Listing 3 uses the .aspx extension instead of the .mvc extension.</span></span>

<span data-ttu-id="7f03b-212">**3 - Global.asax (.aspx 拡張子を持つ変更済み) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7f03b-212">**Listing 3 - Global.asax (modified with .aspx extensions)**</span></span>

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

<span data-ttu-id="7f03b-213">Global.asax ファイルを一覧表示する 3 では、.mvc 拡張機能ではなく、.aspx 拡張機能を使用している点を除いて前の Global.asax ファイルと同じではまったくです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-213">The Global.asax file in Listing 3 is exactly the same as the previous Global.asax file except for the fact that it uses the .aspx extension instead of the .mvc extension.</span></span> <span data-ttu-id="7f03b-214">.Aspx 拡張子を使用して、リモート web サーバーで、セットアップを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-214">You don't have to perform any setup on your remote web server to use the .aspx extension.</span></span>

## <a name="creating-a-wildcard-script-map"></a><span data-ttu-id="7f03b-215">ワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-215">Creating a Wildcard Script Map</span></span>

<span data-ttu-id="7f03b-216">ASP.NET MVC アプリケーションの Url を変更しないし、web サーバーにアクセス権があるがある場合、追加のオプションです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-216">If you don't want to modify the URLs for your ASP.NET MVC application, and you have access to your web server, then you have an additional option.</span></span> <span data-ttu-id="7f03b-217">ASP.NET フレームワークに web サーバーにすべての要求をマップするワイルドカード スクリプト マップを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-217">You can create a wildcard script map that maps all requests to the web server to the ASP.NET framework.</span></span> <span data-ttu-id="7f03b-218">こうすれば、クラシック モードでの IIS 7.0 または IIS 6.0 で既定の ASP.NET MVC ルート テーブルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-218">That way, you can use the default ASP.NET MVC route table with IIS 7.0 (in classic mode) or IIS 6.0.</span></span>

<span data-ttu-id="7f03b-219">このオプションは、IIS web サーバーに対して行われたすべての要求をインターセプトすることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="7f03b-219">Be aware that this option causes IIS to intercept every request made against the web server.</span></span> <span data-ttu-id="7f03b-220">これには、イメージ、従来の ASP ページ、および HTML ページの要求が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-220">This includes requests for images, classic ASP pages, and HTML pages.</span></span> <span data-ttu-id="7f03b-221">そのため、ワイルドカードを有効にする asp.net スクリプト マップがパフォーマンスに影響します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-221">Therefore, enabling a wildcard script map to ASP.NET does have performance implications.</span></span>

<span data-ttu-id="7f03b-222">IIS 7.0、ワイルドカード スクリプト マップを有効にする方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-222">Here's how you enable a wildcard script map for IIS 7.0:</span></span>

1. <span data-ttu-id="7f03b-223">[接続] ウィンドウで、アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-223">Select your application in the Connections window</span></span>
2. <span data-ttu-id="7f03b-224">確認して、**機能**ビューを選択します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-224">Make sure that the **Features** view is selected</span></span>
3. <span data-ttu-id="7f03b-225">ダブルクリックして、**ハンドラー マッピング**ボタン</span><span class="sxs-lookup"><span data-stu-id="7f03b-225">Double-click the **Handler Mappings** button</span></span>
4. <span data-ttu-id="7f03b-226">クリックして、**ワイルドカード スクリプト マップの追加**リンク (図 3 を参照してください)</span><span class="sxs-lookup"><span data-stu-id="7f03b-226">Click the **Add Wildcard Script Map** link (see Figure 3)</span></span>
5. <span data-ttu-id="7f03b-227">Aspnet へのパスを入力\_isapi.dll ファイル (PageHandlerFactory スクリプト マップからこのパスをコピーできます)</span><span class="sxs-lookup"><span data-stu-id="7f03b-227">Enter the path to the aspnet\_isapi.dll file (You can copy this path from the PageHandlerFactory script map)</span></span>
6. <span data-ttu-id="7f03b-228">MVC の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-228">Enter the name MVC</span></span>
7. <span data-ttu-id="7f03b-229">クリックして、 **OK**ボタン</span><span class="sxs-lookup"><span data-stu-id="7f03b-229">Click the **OK** button</span></span>


<span data-ttu-id="7f03b-230">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="7f03b-230">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)</span></span>

<span data-ttu-id="7f03b-231">**図 3**: IIS 7.0 でワイルドカード スクリプト マップを作成する ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7f03b-231">**Figure 3**: Creating a wildcard script map with IIS 7.0([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))</span></span>


<span data-ttu-id="7f03b-232">IIS 6.0 ではワイルドカード スクリプト マップを作成する手順に従います。</span><span class="sxs-lookup"><span data-stu-id="7f03b-232">Follow these steps to create a wildcard script map with IIS 6.0:</span></span>

1. <span data-ttu-id="7f03b-233">Web サイトを右クリックし、[プロパティ]</span><span class="sxs-lookup"><span data-stu-id="7f03b-233">Right-click a website and select Properties</span></span>
2. <span data-ttu-id="7f03b-234">選択、**ホーム ディレクトリ** タブ</span><span class="sxs-lookup"><span data-stu-id="7f03b-234">Select the **Home Directory** tab</span></span>
3. <span data-ttu-id="7f03b-235">クリックして、**構成**ボタン</span><span class="sxs-lookup"><span data-stu-id="7f03b-235">Click the **Configuration** button</span></span>
4. <span data-ttu-id="7f03b-236">選択、**マッピング** タブ</span><span class="sxs-lookup"><span data-stu-id="7f03b-236">Select the **Mappings** tab</span></span>
5. <span data-ttu-id="7f03b-237">クリックして、**挿入**ボタン (図 4 を参照)</span><span class="sxs-lookup"><span data-stu-id="7f03b-237">Click the **Insert** button (see Figure 4)</span></span>
6. <span data-ttu-id="7f03b-238">Aspnet へのパスを貼り付け\_(.aspx ファイルのスクリプト マップからこのパスをコピーできます)、実行可能ファイルのフィールドに isapi.dll</span><span class="sxs-lookup"><span data-stu-id="7f03b-238">Paste the path to the aspnet\_isapi.dll into the Executable field (you can copy this path from the script map for .aspx files)</span></span>
7. <span data-ttu-id="7f03b-239">チェック ボックスをオフに**ファイルの存在を確認してください。**</span><span class="sxs-lookup"><span data-stu-id="7f03b-239">Uncheck the checkbox labeled **Verify that file exists**</span></span>
8. <span data-ttu-id="7f03b-240">クリックして、 **OK**ボタン</span><span class="sxs-lookup"><span data-stu-id="7f03b-240">Click the **OK** button</span></span>


<span data-ttu-id="7f03b-241">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="7f03b-241">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)</span></span>

<span data-ttu-id="7f03b-242">**図 4**: IIS 6.0 ではワイルドカード スクリプト マップを作成する ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="7f03b-242">**Figure 4**: Creating a wildcard script map with IIS 6.0([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))</span></span>


<span data-ttu-id="7f03b-243">ワイルドカード スクリプト マップを有効にした後は、ルートのルートが含まれるように、Global.asax ファイルでルート テーブルを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-243">After you enable wildcard script maps, you need to modify the route table in the Global.asax file so that it includes a Root route.</span></span> <span data-ttu-id="7f03b-244">それ以外の場合、アプリケーションのルート ページに対する要求を行うと図 5 に、エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-244">Otherwise, you'll get the error page in Figure 5 when you make a request for the root page of your application.</span></span> <span data-ttu-id="7f03b-245">リスト 4 変更後の Global.asax ファイルを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-245">You can use the modified Global.asax file in Listing 4.</span></span>


<span data-ttu-id="7f03b-246">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="7f03b-246">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)</span></span>

<span data-ttu-id="7f03b-247">**図 5**: 見つからないルート ルート エラー ([フルサイズのイメージを表示するをクリックして](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="7f03b-247">**Figure 5**: Missing Root route error([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))</span></span>


<span data-ttu-id="7f03b-248">**4 - Global.asax (ルートのルートに変更) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="7f03b-248">**Listing 4 - Global.asax (modified with Root route)**</span></span>

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

<span data-ttu-id="7f03b-249">IIS 7.0 または IIS 6.0 のいずれかのワイルドカード スクリプト マップを有効にした後は、次のように既定のルート テーブルを使用する要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="7f03b-249">After you enable a wildcard script map for either IIS 7.0 or IIS 6.0, you can make requests that work with the default route table that look like this:</span></span>

/

<span data-ttu-id="7f03b-250">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="7f03b-250">/Home/Index</span></span>

<span data-ttu-id="7f03b-251">/Product/Details/3</span><span class="sxs-lookup"><span data-stu-id="7f03b-251">/Product/Details/3</span></span>

<span data-ttu-id="7f03b-252">/製品</span><span class="sxs-lookup"><span data-stu-id="7f03b-252">/Product</span></span>

## <a name="summary"></a><span data-ttu-id="7f03b-253">まとめ</span><span class="sxs-lookup"><span data-stu-id="7f03b-253">Summary</span></span>

<span data-ttu-id="7f03b-254">このチュートリアルの目的は、IIS (またはクラシック モードの IIS 7.0) の古いバージョンを使用する場合、ASP.NET MVC を使用する方法について説明することでした。</span><span class="sxs-lookup"><span data-stu-id="7f03b-254">The goal of this tutorial was to explain how you can use ASP.NET MVC when using an older version of IIS (or IIS 7.0 in classic mode).</span></span> <span data-ttu-id="7f03b-255">古いバージョンの IIS を使用する ASP.NET のルーティングを取得する 2 つの方法を説明した: 既定のルート テーブルを変更またはワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-255">We discussed two methods of getting ASP.NET Routing to work with older versions of IIS: Modify the default route table or create a wildcard script map.</span></span>

<span data-ttu-id="7f03b-256">最初のオプションでは、ASP.NET MVC アプリケーションで使用される Url を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f03b-256">The first option requires you to modify the URLs used in your ASP.NET MVC application.</span></span> <span data-ttu-id="7f03b-257">この最初のオプションの非常に重要な利点の 1 つは、ルート テーブルを変更するのには、web サーバーへのアクセスが必要はないことです。</span><span class="sxs-lookup"><span data-stu-id="7f03b-257">One very significant advantage of this first option is that you do not need access to a web server in order to modify the route table.</span></span> <span data-ttu-id="7f03b-258">つまり、インターネットと ASP.NET MVC アプリケーションをホストしている場合でも、この最初のオプションを使用できるポータルをホストしています。</span><span class="sxs-lookup"><span data-stu-id="7f03b-258">That means that you can use this first option even when hosting your ASP.NET MVC application with an Internet hosting company.</span></span>

<span data-ttu-id="7f03b-259">2 番目のオプションでは、ワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f03b-259">The second option is to create a wildcard script map.</span></span> <span data-ttu-id="7f03b-260">この 2 つ目のオプションの利点は、Url を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7f03b-260">The advantage of this second option is that you do not need to modify your URLs.</span></span> <span data-ttu-id="7f03b-261">この 2 つ目のオプションの欠点は、ASP.NET MVC アプリケーションのパフォーマンスに悪影響です。</span><span class="sxs-lookup"><span data-stu-id="7f03b-261">The disadvantage of this second option is that it can impact the performance of your ASP.NET MVC application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7f03b-262">前へ</span><span class="sxs-lookup"><span data-stu-id="7f03b-262">Previous</span></span>](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
