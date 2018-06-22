---
title: SignalR HubContext
author: rachelappel
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法を説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: ccfcdc8337275fd26e09c1a43db36cf9ab90cf46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277762"
---
# <a name="send-messages-from-outside-a-hub"></a>ハブの外部からのメッセージを送信します。

によって[Mikael メンギストゥ](https://twitter.com/MikaelM_12)

SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象型です。 アプリを使用して、他の場所からメッセージを送信することも、`IHubContext`サービス。 この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>インスタンスを取得します。 `IHubContext`

ASP.NET Core SignalR でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。 インスタンスを挿入する`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。 インスタンスを使用して、クライアントにメッセージを送信します。

> [!NOTE]
> ASP.NET SignalR GlobalHost へのアクセスを提供するために使用される一方、`IHubContext`です。 ASP.NET Core では、このグローバル シングルトンの必要性を削除する依存関係インジェクション フレームワークがあります。

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>インスタンスを挿入`IHubContext`コント ローラー

インスタンスを挿入する`IHubContext`コンス トラクターに追加することでコント ローラーにします。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

インスタンスにアクセスできるようになりました、 `IHubContext`、自体のハブ必要がある場合は、ハブ メソッドを呼び出すことができます。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>インスタンスを取得`IHubContext`ミドルウェア内で

アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。

```csharp
app.Use(next => (context) =>
{
    var hubContext = (IHubContext<MyHub>)context
                        .RequestServices
                        .GetServices<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。 したがってへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティです。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:tutorials/signalr)
* [ハブ](xref:signalr/hubs)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
