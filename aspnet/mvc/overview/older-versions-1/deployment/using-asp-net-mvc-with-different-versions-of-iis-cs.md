---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: ASP.NET MVC を使用して、さまざまなバージョンの IIS (c#) |Microsoft Docs
author: microsoft
description: このチュートリアルでは、異なるバージョンのインターネット インフォメーション サービスで ASP.NET MVC、および URL ルーティングを使用する方法について説明します。 さまざまな方法をについて説明します.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: aa7d00c0f54212d495f48929ed2a453942a1ed7d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828116"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a><span data-ttu-id="bbf0c-104">ASP.NET MVC を使用して、さまざまなバージョンの IIS (c#)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-104">Using ASP.NET MVC with Different Versions of IIS (C#)</span></span>
====================
<span data-ttu-id="bbf0c-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bbf0c-106">このチュートリアルでは、異なるバージョンのインターネット インフォメーション サービスで ASP.NET MVC、および URL ルーティングを使用する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-106">In this tutorial, you learn how to use ASP.NET MVC, and URL Routing, with different versions of Internet Information Services.</span></span> <span data-ttu-id="bbf0c-107">IIS 7.0 (クラシック モード)、IIS 6.0 では、以前のバージョンの IIS と ASP.NET MVC を使用するためのさまざまな方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-107">You learn different strategies for using ASP.NET MVC with IIS 7.0 (classic mode), IIS 6.0, and earlier versions of IIS.</span></span>


<span data-ttu-id="bbf0c-108">ASP.NET MVC フレームワークは、コント ローラー アクションへのブラウザー要求をルーティングする ASP.NET ルーティングに依存します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-108">The ASP.NET MVC framework depends on ASP.NET Routing to route browser requests to controller actions.</span></span> <span data-ttu-id="bbf0c-109">ASP.NET ルーティングを利用するには、web サーバーで追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-109">In order to take advantage of ASP.NET Routing, you might have to perform additional configuration steps on your web server.</span></span> <span data-ttu-id="bbf0c-110">すべては、インターネット インフォメーション サービス (IIS) と処理モードは、アプリケーションの要求のバージョンによって異なります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-110">It all depends on the version of Internet Information Services (IIS) and the request processing mode for your application.</span></span>

<span data-ttu-id="bbf0c-111">異なるバージョンの IIS の概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-111">Here's a summary of the different versions of IIS:</span></span>

- <span data-ttu-id="bbf0c-112">IIS 7.0 (統合モード) - ASP.NET のルーティングを使用するために必要な特別な構成がありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-112">IIS 7.0 (integrated mode) - No special configuration necessary to use ASP.NET Routing.</span></span>
- <span data-ttu-id="bbf0c-113">IIS 7.0 (クラシック モード) - ASP.NET のルーティングを使用する特別な構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-113">IIS 7.0 (classic mode) - You need to perform special configuration to use ASP.NET Routing.</span></span>
- <span data-ttu-id="bbf0c-114">IIS 6.0 または以下の ASP.NET のルーティングを使用する特別な構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-114">IIS 6.0 or below - You need to perform special configuration to use ASP.NET Routing.</span></span>

<span data-ttu-id="bbf0c-115">最新バージョンの IIS バージョン 7.5 (Win7) には</span><span class="sxs-lookup"><span data-stu-id="bbf0c-115">The latest version of IIS is version 7.5 (on Win7).</span></span> <span data-ttu-id="bbf0c-116">IIS の IIS 7 は Windows Server 2008 と VISTA SP1 に含まれる以上です。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-116">IIS 7 of IIS is included with Windows Server 2008 AND VISTA/SP1 and higher.</span></span> <span data-ttu-id="bbf0c-117">インストールすることもできる IIS 7.0 の Home Basic 以外の Vista オペレーティング システムの任意のバージョン (を参照してください[ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-117">You also can install IIS 7.0 on any version of the Vista operating system except Home Basic (see [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).</span></span>

<span data-ttu-id="bbf0c-118">IIS 7.0 では、要求を処理する 2 つのモードをサポートします。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-118">IIS 7.0 supports two modes for processing requests.</span></span> <span data-ttu-id="bbf0c-119">統合モードまたはクラシック モードを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-119">You can use integrated mode or classic mode.</span></span> <span data-ttu-id="bbf0c-120">統合モードで IIS 7.0 を使用する場合は、特別な構成手順を実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-120">You don't need to perform any special configuration steps when using IIS 7.0 in integrated mode.</span></span> <span data-ttu-id="bbf0c-121">ただしは IIS 7.0 を使用してクラシック モードのときに、追加の構成を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-121">However, you do need to perform additional configuration when using IIS 7.0 in classic mode.</span></span>

<span data-ttu-id="bbf0c-122">Microsoft Windows Server 2003 には、IIS 6.0 が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-122">Microsoft Windows Server 2003 includes IIS 6.0.</span></span> <span data-ttu-id="bbf0c-123">Windows Server 2003 オペレーティング システムを使用する場合は、IIS 6.0 を IIS 7.0 にアップグレードできません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-123">You cannot upgrade IIS 6.0 to IIS 7.0 when using the Windows Server 2003 operating system.</span></span> <span data-ttu-id="bbf0c-124">IIS 6.0 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-124">You must perform additional configuration steps when using IIS 6.0.</span></span>

<span data-ttu-id="bbf0c-125">Microsoft Windows XP Professional には、IIS 5.1 が含まれています。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-125">Microsoft Windows XP Professional includes IIS 5.1.</span></span> <span data-ttu-id="bbf0c-126">IIS 5.1 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-126">You must perform additional configuration steps when using IIS 5.1.</span></span>

<span data-ttu-id="bbf0c-127">最後に、Microsoft Windows 2000 および Microsoft Windows 2000 Professional には、IIS 5.0 が含まれます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-127">Finally, Microsoft Windows 2000 and Microsoft Windows 2000 Professional includes IIS 5.0.</span></span> <span data-ttu-id="bbf0c-128">IIS 5.0 を使用する場合は、追加の構成手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-128">You must perform additional configuration steps when using IIS 5.0.</span></span>

## <a name="integrated-versus-classic-mode"></a><span data-ttu-id="bbf0c-129">クラシック モードとの統合</span><span class="sxs-lookup"><span data-stu-id="bbf0c-129">Integrated versus Classic Mode</span></span>

<span data-ttu-id="bbf0c-130">2 つの異なる要求の処理モードを使用して要求を処理できる IIS 7.0: 統合とクラシックします。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-130">IIS 7.0 can process requests using two different request processing modes: integrated and classic.</span></span> <span data-ttu-id="bbf0c-131">統合モードでは、パフォーマンスが向上しより多くの機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-131">Integrated mode provides better performance and more features.</span></span> <span data-ttu-id="bbf0c-132">クラシック モードがの下位に含まれる IIS の以前のバージョンとの互換性。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-132">Classic mode is included for backwards compatibility with earlier versions of IIS.</span></span>

<span data-ttu-id="bbf0c-133">要求の処理モードは、アプリケーション プールによって決定されます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-133">The request processing mode is determined by the application pool.</span></span> <span data-ttu-id="bbf0c-134">処理モードは、アプリケーションに関連付けられているアプリケーション プールを決定することにより、特定の web アプリケーションで使用されているを指定できます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-134">You can determine which processing mode is being used by a particular web application by determining the application pool associated with the application.</span></span> <span data-ttu-id="bbf0c-135">この場合は、以下の手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-135">Follow these steps:</span></span>

1. <span data-ttu-id="bbf0c-136">インターネット インフォメーション サービス マネージャを起動します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-136">Launch the Internet Information Services Manager</span></span>
2. <span data-ttu-id="bbf0c-137">[接続] ウィンドウで、アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-137">In the Connections window, select an application</span></span>
3. <span data-ttu-id="bbf0c-138">アクション ウィンドウ、**基本設定**アプリケーションの編集 ダイアログを開くためのリンク ボックス (図 1 参照)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-138">In the Actions window, click the **Basic Settings** link to open the Edit Application dialog box (see Figure 1)</span></span>
4. <span data-ttu-id="bbf0c-139">選択したアプリケーション プールをメモしてをおきます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-139">Take note of the Application pool selected.</span></span>

<span data-ttu-id="bbf0c-140">2 つのアプリケーション プールをサポートするために既定では、IIS が構成されている: **DefaultAppPool**と**クラシック .NET AppPool**します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-140">By default, IIS is configured to support two application pools: **DefaultAppPool** and **Classic .NET AppPool**.</span></span> <span data-ttu-id="bbf0c-141">DefaultAppPool が選択されている場合は、統合された要求の処理モードでアプリケーションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-141">If DefaultAppPool is selected, then your application is running in integrated request processing mode.</span></span> <span data-ttu-id="bbf0c-142">従来の .NET AppPool が選択されている場合は、従来の要求の処理モードでは、アプリケーションが実行されています。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-142">If Classic .NET AppPool is selected, your application is running in classic request processing mode.</span></span>

<span data-ttu-id="bbf0c-143">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-143">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)</span></span>

<span data-ttu-id="bbf0c-144">**図 1**: 要求の処理モードの検出 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-144">**Figure 1**: Detecting the request processing mode([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))</span></span>

<span data-ttu-id="bbf0c-145">アプリケーションの編集 ダイアログ ボックス内で要求の処理モードを変更できることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-145">Notice that you can modify the request processing mode within the Edit Application dialog box.</span></span> <span data-ttu-id="bbf0c-146">[選択] ボタンをクリックし、アプリケーションに関連付けられているアプリケーション プールを変更します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-146">Click the Select button and change the application pool associated with the application.</span></span> <span data-ttu-id="bbf0c-147">クラシックから ASP.NET アプリケーションを統合モードに変更するときに互換性の問題が認識はことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-147">Realize that there are compatibility issues when changing an ASP.NET application from classic to integrated mode.</span></span> <span data-ttu-id="bbf0c-148">詳細については、次の記事を参照してください。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-148">For more information, see the following articles:</span></span>

- <span data-ttu-id="bbf0c-149">Windows Vista および Windows Server 2008 - 上の iis 7.0 の ASP.NET 1.1 のアップグレード [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-149">Upgrading ASP.NET 1.1 to IIS 7.0 on Windows Vista and Windows Server 2008 -- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)</span></span>
- <span data-ttu-id="bbf0c-150">Iis 7.0 の ASP.NET の統合 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-150">ASP.NET Integration With IIS 7.0 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)</span></span>

<span data-ttu-id="bbf0c-151">ASP.NET アプリケーションは、DefaultAppPool を使用している場合は、ASP.NET のルーティング (とそのため ASP.NET MVC) を機能を取得する追加の手順を実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-151">If an ASP.NET application is using the DefaultAppPool, then you don't need to perform any additional steps to get ASP.NET Routing (and therefore ASP.NET MVC) to work.</span></span> <span data-ttu-id="bbf0c-152">ただし、ASP.NET アプリケーションは、従来の .NET AppPool を使用し、読み取りを保持するように構成、さらに作業があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-152">However, if the ASP.NET application is configured to use the Classic .NET AppPool then keep reading, you have more work to do.</span></span>

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a><span data-ttu-id="bbf0c-153">古いバージョンの IIS と共に ASP.NET MVC を使用します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-153">Using ASP.NET MVC with Older Versions of IIS</span></span>

<span data-ttu-id="bbf0c-154">IIS 7.0 よりも古いバージョンの IIS で ASP.NET MVC を使用する必要がある、または IIS 7.0 を使用してクラシック モードにする必要がある、2 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-154">If you need to use ASP.NET MVC with an older version of IIS than IIS 7.0, or you need to use IIS 7.0 in classic mode, then you have two options.</span></span> <span data-ttu-id="bbf0c-155">最初に、ファイルの拡張機能を使用するルート テーブルを変更できます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-155">First, you can modify the route table to use file extensions.</span></span> <span data-ttu-id="bbf0c-156">たとえば、/Store/Details ような URL を要求するには、代わりに/Store.aspx/Details ような URL を要求します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-156">For example, instead of requesting a URL like /Store/Details, you would request a URL like /Store.aspx/Details.</span></span>

<span data-ttu-id="bbf0c-157">2 番目のオプションと呼ばれるものを作成する、*ワイルドカード スクリプト マップ*します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-157">The second option is to create something called a *wildcard script map*.</span></span> <span data-ttu-id="bbf0c-158">ワイルドカード スクリプト マップでは、ASP.NET フレームワークにすべての要求をマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-158">A wildcard script map enables you to map every request into the ASP.NET framework.</span></span>

<span data-ttu-id="bbf0c-159">Web サーバー (たとえば、アプリケーションは、インターネット サービス プロバイダーによってホストされている ASP.NET MVC) へのアクセスができない場合が、最初のオプションを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-159">If you don't have access to your web server (for example, your ASP.NET MVC application is being hosted by an Internet Service Provider) then you'll need to use the first option.</span></span> <span data-ttu-id="bbf0c-160">、Url の外観を変更する、web サーバーにアクセス権がある場合は、2 番目のオプションを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-160">If you don't want to modify the appearance of your URLs, and you have access to your web server, then you can use the second option.</span></span>

<span data-ttu-id="bbf0c-161">次のセクションで詳しくは、各オプションをについて説明します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-161">We explore each option in detail in the following sections.</span></span>

## <a name="adding-extensions-to-the-route-table"></a><span data-ttu-id="bbf0c-162">ルート テーブルに拡張機能の追加</span><span class="sxs-lookup"><span data-stu-id="bbf0c-162">Adding Extensions to the Route Table</span></span>

<span data-ttu-id="bbf0c-163">古いバージョンの IIS を使用する ASP.NET のルーティングを取得する最も簡単な方法では、Global.asax ファイルで、ルート テーブルを変更します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-163">The easiest way to get ASP.NET Routing to work with older versions of IIS is to modify your route table in the Global.asax file.</span></span> <span data-ttu-id="bbf0c-164">既定値と、リスト 1 で Global.asax ファイルが変更されていない既定のルートをという名前の 1 つのルートを構成します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-164">The default and unmodified Global.asax file in Listing 1 configures one route named the Default route.</span></span>

<span data-ttu-id="bbf0c-165">**1 - Global.asax (未変更の状態) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bbf0c-165">**Listing 1 - Global.asax (unmodified)**</span></span>

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

<span data-ttu-id="bbf0c-166">リスト 1 で構成されている既定のルートを使用すると、次のようにルート Url:</span><span class="sxs-lookup"><span data-stu-id="bbf0c-166">The Default route configured in Listing 1 enables you to route URLs that look like this:</span></span>

<span data-ttu-id="bbf0c-167">/ホーム/インデックス</span><span class="sxs-lookup"><span data-stu-id="bbf0c-167">/Home/Index</span></span>

<span data-ttu-id="bbf0c-168">製品/詳細/3</span><span class="sxs-lookup"><span data-stu-id="bbf0c-168">/Product/Details/3</span></span>

<span data-ttu-id="bbf0c-169">/製品</span><span class="sxs-lookup"><span data-stu-id="bbf0c-169">/Product</span></span>

<span data-ttu-id="bbf0c-170">残念ながら、古いバージョンの IIS、ASP.NET フレームワークにこれらの要求が転送されません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-170">Unfortunately, older versions of IIS won't pass these requests to the ASP.NET framework.</span></span> <span data-ttu-id="bbf0c-171">そのため、これらの要求をコント ローラーにルーティングされません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-171">Therefore, these requests won't get routed to a controller.</span></span> <span data-ttu-id="bbf0c-172">たとえば、URL/Home/インデックスのブラウザー要求を行う場合は、図 2 に、エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-172">For example, if you make a browser request for the URL /Home/Index then you'll get the error page in Figure 2.</span></span>

<span data-ttu-id="bbf0c-173">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-173">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)</span></span>

<span data-ttu-id="bbf0c-174">**図 2**: 404 Not Found エラーが発生する ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-174">**Figure 2**: Receiving a 404 Not Found error([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))</span></span>

<span data-ttu-id="bbf0c-175">古いバージョンの IIS は、ASP.NET フレームワークにのみ特定の要求をマップします。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-175">Older versions of IIS only map certain requests to the ASP.NET framework.</span></span> <span data-ttu-id="bbf0c-176">適切なファイル拡張子を持つ URL に対して要求があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-176">The request must be for a URL with the right file extension.</span></span> <span data-ttu-id="bbf0c-177">たとえば、/SomePage.aspx の要求は、ASP.NET フレームワークにマップを取得します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-177">For example, a request for /SomePage.aspx gets mapped to the ASP.NET framework.</span></span> <span data-ttu-id="bbf0c-178">ただし、/SomePage.htm 要求されていません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-178">However, a request for /SomePage.htm does not.</span></span>

<span data-ttu-id="bbf0c-179">そのため、ASP.NET のルーティングを機能させるには、ASP.NET フレームワークにマップされているファイル拡張子が含まれるように、既定のルートを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-179">Therefore, to get ASP.NET Routing to work, we must modify the Default route so that it includes a file extension that is mapped to the ASP.NET framework.</span></span>

<span data-ttu-id="bbf0c-180">これは、という名前のスクリプトを使用して`registermvc.wsf`します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-180">This is done using a script named `registermvc.wsf`.</span></span> <span data-ttu-id="bbf0c-181">ASP.NET MVC 1 リリースには含まれて`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`、ASP.NET 2 の時点でこのスクリプト」で使用可能な ASP.NET Futures に移動しましたが、 [ http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-181">It was included with the ASP.NET MVC 1 release in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, but as of ASP.NET 2 this script has been moved to the ASP.NET Futures, available at [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).</span></span>

<span data-ttu-id="bbf0c-182">このスクリプトを実行すると、新しい .mvc 拡張機能を IIS に登録します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-182">Executing this script registers a new .mvc extension with IIS.</span></span> <span data-ttu-id="bbf0c-183">.Mvc 拡張機能を登録すると後、は、ルートは、.mvc 拡張機能を使用するために、Global.asax ファイルで、ルートを変更できます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-183">After you register the .mvc extension, you can modify your routes in the Global.asax file so that the routes use the .mvc extension.</span></span>

<span data-ttu-id="bbf0c-184">リスト 2 で修正された Global.asax ファイルは、IIS の以前のバージョンで動作します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-184">The modified Global.asax file in Listing 2 works with older versions of IIS.</span></span>

<span data-ttu-id="bbf0c-185">**2 - Global.asax (拡張機能と変更) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bbf0c-185">**Listing 2 - Global.asax (modified with extensions)**</span></span>

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

<span data-ttu-id="bbf0c-186">**重要な**: Global.asax ファイルを変更した後にもう一度 ASP.NET MVC アプリケーションのビルドに注意してください。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-186">**Important**: remember to build your ASP.NET MVC Application again after changing the Global.asax file.</span></span>

<span data-ttu-id="bbf0c-187">リスト 2 で、Global.asax ファイルに 2 つの重要な変更があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-187">There are two important changes to the Global.asax file in Listing 2.</span></span> <span data-ttu-id="bbf0c-188">Global.asax で定義されている 2 つのルートはようになりました。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-188">There are now two routes defined in the Global.asax.</span></span> <span data-ttu-id="bbf0c-189">既定のルートの最初のルート URL パターンは、ようになります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-189">The URL pattern for the Default route, the first route, now looks like:</span></span>

<span data-ttu-id="bbf0c-190">{controller}.mvc/{action}/{id}</span><span class="sxs-lookup"><span data-stu-id="bbf0c-190">{controller}.mvc/{action}/{id}</span></span>

<span data-ttu-id="bbf0c-191">.Mvc 拡張機能の追加は、ASP.NET ルーティング モジュールをインターセプトするファイルの種類を変更します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-191">The addition of the .mvc extension changes the type of files that the ASP.NET Routing module intercepts.</span></span> <span data-ttu-id="bbf0c-192">この変更により、ASP.NET MVC アプリケーションを今すぐは、次のような要求をルーティングします。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-192">With this change, the ASP.NET MVC application now routes requests like the following:</span></span>

<span data-ttu-id="bbf0c-193">/Home.mvc/Index/</span><span class="sxs-lookup"><span data-stu-id="bbf0c-193">/Home.mvc/Index/</span></span>

<span data-ttu-id="bbf0c-194">/Product.mvc/Details/3</span><span class="sxs-lookup"><span data-stu-id="bbf0c-194">/Product.mvc/Details/3</span></span>

<span data-ttu-id="bbf0c-195">/Product.mvc/</span><span class="sxs-lookup"><span data-stu-id="bbf0c-195">/Product.mvc/</span></span>

<span data-ttu-id="bbf0c-196">2 つ目のルートでは、ルートのルートは追加されました。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-196">The second route, the Root route, is new.</span></span> <span data-ttu-id="bbf0c-197">ルートのルートの場合は、この URL パターンは、空の文字列です。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-197">This URL pattern for the Root route is an empty string.</span></span> <span data-ttu-id="bbf0c-198">このルートは、アプリケーションのルートに対して行われた要求に一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-198">This route is necessary for matching requests made against the root of your application.</span></span> <span data-ttu-id="bbf0c-199">たとえば、ルートのルートは、次のような要求と一致は。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-199">For example, the Root route will match a request that looks like this:</span></span>

[http://www.YourApplication.com/](http://www.YourApplication.com/)

<span data-ttu-id="bbf0c-200">ルート テーブルに次の変更を行った後は、すべてのアプリケーションのリンクが、これらの新しい URL パターンと互換性があるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-200">After making these modifications to your route table, you'll need to make sure that all of the links in your application are compatible with these new URL patterns.</span></span> <span data-ttu-id="bbf0c-201">つまり、.mvc 拡張機能にはすべてのリンクが含まれているを確認します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-201">In other words, make sure that all of your links include the .mvc extension.</span></span> <span data-ttu-id="bbf0c-202">Html.ActionLink() のヘルパー メソッドを使用して、リンクを生成する場合何も変更する必要がありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-202">If you use the Html.ActionLink() helper method to generate your links, then you should not need to make any changes.</span></span>

<span data-ttu-id="bbf0c-203">Registermvc.wcf スクリプトを使用する代わりに iis を手動で ASP.NET フレームワークにマップされている新しい拡張機能を追加できます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-203">Instead of using the registermvc.wcf script, you can add a new extension to IIS that is mapped to the ASP.NET framework by hand.</span></span> <span data-ttu-id="bbf0c-204">新しい拡張機能を自分で追加するときに、チェック ボックスをオンにというラベルが付いたこと確認**ファイルが存在することを確認**はチェックされません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-204">When adding a new extension yourself, make sure that the checkbox labeled **Verify that file exists** is not checked.</span></span>

## <a name="hosted-server"></a><span data-ttu-id="bbf0c-205">ホストされているサーバー</span><span class="sxs-lookup"><span data-stu-id="bbf0c-205">Hosted Server</span></span>

<span data-ttu-id="bbf0c-206">Web サーバーへのアクセスを常に必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-206">You don't always have access to your web server.</span></span> <span data-ttu-id="bbf0c-207">たとえば、インターネット ホストのプロバイダーを使用して、ASP.NET MVC アプリケーションをホストしている場合、必要がある必ずしも IIS へのアクセス。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-207">For example, if you are hosting your ASP.NET MVC application using an Internet Hosting Provider, then you won't necessarily have access to IIS.</span></span>

<span data-ttu-id="bbf0c-208">その場合は、ASP.NET フレームワークにマップされている既存のファイル拡張子のいずれかを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-208">In that case, you should use one of the existing file extensions that are mapped to the ASP.NET framework.</span></span> <span data-ttu-id="bbf0c-209">ASP.NET にマップされているファイルの拡張機能の例には、.aspx、.axd、および .ashx 拡張機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-209">Examples of file extensions mapped to ASP.NET include the .aspx, .axd, and .ashx extensions.</span></span>

<span data-ttu-id="bbf0c-210">たとえば、リスト 3 の変更の Global.asax ファイルでは、.mvc 拡張機能ではなく .aspx 拡張機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-210">For example, the modified Global.asax file in Listing 3 uses the .aspx extension instead of the .mvc extension.</span></span>

<span data-ttu-id="bbf0c-211">**3 - Global.asax (拡張子が .aspx 変更) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bbf0c-211">**Listing 3 - Global.asax (modified with .aspx extensions)**</span></span>

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

<span data-ttu-id="bbf0c-212">リスト 3 の Global.asax ファイルは、正確に .mvc 拡張機能ではなく .aspx 拡張機能を使用してそれを除けば、以前の Global.asax ファイルと同じです。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-212">The Global.asax file in Listing 3 is exactly the same as the previous Global.asax file except for the fact that it uses the .aspx extension instead of the .mvc extension.</span></span> <span data-ttu-id="bbf0c-213">.Aspx 拡張機能を使用するリモートの web サーバーでセットアップを実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-213">You don't have to perform any setup on your remote web server to use the .aspx extension.</span></span>

## <a name="creating-a-wildcard-script-map"></a><span data-ttu-id="bbf0c-214">ワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-214">Creating a Wildcard Script Map</span></span>

<span data-ttu-id="bbf0c-215">ASP.NET MVC アプリケーションの Url を変更する、web サーバーにアクセス権がある場合は、1 つのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-215">If you don't want to modify the URLs for your ASP.NET MVC application, and you have access to your web server, then you have an additional option.</span></span> <span data-ttu-id="bbf0c-216">ASP.NET framework web サーバーにすべての要求をマップするワイルドカード スクリプト マップを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-216">You can create a wildcard script map that maps all requests to the web server to the ASP.NET framework.</span></span> <span data-ttu-id="bbf0c-217">これにより、(クラシック モード) では IIS 7.0 または IIS 6.0 は既定の ASP.NET MVC ルート テーブルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-217">That way, you can use the default ASP.NET MVC route table with IIS 7.0 (in classic mode) or IIS 6.0.</span></span>

<span data-ttu-id="bbf0c-218">このオプションは、IIS web サーバーに対して行われたすべての要求をインターセプトすることに注意します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-218">Be aware that this option causes IIS to intercept every request made against the web server.</span></span> <span data-ttu-id="bbf0c-219">これには、イメージ、従来の ASP ページ、および HTML ページの要求が含まれます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-219">This includes requests for images, classic ASP pages, and HTML pages.</span></span> <span data-ttu-id="bbf0c-220">そのため、ワイルドカードの有効化スクリプト マップの asp.net がパフォーマンスに影響します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-220">Therefore, enabling a wildcard script map to ASP.NET does have performance implications.</span></span>

<span data-ttu-id="bbf0c-221">IIS 7.0 の場合、ワイルドカード スクリプト マップを有効にする方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-221">Here's how you enable a wildcard script map for IIS 7.0:</span></span>

1. <span data-ttu-id="bbf0c-222">[接続] ウィンドウで、アプリケーションを選択します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-222">Select your application in the Connections window</span></span>
2. <span data-ttu-id="bbf0c-223">必ず、**機能**ビューが選択されています。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-223">Make sure that the **Features** view is selected</span></span>
3. <span data-ttu-id="bbf0c-224">ダブルクリックして、**ハンドラー マッピング**ボタン</span><span class="sxs-lookup"><span data-stu-id="bbf0c-224">Double-click the **Handler Mappings** button</span></span>
4. <span data-ttu-id="bbf0c-225">をクリックして、**ワイルドカード スクリプト マップの追加**(図 3 参照) リンク</span><span class="sxs-lookup"><span data-stu-id="bbf0c-225">Click the **Add Wildcard Script Map** link (see Figure 3)</span></span>
5. <span data-ttu-id="bbf0c-226">Aspnet へのパスを入力\_isapi.dll のファイル (PageHandlerFactory スクリプト マップからこのパスをコピーできます)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-226">Enter the path to the aspnet\_isapi.dll file (You can copy this path from the PageHandlerFactory script map)</span></span>
6. <span data-ttu-id="bbf0c-227">MVC の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-227">Enter the name MVC</span></span>
7. <span data-ttu-id="bbf0c-228">をクリックして、 **OK**ボタン</span><span class="sxs-lookup"><span data-stu-id="bbf0c-228">Click the **OK** button</span></span>

<span data-ttu-id="bbf0c-229">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-229">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)</span></span>

<span data-ttu-id="bbf0c-230">**図 3**: IIS 7.0 でワイルドカード スクリプト マップの作成 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-230">**Figure 3**: Creating a wildcard script map with IIS 7.0([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))</span></span>

<span data-ttu-id="bbf0c-231">IIS 6.0 ではワイルドカード スクリプト マップを作成する次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-231">Follow these steps to create a wildcard script map with IIS 6.0:</span></span>

1. <span data-ttu-id="bbf0c-232">Web サイトを右クリックし、プロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-232">Right-click a website and select Properties</span></span>
2. <span data-ttu-id="bbf0c-233">選択、**ホーム ディレクトリ** タブ</span><span class="sxs-lookup"><span data-stu-id="bbf0c-233">Select the **Home Directory** tab</span></span>
3. <span data-ttu-id="bbf0c-234">をクリックして、**構成**ボタン</span><span class="sxs-lookup"><span data-stu-id="bbf0c-234">Click the **Configuration** button</span></span>
4. <span data-ttu-id="bbf0c-235">選択、**マッピング** タブ</span><span class="sxs-lookup"><span data-stu-id="bbf0c-235">Select the **Mappings** tab</span></span>
5. <span data-ttu-id="bbf0c-236">をクリックして、**挿入**ボタン (図 4 参照)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-236">Click the **Insert** button (see Figure 4)</span></span>
6. <span data-ttu-id="bbf0c-237">Aspnet へのパスを貼り付けます\_isapi.dll (.aspx ファイルのスクリプト マップからこのパスをコピーできます)、実行可能ファイルのフィールドに</span><span class="sxs-lookup"><span data-stu-id="bbf0c-237">Paste the path to the aspnet\_isapi.dll into the Executable field (you can copy this path from the script map for .aspx files)</span></span>
7. <span data-ttu-id="bbf0c-238">チェック ボックスをオフに**ファイルの存在を確認します。**</span><span class="sxs-lookup"><span data-stu-id="bbf0c-238">Uncheck the checkbox labeled **Verify that file exists**</span></span>
8. <span data-ttu-id="bbf0c-239">をクリックして、 **OK**ボタン</span><span class="sxs-lookup"><span data-stu-id="bbf0c-239">Click the **OK** button</span></span>

<span data-ttu-id="bbf0c-240">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-240">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)</span></span>

<span data-ttu-id="bbf0c-241">**図 4**: IIS 6.0 ではワイルドカード スクリプト マップの作成 ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-241">**Figure 4**: Creating a wildcard script map with IIS 6.0([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))</span></span>

<span data-ttu-id="bbf0c-242">ワイルドカード スクリプト マップを有効にした後は、ルートのルートを含むように、Global.asax ファイルのルート テーブルを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-242">After you enable wildcard script maps, you need to modify the route table in the Global.asax file so that it includes a Root route.</span></span> <span data-ttu-id="bbf0c-243">それ以外の場合、アプリケーションのルート ページの要求を行うときに、図 5 で、エラー ページが表示されます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-243">Otherwise, you'll get the error page in Figure 5 when you make a request for the root page of your application.</span></span> <span data-ttu-id="bbf0c-244">リスト 4 変更後の Global.asax ファイルを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-244">You can use the modified Global.asax file in Listing 4.</span></span>

<span data-ttu-id="bbf0c-245">[![[新しいプロジェクト] ダイアログ ボックス](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="bbf0c-245">[![The New Project dialog box](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)</span></span>

<span data-ttu-id="bbf0c-246">**図 5**: 不足しているルートのルートのエラー ([フルサイズの画像を表示する をクリックします](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-246">**Figure 5**: Missing Root route error([Click to view full-size image](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))</span></span>

<span data-ttu-id="bbf0c-247">**4 - Global.asax (ルートのルートで変更) を一覧表示します。**</span><span class="sxs-lookup"><span data-stu-id="bbf0c-247">**Listing 4 - Global.asax (modified with Root route)**</span></span>

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

<span data-ttu-id="bbf0c-248">IIS 7.0 または IIS 6.0 のいずれかのワイルドカード スクリプト マップを有効にした後は、次のように既定のルーティング テーブルを使用する要求を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-248">After you enable a wildcard script map for either IIS 7.0 or IIS 6.0, you can make requests that work with the default route table that look like this:</span></span>

/

<span data-ttu-id="bbf0c-249">/ホーム/インデックス</span><span class="sxs-lookup"><span data-stu-id="bbf0c-249">/Home/Index</span></span>

<span data-ttu-id="bbf0c-250">製品/詳細/3</span><span class="sxs-lookup"><span data-stu-id="bbf0c-250">/Product/Details/3</span></span>

<span data-ttu-id="bbf0c-251">/製品</span><span class="sxs-lookup"><span data-stu-id="bbf0c-251">/Product</span></span>

## <a name="summary"></a><span data-ttu-id="bbf0c-252">まとめ</span><span class="sxs-lookup"><span data-stu-id="bbf0c-252">Summary</span></span>

<span data-ttu-id="bbf0c-253">このチュートリアルの目的は、以前のバージョンの IIS (またはクラシック モードでの IIS 7.0) を使用する場合、ASP.NET MVC を使用する方法について説明することでした。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-253">The goal of this tutorial was to explain how you can use ASP.NET MVC when using an older version of IIS (or IIS 7.0 in classic mode).</span></span> <span data-ttu-id="bbf0c-254">古いバージョンの IIS を使用する ASP.NET のルーティングを取得する 2 つの方法について説明しました。 既定のルーティング テーブルを変更またはワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-254">We discussed two methods of getting ASP.NET Routing to work with older versions of IIS: Modify the default route table or create a wildcard script map.</span></span>

<span data-ttu-id="bbf0c-255">最初のオプションでは、ASP.NET MVC アプリケーションで使用される Url を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-255">The first option requires you to modify the URLs used in your ASP.NET MVC application.</span></span> <span data-ttu-id="bbf0c-256">この最初のオプションの非常に重要な利点の 1 つは、ルート テーブルを変更するには、web サーバーへのアクセスが不要することです。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-256">One very significant advantage of this first option is that you do not need access to a web server in order to modify the route table.</span></span> <span data-ttu-id="bbf0c-257">つまり、インターネットと ASP.NET MVC アプリケーションをホストしている場合でも、この最初のオプションを使用できますホスティング会社。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-257">That means that you can use this first option even when hosting your ASP.NET MVC application with an Internet hosting company.</span></span>

<span data-ttu-id="bbf0c-258">2 番目のオプションでは、ワイルドカード スクリプト マップを作成します。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-258">The second option is to create a wildcard script map.</span></span> <span data-ttu-id="bbf0c-259">この 2 つ目のオプションの利点は、Url を変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-259">The advantage of this second option is that you do not need to modify your URLs.</span></span> <span data-ttu-id="bbf0c-260">この 2 つ目のオプションの欠点は、ASP.NET MVC アプリケーションのパフォーマンス影響を与えることができます。</span><span class="sxs-lookup"><span data-stu-id="bbf0c-260">The disadvantage of this second option is that it can impact the performance of your ASP.NET MVC application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bbf0c-261">次へ</span><span class="sxs-lookup"><span data-stu-id="bbf0c-261">Next</span></span>](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
