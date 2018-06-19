---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR ハブ API ガイド - .NET クライアント (c#) |Microsoft ドキュメント
author: pfletcher
description: このドキュメントでは、SignalR、WPF、Silverlight、および短所を Windows ストア (WinRT) など、.NET クライアントでは、バージョン 2 のハブ API を使用して紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043939"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a>ASP.NET SignalR ハブ API ガイド - .NET クライアント (c#)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

> このドキュメントでは、SignalR バージョン 2 で Windows ストア (WinRT)、WPF、Silverlight、およびコンソール アプリケーションなどの .NET クライアントのハブ API を使用してに紹介します。
> 
> SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。
> 
> SignalR では、永続的な接続と呼ばれる、下位の API も提供します。 SignalR、ハブ、および永続的な接続の概要または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照して[SignalR - はじめ](../getting-started/index.md)です。
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [クライアントのセットアップ](#clientsetup)
- [接続を確立する方法](#establishconnection)

    - [Silverlight クライアントからドメイン間の接続](#slcrossdomain)
- [接続を構成する方法](#configureconnection)

    - [WPF クライアントに同時接続の最大数を設定する方法](#maxconnections)
    - [クエリ文字列パラメーターを指定する方法](#querystring)
    - [転送方法を指定する方法](#transport)
    - [HTTP ヘッダーを指定する方法](#httpheaders)
    - [クライアント証明書を指定する方法](#clientcertificate)
- [ハブ プロキシを作成する方法](#proxy)
- [サーバーが呼び出すことができるクライアント上のメソッドを定義する方法](#callclient)

    - [パラメーターのないメソッド](#clientmethodswithoutparms)
    - [パラメーターの型を指定するパラメーターを持つメソッド](#clientmethodswithparmtypes)
    - [動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド](#clientmethodswithdynamparms)
    - [ハンドラーを削除する方法](#removehandler)
- [クライアントからサーバーのメソッドを呼び出す方法](#callserver)
- [接続の有効期間イベントを処理する方法](#connectionlifetime)
- [エラーを処理する方法](#handleerrors)
- [クライアント側のログ記録を有効にする方法](#logging)
- [WPF、Silverlight、および、サーバーを呼び出すことができるクライアント メソッドのコンソール アプリケーションのコード サンプル](#wpfsl)

サンプル .NET クライアント プロジェクトでは、次のリソースを参照してください。

- [gustavo armenta/SignalR サンプル](https://github.com/gustavo-armenta/SignalR-Samples)GitHub.com (WinRT、Silverlight、コンソール アプリケーション例) にします。
- [DamianEdwards/SignalR MoveShapeDemo/MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) GitHub.com (WPF など) にします。
- [SignalR/Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) GitHub.com (コンソール アプリなど) にします。

サーバーまたは JavaScript クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。

- [SignalR ハブ API ガイド - サーバー](hubs-api-guide-server.md)
- [SignalR ハブ API ガイド - JavaScript クライアント](hubs-api-guide-javascript-client.md)

API リファレンス トピックへのリンクは、.NET 4.5 のバージョンの API といます。 .NET 4 を使用している場合は、次を参照してください。[の API のトピックは、.NET 4 バージョン](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="clientsetup"></a>

## <a name="client-setup"></a>クライアントのセットアップ

インストール、 [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet パッケージ (されません、 [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)パッケージ)。 このパッケージは、.NET 4 および .NET 4.5 の両方の WinRT、Silverlight、WPF、コンソール アプリケーション、および Windows Phone クライアントをサポートします。

クライアント上にある SignalR のバージョンがサーバー上にあるバージョンと異なる場合は、SignalR は違いに合わせて調整することが多くの場合です。 たとえば、SignalR バージョン 2 を実行しているサーバーはバージョン 2 がインストールされているクライアントと同様にインストールされている 1.1.x を持つクライアントをサポートします。 サーバー上のバージョンと、クライアントのバージョンの違いが大きすぎるか、クライアントがサーバーよりも新しい場合は、SignalR がスローされます、`InvalidOperationException`クライアントが接続を確立しようとした場合に例外が発生します。 エラー メッセージは"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"です。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>接続を確立する方法

作成する必要が接続を確立することができます、前に、`HubConnection`オブジェクトおよびプロキシを作成します。 接続を確立するために呼び出す、`Start`メソッドを`HubConnection`オブジェクト。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> JavaScript クライアントを呼び出す前に、少なくとも 1 つのイベント ハンドラーを登録する必要がある、`Start`接続を確立する方法です。 これは、.NET クライアントの必要はありません。 JavaScript クライアントは、生成されたプロキシ コードが自動的に作成が存在するすべてのハブのプロキシ サーバーで、およびどのハブを指定する方法は、ハンドラーの登録を使用しているつもりのクライアントです。 .NET クライアントのハブ プロキシ手動で作成するため、SignalR のプロキシを作成するすべてのハブを使用することを想定しています。


既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)です。

`Start`メソッドが非同期的に実行します。 接続が確立された後にするまで以降の行のコードを実行しないようにするには、次のように使用します。 `await` 、ASP.NET 4.5 非同期メソッドまたは`.Wait()`同期メソッドにします。 使用しない`.Wait()`WinRT クライアントにします。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Silverlight クライアントからドメイン間の接続

Silverlight クライアントからドメイン間の接続を有効にする方法については、次を参照してください。 [、サービス利用可能なドメインの境界を越えてを行う](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)です。

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>接続を構成する方法

接続を確立する前に、次のオプションのいずれかを指定できます。

- 同時接続数を制限します。
- クエリ文字列パラメーター。
- トランスポート メソッド。
- HTTP ヘッダー。
- クライアント証明書。

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>WPF クライアントに同時接続の最大数を設定する方法

WPF クライアントには、既定値から 2 の同時接続の最大数を増やす必要があります。 推奨値は 10 です。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

詳細については、次を参照してください。 [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)です。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>クエリ文字列パラメーターを指定する方法

クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。 次の例では、クライアント コードでクエリ文字列パラメーターを設定する方法を示します。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>転送方法を指定する方法

接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定するサーバーとネゴシエートします。 使用するトランスポートが既に把握している場合は、このネゴシエーション プロセスをバイパスできます。 転送方法を指定するには、Start メソッドをトランスポート オブジェクトで渡します。 次の例では、クライアント コードで転送方法を指定する方法を示します。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)名前空間には、次のトランスポートを指定に使用できるクラスが含まれています。

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (サーバーとクライアントの両方が .NET 4.5 を使用する場合にのみ使用できます)。
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (クライアントとサーバーの両方でサポートされている最適なトランスポートと自動的に選択します。 これは、既定のトランスポートです。 渡すをこれには、`Start`メソッドは何もで渡されていない場合と同様です)。

ブラウザーでのみ使用されているために、ForeverFrame トランスポートはこの一覧に含まれていません。

サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](hubs-api-guide-server.md#contextproperty)です。 トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)です。

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>HTTP ヘッダーを指定する方法

HTTP ヘッダーを設定するには、使用、`Headers`接続オブジェクトのプロパティです。 次の例では、HTTP ヘッダーを追加する方法を示します。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>クライアント証明書を指定する方法

クライアント証明書を追加するには、使用、`AddClientCertificate`接続オブジェクトのメソッドです。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>ハブ プロキシを作成する方法

ハブは、サーバーから呼び出すことができるクライアントのメソッドを定義するのには、サーバー側ハブのメソッドを呼び出すには、呼び出すことによって、ハブのプロキシを作成`CreateHubProxy`接続オブジェクトにします。 文字列に渡す`CreateHubProxy`が、クラスの名前、ハブ、または指定した名前、`HubName`場合は、サーバーで使用されていた 1 つの属性です。 名前の一致が区別されません。

**サーバーの hub クラス**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**Hub クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

使用してハブ クラスを装飾する場合、`HubName`属性、その名前を使用します。

**サーバーの hub クラス**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

**Hub クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

呼び出す場合`HubConnection.CreateHubProxy`で複数回、同じ`hubName`、同じキャッシュ`IHubProxy`オブジェクト。

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアント上のメソッドを定義する方法

サーバーが呼び出すことができるメソッドを定義するには、プロキシを使用`On`メソッドをイベント ハンドラーを登録します。

メソッド名に一致する小文字が区別されません。 たとえば、`Clients.All.UpdateStockPrice`サーバーで実行`updateStockPrice`、 `updatestockprice`、または`UpdateStockPrice`クライアントにします。

別のクライアント プラットフォームには、UI を更新するメソッドのコードを記述する方法のさまざまな要件があります。 WinRT (Windows ストア .NET) クライアントの例を示します。 WPF、Silverlight、およびコンソール アプリケーションの例がで提供される[このトピックの後半の個別のセクション](#wpfsl)です。

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>パラメーターのないメソッド

処理しているメソッドがパラメーターを持たない場合は、非ジェネリック オーバー ロードを使用して、`On`メソッド。

**パラメーターなしのクライアント メソッドを呼び出すとサーバー コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**パラメーターなしのサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>パラメーターの型を指定するパラメーターを持つメソッド

処理しているメソッドは、パラメーターを持ち場合のジェネリック型としてパラメーターの種類の指定、`On`メソッドです。 ジェネリック オーバー ロードがあります、`On`メソッドを使用すると、最大 8 個のパラメーター (Windows Phone 7 で 4) を指定します。 次の例では、1 つのパラメーターに送信され、`UpdateStockPrice`メソッドです。

**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**パラメーターに使用される Stock クラス**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

**パラメーターを持つサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド

ジェネリック型としてパラメーターを指定する代わりに、`On`メソッドを動的オブジェクトとしてパラメーターを指定することができます。

**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**パラメーターに使用される Stock クラス**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用の WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照して](#wpfsl))**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>ハンドラーを削除する方法

削除するハンドラーを呼び出してその`Dispose`メソッドです。

**サーバーからメソッド用のクライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**クライアント コードで、ハンドラーを削除するには**

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>クライアントからサーバーのメソッドを呼び出す方法

サーバー上のメソッドを呼び出すを使用して、`Invoke`ハブ プロキシのメソッドです。

サーバー メソッドが戻り値を持たない場合は、非ジェネリック オーバー ロードを使用して、`Invoke`メソッドです。

**メソッドの戻り値がないサーバー コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**戻り値を持たないメソッドを呼び出すクライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

サーバー メソッドの戻り値の場合は、戻り値の型を指定のジェネリック型として、`Invoke`メソッドです。

**サーバー コードを戻り値を持つ複合型パラメーターを受け取るメソッド**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**パラメーターと戻り値に使用される Stock クラス**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

**戻り値を持ち、ASP.NET 4.5 非同期メソッドでは、複合型パラメーターを受け取るメソッドを呼び出すクライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**戻り値を同期メソッドの複合型パラメーターを受け取るメソッドを呼び出すクライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke`メソッドが非同期的に実行し、返します、`Task`オブジェクト。 指定しない場合は`await`または`.Wait()`、呼び出すメソッドの実行が完了する前に、次のコード行は実行されます。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>接続の有効期間イベントを処理する方法

SignalR では、有効期間イベントを処理することができますを次の接続を提供します。

- `Received`: 接続ですべてのデータが受信したときに発生します。 受信したデータを提供します。
- `ConnectionSlow`: クライアントが低速または頻繁に削除の接続を検出したときに発生します。
- `Reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。
- `Reconnected`: 基になるトランスポートが再接続されたときに発生します。
- `StateChanged`: 接続の状態が変更されたときに発生します。 以前の状態と新しい状態を提供します。 接続状態の値について[表す ConnectionState 列挙](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)です。
- `Closed`: 接続を切断したときに発生します。

たとえば、致命的ではありませんが、断続的な接続の問題が発生したエラーを示す警告メッセージを表示する場合は、パフォーマンスの低下や頻繁に行われるなど、接続の削除処理、`ConnectionSlow`イベント。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](handling-connection-lifetime-events.md)です。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>エラーを処理する方法

場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、エラーが発生した SignalR を返す例外オブジェクトには、エラーについての最小限の情報が含まれています。 たとえばへの呼び出し`newContosoChatMessage`失敗した場合、エラー オブジェクト内のエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを実稼働環境でクライアントには推奨されませんトラブルシューティングの目的では、サーバーで次のコードを使用します。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

SignalR が発生したエラーを処理するためのハンドラーを追加できる、`Error`接続オブジェクトのイベントです。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

メソッドの呼び出しからのエラーを処理するには、try catch ブロックでコードをラップします。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>クライアント側のログ記録を有効にする方法

クライアント側のログ記録を有効にするには設定、`TraceLevel`と`TraceWriter`接続オブジェクトのプロパティです。

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF、Silverlight、および、サーバーを呼び出すことができるクライアント メソッドのコンソール アプリケーションのコード サンプル

サーバーが呼び出すことができるクライアント メソッドを定義する前に示したコード サンプルは、WinRT クライアントに適用されます。 次のサンプルでは、WPF、Silverlight、およびクライアントのコンソール アプリケーションのコードは同等のコードを表示します。

### <a name="methods-without-parameters"></a>パラメーターのないメソッド

**パラメーターなしのサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**パラメーターなしのサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**パラメーターなしのサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>パラメーターの型を指定するパラメーターを持つメソッド

**パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**パラメーターを持つサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**パラメーターを持つサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>動的オブジェクトのパラメーターを指定するパラメーターを持つメソッド

**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用の Silverlight クライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**パラメーターの動的なオブジェクトを使用して、パラメーターを持つサーバーからメソッド用のコンソール アプリケーションのクライアント コードが呼び出されます**

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
