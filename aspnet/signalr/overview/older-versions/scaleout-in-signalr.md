---
uid: signalr/overview/older-versions/scaleout-in-signalr
title: SignalR のスケール アウト入門 1.x |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/29/2013
ms.topic: article
ms.assetid: 3fd9f11c-799b-4001-bd60-1e70cfc61c19
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: a492eabfb5a74472b44095f24704028f6cd8a12a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375222"
---
<a name="introduction-to-scaleout-in-signalr-1x"></a>SignalR のスケール アウト入門 1.x
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Patrick Fletcher](https://github.com/pfletcher)

一般は、web アプリケーションのスケール設定の 2 つの方法があります:*スケール アップ*と*スケール アウト*します。

- スケール アップは、RAM、Cpu などの大規模なサーバー (またはより大きな VM) を使用することを意味します。
- スケール アウト、負荷を処理するより多くのサーバーを追加することを意味します。

スケール アップの問題は、迅速に、マシンのサイズの上限に達したことです。 さらに、スケール アウトする必要があります。ただし、スケール アウトするときにクライアントを別のサーバーにルーティングを取得できます。 1 つのサーバーに接続されているクライアントは、別のサーバーから送信されたメッセージを受信しません。

![](scaleout-in-signalr/_static/image1.png)

1 つのソリューションと呼ばれるコンポーネントを使用して、サーバー間でメッセージを転送するように、*バック プレーン*します。 有効になっているバック プレーン、各アプリケーション インスタンスが、バック プレーンにメッセージを送信し、バック プレーンの他のアプリケーション インスタンスに転送します。 (電子機器、バック プレーンです並列コネクタのグループ。 たとえて、SignalR のバック プレーン接続する複数のサーバーです。)

![](scaleout-in-signalr/_static/image2.png)

現在、SignalR は、次の 3 つのバック プレーンを提供します。

- **Azure Service Bus**します。 Service Bus は、メッセージング インフラストラクチャを使用する疎結合方式でメッセージを送信するコンポーネントです。
- **Redis**します。 Redis はメモリ内のキー値ストアです。 Redis では、メッセージを送信するため、(「パブリッシュ/サブスクライブ」) の発行/サブスクライブ パターンをサポートしています。
- **SQL Server**します。 SQL Server バック プレーンでは、SQL テーブルにメッセージを書き込みます。 バック プレーンでは、効率的なメッセージングを Service Broker を使用します。 ただし、これは Service Broker が有効になっていない場合にも機能します。

Azure 上のアプリケーションをデプロイする場合は、Azure Service Bus のバック プレーンの使用を検討します。 サーバー ファームに展開する場合は、SQL Server または Redis のバック プレーンを検討してください。

次のトピックには、各バック プレーンのステップバイ ステップ チュートリアルが含まれます。

- [Azure Service Bus による SignalR スケールアウト](scaleout-with-windows-azure-service-bus.md)
- [Redis による SignalR スケールアウト](scaleout-with-redis.md)
- [SQL Server による SignalR スケールアウト](scaleout-with-sql-server.md)

## <a name="implementation"></a>実装

Signalr では、すべてのメッセージはメッセージ バスを通じて送信されます。 メッセージ バス実装、 [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx)インターフェイスで、パブリッシュ/サブスクライブ抽象化を提供します。 既定値を置き換えることで、バック プレーンの動作**IMessageBus**バス バック プレーンに対するに設計されています。 Redis メッセージ バスは、たとえば、 [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)、Redis を使用して[パブリッシュ/サブスクライブ](http://redis.io/topics/pubsub)メッセージを送受信するためのメカニズムです。

各サーバー インスタンスは、バスを介してバック プレーンに接続します。 メッセージが送信されると、バック プレーンに移動して、バック プレーンのすべてのサーバーに送信します。 サーバーは、バック プレーンからメッセージを取得、そのローカル キャッシュ内でメッセージが保存されます。 サーバーはし、そのローカル キャッシュから、クライアントにメッセージを配信します。

各クライアント接続のため、カーソルを使用して、メッセージ ストリームを読み取り中に、クライアントの進行状況が追跡されます。 (カーソルは、メッセージ ストリーム内の位置を表します)。クライアントが切断して再接続して、クライアントのカーソル値の後に到着したすべてのメッセージ バスを要求します。 接続を使用する場合に、同じことが起こります[ポーリング時間の長い](../getting-started/introduction-to-signalr.md#transports)します。 長いポーリング要求が完了した後、クライアントは新しい接続を開き、カーソルより後に到着したメッセージを要求します。

カーソルのメカニズムの動作でクライアントが別のサーバーにルーティングされる場合でも再接続します。 バック プレーンは、すべてのサーバーを認識しており、クライアントが接続する先のサーバーかは関係ありません。

## <a name="limitations"></a>制限事項

バック プレーンを使用して、メッセージの最大スループットは、クライアントは、1 台のサーバー ノードに直接話すときよりも低い。 バック プレーンがバック プレーンがボトルネックになることができますので、すべてのノードにすべてのメッセージを転送するためです。 この制限は、問題であるかどうかは、アプリケーションによって異なります。 たとえば、SignalR の一般的なシナリオを示します。

- [サーバー ブロードキャスト](tutorial-server-broadcast-with-aspnet-signalr.md)(たとえば、株価表示器): サーバー メッセージが送信される速度を制御するために、バック プレーンがこのシナリオに動作します。
- [クライアントで](tutorial-getting-started-with-signalr.md)(チャットなど)。 このシナリオでのバック プレーン場合がありますボトルネックになっているクライアントの数のメッセージの数に合わせて; は、メッセージの数が増えた場合それに比例して増えるクライアントが参加します。
- [高頻度リアルタイム メッセージング](tutorial-high-frequency-realtime-with-signalr.md)(リアルタイムのゲームなど)。 このシナリオのバック プレーンは推奨されません。

## <a name="enabling-tracing-for-signalr-scaleout"></a>SignalR スケール アウトのトレースを有効にします。

バック プレーンのトレースを有効にするには、web.config ファイルのルートの下に次のセクションを追加**構成**要素。

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
