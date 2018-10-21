---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477541"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core SignalR でのセキュリティに関する考慮事項

によって[Andrew Stanton-nurse](https://twitter.com/anurse)

この記事では、SignalR のセキュリティ保護についてを説明します。

## <a name="cross-origin-resource-sharing"></a>クロス オリジン リソース共有

[クロス オリジン リソース共有 (CORS)](https://www.w3.org/TR/cors/)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。 JavaScript コードは、SignalR アプリから別のドメインでホストされている場合[CORS ミドルウェア](xref:security/cors)SignalR アプリへの接続に JavaScript を許可する有効にする必要があります。 クロス オリジン要求を信頼するドメインまたはコントロールからのみを許可します。 例えば:

* サイトがホストされています。 `http://www.example.com`
* SignalR アプリがホストされています。 `http://signalr.example.com`

のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。

CORS の構成の詳細については、次を参照してください。[を有効にするクロス オリジン要求 (CORS)](xref:security/cors)します。 SignalR**必要**以下の CORS ポリシー。

* 特定の想定されるオリジンを許可します。 任意のオリジンを許可することができますが、**いない**セキュリティで保護されたかをお勧めします。
* HTTP メソッド`GET`と`POST`許可する必要があります。
* 認証を使用しない場合でも、資格情報を有効にする必要があります。

たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます`http://example.com`でホストされている SignalR アプリにアクセスする`http://signalr.example.com`:。

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。

## <a name="websocket-origin-restriction"></a>WebSocket の配信元の制限

CORS で提供される保護は、Websocket に適用されません。 ブラウザー操作を実行**いない**:

* CORS の事前要求を実行します。
* 指定された制限を尊重`Access-Control`WebSocket 要求を行うときにヘッダー。

ただし、ブラウザーを送信しないでください、 `Origin` WebSocket 要求を発行するときにヘッダー。 Websocket の予期される配信元からのみが許可されていることを確認するこれらのヘッダーを検証するアプリケーションを構成する必要があります。

配置のカスタム ミドルウェアを使用してヘッダーの検証を実現できる ASP.NET Core 2.1 以降では、**する前に`UseSignalR`、認証ミドルウェアと**で`Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin`ヘッダーは、クライアントと同様、制御、`Referer`ヘッダーを送んだことができます。 これらのヘッダーにする必要があります**いない**認証メカニズムとして使用します。

## <a name="access-token-logging"></a>アクセス トークンのログ記録

Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。 標準を使用してとして一般に安全ではクエリ文字列を使用してアクセス トークンを受信`Authorization`ヘッダー。 ただし、多くの web サーバーは、クエリ文字列を含む、各要求の URL をログインします。 Url をログには、アクセス トークンがログに記録可能性があります。 Web ログ アクセス トークンを防ぐためにサーバーのログ記録の設定を設定することをお勧めします。

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
