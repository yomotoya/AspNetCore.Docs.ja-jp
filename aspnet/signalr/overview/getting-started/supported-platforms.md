---
uid: signalr/overview/getting-started/supported-platforms
title: サポートされているプラットフォーム |Microsoft Docs
author: bradygaster
description: この記事では、どのようなクライアントとサーバーは、SignalR でサポートされてについて説明します。
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 60fa74b54797efbe14ba525160b2f750a4f5a451
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836052"
---
<a name="supported-platforms"></a><span data-ttu-id="871e2-103">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="871e2-103">Supported Platforms</span></span>
====================
<span data-ttu-id="871e2-104">提供者: [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="871e2-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="871e2-105">この記事では、どのようなクライアントとサーバーは、SignalR でサポートされてについて説明します。</span><span class="sxs-lookup"><span data-stu-id="871e2-105">This article describes what clients and servers are supported by SignalR.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="871e2-106">意見やご質問</span><span class="sxs-lookup"><span data-stu-id="871e2-106">Questions and comments</span></span>
> 
> <span data-ttu-id="871e2-107">このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。</span><span class="sxs-lookup"><span data-stu-id="871e2-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="871e2-108">チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。</span><span class="sxs-lookup"><span data-stu-id="871e2-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="871e2-109">SignalR は、さまざまなサーバーおよびクライアントの構成でサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-109">SignalR is supported under a variety of server and client configurations.</span></span> <span data-ttu-id="871e2-110">また、各トランスポート オプションでは、一連の; 独自の要件トランスポートのシステム要件が利用できない場合は、SignalR は適切に他のトランスポートへのフェールオーバーにします。</span><span class="sxs-lookup"><span data-stu-id="871e2-110">In addition, each transport option has a set of requirements of its own; if the system requirements for a transport are not available, SignalR will gracefully failover to other transports.</span></span> <span data-ttu-id="871e2-111">SignalR をサポートするトランスポートの詳細については、次を参照してください。[トランスポートとフォールバック](introduction-to-signalr.md#transports)します。</span><span class="sxs-lookup"><span data-stu-id="871e2-111">For more information on the transports that SignalR supports, see [Transports and Fallbacks](introduction-to-signalr.md#transports).</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="871e2-112">サーバーのシステム要件</span><span class="sxs-lookup"><span data-stu-id="871e2-112">Server system requirements</span></span>

<span data-ttu-id="871e2-113">さまざまなサーバー構成では、SignalR サーバー コンポーネントをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="871e2-113">The SignalR server component can be hosted on a variety of server configurations.</span></span> <span data-ttu-id="871e2-114">このセクションでは、オペレーティング システム、.NET framework、インターネット インフォメーション サーバー、およびその他のコンポーネントのサポートされているバージョンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="871e2-114">This section describes the supported versions of operating systems, .NET framework, Internet Information Server, and other components.</span></span>

### <a name="supported-server-operating-systems"></a><span data-ttu-id="871e2-115">サポートされているサーバー オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="871e2-115">Supported server operating systems</span></span>

<span data-ttu-id="871e2-116">SignalR のサーバー コンポーネントは、次のサーバーまたはクライアントのオペレーティング システムでホストできます。</span><span class="sxs-lookup"><span data-stu-id="871e2-116">The SignalR server component can be hosted in the following server or client operating systems.</span></span> <span data-ttu-id="871e2-117">Websocket を使用する SignalR の Windows Server 2012、Windows Server 2016 または Windows 8 が必要であるに注意してください (WebSocket を使用できます Windows Azure Web サイトでは、サイトの .NET framework のバージョンは 4.5 に設定され、サイトの Web ソケットが有効になっている限り、構成ページの場合)。</span><span class="sxs-lookup"><span data-stu-id="871e2-117">Note that for SignalR to use WebSockets, Windows Server 2012, Windows Server 2016, or Windows 8 is required (WebSocket can be used on Windows Azure Web Sites, as long as the site's .NET framework version is set to 4.5, and Web Sockets is enabled in the site's Configuration page).</span></span>

- <span data-ttu-id="871e2-118">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="871e2-118">Windows Server 2016</span></span>
- <span data-ttu-id="871e2-119">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="871e2-119">Windows Server 2012</span></span>
- <span data-ttu-id="871e2-120">Windows Server 2008 r2</span><span class="sxs-lookup"><span data-stu-id="871e2-120">Windows Server 2008 r2</span></span>
- <span data-ttu-id="871e2-121">Windows 10</span><span class="sxs-lookup"><span data-stu-id="871e2-121">Windows 10</span></span>
- <span data-ttu-id="871e2-122">Windows 8</span><span class="sxs-lookup"><span data-stu-id="871e2-122">Windows 8</span></span>
- <span data-ttu-id="871e2-123">Windows 7</span><span class="sxs-lookup"><span data-stu-id="871e2-123">Windows 7</span></span>
- <span data-ttu-id="871e2-124">Windows Azure</span><span class="sxs-lookup"><span data-stu-id="871e2-124">Windows Azure</span></span>

### <a name="supported-server-net-framework-version"></a><span data-ttu-id="871e2-125">サポートされているサーバーの .NET Framework のバージョン</span><span class="sxs-lookup"><span data-stu-id="871e2-125">Supported server .NET Framework version</span></span>

<span data-ttu-id="871e2-126">SignalR 2 は、.NET Framework 4.5 でのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="871e2-126">SignalR 2 is only supported on .NET Framework 4.5.</span></span> <span data-ttu-id="871e2-127">参照してください、[推奨される更新プログラム](#updates)信頼性、互換性、安定性、およびパフォーマンスを向上させる更新プログラムのセクション。</span><span class="sxs-lookup"><span data-stu-id="871e2-127">See the [Recommended Updates](#updates) section for updates that enhance reliability, compatibility, stability, and performance.</span></span>

### <a name="supported-server-iis-versions"></a><span data-ttu-id="871e2-128">サポートされているサーバーのバージョンの IIS</span><span class="sxs-lookup"><span data-stu-id="871e2-128">Supported server IIS versions</span></span>

<span data-ttu-id="871e2-129">SignalR は、IIS でホストされている、次のバージョンがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="871e2-129">When SignalR is hosted in IIS, the following versions are supported.</span></span> <span data-ttu-id="871e2-130">クライアントのオペレーティング システムを使用する場合などの開発 (Windows 8 または Windows 7) の完全なバージョンの IIS または Cassini する必要があります使用しないこと、接続から非常に短時間に達したが 10 の同時接続、課せられる制限があるために注意してください。頻繁に再確立されると、一時的なものとは、破棄されていないすぐに使用されていないとします。</span><span class="sxs-lookup"><span data-stu-id="871e2-130">Note that if a client operating system is used, such as for development (Windows 8 or Windows 7), full versions of IIS or Cassini should not be used, since there will be a limit of 10 simultaneous connections imposed, which will be reached very quickly since connections are transient, frequently re-established, and are not disposed immediately upon no longer being used.</span></span> <span data-ttu-id="871e2-131">クライアント オペレーティング システムでは、IIS Express を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-131">IIS Express should be used on client operating systems.</span></span>

<span data-ttu-id="871e2-132">また、WebSocket を使用する SignalR の IIS 8 または IIS 8 の Express を使用する必要があります、サーバーに Windows 8、Windows Server 2012、またはそれ以降、する必要がありますを使用して、IIS で WebSocket を有効にする必要がありますに注意してください。</span><span class="sxs-lookup"><span data-stu-id="871e2-132">Also note that for SignalR to use WebSocket, IIS 8 or IIS 8 Express must be used, the server must be using Windows 8, Windows Server 2012, or later, and WebSocket must be enabled in IIS.</span></span> <span data-ttu-id="871e2-133">IIS で WebSocket を有効にする方法については、次を参照してください。 [IIS 8.0 WebSocket プロトコルのサポート](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)します。</span><span class="sxs-lookup"><span data-stu-id="871e2-133">For information on how to enable WebSocket in IIS, see [IIS 8.0 WebSocket Protocol Support](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

- <span data-ttu-id="871e2-134">IIS 8、または IIS 8 Express。</span><span class="sxs-lookup"><span data-stu-id="871e2-134">IIS 8 or IIS 8 Express.</span></span>
- <span data-ttu-id="871e2-135">IIS 7 および 7.5 です。</span><span class="sxs-lookup"><span data-stu-id="871e2-135">IIS 7 and 7.5.</span></span> <span data-ttu-id="871e2-136">サポート[拡張子のない Url](https://support.microsoft.com/kb/980368)が必要です。</span><span class="sxs-lookup"><span data-stu-id="871e2-136">Support for [extensionless URLs](https://support.microsoft.com/kb/980368) is required.</span></span>
- <span data-ttu-id="871e2-137">IIS は、統合モードで実行する必要があります。クラシック モードがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="871e2-137">IIS must be running in integrated mode; classic mode is not supported.</span></span> <span data-ttu-id="871e2-138">最大で 30 秒間のメッセージの待ち時間は、IIS Server-Sent イベント トランスポートを使用してクラシック モードで実行している場合に発生する可能性です。</span><span class="sxs-lookup"><span data-stu-id="871e2-138">Message delays of up to 30 seconds may be experienced if IIS is run in classic mode using the Server-Sent Events transport.</span></span>
- <span data-ttu-id="871e2-139">ホスト アプリケーションは、完全な信頼モードで実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-139">The hosting application must be running in full trust mode.</span></span>

## <a name="client-system-requirements"></a><span data-ttu-id="871e2-140">クライアントのシステム要件</span><span class="sxs-lookup"><span data-stu-id="871e2-140">Client system requirements</span></span>

<span data-ttu-id="871e2-141">SignalR は、さまざまなクライアント プラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="871e2-141">SignalR can be used in a variety of client platforms.</span></span> <span data-ttu-id="871e2-142">このセクションでは、web ブラウザー、Windows デスクトップ アプリケーション、Silverlight アプリケーション、モバイル デバイスで SignalR を使用するためのシステム要件について説明します。</span><span class="sxs-lookup"><span data-stu-id="871e2-142">This section describes the system requirements for using SignalR in web browsers, Windows desktop applications, Silverlight applications, and mobile devices.</span></span>

### <a name="web-browsers"></a><span data-ttu-id="871e2-143">Web ブラウザー</span><span class="sxs-lookup"><span data-stu-id="871e2-143">Web browsers</span></span>

<span data-ttu-id="871e2-144">SignalR は、さまざまな web ブラウザーで使用できますが、通常、最新の 2 つのバージョンのみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-144">SignalR can be used in a variety of web browsers, but typically, only the latest two versions are supported.</span></span>

<span data-ttu-id="871e2-145">ブラウザーで SignalR を使用するアプリケーションには、jQuery バージョン 1.6.4 またはメジャーの以降 (1.7.2、1.8.2、1.9.1 など) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-145">Applications that use SignalR in browsers must use jQuery version 1.6.4 or major later versions (such as 1.7.2, 1.8.2, or 1.9.1).</span></span>

<span data-ttu-id="871e2-146">SignalR は、次のブラウザーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="871e2-146">SignalR can be used in the following browsers:</span></span>

- <span data-ttu-id="871e2-147">Microsoft Internet Explorer 8、9、10、および 11 のバージョン。</span><span class="sxs-lookup"><span data-stu-id="871e2-147">Microsoft Internet Explorer versions 8, 9, 10, and 11.</span></span> <span data-ttu-id="871e2-148">最新のデスクトップ、モバイル バージョンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="871e2-148">Modern, Desktop, and Mobile versions are supported.</span></span>
- <span data-ttu-id="871e2-149">現在のバージョンの Mozilla Firefox: - 1 の場合、Windows と Mac の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="871e2-149">Mozilla Firefox: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="871e2-150">Google Chrome: 現在のバージョンでは、1、Windows と Mac の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="871e2-150">Google Chrome: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="871e2-151">Safari: 現在のバージョンの - 1 の場合、Mac、iOS の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="871e2-151">Safari: current version - 1, both Mac and iOS versions.</span></span>
- <span data-ttu-id="871e2-152">Opera: 現在のバージョン - 1、Windows のみです。</span><span class="sxs-lookup"><span data-stu-id="871e2-152">Opera: current version - 1, Windows only.</span></span>
- <span data-ttu-id="871e2-153">Android ブラウザー</span><span class="sxs-lookup"><span data-stu-id="871e2-153">Android browser</span></span>

<span data-ttu-id="871e2-154">SignalR を使用するさまざまなトランスポートでは特定のブラウザーを必要とするだけでなく、独自の要件があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-154">In addition to requiring certain browsers, the various transports that SignalR uses have requirements of their own.</span></span> <span data-ttu-id="871e2-155">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-155">The following transports are supported under the following configurations:</span></span>

<a id="browser"></a>

<span data-ttu-id="871e2-156">**Web ブラウザーのトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="871e2-156">**Web Browser Transport Requirements**</span></span>

| <span data-ttu-id="871e2-157">Transport</span><span class="sxs-lookup"><span data-stu-id="871e2-157">Transport</span></span> | <span data-ttu-id="871e2-158">Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="871e2-158">Internet Explorer</span></span> | <span data-ttu-id="871e2-159">Chrome (Windows または iOS)</span><span class="sxs-lookup"><span data-stu-id="871e2-159">Chrome (Windows or iOS)</span></span> | <span data-ttu-id="871e2-160">Firefox</span><span class="sxs-lookup"><span data-stu-id="871e2-160">Firefox</span></span> | <span data-ttu-id="871e2-161">Safari (OSX または iOS)</span><span class="sxs-lookup"><span data-stu-id="871e2-161">Safari (OSX or iOS)</span></span> | <span data-ttu-id="871e2-162">Android</span><span class="sxs-lookup"><span data-stu-id="871e2-162">Android</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="871e2-163">WebSocket</span><span class="sxs-lookup"><span data-stu-id="871e2-163">WebSockets</span></span> | <span data-ttu-id="871e2-164">10+</span><span class="sxs-lookup"><span data-stu-id="871e2-164">10+</span></span> | <span data-ttu-id="871e2-165">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-165">current - 1</span></span> | <span data-ttu-id="871e2-166">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-166">current - 1</span></span> | <span data-ttu-id="871e2-167">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-167">current - 1</span></span> | <span data-ttu-id="871e2-168">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-168">N/A</span></span> |
| <span data-ttu-id="871e2-169">Server-Sent Events</span><span class="sxs-lookup"><span data-stu-id="871e2-169">Server-Sent Events</span></span> | <span data-ttu-id="871e2-170">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-170">N/A</span></span> | <span data-ttu-id="871e2-171">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-171">current - 1</span></span> | <span data-ttu-id="871e2-172">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-172">current - 1</span></span> | <span data-ttu-id="871e2-173">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-173">current - 1</span></span> | <span data-ttu-id="871e2-174">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-174">N/A</span></span> |
| <span data-ttu-id="871e2-175">ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="871e2-175">ForeverFrame</span></span> | <span data-ttu-id="871e2-176">8+</span><span class="sxs-lookup"><span data-stu-id="871e2-176">8+</span></span> | <span data-ttu-id="871e2-177">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-177">N/A</span></span> | <span data-ttu-id="871e2-178">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-178">N/A</span></span> | <span data-ttu-id="871e2-179">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-179">N/A</span></span> | <span data-ttu-id="871e2-180">4.1</span><span class="sxs-lookup"><span data-stu-id="871e2-180">4.1</span></span> |
| <span data-ttu-id="871e2-181">ロング ポーリング</span><span class="sxs-lookup"><span data-stu-id="871e2-181">Long Polling</span></span> | <span data-ttu-id="871e2-182">8+</span><span class="sxs-lookup"><span data-stu-id="871e2-182">8+</span></span> | <span data-ttu-id="871e2-183">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-183">current - 1</span></span> | <span data-ttu-id="871e2-184">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-184">current - 1</span></span> | <span data-ttu-id="871e2-185">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="871e2-185">current - 1</span></span> | <span data-ttu-id="871e2-186">4.1</span><span class="sxs-lookup"><span data-stu-id="871e2-186">4.1</span></span> |

<span data-ttu-id="871e2-187">\*:6 + のすべての機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="871e2-187">\*: 6+ required for full functionality.</span></span>

#### <a name="unsupported-browsers"></a><span data-ttu-id="871e2-188">サポートされていないブラウザー</span><span class="sxs-lookup"><span data-stu-id="871e2-188">Unsupported Browsers</span></span>

<span data-ttu-id="871e2-189">SignalR を while*可能性があります*を古いバージョンのブラウザーでの主要な問題なく実行に積極的に SignalR をテストしないでくださいし、一般に含まれるバグを修正しません。</span><span class="sxs-lookup"><span data-stu-id="871e2-189">While SignalR *may* run without major issues in older browser versions, we do not actively test SignalR in them and generally do not fix bugs that may appear in them.</span></span>

### <a name="windows-desktop-and-silverlight-applications"></a><span data-ttu-id="871e2-190">Windows デスクトップ アプリケーションと Silverlight アプリケーション</span><span class="sxs-lookup"><span data-stu-id="871e2-190">Windows Desktop and Silverlight Applications</span></span>

<span data-ttu-id="871e2-191">Web ブラウザーで実行されている、だけでなく、スタンドアロンの Windows クライアントまたは Silverlight アプリケーションで SignalR をホストできます。</span><span class="sxs-lookup"><span data-stu-id="871e2-191">In addition to running in a web browser, SignalR can be hosted in standalone Windows client or Silverlight applications.</span></span> <span data-ttu-id="871e2-192">Windows デスクトップおよび Silverlight SignalR アプリケーションでは、次のシステム要件があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-192">Windows Desktop and Silverlight SignalR applications have the following system requirements.</span></span>

- <span data-ttu-id="871e2-193">.NET 4 を使用してアプリケーションには、Windows XP SP3 以降がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="871e2-193">Applications using .NET 4 are supported on Windows XP SP3 or later.</span></span>
- <span data-ttu-id="871e2-194">.NET Framework 4.5 を使用してアプリケーションには、Windows Vista 以降ではサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-194">Applications using .NET Framework 4.5 are supported on Windows Vista or later.</span></span>

<span data-ttu-id="871e2-195">SignalR を使用可能なトランスポートではオペレーティング システムと .NET framework の要件だけでなく、独自の要件があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-195">In addition to operating system and .NET framework requirements, the transports available to SignalR have requirements of their own.</span></span> <span data-ttu-id="871e2-196">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-196">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="871e2-197">**Windows デスクトップおよび Silverlight のトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="871e2-197">**Windows Desktop and Silverlight Transport Requirements**</span></span>

| <span data-ttu-id="871e2-198">Transport</span><span class="sxs-lookup"><span data-stu-id="871e2-198">Transport</span></span> | <span data-ttu-id="871e2-199">.NET アプリケーション</span><span class="sxs-lookup"><span data-stu-id="871e2-199">.NET application</span></span> | <span data-ttu-id="871e2-200">Silverlight</span><span class="sxs-lookup"><span data-stu-id="871e2-200">Silverlight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="871e2-201">Web ソケット</span><span class="sxs-lookup"><span data-stu-id="871e2-201">Web Sockets</span></span> | <span data-ttu-id="871e2-202">Windows 8 以降、.NET 4.5 以降</span><span class="sxs-lookup"><span data-stu-id="871e2-202">Windows 8+ and .NET 4.5+</span></span> | <span data-ttu-id="871e2-203">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-203">N/A</span></span> |
| <span data-ttu-id="871e2-204">永遠にフレーム</span><span class="sxs-lookup"><span data-stu-id="871e2-204">Forever Frame</span></span> | <span data-ttu-id="871e2-205">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-205">N/A</span></span> | <span data-ttu-id="871e2-206">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-206">N/A</span></span> |
| <span data-ttu-id="871e2-207">Server-Sent Events</span><span class="sxs-lookup"><span data-stu-id="871e2-207">Server-Sent Events</span></span> | <span data-ttu-id="871e2-208">.NET 4+</span><span class="sxs-lookup"><span data-stu-id="871e2-208">.NET 4+</span></span> | <span data-ttu-id="871e2-209">5+</span><span class="sxs-lookup"><span data-stu-id="871e2-209">5+</span></span> |
| <span data-ttu-id="871e2-210">ロング ポーリング</span><span class="sxs-lookup"><span data-stu-id="871e2-210">Long Polling</span></span> | <span data-ttu-id="871e2-211">.NET 4+</span><span class="sxs-lookup"><span data-stu-id="871e2-211">.NET 4+</span></span> | <span data-ttu-id="871e2-212">5+</span><span class="sxs-lookup"><span data-stu-id="871e2-212">5+</span></span> |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a><span data-ttu-id="871e2-213">Windows ストアおよび Windows Phone アプリケーション</span><span class="sxs-lookup"><span data-stu-id="871e2-213">Windows Store and Windows Phone Applications</span></span>

<span data-ttu-id="871e2-214">SignalR は、Windows ストア アプリケーションと Windows Phone 8 アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="871e2-214">SignalR can be used in Windows Store applications and Windows Phone 8 applications.</span></span> <span data-ttu-id="871e2-215">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="871e2-215">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="871e2-216">**Windows ストアおよび Windows Phone のトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="871e2-216">**Windows Store and Windows Phone Transport Requirements**</span></span>

| <span data-ttu-id="871e2-217">Transport</span><span class="sxs-lookup"><span data-stu-id="871e2-217">Transport</span></span> | <span data-ttu-id="871e2-218">Windows ストア/.NET</span><span class="sxs-lookup"><span data-stu-id="871e2-218">Windows Store/ .NET</span></span> | <span data-ttu-id="871e2-219">Windows ストア/JavaScript</span><span class="sxs-lookup"><span data-stu-id="871e2-219">Windows Store/ JavaScript</span></span> | <span data-ttu-id="871e2-220">Windows Phone、IE</span><span class="sxs-lookup"><span data-stu-id="871e2-220">Windows Phone/ IE</span></span> | <span data-ttu-id="871e2-221">Windows Phone/ .NET</span><span class="sxs-lookup"><span data-stu-id="871e2-221">Windows Phone/ .NET</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="871e2-222">WebSocket</span><span class="sxs-lookup"><span data-stu-id="871e2-222">WebSockets</span></span> | <span data-ttu-id="871e2-223">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-223">N/A</span></span> | <span data-ttu-id="871e2-224">Win8 +</span><span class="sxs-lookup"><span data-stu-id="871e2-224">Win8+</span></span> | <span data-ttu-id="871e2-225">8+</span><span class="sxs-lookup"><span data-stu-id="871e2-225">8+</span></span> | <span data-ttu-id="871e2-226">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-226">N/A</span></span> |
| <span data-ttu-id="871e2-227">永遠にフレーム</span><span class="sxs-lookup"><span data-stu-id="871e2-227">Forever Frame</span></span> | <span data-ttu-id="871e2-228">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-228">N/A</span></span> | <span data-ttu-id="871e2-229">Win8 +</span><span class="sxs-lookup"><span data-stu-id="871e2-229">Win8+</span></span> | <span data-ttu-id="871e2-230">7.5+</span><span class="sxs-lookup"><span data-stu-id="871e2-230">7.5+</span></span> | <span data-ttu-id="871e2-231">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-231">N/A</span></span> |
| <span data-ttu-id="871e2-232">Server-Sent Events</span><span class="sxs-lookup"><span data-stu-id="871e2-232">Server-Sent Events</span></span> | <span data-ttu-id="871e2-233">Win8 +</span><span class="sxs-lookup"><span data-stu-id="871e2-233">Win8+</span></span> | <span data-ttu-id="871e2-234">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-234">N/A</span></span> | <span data-ttu-id="871e2-235">N/A</span><span class="sxs-lookup"><span data-stu-id="871e2-235">N/A</span></span> | <span data-ttu-id="871e2-236">8+</span><span class="sxs-lookup"><span data-stu-id="871e2-236">8+</span></span> |
| <span data-ttu-id="871e2-237">ロング ポーリング</span><span class="sxs-lookup"><span data-stu-id="871e2-237">Long Polling</span></span> | <span data-ttu-id="871e2-238">Win8 +</span><span class="sxs-lookup"><span data-stu-id="871e2-238">Win8+</span></span> | <span data-ttu-id="871e2-239">Win8 +</span><span class="sxs-lookup"><span data-stu-id="871e2-239">Win8+</span></span> | <span data-ttu-id="871e2-240">7.5+</span><span class="sxs-lookup"><span data-stu-id="871e2-240">7.5+</span></span> | <span data-ttu-id="871e2-241">8+</span><span class="sxs-lookup"><span data-stu-id="871e2-241">8+</span></span> |

<a id="updates"></a>

## <a name="recommended-updates"></a><span data-ttu-id="871e2-242">推奨される更新プログラム</span><span class="sxs-lookup"><span data-stu-id="871e2-242">Recommended Updates</span></span>

<span data-ttu-id="871e2-243">SignalR のサーバーは、次の更新プログラムがお勧めします。</span><span class="sxs-lookup"><span data-stu-id="871e2-243">The following updates are recommended for SignalR servers:</span></span>

- <span data-ttu-id="871e2-244">.NET Framework 4.5 の更新プログラムが利用可能な[ここ](https://support.microsoft.com/kb/2750149)します。</span><span class="sxs-lookup"><span data-stu-id="871e2-244">An update for .NET Framework 4.5 is available [here](https://support.microsoft.com/kb/2750149).</span></span>
- <span data-ttu-id="871e2-245">Microsoft は、ASP.NET の Qfe を定期的にリリースされます。</span><span class="sxs-lookup"><span data-stu-id="871e2-245">Microsoft will periodically release QFEs for ASP.NET.</span></span> <span data-ttu-id="871e2-246">これらは、利用可能として適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="871e2-246">These should be applied as available.</span></span>
