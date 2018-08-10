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

* サーバーから頻度の高い更新を必要とするアプリ。例えば、ゲーム、ソーシャル ネットワーク、投票、オークション、マップ、および GPS アプリです。
* ダッシュ ボードや監視アプリ。会社のダッシュ ボード、即座に更新される販売情報、渡航に関する危険情報、が例に含まれます。
* コラボレーション アプリ。ホワイト ボード アプリやチーム会議ソフトウェアがコラボレーション アプリの例です。
* 通知を必要とするアプリ。ソーシャル ネットワーク、電子メール、チャット、ゲーム、渡航に関する危険情報、およびその他の多くのアプリが通知を使用します。

SignalR は、サーバー・クライアント間の[リモート プロシージャ コール (RPC)](https://wikipedia.org/wiki/Remote_procedure_call)を作成するための API を提供します。RPC は、サーバー側の .NET Core コードからクライアントの JavaScript 関数を呼び出します。

ASP.NET core SignalR の機能の一部を次に示します。

* 接続の管理を自動的に処理します。
* 同時に接続されているすべてのクライアントにメッセージを送信します。例えば、チャット ルーム。
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

ハブは、相互にメソッドを呼び出すことをクライアントとサーバーに許可する、高度なパイプラインです。SignalR は、コンピューターの境界を越えてディスパッチを自動的に処理します。これにより、クライアントはサーバーのメソッドを呼び出すことが許可され、その逆も許可されます。モデル バインディングを有効にする厳密に型指定されたパラメーターを、メソッドに渡すことができます。SignalR は、2 つの組み込みハブプロトコルを提供します。JSONに基づくテキスト プロトコルと、[MessagePack](https://msgpack.org/)に基づくバイナリ プロトコルです。MessagePack は一般に、JSON と比較して小さいメッセージを作成します。MessagePack プロトコル サポートを提供するため、古いブラウザーは[XHR レベル 2](https://caniuse.com/#feat=xhr2)をサポートする必要があります。

ハブは、クライアント側メソッドのパラメーターと名前を含むメッセージを送信することによって、クライアント側のコードを呼び出します。メソッドのパラメーターとして送信されたオブジェクトは、設定されたプロトコルを使用して逆シリアル化されます。クライアントは、クライアント側コードのメソッド名との一致を試みます。クライアントが一致を検出すると、逆シリアル化されたパラメーター データを渡してメソッドを呼び出します。

## <a name="additional-resources"></a>その他の技術情報

* [ASP.NET Core の SignalR 概要](xref:tutorials/signalr)
* [サポートされているプラットフォーム](xref:signalr/supported-platforms)
* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
