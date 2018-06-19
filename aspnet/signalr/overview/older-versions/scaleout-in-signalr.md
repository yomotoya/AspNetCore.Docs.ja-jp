---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: SignalR でスケール アウトの概要 1.x |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: ee3384046bf8a0f363aa6801d7a46f68b2bf125a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043747"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>SignalR でスケール アウトの概要 1.x
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

一般は、web アプリケーションのスケールの 2 つの方法があります:*スケール アップ*と*スケール アウト*です。

- スケール アップ RAM や Cpu などのより大きなサーバー (または大規模な VM) を使用することを意味します。
- スケール アウトへの負荷がより多くのサーバーを追加します。

スケール アップに関する問題は、マシンのサイズの制限を迅速にヒットすることです。 さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーに送られますことができます。 1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。

![](scaleout-in-signalr/_static/image1.png)

1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送する、*バック プレーン*です。 有効になっているバック プレーンと各アプリケーション インスタンスが、バック プレーンにメッセージを送信バック プレーンに転送し、その他のアプリケーション インスタンス。 (エレクトロニクス バック プレーンは並列コネクタのグループです。 たとえて、SignalR のバック プレーン接続する複数のサーバーです。)

![](scaleout-in-signalr/_static/image2.png)

SignalR には、次の 3 つのバック プレーン現在提供しています。

- **Azure Service Bus**です。 Service Bus は、メッセージング インフラストラクチャにより、疎結合の方法でメッセージを送信するコンポーネントです。
- **Redis**です。 Redis は、メモリ内キー値ストアです。 Redis は、メッセージを送信する (「パブリッシュ/サブスクライブ」) の公開/定期受信パターンをサポートします。
- **SQL Server**です。 SQL Server のバック プレーンでは、SQL テーブルにメッセージを書き込みます。 バック プレーンでは、効率的なメッセージの Service Broker を使用します。 ただし、これは Service Broker が有効になっていない場合にでも動作します。

Azure でアプリケーションを配置する場合は、Azure Service Bus バック プレーンの使用を検討します。 サーバー ファームに配置する場合は、SQL Server または Redis のバック プレーンに検討してください。

次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれています。

- [Azure Service Bus による SignalR スケールアウト](scaleout-with-windows-azure-service-bus.md)
- [Redis による SignalR スケールアウト](scaleout-with-redis.md)
- [SQL Server による SignalR スケールアウト](scaleout-with-sql-server.md)

## <a name="implementation"></a>実装

、SignalR メッセージ バスを介してすべてのメッセージが送信されます。 メッセージ バスを実装して、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。 既定値に置き換えることで、バック プレーンの作業**IMessageBus**そのバック プレーン用に設計されたバスとします。 Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、し、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送信するためのメカニズムです。

各サーバー インスタンスは、バスを介してバック プレーンに接続します。 メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。 サーバーでは、バック プレーンからメッセージが取得されるメッセージをローカル キャッシュに保存されます。 サーバーはのローカル キャッシュから、クライアントにメッセージを配信します。

各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取る際に、クライアントの進行状況が追跡されます。 (カーソルは、メッセージ ストリーム内の位置を表します)。クライアントは、接続が切断され、再接続する場合、クライアントのカーソルの値の後に到着したメッセージのバスを要求します。 接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)です。 長いポーリング要求が完了した後、クライアントは新しい接続が開かし、カーソルより後に受信したメッセージを要求します。

カーソルのメカニズムのしくみに、クライアントが別のサーバーにルーティングされる場合でも再接続します。 バック プレーンは、すべてのサーバーを認識しており、クライアントに接続するサーバーでもかまいません。

## <a name="limitations"></a>制限事項

バック プレーン使用すると、メッセージの最大スループットは、クライアントと 1 台のサーバー ノードに直接通信できる場合はよりも低いです。 バック プレーンがバック プレーンがボトルネックになるために、すべてのノードにすべてのメッセージを転送するためです。 この制限に問題があるかどうかは、アプリケーションによって異なります。 たとえば、SignalR の一般的なシナリオを次に示します。

- [サーバーのブロードキャスト](tutorial-server-broadcast-with-aspnet-signalr.md)(たとえば、株価表示器): サーバーにメッセージが送信される速度を制御するために、バック プレーンがこのシナリオの適切に動作します。
- [クライアントからクライアントへの](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでバック プレーン可能性がありますするボトルネックになっている場合は、クライアントの数によるメッセージの数が拡大縮小。 つまり、メッセージの数が増えた場合比例して増えるにつれてクライアントが参加します。
- [高周波数のリアルタイム](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。

## <a name="enabling-tracing-for-signalr-scaleout"></a>SignalR スケール アウトのトレースを有効にします。

バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
