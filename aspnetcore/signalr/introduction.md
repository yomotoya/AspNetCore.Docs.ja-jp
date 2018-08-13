---
title: ASP.NET Core SignalR の概要
author: tdykstra
description: ASP.NET Core SignalR ライブラリがアプリへのリアルタイムの機能の追加を簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 2fff24609caf7592bad763a077288990a29617aa
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342550"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR の概要

## <a name="what-is-signalr"></a>SignalR とは何か

ASP.NET Core SignalR は、アプリへのリアルタイム Web 機能の追加を簡素化するオープン ソース ライブラリです。 リアルタイム Web 機能は、サーバー側コードからクライアントにコンテンツを即座にプッシュすることを可能にします。

SignalR に適した候補:

* サーバーから頻度の高い更新プログラムを必要とするアプリです。 例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。
* ダッシュ ボードとアプリを監視します。 例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、移動アラートします。
* コラボレーション アプリケーション。 ホワイト ボード アプリとチームのソフトウェアの会議、コラボレーション アプリケーションの例を示します。
* 通知を必要とするアプリです。 ソーシャル ネットワーク、電子メール、チャット、ゲーム、移動のアラート、およびその他の多くのアプリ通知を使用します。

SignalR がサーバーからクライアントを作成するための API を提供[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)します。 Rpc では、サーバー側の .NET Core コードからのクライアントで JavaScript 関数を呼び出します。

ASP.NET core SignalR の機能の一部を次に示します。

* 接続の管理を自動的に処理します。
* 同時に接続されているすべてのクライアントにメッセージを送信します。 たとえば、チャット ルーム。
* 特定のクライアントまたはクライアントのグループにメッセージを送信します。
* トラフィックの増加の対応にスケールします。

[GitHub の SignalR リポジトリ](https://github.com/aspnet/signalr)にソースがホストされています。

## <a name="transports"></a>トランスポート

SignalR には、リアルタイム通信を処理するためのいくつかの手法がサポートされています。

* [WebSocket](https://tools.ietf.org/html/rfc7118)
* Server-Sent Events
* ロング ポーリング

SignalR は、サーバーおよびクライアントの機能に応じて最適なトランスポート メソッドを自動的に選択します。

## <a name="hubs"></a>ハブ

SignalR は、クライアントとサーバー間の通信に*ハブ*を使用します。

ハブは、相互にメソッドを呼び出すには、クライアントとサーバーを許可する高度なパイプラインです。 SignalR は、クライアント、サーバーで、その逆のメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。 厳密に型指定されたパラメーターは、モデル バインディングを有効、メソッドに渡すことができます。 SignalR は、2 つのプロトコルの組み込みのハブを提供します。 テキスト プロトコルでは、JSON とに基づいてバイナリ プロトコルに基づいて[MessagePack](https://msgpack.org/)します。  MessagePack は一般に、JSON と比較して小さいメッセージを作成します。 古いブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコル サポートを提供します。

ハブは、クライアント側メソッドのパラメーターと名前を含むメッセージを送信することによってクライアント側のコードを呼び出します。 メソッドのパラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。 クライアントは、クライアント側コードのメソッド名と一致を試みます。 クライアントには、一致が検出されると、メソッドを呼び出すし、逆シリアル化されたパラメーター データを渡します。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core の SignalR 概要](xref:tutorials/signalr)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
