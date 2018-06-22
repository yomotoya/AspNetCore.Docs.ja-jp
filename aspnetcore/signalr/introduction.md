---
title: ASP.NET Core SignalR の概要
author: rachelappel
description: ASP.NET Core SignalR ライブラリがリアルタイムの機能をアプリに追加する簡略化する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 0c833acea139d22883a69a02c2357a71f3ac8db8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277905"
---
# <a name="introduction-to-aspnet-core-signalr"></a>ASP.NET Core SignalR の概要

作成者: [Rachel Appel](https://twitter.com/rachelappel)

## <a name="what-is-signalr"></a>SignalR とは何ですか。

ASP.NET Core SignalR は、アプリに追加のリアルタイム web 機能を簡略化するライブラリです。 リアルタイム web 機能は、クライアントへのプッシュ コンテンツ サーバー側コードを即座になります。

SignalR の適切な候補は:

* 高周波数の更新プログラムをサーバーを必要とするアプリです。 例は、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。
* ダッシュ ボードとアプリを監視します。 例では、自社のダッシュ ボード、販売のインスタントの更新プログラムを含めるか、アラートを移動します。
* コラボレーション アプリケーションです。 ホワイト ボード アプリとソフトウェアの会議チーム コラボレーション アプリケーションの例に示します。
* 通知を必要とするアプリです。 ソーシャル ネットワーク、電子メール、チャット、ゲーム、旅行のアラート、およびその他の多くのアプリは、通知を使用します。

SignalR では、サーバーからクライアントを作成するため、API が用意されています[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)です。 Rpc では、サーバー側の .NET Core コードから、クライアントでの JavaScript 関数を呼び出します。

ASP.NET Core の SignalR:

* 接続の管理を自動的に処理します。
* 同時に接続されているすべてのクライアントにメッセージのブロードキャストを有効にします。 チャット ルームなどです。
* 特定のクライアントまたはクライアントのグループへのメッセージ送信を有効にします。
* オープン ソースでは、 [GitHub](https://github.com/aspnet/signalr)です。
* スケーラブルなです。

クライアントとサーバー間の接続は HTTP 接続とは異なり、永続的です。

## <a name="transports"></a>トランスポート

リアルタイムの web アプリケーションを構築するための手法の数を SignalR 要約します。 [Websocket](https://tools.ietf.org/html/rfc7118)最適なトランスポートが、ものでは使用できないときに、Server-Sent イベントや長いポーリングのようなその他の手法を使用できます。 SignalR は自動的に検出して、サーバーとクライアントでサポートされる機能に基づいて、適切なトランスポートを初期化します。

## <a name="hubs"></a>ハブ

SignalR では、クライアントとサーバー間の通信にハブを使用します。

ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインが。 SignalR は、クライアントとして、ローカルのメソッドとして簡単にし、その逆の場合、サーバー上のメソッドを呼び出すをできるように、自動的に、コンピューターの境界を越えてディスパッチを処理します。 ハブは、モデル バインディングを有効にする方法を厳密に型指定されたパラメーターを渡すことです。 SignalR が 2 つの組み込みのハブ プロトコルを備えています。 テキスト プロトコルは、JSON とに基づいてバイナリ プロトコルに基づく[MessagePack](https://msgpack.org/)です。  MessagePack は通常 JSON を使用するよりも小さいメッセージを作成します。 古いバージョンのブラウザーをサポートする必要があります[XHR レベル 2](https://caniuse.com/#feat=xhr2) MessagePack プロトコルのサポートを提供します。

ハブは、アクティブなトランスポートを使用してメッセージを送信することによって、クライアント側のコードを呼び出します。 メッセージには、クライアント側のメソッドのパラメーターと名前が含まれます。 メソッド パラメーターとして送信されたオブジェクトが構成されているプロトコルを使用して逆シリアル化します。 クライアントでは、クライアント側のコード内のメソッドに名前を照合します。 一致が見つかると、逆シリアル化されたパラメーター データを使用してクライアントのメソッドが実行されます。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core 用 SignalR を概要します。](xref:tutorials/signalr)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
