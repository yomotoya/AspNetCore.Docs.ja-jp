---
uid: signalr/overview/getting-started/supported-platforms
title: サポートされているプラットフォーム |Microsoft ドキュメント
author: pfletcher
description: この記事では、SignalR でどのようなクライアントとサーバーがサポートについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/18/2018
ms.topic: article
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 4d3dc028ff67d0a9cfa03627b5f98f6541ecfff8
ms.sourcegitcommit: 7c8fd9b7445cd77eb7f7d774bfd120c26f3b5d84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/19/2018
ms.locfileid: "31585232"
---
<a name="supported-platforms"></a>サポートされているプラットフォーム
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> この記事では、SignalR でどのようなクライアントとサーバーがサポートについて説明します。 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


さまざまなサーバーおよびクライアントの構成では、SignalR をサポートします。 さらに、各トランスポート オプションには、一連の; 独自の要件トランスポートのシステム要件が利用できない場合は、SignalR は正常に他のトランスポートへのフェールオーバーにします。 SignalR をサポートするトランスポートの詳細については、次を参照してください。[トランスポートとフォールバック](introduction-to-signalr.md#transports)です。

## <a name="server-system-requirements"></a>サーバーのシステム要件

さまざまなサーバー構成では、SignalR のサーバー コンポーネントをホストすることができます。 このセクションでは、サポートされるオペレーティング システム、.NET framework、インターネット インフォメーション サーバー、およびその他のコンポーネントのバージョンについて説明します。

### <a name="supported-server-operating-systems"></a>サポートされているサーバー オペレーティング システム

次のサーバーまたはクライアントのオペレーティング システムでは、SignalR のサーバー コンポーネントをホストすることができます。 SignalR Websocket を使用するの Windows Server 2012、Windows Server 2016 または Windows 8 が必要です (WebSocket 使えます Windows Azure Web サイトで、サイトの .NET framework のバージョンが 4.5 に設定されているし、サイトの [Web ソケットが有効になっている限り、構成] ページ)。

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Windows Azure

### <a name="supported-server-net-framework-version"></a>サポートされているサーバーの .NET Framework のバージョン

SignalR 2 は、.NET Framework 4.5 でのみサポートされます。 参照してください、[推奨される更新プログラム](#updates)信頼性、互換性、安定性、およびパフォーマンスを向上させる更新プログラムのセクションでします。

### <a name="supported-server-iis-versions"></a>IIS のバージョンがサポートされているサーバー

SignalR が IIS でホストされている場合は、次のバージョンがサポートされます。 クライアント オペレーティング システムを使用する場合など、開発用 (Windows 8 または Windows 7) の完全バージョンの IIS または Cassini 必要がありますを使用しないこと、10 個の同時接続の適用されると、接続から非常に短時間に到達なります制限がありますので注意してください。頻繁に再確立されると、一時的なものであり、破棄されていないすぐに使用されていないとします。 クライアント オペレーティング システムでは、IIS Express を使用する必要があります。

している、WebSocket を使用する SignalR の IIS 8、または IIS 8 Express を使用する必要があります、Windows 8、Windows Server 2012、またはその後、サーバーを使用する必要があります IIS で WebSocket を有効にする必要があります。 IIS で WebSocket を有効にする方法については、次を参照してください。 [IIS 8.0 WebSocket プロトコルのサポート](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support)です。

- IIS 8、または IIS 8 を表現します。
- IIS 7 および 7.5 です。 サポート[拡張子のない Url](https://support.microsoft.com/kb/980368)が必要です。
- IIS は、統合モードで実行されている必要があります。クラシック モードがサポートされていません。 Server-Sent イベント トランスポートを使用して、クラシック モードで IIS が実行される場合に 30 秒のメッセージの遅延が発生する可能性があります。
- ホスト アプリケーションは、完全信頼モードで実行されている必要があります。

## <a name="client-system-requirements"></a>クライアントのシステム要件

SignalR は、さまざまなクライアント プラットフォームで使用できます。 このセクションでは、SignalR を使用して、web ブラウザー、Windows デスクトップ アプリケーション、Silverlight アプリケーション、およびモバイル デバイスでのシステム要件について説明します。

### <a name="web-browsers"></a>Web ブラウザー

SignalR は、さまざまな web ブラウザーで使用できますが、通常、最新の 2 つのバージョンのみがサポートされています。

ブラウザーで SignalR を使用するアプリケーションには、jQuery バージョン 1.6.4 以降メジャー (1.7.2、1.8.2、1.9.1 など) を使用する必要があります。

SignalR は、以下のブラウザーで使用できます。

- Microsoft Internet Explorer 8、9、10、および 11 のバージョン。 最近、デスクトップ、およびモバイルのバージョン サポートされます。
- Mozilla Firefox: 現在のバージョンの 1 の場合、Windows と Mac の両方のバージョン。
- Google Chrome: 現在のバージョンの 1 の場合、Windows と Mac の両方のバージョン。
- Safari: 現在のバージョンの 1 の場合、iOS と Mac の両方のバージョン。
- Opera: 現在のバージョンの 1、Windows のみです。
- Android ブラウザー

SignalR を使用する各種のトランスポートでは一部のブラウザーを必要とするだけでなく、独自の要件があります。 次の構成では、次のトランスポートがサポートされています。

<a id="browser"></a>

**Web ブラウザーのトランスポートの要件**

| Transport | Internet Explorer | Chrome (Windows または iOS) | Firefox | Safari (OSX または iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSocket | 10+ | 現在の値-1 | 現在の値-1 | 現在の値-1 | N/A |
| サーバー送信イベント | N/A | 現在の値-1 | 現在の値-1 | 現在の値-1 | N/A |
| ForeverFrame | 8+ | N/A | N/A | N/A | 4.1 |
| ポーリング時間の長い | 8+ | 現在の値-1 | 現在の値-1 | 現在の値-1 | 4.1 |

\*: 6 以降のすべての機能が必要です。

#### <a name="unsupported-browsers"></a>サポートされていないブラウザー

SignalR の while*可能性があります*を古いバージョンのブラウザーでの主な問題なく実行おに積極的に SignalR をテストしないし、一般的に表示されるバグが解決しません。

### <a name="windows-desktop-and-silverlight-applications"></a>Windows デスクトップ アプリケーションと Silverlight アプリケーション

Web ブラウザーでは、だけでなく SignalR はスタンドアロンの Windows クライアントや Silverlight アプリケーションでホストできます。 Windows デスクトップ アプリケーションおよび Silverlight SignalR アプリケーションでは、次のシステム要件があります。

- .NET 4 を使用するアプリケーションは、Windows XP SP3 以降にサポートされます。
- .NET Framework 4.5 を使用してアプリケーションには、Windows Vista 以降ではサポートされています。

SignalR に利用可能なトランスポートではオペレーティング システムと .NET framework の要件に加えて、独自の要件があります。 次の構成では、次のトランスポートがサポートされています。

**Windows デスクトップおよび Silverlight のトランスポートの要件**

| Transport | .NET アプリケーション | Silverlight |
| --- | --- | --- |
| Web ソケット | Windows 8 以降と .NET 4.5 以降 | N/A |
| 無限フレーム | N/A | N/A |
| サーバー送信イベント | .NET 4+ | 5+ |
| ポーリング時間の長い | .NET 4+ | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Windows ストアおよび Windows Phone アプリケーション

SignalR は、Windows ストア アプリケーションと Windows Phone 8 アプリケーションで使用できます。 次の構成では、次のトランスポートがサポートされています。

**Windows ストアおよび Windows Phone のトランスポートの要件**

| Transport | Windows ストア/.NET | Windows ストア/JavaScript | Windows Phone/IE | Windows Phone/ .NET |
| --- | --- | --- | --- | --- |
| WebSocket | N/A | Win8 + | 8+ | N/A |
| 無限フレーム | N/A | Win8 + | 7.5+ | N/A |
| サーバー送信イベント | Win8 + | N/A | N/A | 8+ |
| ポーリング時間の長い | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>推奨される更新プログラム

SignalR のサーバーは、次の更新プログラムがお勧めします。

- .NET Framework 4.5 用の更新プログラムが利用可能な[ここ](https://support.microsoft.com/kb/2750149)です。
- Microsoft は、ASP.NET の Qfe を定期的に解放されます。 これらは、使用可能として適用する必要があります。
