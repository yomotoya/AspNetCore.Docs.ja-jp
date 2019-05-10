---
title: ASP.NET Core SignalR の運用ホスティングとスケーリング
author: bradygaster
description: パフォーマンスとスケーリングの ASP.NET Core SignalR を使用するアプリで問題を回避する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/28/2018
uid: signalr/scale
ms.openlocfilehash: 4ac4509acc89d0091a3757c7cfbc9981614f29ad
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895079"
---
# <a name="aspnet-core-signalr-hosting-and-scaling"></a>ASP.NET Core SignalR ホスティングとスケーリング

によって[Andrew Stanton-nurse](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)、および[Tom Dykstra](https://github.com/tdykstra)、

この記事では、ホスティングとスケーリングの ASP.NET Core SignalR を使用するトラフィック量の多いアプリの考慮事項について説明します。

## <a name="tcp-connection-resources"></a>TCP 接続のリソース

Web サーバーがサポートできる同時 TCP 接続の数は制限されます。 標準の HTTP クライアントを使用して、*短期*接続します。 クライアントは、アイドル状態し、後で再度開くときに、これらの接続を終了できます。 SignalR 接続は、その一方で、*永続的な*します。 SignalR 接続は、クライアントはアイドル状態時も開いたままです。 多くのクライアント トラフィックの多いアプリをこれらの永続的な接続サーバー接続の最大数をヒットする可能性があります。

永続的な接続では、各接続を追跡するために、追加のメモリも消費します。

SignalR 接続に関連するリソースを多用する同じサーバーでホストされている他の web アプリに影響を与えることができます。 SignalR を開くし、最後の利用可能な TCP 接続を保持している、ときに、同じサーバー上の他の web アプリが使用できるようにする接続がもありません。

サーバーは、接続から実行する場合は、ランダムなソケット エラーが表示されますおよび接続リセット エラー。 例:

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

他の web apps でのエラーの原因から SignalR リソース使用率を維持するには、他の web アプリとは異なるサーバーで SignalR を実行します。

SignalR のリソース使用量が SignalR アプリでエラーが発生するを防ぐ、スケール アウト処理するために、サーバーには接続の数を制限します。

## <a name="scale-out"></a>スケール アウト

SignalR を使用するアプリは、サーバー ファームの問題を作成しますが、そのすべての接続を追跡する必要があります。 サーバーを追加し、他のサーバーについて把握していない新しい接続を取得します。 たとえば、次の図の各サーバーで SignalR では、他のサーバー上の接続の認識しません。 SignalR のサーバーのいずれかでは、すべてのクライアントにメッセージを送信する場合、そのサーバーに接続しているクライアントにのみ、メッセージが移動します。

![SignalR のバック プレーンのないスケーリング](scale/_static/scale-no-backplane.png)

この問題を解決するためのオプションは、 [Azure SignalR サービス](#azure-signalr-service)と[バック プレーンの Redis](#redis-backplane)します。

## <a name="azure-signalr-service"></a>Azure SignalR Service

Azure SignalR サービスには、バック プレーンではなく、プロキシです。 クライアントが、サーバーへの接続を開始するたびに、クライアントは、サービスへの接続にリダイレクトされます。 そのプロセスは、次の図に示します。

![Azure SignalR サービスへの接続を確立します。](scale/_static/azure-signalr-service-one-connection.png)

結果は、サービスを管理するすべてのクライアント接続では、各サーバーは、次の図に示すように小さな一定数のサービスへの接続のみを必要があります。

![クライアントは、サービス、サービスに接続されているサーバーに接続](scale/_static/azure-signalr-service-multiple-connections.png)

この方法でスケール アウトするには、Redis バック プレーンの代わりとして十いくつかの利点があります。

* スティッキー セッションとも呼ばれます[クライアント アフィニティ](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)、接続するときに、クライアントは Azure SignalR サービスにリダイレクトされますすぐにために必須ではありません。
* アプリがスケール アウト SignalR は、任意の数の接続を処理するために、Azure SignalR サービスが自動的にスケーリング中に送信されたメッセージの数に基づきます。 たとえば、何千ものクライアントがありますが、SignalR のアプリを接続自体の処理に複数のサーバーへのスケール アウトする必要はありません、1 秒あたりのいくつかのメッセージが送信専用の場合。
* SignalR アプリケーションでは、SignalR せず web アプリよりもはるかに多くの接続リソースを使用しません。

これらの理由から、App Service、Vm、およびコンテナーを含む、Azure でホストされているすべての ASP.NET Core SignalR アプリ Azure SignalR サービスはお勧めします。

詳細については、次を参照してください。、 [Azure SignalR サービスのドキュメント](/azure/azure-signalr/signalr-overview)します。

## <a name="redis-backplane"></a>Redis バックプレーン

[Redis](https://redis.io/)はパブリッシュ/サブスクライブ モデルでのメッセージング システムをサポートするメモリ内キー値ストアです。 SignalR で Redis バック プレーンでは、パブリッシュ/サブスクライブ機能を使用して、他のサーバーにメッセージを転送します。 クライアントが接続した場合、接続情報は、バック プレーンに渡されます。 サーバーは、すべてのクライアントにメッセージを送信する場合、バック プレーンに送信します。 接続されているすべてのクライアントとのバック プレーンを知っているサーバー上にいます。 それぞれのサーバーを経由してすべてのクライアントにメッセージを送信します。 このプロセスは、次の図に示します。

![Redis のバック プレーン、1 つのサーバーからすべてのクライアントに送信されるメッセージ](scale/_static/redis-backplane.png)

Redis のバック プレーンは、独自のインフラストラクチャでホストされているアプリの推奨されるスケール アウト アプローチです。 Azure SignalR サービスでは、運用環境で使用、データ センターと Azure データ センター間の接続の待機時間により、オンプレミスのアプリを使用して実用的な選択肢はありません。

前に説明した Azure SignalR サービスの利点は、Redis のバック プレーンの短所は。

* スティッキー セッションとも呼ばれます[クライアント アフィニティ](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)が必要です。 サーバー上の接続が開始されると、接続にはそのサーバーに留まりますが。
* SignalR のアプリのスケールする必要があります、いくつかのメッセージが送信される場合でもに、クライアントの数に基づいてアウト。
* SignalR アプリケーションでは、SignalR せず web アプリよりもはるかに多くの接続リソースを使用します。

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [Azure SignalR サービスのドキュメント](/azure/azure-signalr/signalr-overview)
* [Redis のバック プレーンを設定します。](xref:signalr/redis-backplane)
