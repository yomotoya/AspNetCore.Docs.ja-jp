---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: ASP.NET Core SignalR ライブラリがアプリへのリアルタイムの機能の追加を簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: bc6f25c3f35e7fb0c2c68220697f2e0fdc6a9958
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095390"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR の概要

作成者: [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>SignalR は何ですか。

ASP.NET Core SignalR は、リアルタイム web 機能を追加してアプリを簡単にするライブラリです。 リアルタイム web 機能は、プッシュのコンテンツをクライアントにサーバー側コードをすぐになります。

SignalR に適した候補:

* サーバーから頻度の高い更新プログラムを必要とするアプリです。 例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。
* ダッシュ ボードとアプリを監視します。 例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、移動アラートします。
* コラボレーション アプリケーション。 ホワイト ボード アプリとチームのソフトウェアの会議、コラボレーション アプリケーションの例を示します。
* 通知を必要とするアプリです。 ソーシャル ネットワーク、電子メール、チャット、ゲーム、移動のアラート、およびその他の多くのアプリ通知を使用します。

SignalR がサーバーからクライアントを作成するための API を提供[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)します。 Rpc では、サーバー側の .NET Core コードからのクライアントで JavaScript 関数を呼び出します。

ASP.NET core SignalR:

* 接続の管理を自動的に処理します。
* 同時に接続されているすべてのクライアントにメッセージのブロードキャストを有効にします。 たとえば、チャット ルーム。
* 特定のクライアントまたはクライアントのグループにメッセージを送信できるようにします。
* オープン ソース化では、 [GitHub](https://github.com/aspnet/signalr)します。
* 高い拡張性。

クライアントとサーバー間の接続は、HTTP 接続とは異なり、永続的です。

## <a name="transports"></a>トランスポート

多くのリアルタイムの web アプリケーションを構築するための手法、SignalR の要約。 [Websocket](https://tools.ietf.org/html/rfc7118)で最適なトランスポートが、それらが使用できない場合、Server-Sent イベント、長いポーリングなどの他の手法を使用できます。 SignalR は自動的に検出し、サーバーとクライアントでサポートされる機能に基づいて、適切なトランスポートを初期化します。

## <a name="hubs"></a>ハブ

SignalR では、クライアントとサーバー間の通信にハブを使用します。

ハブは、他のメソッドを呼び出すには、クライアントとサーバーを許可する高度なパイプラインです。 SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバーでメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。 ハブは、モデル バインディングを有効、メソッドに厳密に型指定されたパラメーターを渡します。 SignalR は、2 つのプロトコルの組み込みのハブを提供します。 テキスト プロトコルでは、JSON とに基づいてバイナリ プロトコルに基づいて[MessagePack](https://msgpack.org/)します。  MessagePack は一般に、JSON を使用するよりも小さいメッセージを作成します。 古いブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコル サポートを提供します。

ハブは、アクティブなトランスポートを使用してメッセージを送信することによって、クライアント側のコードを呼び出します。 メッセージには、クライアント側メソッドのパラメーターと名前が含まれます。 メソッドのパラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。 クライアントは、クライアント側コードのメソッド名と一致を試みます。 一致が見つかると、クライアントのメソッドは、逆シリアル化されたパラメーターのデータを使用してを実行します。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET core SignalR を概要します。](xref:tutorials/signalr)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
