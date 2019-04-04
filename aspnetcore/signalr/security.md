---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: bradygaster
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 6e9f849ed856cf1cbf989b8b16cab5209c465471
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56743788"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core SignalR でのセキュリティに関する考慮事項

によって[Andrew Stanton-nurse](https://twitter.com/anurse)

この記事では、SignalR のセキュリティ保護についてを説明します。

## <a name="cross-origin-resource-sharing"></a>クロス オリジン リソース共有

[クロス オリジン リソース共有 (CORS)](https://www.w3.org/TR/cors/)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。 JavaScript コードは、SignalR アプリから別のドメインでホストされている場合[CORS ミドルウェア](xref:security/cors)SignalR アプリへの接続に JavaScript を許可する有効にする必要があります。 クロス オリジン要求を信頼するドメインまたはコントロールからのみを許可します。 例:

* サイトがホストされています。 `http://www.example.com`
* SignalR アプリがホストされています。 `http://signalr.example.com`

のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。

CORS の構成の詳細については、[を有効にするクロス オリジン要求 (CORS)](xref:security/cors)を参照してください。 SignalR**必要**以下の CORS ポリシー。

* 特定の想定されるオリジンを許可します。 任意のオリジンを許可することができますが、**いない**セキュリティで保護されたかをお勧めします。
* HTTP メソッド`GET`と`POST`許可する必要があります。
* 認証を使用しない場合でも、資格情報を有効にする必要があります。

たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます`https://example.com`でホストされている SignalR アプリにアクセスする`https://signalr.example.com`:。

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。

## <a name="websocket-origin-restriction"></a>WebSocket の配信元の制限

::: moniker range=">= aspnetcore-2.2"

CORS で提供される保護は、WebSocket には適用されません。 Websocket を配信元の制限、読み取り[Websocket 原点制限](xref:fundamentals/websockets#websocket-origin-restriction)します。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS で提供される保護は、WebSocket には適用されません。 ブラウザーでは以下を実行**しません**。

* CORS の事前要求を実行する。
* WebSocket 要求を行うときに `Access-Control` ヘッダーに指定された制限を考慮する。

ただし、WebSocket 要求を発行するときにはブラウザーから `Origin` ヘッダーが送信されます。 予期した配信元からの WebSocket のみが許可されるように、アプリケーションでこれらのヘッダーが検証されるように構成する必要があります。

配置のカスタム ミドルウェアを使用してヘッダーの検証を実現できる ASP.NET Core 2.1 以降では、**する前に`UseSignalR`、認証ミドルウェアと**で`Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin` ヘッダーは、クライアントによって制御され、`Referer` のように偽装することができます。 これらのヘッダーにする必要があります**いない**認証メカニズムとして使用します。

::: moniker-end

## <a name="access-token-logging"></a>アクセス トークンのログ記録

Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。 標準を使用してとして一般に安全ではクエリ文字列を使用してアクセス トークンを受信`Authorization`ヘッダー。 クライアントとサーバー間のセキュリティで保護されたエンド ツー エンド接続を確実に常に HTTPS を使用する必要があります。 クエリ文字列を含む、各要求の URL を多くの web サーバーにログインします。 Url をログには、アクセス トークンがログに記録可能性があります。 ASP.NET Core では、既定で、クエリ文字列が含まれる各要求の URL を記録します。 例:

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

サーバーのログでこのデータのログ記録に関する懸念がある場合は場合、構成によっては、完全でこのログ記録を無効にできます、`Microsoft.AspNetCore.Hosting`ロガー、`Warning`レベル以上 (でこれらのメッセージが書き込まれます`Info`レベル)。 上のドキュメントを参照して[ログのフィルター処理](xref:fundamentals/logging/index#log-filtering)詳細についてはします。 特定の要求情報を記録する場合は、 [、ミドルウェアを作成する](xref:fundamentals/middleware/write)フィルターで除外、必要なデータをログ記録、 `access_token` (存在する) 場合、クエリ文字列の値。

## <a name="exceptions"></a>例外

例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。 既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。 代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。 クライアントに例外メッセージを配信 (たとえば開発またはテスト) でオーバーライドできる[ `EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)します。 例外メッセージは、運用アプリでクライアントに公開されませんする必要があります。

## <a name="buffer-management"></a>バッファー管理

SignalR では、接続ごとのバッファーを使用して、受信および送信メッセージを管理します。 既定では、SignalR は、これらを 32 KB のバッファーを制限します。 クライアントまたはサーバーに送信できる最大のメッセージは、32 KB です。 メッセージの接続によって消費される最大メモリは、32 KB です。 メッセージには、32 KB 未満が常に場合、制限値を減らすことができますが。

* クライアントがより大きなメッセージを送信できるようにされたりするを防止します。
* サーバーは、メッセージを受信する大きなバッファーを割り当てる必要はありません。

メッセージが 32 KB を超える場合は、制限を増やすことができます。 この制限を増やすことを意味します。

* クライアントには、サーバーを大規模メモリ バッファーを割り当てる可能性があります。
* ラージ バッファーの割り当てをサーバーには、同時接続数を減らすことができます。

受信および送信メッセージの制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:

* `ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。 クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。
* `TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。 サーバーがこの制限を超えるメッセージ (ハブ メソッドからの含む戻り値の値) を送信しようとすると、例外がスローされます。

制限値を設定`0`制限を無効にします。 制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。 悪意のあるクライアントがサイズの大きいメッセージを送信するには、過度のメモリを割り当てることがあります。 過度のメモリ使用量は、同時接続数を大幅に短縮できます。
