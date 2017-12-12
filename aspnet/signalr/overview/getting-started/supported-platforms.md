---
uid: signalr/overview/getting-started/supported-platforms
title: "サポートされているプラットフォーム |Microsoft ドキュメント"
author: pfletcher
description: "この記事では、SignalR でどのようなクライアントとサーバーがサポートについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 7f41017a2a8c058c01fe6f89a2503eb5fa77048e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="supported-platforms"></a><span data-ttu-id="4dc2d-103">サポートされているプラットフォーム</span><span class="sxs-lookup"><span data-stu-id="4dc2d-103">Supported Platforms</span></span>
====================
<span data-ttu-id="4dc2d-104">によって[Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4dc2d-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="4dc2d-105">この記事では、SignalR でどのようなクライアントとサーバーがサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-105">This article describes what clients and servers are supported by SignalR.</span></span> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="4dc2d-106">質問やコメント</span><span class="sxs-lookup"><span data-stu-id="4dc2d-106">Questions and comments</span></span>
> 
> <span data-ttu-id="4dc2d-107">このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-107">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="4dc2d-108">チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-108">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="4dc2d-109">さまざまなサーバーおよびクライアントの構成では、SignalR をサポートします。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-109">SignalR is supported under a variety of server and client configurations.</span></span> <span data-ttu-id="4dc2d-110">さらに、各トランスポート オプションには、一連の; 独自の要件トランスポートのシステム要件が利用できない場合は、SignalR は正常に他のトランスポートへのフェールオーバーにします。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-110">In addition, each transport option has a set of requirements of its own; if the system requirements for a transport are not available, SignalR will gracefully failover to other transports.</span></span> <span data-ttu-id="4dc2d-111">SignalR をサポートするトランスポートの詳細については、次を参照してください。[トランスポートとフォールバック](introduction-to-signalr.md#transports)です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-111">For more information on the transports that SignalR supports, see [Transports and Fallbacks](introduction-to-signalr.md#transports).</span></span>

## <a name="server-system-requirements"></a><span data-ttu-id="4dc2d-112">サーバーのシステム要件</span><span class="sxs-lookup"><span data-stu-id="4dc2d-112">Server system requirements</span></span>

<span data-ttu-id="4dc2d-113">さまざまなサーバー構成では、SignalR のサーバー コンポーネントをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-113">The SignalR server component can be hosted on a variety of server configurations.</span></span> <span data-ttu-id="4dc2d-114">このセクションでは、サポートされるオペレーティング システム、.NET framework、インターネット インフォメーション サーバー、およびその他のコンポーネントのバージョンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-114">This section describes the supported versions of operating systems, .NET framework, Internet Information Server, and other components.</span></span>

### <a name="supported-server-operating-systems"></a><span data-ttu-id="4dc2d-115">サポートされているサーバー オペレーティング システム</span><span class="sxs-lookup"><span data-stu-id="4dc2d-115">Supported server operating systems</span></span>

<span data-ttu-id="4dc2d-116">次のサーバーまたはクライアントのオペレーティング システムでは、SignalR のサーバー コンポーネントをホストすることができます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-116">The SignalR server component can be hosted in the following server or client operating systems.</span></span> <span data-ttu-id="4dc2d-117">SignalR Websocket を使用するの Windows Server 2012 または Windows 8 が必要です (WebSocket 使えます Windows Azure Web サイトで、サイトの .NET framework バージョン 4.5 に設定されており、Web ソケットが、サイトの構成 ページで有効になっている限り)。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-117">Note that for SignalR to use WebSockets, Windows Server 2012 or Windows 8 is required (WebSocket can be used on Windows Azure Web Sites, as long as the site's .NET framework version is set to 4.5, and Web Sockets is enabled in the site's Configuration page).</span></span>

- <span data-ttu-id="4dc2d-118">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="4dc2d-118">Windows Server 2012</span></span>
- <span data-ttu-id="4dc2d-119">Windows Server 2008 r2</span><span class="sxs-lookup"><span data-stu-id="4dc2d-119">Windows Server 2008 r2</span></span>
- <span data-ttu-id="4dc2d-120">Windows 8</span><span class="sxs-lookup"><span data-stu-id="4dc2d-120">Windows 8</span></span>
- <span data-ttu-id="4dc2d-121">Windows 7</span><span class="sxs-lookup"><span data-stu-id="4dc2d-121">Windows 7</span></span>
- <span data-ttu-id="4dc2d-122">Windows Azure</span><span class="sxs-lookup"><span data-stu-id="4dc2d-122">Windows Azure</span></span>

### <a name="supported-server-net-framework-version"></a><span data-ttu-id="4dc2d-123">サポートされているサーバーの .NET Framework のバージョン</span><span class="sxs-lookup"><span data-stu-id="4dc2d-123">Supported server .NET Framework version</span></span>

<span data-ttu-id="4dc2d-124">SignalR 2 は、.NET Framework 4.5 でのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-124">SignalR 2 is only supported on .NET Framework 4.5.</span></span> <span data-ttu-id="4dc2d-125">参照してください、[推奨される更新プログラム](#updates)信頼性、互換性、安定性、およびパフォーマンスを向上させる更新プログラムのセクションでします。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-125">See the [Recommended Updates](#updates) section for updates that enhance reliability, compatibility, stability, and performance.</span></span>

### <a name="supported-server-iis-versions"></a><span data-ttu-id="4dc2d-126">IIS のバージョンがサポートされているサーバー</span><span class="sxs-lookup"><span data-stu-id="4dc2d-126">Supported server IIS versions</span></span>

<span data-ttu-id="4dc2d-127">SignalR が IIS でホストされている場合は、次のバージョンがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-127">When SignalR is hosted in IIS, the following versions are supported.</span></span> <span data-ttu-id="4dc2d-128">クライアント オペレーティング システムを使用する場合など、開発用 (Windows 8 または Windows 7) の完全バージョンの IIS または Cassini 必要がありますを使用しないこと、10 個の同時接続の適用されると、接続から非常に短時間に到達なります制限がありますので注意してください。頻繁に再確立されると、一時的なものであり、破棄されていないすぐに使用されていないとします。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-128">Note that if a client operating system is used, such as for development (Windows 8 or Windows 7), full versions of IIS or Cassini should not be used, since there will be a limit of 10 simultaneous connections imposed, which will be reached very quickly since connections are transient, frequently re-established, and are not disposed immediately upon no longer being used.</span></span> <span data-ttu-id="4dc2d-129">クライアント オペレーティング システムでは、IIS Express を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-129">IIS Express should be used on client operating systems.</span></span>

<span data-ttu-id="4dc2d-130">している、WebSocket を使用する SignalR の IIS 8、または IIS 8 Express を使用する必要があります、Windows 8、Windows Server 2012、またはその後、サーバーを使用する必要があります IIS で WebSocket を有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-130">Also note that for SignalR to use WebSocket, IIS 8 or IIS 8 Express must be used, the server must be using Windows 8, Windows Server 2012, or later, and WebSocket must be enabled in IIS.</span></span> <span data-ttu-id="4dc2d-131">IIS で WebSocket を有効にする方法については、次を参照してください。 [IIS 8.0 WebSocket プロトコルのサポート](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-131">For information on how to enable WebSocket in IIS, see [IIS 8.0 WebSocket Protocol Support](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).</span></span>

- <span data-ttu-id="4dc2d-132">IIS 8、または IIS 8 を表現します。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-132">IIS 8 or IIS 8 Express.</span></span>
- <span data-ttu-id="4dc2d-133">IIS 7 および 7.5 です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-133">IIS 7 and 7.5.</span></span> <span data-ttu-id="4dc2d-134">サポート[拡張子のない Url](https://support.microsoft.com/kb/980368)が必要です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-134">Support for [extensionless URLs](https://support.microsoft.com/kb/980368) is required.</span></span>
- <span data-ttu-id="4dc2d-135">IIS は、統合モードで実行されている必要があります。クラシック モードがサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-135">IIS must be running in integrated mode; classic mode is not supported.</span></span> <span data-ttu-id="4dc2d-136">Server-Sent イベント トランスポートを使用して、クラシック モードで IIS が実行される場合に 30 秒のメッセージの遅延が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-136">Message delays of up to 30 seconds may be experienced if IIS is run in classic mode using the Server-Sent Events transport.</span></span>
- <span data-ttu-id="4dc2d-137">ホスト アプリケーションは、完全信頼モードで実行されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-137">The hosting application must be running in full trust mode.</span></span>

## <a name="client-system-requirements"></a><span data-ttu-id="4dc2d-138">クライアントのシステム要件</span><span class="sxs-lookup"><span data-stu-id="4dc2d-138">Client system requirements</span></span>

<span data-ttu-id="4dc2d-139">SignalR は、さまざまなクライアント プラットフォームで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-139">SignalR can be used in a variety of client platforms.</span></span> <span data-ttu-id="4dc2d-140">このセクションでは、SignalR を使用して、web ブラウザー、Windows デスクトップ アプリケーション、Silverlight アプリケーション、およびモバイル デバイスでのシステム要件について説明します。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-140">This section describes the system requirements for using SignalR in web browsers, Windows desktop applications, Silverlight applications, and mobile devices.</span></span>

### <a name="web-browsers"></a><span data-ttu-id="4dc2d-141">Web ブラウザー</span><span class="sxs-lookup"><span data-stu-id="4dc2d-141">Web browsers</span></span>

<span data-ttu-id="4dc2d-142">SignalR は、さまざまな web ブラウザーで使用できますが、通常、最新の 2 つのバージョンのみがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-142">SignalR can be used in a variety of web browsers, but typically, only the latest two versions are supported.</span></span>

<span data-ttu-id="4dc2d-143">ブラウザーで SignalR を使用するアプリケーションには、jQuery バージョン 1.6.4 以降メジャー (1.7.2、1.8.2、1.9.1 など) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-143">Applications that use SignalR in browsers must use jQuery version 1.6.4 or major later versions (such as 1.7.2, 1.8.2, or 1.9.1).</span></span>

<span data-ttu-id="4dc2d-144">SignalR は、以下のブラウザーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-144">SignalR can be used in the following browsers:</span></span>

- <span data-ttu-id="4dc2d-145">Microsoft Internet Explorer 8、9、10、および 11 のバージョン。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-145">Microsoft Internet Explorer versions 8, 9, 10, and 11.</span></span> <span data-ttu-id="4dc2d-146">最近、デスクトップ、およびモバイルのバージョン サポートされます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-146">Modern, Desktop, and Mobile versions are supported.</span></span>
- <span data-ttu-id="4dc2d-147">Mozilla Firefox: 現在のバージョンの 1 の場合、Windows と Mac の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-147">Mozilla Firefox: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="4dc2d-148">Google Chrome: 現在のバージョンの 1 の場合、Windows と Mac の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-148">Google Chrome: current version - 1, both Windows and Mac versions.</span></span>
- <span data-ttu-id="4dc2d-149">Safari: 現在のバージョンの 1 の場合、iOS と Mac の両方のバージョン。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-149">Safari: current version - 1, both Mac and iOS versions.</span></span>
- <span data-ttu-id="4dc2d-150">Opera: 現在のバージョンの 1、Windows のみです。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-150">Opera: current version - 1, Windows only.</span></span>
- <span data-ttu-id="4dc2d-151">Android ブラウザー</span><span class="sxs-lookup"><span data-stu-id="4dc2d-151">Android browser</span></span>

<span data-ttu-id="4dc2d-152">SignalR を使用する各種のトランスポートでは一部のブラウザーを必要とするだけでなく、独自の要件があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-152">In addition to requiring certain browsers, the various transports that SignalR uses have requirements of their own.</span></span> <span data-ttu-id="4dc2d-153">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-153">The following transports are supported under the following configurations:</span></span>

<a id="browser"></a>

<span data-ttu-id="4dc2d-154">**Web ブラウザーのトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="4dc2d-154">**Web Browser Transport Requirements**</span></span>

| <span data-ttu-id="4dc2d-155">Transport</span><span class="sxs-lookup"><span data-stu-id="4dc2d-155">Transport</span></span> | <span data-ttu-id="4dc2d-156">Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="4dc2d-156">Internet Explorer</span></span> | <span data-ttu-id="4dc2d-157">Chrome (Windows または iOS)</span><span class="sxs-lookup"><span data-stu-id="4dc2d-157">Chrome (Windows or iOS)</span></span> | <span data-ttu-id="4dc2d-158">Firefox</span><span class="sxs-lookup"><span data-stu-id="4dc2d-158">Firefox</span></span> | <span data-ttu-id="4dc2d-159">Safari (OSX または iOS)</span><span class="sxs-lookup"><span data-stu-id="4dc2d-159">Safari (OSX or iOS)</span></span> | <span data-ttu-id="4dc2d-160">Android</span><span class="sxs-lookup"><span data-stu-id="4dc2d-160">Android</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="4dc2d-161">WebSocket</span><span class="sxs-lookup"><span data-stu-id="4dc2d-161">WebSockets</span></span> | <span data-ttu-id="4dc2d-162">10+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-162">10+</span></span> | <span data-ttu-id="4dc2d-163">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-163">current - 1</span></span> | <span data-ttu-id="4dc2d-164">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-164">current - 1</span></span> | <span data-ttu-id="4dc2d-165">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-165">current - 1</span></span> | <span data-ttu-id="4dc2d-166">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-166">N/A</span></span> |
| <span data-ttu-id="4dc2d-167">サーバー送信イベント</span><span class="sxs-lookup"><span data-stu-id="4dc2d-167">Server-Sent Events</span></span> | <span data-ttu-id="4dc2d-168">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-168">N/A</span></span> | <span data-ttu-id="4dc2d-169">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-169">current - 1</span></span> | <span data-ttu-id="4dc2d-170">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-170">current - 1</span></span> | <span data-ttu-id="4dc2d-171">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-171">current - 1</span></span> | <span data-ttu-id="4dc2d-172">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-172">N/A</span></span> |
| <span data-ttu-id="4dc2d-173">ForeverFrame</span><span class="sxs-lookup"><span data-stu-id="4dc2d-173">ForeverFrame</span></span> | <span data-ttu-id="4dc2d-174">8+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-174">8+</span></span> | <span data-ttu-id="4dc2d-175">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-175">N/A</span></span> | <span data-ttu-id="4dc2d-176">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-176">N/A</span></span> | <span data-ttu-id="4dc2d-177">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-177">N/A</span></span> | <span data-ttu-id="4dc2d-178">4.1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-178">4.1</span></span> |
| <span data-ttu-id="4dc2d-179">ポーリング時間の長い</span><span class="sxs-lookup"><span data-stu-id="4dc2d-179">Long Polling</span></span> | <span data-ttu-id="4dc2d-180">8+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-180">8+</span></span> | <span data-ttu-id="4dc2d-181">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-181">current - 1</span></span> | <span data-ttu-id="4dc2d-182">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-182">current - 1</span></span> | <span data-ttu-id="4dc2d-183">現在の値-1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-183">current - 1</span></span> | <span data-ttu-id="4dc2d-184">4.1</span><span class="sxs-lookup"><span data-stu-id="4dc2d-184">4.1</span></span> |

<span data-ttu-id="4dc2d-185">\*: 6 以降のすべての機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-185">\*: 6+ required for full functionality.</span></span>

#### <a name="unsupported-browsers"></a><span data-ttu-id="4dc2d-186">サポートされていないブラウザー</span><span class="sxs-lookup"><span data-stu-id="4dc2d-186">Unsupported Browsers</span></span>

<span data-ttu-id="4dc2d-187">SignalR の while*可能性があります*を古いバージョンのブラウザーでの主な問題なく実行おに積極的に SignalR をテストしないし、一般的に表示されるバグが解決しません。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-187">While SignalR *may* run without major issues in older browser versions, we do not actively test SignalR in them and generally do not fix bugs that may appear in them.</span></span>

### <a name="windows-desktop-and-silverlight-applications"></a><span data-ttu-id="4dc2d-188">Windows デスクトップ アプリケーションと Silverlight アプリケーション</span><span class="sxs-lookup"><span data-stu-id="4dc2d-188">Windows Desktop and Silverlight Applications</span></span>

<span data-ttu-id="4dc2d-189">Web ブラウザーでは、だけでなく SignalR はスタンドアロンの Windows クライアントや Silverlight アプリケーションでホストできます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-189">In addition to running in a web browser, SignalR can be hosted in standalone Windows client or Silverlight applications.</span></span> <span data-ttu-id="4dc2d-190">Windows デスクトップ アプリケーションおよび Silverlight SignalR アプリケーションでは、次のシステム要件があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-190">Windows Desktop and Silverlight SignalR applications have the following system requirements.</span></span>

- <span data-ttu-id="4dc2d-191">.NET 4 を使用するアプリケーションは、Windows XP SP3 以降にサポートされます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-191">Applications using .NET 4 are supported on Windows XP SP3 or later.</span></span>
- <span data-ttu-id="4dc2d-192">.NET Framework 4.5 を使用してアプリケーションには、Windows Vista 以降ではサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-192">Applications using .NET Framework 4.5 are supported on Windows Vista or later.</span></span>

<span data-ttu-id="4dc2d-193">SignalR に利用可能なトランスポートではオペレーティング システムと .NET framework の要件に加えて、独自の要件があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-193">In addition to operating system and .NET framework requirements, the transports available to SignalR have requirements of their own.</span></span> <span data-ttu-id="4dc2d-194">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-194">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="4dc2d-195">**Windows デスクトップおよび Silverlight のトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="4dc2d-195">**Windows Desktop and Silverlight Transport Requirements**</span></span>

| <span data-ttu-id="4dc2d-196">Transport</span><span class="sxs-lookup"><span data-stu-id="4dc2d-196">Transport</span></span> | <span data-ttu-id="4dc2d-197">.NET アプリケーション</span><span class="sxs-lookup"><span data-stu-id="4dc2d-197">.NET application</span></span> | <span data-ttu-id="4dc2d-198">Silverlight</span><span class="sxs-lookup"><span data-stu-id="4dc2d-198">Silverlight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4dc2d-199">Web ソケット</span><span class="sxs-lookup"><span data-stu-id="4dc2d-199">Web Sockets</span></span> | <span data-ttu-id="4dc2d-200">Windows 8 以降と .NET 4.5 以降</span><span class="sxs-lookup"><span data-stu-id="4dc2d-200">Windows 8+ and .NET 4.5+</span></span> | <span data-ttu-id="4dc2d-201">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-201">N/A</span></span> |
| <span data-ttu-id="4dc2d-202">無限フレーム</span><span class="sxs-lookup"><span data-stu-id="4dc2d-202">Forever Frame</span></span> | <span data-ttu-id="4dc2d-203">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-203">N/A</span></span> | <span data-ttu-id="4dc2d-204">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-204">N/A</span></span> |
| <span data-ttu-id="4dc2d-205">サーバー送信イベント</span><span class="sxs-lookup"><span data-stu-id="4dc2d-205">Server-Sent Events</span></span> | <span data-ttu-id="4dc2d-206">.NET 4 以降</span><span class="sxs-lookup"><span data-stu-id="4dc2d-206">.NET 4+</span></span> | <span data-ttu-id="4dc2d-207">5+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-207">5+</span></span> |
| <span data-ttu-id="4dc2d-208">ポーリング時間の長い</span><span class="sxs-lookup"><span data-stu-id="4dc2d-208">Long Polling</span></span> | <span data-ttu-id="4dc2d-209">.NET 4 以降</span><span class="sxs-lookup"><span data-stu-id="4dc2d-209">.NET 4+</span></span> | <span data-ttu-id="4dc2d-210">5+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-210">5+</span></span> |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a><span data-ttu-id="4dc2d-211">Windows ストアおよび Windows Phone アプリケーション</span><span class="sxs-lookup"><span data-stu-id="4dc2d-211">Windows Store and Windows Phone Applications</span></span>

<span data-ttu-id="4dc2d-212">SignalR は、Windows ストア アプリケーションと Windows Phone 8 アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-212">SignalR can be used in Windows Store applications and Windows Phone 8 applications.</span></span> <span data-ttu-id="4dc2d-213">次の構成では、次のトランスポートがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-213">The following transports are supported under the following configurations:</span></span>

<span data-ttu-id="4dc2d-214">**Windows ストアおよび Windows Phone のトランスポートの要件**</span><span class="sxs-lookup"><span data-stu-id="4dc2d-214">**Windows Store and Windows Phone Transport Requirements**</span></span>

| <span data-ttu-id="4dc2d-215">Transport</span><span class="sxs-lookup"><span data-stu-id="4dc2d-215">Transport</span></span> | <span data-ttu-id="4dc2d-216">Windows ストア/.NET</span><span class="sxs-lookup"><span data-stu-id="4dc2d-216">Windows Store/ .NET</span></span> | <span data-ttu-id="4dc2d-217">Windows ストア/JavaScript</span><span class="sxs-lookup"><span data-stu-id="4dc2d-217">Windows Store/ JavaScript</span></span> | <span data-ttu-id="4dc2d-218">Windows Phone/IE</span><span class="sxs-lookup"><span data-stu-id="4dc2d-218">Windows Phone/ IE</span></span> | <span data-ttu-id="4dc2d-219">Windows Phone/.NET</span><span class="sxs-lookup"><span data-stu-id="4dc2d-219">Windows Phone/ .NET</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="4dc2d-220">WebSocket</span><span class="sxs-lookup"><span data-stu-id="4dc2d-220">WebSockets</span></span> | <span data-ttu-id="4dc2d-221">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-221">N/A</span></span> | <span data-ttu-id="4dc2d-222">Win8 +</span><span class="sxs-lookup"><span data-stu-id="4dc2d-222">Win8+</span></span> | <span data-ttu-id="4dc2d-223">8+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-223">8+</span></span> | <span data-ttu-id="4dc2d-224">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-224">N/A</span></span> |
| <span data-ttu-id="4dc2d-225">無限フレーム</span><span class="sxs-lookup"><span data-stu-id="4dc2d-225">Forever Frame</span></span> | <span data-ttu-id="4dc2d-226">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-226">N/A</span></span> | <span data-ttu-id="4dc2d-227">Win8 +</span><span class="sxs-lookup"><span data-stu-id="4dc2d-227">Win8+</span></span> | <span data-ttu-id="4dc2d-228">7.5+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-228">7.5+</span></span> | <span data-ttu-id="4dc2d-229">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-229">N/A</span></span> |
| <span data-ttu-id="4dc2d-230">サーバー送信イベント</span><span class="sxs-lookup"><span data-stu-id="4dc2d-230">Server-Sent Events</span></span> | <span data-ttu-id="4dc2d-231">Win8 +</span><span class="sxs-lookup"><span data-stu-id="4dc2d-231">Win8+</span></span> | <span data-ttu-id="4dc2d-232">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-232">N/A</span></span> | <span data-ttu-id="4dc2d-233">N/A</span><span class="sxs-lookup"><span data-stu-id="4dc2d-233">N/A</span></span> | <span data-ttu-id="4dc2d-234">8+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-234">8+</span></span> |
| <span data-ttu-id="4dc2d-235">ポーリング時間の長い</span><span class="sxs-lookup"><span data-stu-id="4dc2d-235">Long Polling</span></span> | <span data-ttu-id="4dc2d-236">Win8 +</span><span class="sxs-lookup"><span data-stu-id="4dc2d-236">Win8+</span></span> | <span data-ttu-id="4dc2d-237">Win8 +</span><span class="sxs-lookup"><span data-stu-id="4dc2d-237">Win8+</span></span> | <span data-ttu-id="4dc2d-238">7.5+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-238">7.5+</span></span> | <span data-ttu-id="4dc2d-239">8+</span><span class="sxs-lookup"><span data-stu-id="4dc2d-239">8+</span></span> |

<a id="updates"></a>

## <a name="recommended-updates"></a><span data-ttu-id="4dc2d-240">推奨される更新プログラム</span><span class="sxs-lookup"><span data-stu-id="4dc2d-240">Recommended Updates</span></span>

<span data-ttu-id="4dc2d-241">SignalR のサーバーは、次の更新プログラムがお勧めします。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-241">The following updates are recommended for SignalR servers:</span></span>

- <span data-ttu-id="4dc2d-242">.NET Framework 4.5 用の更新プログラムが利用可能な[ここ](https://support.microsoft.com/kb/2750149)です。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-242">An update for .NET Framework 4.5 is available [here](https://support.microsoft.com/kb/2750149).</span></span>
- <span data-ttu-id="4dc2d-243">Microsoft は、ASP.NET の Qfe を定期的に解放されます。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-243">Microsoft will periodically release QFEs for ASP.NET.</span></span> <span data-ttu-id="4dc2d-244">これらは、使用可能として適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4dc2d-244">These should be applied as available.</span></span>
