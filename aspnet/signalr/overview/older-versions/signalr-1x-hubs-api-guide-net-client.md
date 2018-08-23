---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このドキュメントでは、Windows ストア (WinRT)、WPF、Silverlight、および短所など、.NET クライアントでは、バージョン 2 の SignalR のハブ API を使用して紹介しています.
ms.author: riande
ms.date: 04/17/2013
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 5889429645ea1c682ea43c4b17afb3745318e32d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837120"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a>ASP.NET SignalR ハブ API ガイド - .NET クライアント (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

> このドキュメントでは、SignalR など、Windows ストア (WinRT)、WPF、Silverlight、およびコンソール アプリケーションの .NET クライアントのバージョン 2 の Hubs API の使用の概要を示します。
> 
> SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 SignalR は、のすべてのクライアントとサーバーが処理されます。
> 
> SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。 概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](../getting-started/index.md)します。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [クライアントのセットアップ](#clientsetup)
- [接続を確立する方法](#establishconnection)

    - [Silverlight クライアントからドメイン間の接続](#slcrossdomain)
- [接続を構成する方法](#configureconnection)

    - [WPF クライアントでの同時接続の最大数を設定する方法](#maxconnections)
    - [クエリ文字列パラメーターを指定する方法](#querystring)
    - [トランスポート メソッドを指定する方法](#transport)
    - [HTTP ヘッダーを指定する方法](#httpheaders)
    - [クライアント証明書を指定する方法](#clientcertificate)
- [ハブ プロキシを作成する方法](#proxy)
- [サーバーが呼び出すことができるクライアントでメソッドを定義する方法](#callclient)

    - [パラメーターなしのメソッド](#clientmethodswithoutparms)
    - [パラメーターの型を指定するパラメーターを持つメソッド](#clientmethodswithparmtypes)
    - [パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド](#clientmethodswithdynamparms)
    - [ハンドラーを削除する方法](#removehandler)
- [クライアントからサーバーのメソッドを呼び出す方法](#callserver)
- [接続の有効期間イベントを処理する方法](#connectionlifetime)
- [エラーを処理する方法](#handleerrors)
- [クライアント側のログ記録を有効にする方法](#logging)
- [WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド](#wpfsl)

サンプル .NET クライアント プロジェクトでは、次のリソースを参照してください。

- [gustavo armenta/SignalR サンプル](https://github.com/gustavo-armenta/SignalR-Samples)github.com (WinRT、Silverlight、コンソール アプリの例)。
- [DamianEdwards/SignalR MoveShapeDemo/MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) github.com (WPF など)。
- [SignalR/Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) github.com (コンソール アプリなど)。

サーバーや JavaScript クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。

- [SignalR ハブ API ガイド - サーバー](../guide-to-the-api/hubs-api-guide-server.md)
- [SignalR ハブ API ガイド - JavaScript クライアント](../guide-to-the-api/hubs-api-guide-javascript-client.md)

.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。 .NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="clientsetup"></a>

## <a name="client-setup"></a>クライアントのセットアップ

インストール、 [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet パッケージ (いない、 [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr)パッケージ)。 このパッケージは、.NET 4 および .NET 4.5 の両方の WinRT、Silverlight、WPF、コンソール アプリケーション、および Windows Phone のクライアントをサポートします。

SignalR が存在するクライアントのバージョンがサーバー上にあるバージョンと異なる場合は、SignalR は、違いに適応することが多くの場合。 たとえば、SignalR バージョン 2.0 がリリースされ、サーバーをインストールする、サーバーは 1.1.x および 2.0 がインストールされているクライアントをインストールしているクライアントをサポートします。 サーバー上のバージョンと、クライアントのバージョンの間の差が大きすぎる場合は、SignalR がスローされます、`InvalidOperationException`クライアントが接続を確立しようとした場合に例外が発生します。 エラー メッセージは"`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`"。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>接続を確立する方法

作成する必要が接続を確立する前に、`HubConnection`オブジェクトし、プロキシを作成します。 接続を確立するために呼び出す、`Start`メソッドを`HubConnection`オブジェクト。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> JavaScript クライアントには、呼び出す前に少なくとも 1 つのイベント ハンドラーを登録する必要が、`Start`メソッドは、接続を確立します。 これは、.NET クライアントの必要はありません。 JavaScript クライアントは、生成されたプロキシ コードに自動的に存在するすべてのハブ プロキシをサーバーを作成、およびハンドラーの登録は、どのハブを指定する方法、クライアントが使用します。 .NET クライアントのハブ プロキシ手動で作成するため、SignalR のプロキシを作成するのいずれかのハブを使用することを想定しています。


サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。

`Start`メソッドを非同期的に実行します。 後続行のコードは、接続が確立された後まで実行されないようにするには、次のように使用します。 `await` ASP.NET 4.5 の非同期メソッドまたは`.Wait()`同期メソッドにします。 使用しない`.Wait()`WinRT クライアント。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

`HubConnection` クラスはスレッド セーフです。

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a>Silverlight クライアントからドメイン間の接続

Silverlight クライアントからドメイン間の接続を有効にする方法については、次を参照してください。[使用可能なドメインの境界を越えてサービスを行う](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx)します。

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>接続を構成する方法

接続を確立する前に、次のオプションのいずれかを指定できます。

- 同時接続数を制限します。
- クエリ文字列パラメーター。
- トランスポート メソッド。
- HTTP ヘッダー。
- クライアント証明書。

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a>WPF クライアントでの同時接続の最大数を設定する方法

、WPF のクライアントでは、2 の既定値からの同時接続の最大数を増やす必要があります。 推奨値には 10 です。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

詳細については、次を参照してください。 [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx)します。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>クエリ文字列パラメーターを指定する方法

クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。 次の例では、クライアント コードで、クエリ文字列パラメーターを設定する方法を示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>トランスポート メソッドを指定する方法

接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定する、サーバーとネゴシエートします。 使用するトランスポートを既に知っている場合は、このネゴシエーション プロセスをバイパスできます。 トランスポートの方法を指定するには、Start メソッドにトランスポート オブジェクトで渡します。 次の例では、クライアント コードでトランスポート メソッドを指定する方法を示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx)名前空間には、トランスポートを指定する際、次のクラスが含まれています。

- [LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)
- [ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)
- [WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (サーバーとクライアントの両方が .NET 4.5 を使用した場合にのみ表示されます)。
- [AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (クライアントとサーバーの両方でサポートされている最適なトランスポートを自動的に選択します。 これは、既定のトランスポートです。 渡すをこれには、`Start`メソッドが何もで渡されていないのと同じ効果です)。

ブラウザーでのみ使用されているために、ForeverFrame トランスポートはこの一覧に含まれていません。

サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)します。 トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)します。

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a>HTTP ヘッダーを指定する方法

HTTP ヘッダーを設定するには、使用、`Headers`接続オブジェクトのプロパティ。 次の例では、HTTP ヘッダーを追加する方法を示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a>クライアント証明書を指定する方法

クライアント証明書を追加するには、使用、`AddClientCertificate`接続オブジェクトのメソッド。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a>ハブ プロキシを作成する方法

ハブは、サーバーから呼び出すことができるクライアントでメソッドを定義するとで、サーバーでのハブ メソッドを呼び出すには、呼び出すことによって、ハブのプロキシの作成`CreateHubProxy`接続オブジェクトにします。 文字列に渡す`CreateHubProxy`は、ハブ クラスの名前またはで指定された名前、`HubName`属性が 1 つは、サーバーで使用されていた場合。 大文字と小文字が一致する名前です。

**サーバー上のハブ クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

**ハブ クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

使用してハブ クラスを修飾する場合、`HubName`属性、その名前を使用します。

**サーバー上のハブ クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

**ハブ クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

プロキシ オブジェクトは、スレッド セーフです。 実際には、呼び出した場合`HubConnection.CreateHubProxy`で複数回、同じ`hubName`、キャッシュされた同じ`IHubProxy`オブジェクト。

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアントでメソッドを定義する方法

サーバーが呼び出すことができるメソッドを定義するプロキシを使用して、`On`イベント ハンドラーを登録するメソッド。

大文字と小文字が一致するメソッドの名前です。 たとえば、`Clients.All.UpdateStockPrice`サーバーで実行`updateStockPrice`、 `updatestockprice`、または`UpdateStockPrice`クライアント。

別のクライアント プラットフォームでは、UI を更新するメソッドのコードを記述する方法のさまざまな要件があります。 WinRT (Windows ストア .NET) クライアントの表示例を示します。 WPF、Silverlight、およびコンソール アプリケーションの例がで提供される[別のセクションでは、このトピックで後述](#wpfsl)します。

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a>パラメーターなしのメソッド

処理しているメソッドにパラメーターがあるない場合は、非ジェネリック オーバー ロードを使用して、`On`メソッド。

**パラメーターなしのクライアント メソッドを呼び出すサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

**パラメーターなしのサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>パラメーターの型を指定するパラメーターを持つメソッド

処理しているメソッドにパラメーターがある場合は、パラメーターの型を指定のジェネリック型として、`On`メソッド。 ジェネリック オーバー ロードがある、`On`メソッドを使用すると、最大 8 個のパラメーター (Windows Phone 7 では 4) を指定します。 次の例に 1 つのパラメーターを送信、`UpdateStockPrice`メソッド。

**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

**パラメーターに使用される Stock クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

**パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド

ジェネリック型としてパラメーターを指定する代わりに、`On`メソッドでは、動的オブジェクトとしてパラメーターを指定することができます。

**サーバー コードのパラメーターを持つクライアント メソッドを呼び出す**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

**パラメーターに使用される Stock クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドの WinRT クライアント コードが呼び出されます ([WPF および Silverlight の例をこのトピックの後半を参照してください](#wpfsl))。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a>ハンドラーを削除する方法

ハンドラーを削除するには、呼び出すその`Dispose`メソッド。

**サーバーから呼び出されるメソッドのクライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

**クライアントのコード ハンドラーを削除するには**

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>クライアントからサーバーのメソッドを呼び出す方法

サーバー上のメソッドを呼び出すを使用して、`Invoke`ハブ プロキシのメソッド。

サーバー メソッドが戻り値を持たない場合は、非ジェネリック オーバー ロードを使用して、`Invoke`メソッド。

**サーバー コードのメソッドの戻り値がありません。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

**戻り値を持たないメソッドを呼び出すクライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

サーバー メソッドの戻り値の場合は、戻り値の型を指定のジェネリック型として、`Invoke`メソッド。

**サーバー コードのメソッドの戻り値があり、複雑な型パラメーターを受け取る**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

**パラメーターと戻り値に使用される Stock クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

**ASP.NET 4.5 の非同期メソッドで複合型のパラメーターを受け取って戻り値を持つメソッドを呼び出すクライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

**戻り値を同期メソッドで複合型のパラメーターを受け取るメソッドを呼び出すクライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

`Invoke`メソッドが非同期で実行し、返します、`Task`オブジェクト。 指定しない場合は`await`または`.Wait()`、呼び出すメソッドの実行が完了する前に次のコード行が実行されます。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>接続の有効期間イベントを処理する方法

SignalR は、次の接続に処理できる有効期間イベントを提供します。

- `Received`: 接続でのデータが受信したときに発生します。 受信したデータを提供します。
- `ConnectionSlow`: クライアントが低速または削除が頻繁に接続を検出したときに発生します。
- `Reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。
- `Reconnected`: 基になるトランスポートが再接続されたときに発生します。
- `StateChanged`: 接続状態が変更されたときに発生します。 以前の状態と新しい状態を提供します。 接続に関する情報の状態の値を参照してください[ConnectionState 列挙](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx)します。
- `Closed`: 接続が切断されたときに発生します。

たとえば、致命的ではありませんが、断続的な接続の問題が発生するエラーを警告メッセージを表示する場合は、パフォーマンスの低下や頻繁になど、接続の削除を処理、`ConnectionSlow`イベント。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](../guide-to-the-api/handling-connection-lifetime-events.md)します。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>エラーを処理する方法

場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、SignalR が返すエラーが発生した例外オブジェクトには、エラーに関する最小限の情報が含まれています。 呼び出しなど`newContosoChatMessage`失敗した場合、エラー オブジェクトにエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを運用環境でクライアントには推奨されませんトラブルシューティングのため、サーバーで次のコードを使用します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

SignalR が発生したエラーを処理するためのハンドラーを追加できる、`Error`接続オブジェクトのイベント。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

メソッドの呼び出しからエラーを処理するには、try-catch ブロックでコードをラップします。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>クライアント側のログ記録を有効にする方法

クライアント側のログ記録を有効にするには設定、`TraceLevel`と`TraceWriter`接続オブジェクトのプロパティ。

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a>WPF、Silverlight、およびコンソール アプリケーションのコード サンプル、サーバーが呼び出すことができるクライアント メソッド

サーバーが呼び出すことができるクライアント メソッドを定義する前に示したコード サンプルは、WinRT クライアントに適用されます。 次のサンプルでは、WPF、Silverlight、およびコンソール アプリケーションのクライアントの同等のコードを表示します。

### <a name="methods-without-parameters"></a>パラメーターなしのメソッド

**パラメーターなしのサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

**パラメーターなしのサーバーから呼び出されるメソッドの Silverlight クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

**メソッドのコンソール アプリケーションのクライアント コードは、パラメーターなしのサーバーから呼び出されます**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a>パラメーターの型を指定するパラメーターを持つメソッド

**パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

**パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

**パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a>パラメーターの動的オブジェクトを指定するパラメーターを持つメソッド

**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの WPF クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーから呼び出されるメソッドの Silverlight クライアント コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

**パラメーターの動的オブジェクトを使用して、パラメーターを持つサーバーからメソッドのコンソール アプリケーションのクライアント コードが呼び出されます**

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
