---
title: ASP.NET Core SignalR でのセキュリティに関する考慮事項
author: tdykstra
description: ASP.NET Core SignalR での認証と承認を使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: b66c7fbfbaee4c70a68f3132875fbc81018c3e20
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095133"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>ASP.NET Core SignalR でのセキュリティに関する考慮事項

によって[Andrew Stanton-nurse](https://twitter.com/anurse)

## <a name="overview"></a>概要

SignalR は、既定では、さまざまなセキュリティ保護を提供します。 このような保護を構成する方法を理解しておく必要があります。

### <a name="cross-origin-resource-sharing"></a>クロス オリジン リソース共有

[クロス オリジン リソース共有 (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)ブラウザーでクロス オリジンの SignalR 接続を許可するために使用できます。 有効にする必要がある場合は、JavaScript コードは、SignalR アプリから別のドメイン名でホストされているが、 [ASP.NET Core の CORS ミドルウェア](xref:security/cors)接続できるようにします。 一般に、クロス オリジン要求を制御するドメインからのみを許可します。 サイトがホストされている場合など、`http://www.example.com`で SignalR アプリケーションがホストされていると`http://signalr.example.com`、のみ、配信元を許可する SignalR アプリで CORS を構成する必要があります`www.example.com`します。

CORS の構成の詳細については、次を参照してください。 [ASP.NET Core の CORS にドキュメント](xref:security/cors)します。 SignalR では、正常に動作するために、以下の CORS ポリシーが必要です。

* ポリシーには、期待するか (推奨されません) 任意のオリジンを許可する特定のオリジンを許可する必要があります。
* HTTP メソッド`GET`と`POST`許可する必要があります。
* 認証を使用していない場合でも、資格情報を有効にする必要があります。

たとえば、次の CORS ポリシーには、SignalR クライアント ブラウザーでホストされていることができます。 `http://example.com` SignalR アプリにアクセスします。

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> SignalR は、Azure App Service での組み込み CORS 機能と互換性がありません。

### <a name="access-token-logging"></a>アクセス トークンのログ記録

Websocket または Server-Sent イベントを使用する場合、クライアントのブラウザーは、クエリ文字列でアクセス トークンを送信します。 標準を使用してとして一般に安全ではこの`Authorization`ヘッダー、クエリ文字列を含むが、多くの web サーバーが各要求の URL を記録します。 つまり、アクセス トークンをログに含めることができます。 この情報をログ記録を回避するために、web サーバーのログ設定を確認してください。

### <a name="exceptions"></a>例外

例外メッセージは、一般に、機密データをクライアントに公開しないでくださいと見なされます。 既定では、SignalR は、クライアント ハブ メソッドによってスローされる例外の詳細を送信しません。 代わりに、クライアントは、エラーの発生を示す汎用メッセージを受信します。 この動作をオーバーライドするには、設定、 [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options)設定します。

### <a name="buffer-management"></a>バッファー管理

SignalR では、受信および送信メッセージを管理するために、接続ごとのバッファーを使用します。 既定では、SignalR は、これらを 32 KB のバッファーを制限します。 これは、クライアントまたはサーバーに送信できる最大の可能なメッセージが 32 KB を意味します。 メッセージの接続によって消費されるメモリの最大量が 32 KB も意味します。 メッセージがこの制限より小さいが常にわかっている場合は、クライアントの大きいメッセージを送信して、そのまま使用するメモリを割り当てるサーバーを強制的にできることを防ぐためには、このサイズを削減できます。 同様に、メッセージがこの制限を超える場合は、増やすことができます。 ただし、注意をこの制限を増やすことつまりクライアントは追加メモリを割り当てるサーバーを起こすことと、アプリが処理できる同時接続数を減らすことができます。

受信および送信メッセージの個別の制限は、両方で構成できる、 [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options)オブジェクトで構成されている`MapHub`:

* `ApplicationMaxBufferSize` クライアントからのバイトの最大数を表しますサーバーのバッファー。 クライアントがこの制限を超えるメッセージを送信しようとすると、接続は閉じ可能性があります。
* `TransportMaxBufferSize` サーバーに送信できるバイトの最大数を表します。 サーバーが、メッセージを送信しようとしています。 場合 (ハブ メソッドの戻り値が含まれています)、この制限を超える、例外がスローされます。

制限値を設定`0`制限を完全に無効にします。 ただし、これは十分注意して行ってください。 制限を削除すると、任意のサイズのメッセージを送信するクライアントができます。 これは、アプリがサポートできる同時接続数を大幅に削減できますが、過度のメモリを割り当てられると、悪意のあるクライアントで使用できます。
