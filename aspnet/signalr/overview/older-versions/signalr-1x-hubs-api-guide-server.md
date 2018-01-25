---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "このドキュメントでは、SignalR バージョン 1.1 では、コード サンプル demonstratin と for ASP.NET SignalR ハブ API のサーバー側のプログラミングを紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: 96155b1c648e5f6092b3ba67a560197f86a593b9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

> このドキュメントでは、バージョン 1.1 では、SignalR の一般的なオプションを示すコード サンプルでの ASP.NET SignalR ハブ API のサーバー側のプログラミングの概要を示します。
> 
> SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。
> 
> SignalR では、永続的な接続と呼ばれる、下位の API も提供します。 SignalR、ハブ、および永続的な接続の概要または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照して[SignalR - はじめ](index.md)です。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [SignalR のルートを登録し、SignalR のオプションを構成する方法](#route)

    - [/Signalr URL](#signalrurl)
    - [SignalR のオプションを構成します。](#options)
- [作成し、ハブ クラスを使用する方法](#hubclass)

    - [ハブ オブジェクトの有効期間](#transience)
    - [JavaScript クライアントにハブ名の camel 形式の大文字と小文字](#hubnames)
    - [複数のハブ](#multiplehubs)
- [クライアントが呼び出すことができる、ハブ クラスでメソッドを定義する方法](#hubmethods)

    - [JavaScript クライアントでメソッド名の camel 形式の大文字と小文字](#methodnames)
    - [非同期的に実行するタイミング](#asyncmethods)
    - [オーバー ロードを定義します。](#overloads)
- [クライアントに、ハブ クラスからのメソッドを呼び出す方法](#callfromhub)

    - [どのクライアントを選択すると、RPC が表示されます。](#selectingclients)
    - [メソッド名のないコンパイル時の検証](#dynamicmethodnames)
    - [大文字と小文字のメソッドの名前の一致](#caseinsensitive)
    - [非同期の実行](#asyncclient)
- [ハブ クラスからグループのメンバーシップを管理する方法](#groupsfromhub)

    - [Add および Remove メソッドの非同期実行](#asyncgroupmethods)
    - [グループ メンバーシップの永続化](#grouppersistence)
    - [シングル ユーザー グループ](#singleusergroups)
- [ハブ クラスでの接続の有効期間のイベントを処理する方法](#connectionlifetime)

    - [OnConnected、OnDisconnected、および OnReconnected が呼び出されるタイミング](#onreconnected)
    - [呼び出し元の状態値を返さない](#nocallerstate)
- [コンテキスト プロパティからのクライアントに関する情報を取得する方法](#contextproperty)
- [クライアントと、ハブ クラス間で状態を渡す方法](#passstate)
- [ハブ クラスのエラーを処理する方法](#handleErrors)
- [クライアントのメソッドを呼び出すし、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)

    - [クライアントのメソッドを呼び出す](#callingclientsoutsidehub)
    - [グループ メンバーシップを管理します。](#managinggroupsoutsidehub)
- [トレースを有効にする方法](#tracing)
- [ハブ パイプラインをカスタマイズする方法](#hubpipeline)

クライアントのプログラム方法については、次のリソースを参照してください。

- [SignalR ハブ API ガイド - JavaScript クライアント](index.md)
- [SignalR ハブ API ガイド - .NET クライアント](index.md)

API リファレンス トピックへのリンクは、.NET 4.5 のバージョンの API といます。 .NET 4 を使用している場合は、次を参照してください。[の API のトピックは、.NET 4 バージョン](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>SignalR のルートを登録し、SignalR のオプションを構成する方法

クライアントがハブへの接続に使用するルートを定義するには、呼び出し、 [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)メソッド アプリケーションの起動時にします。 `MapHubs`[拡張メソッド](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)の`System.Web.Routing.RouteCollection`クラスです。 次の例の SignalR ハブ ルートを定義する方法を示しています、 *Global.asax*ファイル。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

ASP.NET MVC アプリケーションに SignalR 機能を追加する場合は、SignalR のルートが他のルートの前に追加されたことを確認します。 詳細については、次を参照してください。[チュートリアル: SignalR と MVC 4 の概要](index.md)です。

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL

クライアントがハブに接続に使用するルートの URL は、既定では、"/signalr"です。 (これは、自動的に生成された JavaScript ファイルの「/signalr ハブ」URL では、この URL を混同しないでください。 生成されたプロキシの詳細については、次を参照してください[SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとそれが何を](index.md)。)。

SignalR; のこのベース URL を使用できないようにする特殊な状況である可能性があります。たとえば、同じ名前のプロジェクトのフォルダーがある*signalr*名前を変更するとします。 次の例に示すように、ベース URL を変更する場合は、(交換"/signalr"を目的の URL のサンプル コードで)。

**サーバー コード、URL を指定します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

**JavaScript クライアント コード (生成されたプロキシ) を使用して URL を指定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

**JavaScript クライアント コード (せずに、生成されたプロキシ) URL を指定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

**.NET クライアント コード、URL を指定します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>SignalR のオプションを構成します。

オーバー ロードが、`MapHubs`メソッドを使用すると、カスタムの URL、カスタムの依存関係競合回避モジュール、および、次のオプションを指定します。

- ブラウザー クライアントからドメイン間の呼び出しを有効にします。

    通常、ブラウザーからページを読み込む場合`http://contoso.com`、SignalR 接続は、同じドメインに`http://contoso.com/signalr`です。 場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。 セキュリティ上の理由から、ドメイン間の接続は、既定で無効にします。 詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - JavaScript クライアントのドメイン間の接続を確立する方法](index.md)です。
- 詳細なエラー メッセージを有効にします。

    エラーが発生すると、SignalR の既定の動作の変更点に関する詳細情報なしの通知メッセージをクライアントに送信を開始します。 詳細なエラー情報をクライアントに送信されてために推奨されません、実稼働環境で悪意のあるユーザーは、アプリケーションに対する攻撃の情報を使用できる場合があります。 トラブルシューティングについては、一時的にもっとわかりやすいエラー報告を有効にするのには、このオプションを使用できます。
- 自動的に生成された JavaScript プロキシ ファイルを無効にします。

    既定では、プロキシの Hub クラス用の JavaScript ファイルが「/signalr ハブ」URL への応答で生成されます。 JavaScript プロキシを使用しない場合、またはをこのファイルを手動で生成し、クライアントの物理ファイルを参照する場合は、プロキシの生成を無効にするのには、このオプションを使用できます。 詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントの SignalR の物理ファイルを作成する方法については、プロキシを生成](index.md)です。

次の例への呼び出しで、URL の SignalR 接続とこれらのオプションを指定する方法を示しています、`MapHubs`メソッドです。 カスタムの URL を指定するには、次のように置換します。"/signalr"を使用する URL の例です。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>作成し、ハブ クラスを使用する方法

ハブを作成するから派生するクラスを作成[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)です。 次の例では、チャット アプリケーション用の単純なハブ クラスを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

この例では、接続されたクライアントが呼び出すことができます、`NewContosoChatMessage`メソッド、受信したデータが接続されているすべてのクライアントにブロードキャストするときとします。

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>ハブ オブジェクトの有効期間

ハブ クラスのインスタンスを作成したり、サーバーで、独自のコードからメソッドを呼び出しますしません。すべてが完了するの SignalR ハブ パイプラインによってです。 SignalR は、ハブなどの操作とクライアント接続、切断されると、により、サーバーに呼び出すメソッドを処理する必要があるたびに、ハブ クラスの新しいインスタンスを作成します。

ハブ クラスのインスタンスは、一時的なものであるためには 1 つのメソッド呼び出しからの状態を維持するために使用することはできません。 たびに、サーバー メッセージを受信メソッド呼び出し、クライアントでは、ハブ クラスのプロセスの新しいインスタンス。 状態を保持する複数の接続とメソッドの呼び出しを通じて、ハブ クラス、または別のクラスから派生していないことに、データベース、または静的変数などの他の方法を使用して`Hub`です。 メモリ内のデータを保存する場合、ハブ クラスに静的変数などのメソッドを使用して、データは失われますアプリケーション ドメインがリサイクルされるときにします。

ハブ クラス外部で実行されるコードからクライアントにメッセージを送信する場合は、ハブ クラスのインスタンスをインスタンス化して行うことはできませんが、ハブ クラスの SignalR コンテキスト オブジェクトへの参照を取得することによって行うことができます。 詳細については、次を参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)このトピックで後述します。

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>JavaScript クライアントにハブ名の camel 形式の大文字と小文字

既定では、JavaScript クライアントはクラス名の camel 形式のバージョンを使用してハブを参照します。 SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。 前の例と呼ばれる`contosoChatHub`JavaScript コードにします。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

クライアントを使用して、追加する別の名前を指定するかどうか、`HubName`属性。 使用すると、`HubName`属性は、JavaScript クライアントでのキャメル ケースに名前の変更はありません。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>複数のハブ

アプリケーションでは、複数の Hub クラスを定義できます。 操作を実行すると、接続の共有が、グループは別。

- すべてのクライアントは、サービスとの SignalR 接続を確立するために、同じ URL を使用 ("/signalr"またはいずれかを指定した場合、カスタムの URL)、すべてのハブの接続を使用して、サービスで定義されています。

    1 つのクラスのすべてのハブ機能の定義と比較して複数のハブのパフォーマンスの違いはありません。
- すべてのハブでは、同じ HTTP 要求の情報を取得します。

    すべてのハブが同じ接続を共有するため、サーバーを取得する唯一の HTTP 要求情報は、SignalR 接続を確立する元の HTTP 要求で行うことです。 クエリ文字列を指定することによって、サーバーにクライアントから情報を渡すため、接続要求を使用する場合は、さまざまなハブに別のクエリ文字列を提供できません。 すべてのハブでは、同じ情報を受信します。
- 生成された JavaScript プロキシ ファイルは、1 つのファイルですべてのハブ プロキシが含まれます。

    JavaScript プロキシの詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとそれが何を](index.md)です。
- グループは、ハブ内で定義されます。

    SignalR を定義することで接続しているクライアントのサブセットにブロードキャストするグループの名前。 グループは、各ハブを別々 に管理されます。 たとえば、"Administrators"をという名前のグループは 1 つのクライアントのセットを追加、`ContosoChatHub`クラス、および同じグループ名が別のクライアントのセットを参照してください、`StockTickerHub`クラスです。

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>クライアントが呼び出すことができる、ハブ クラスでメソッドを定義する方法

クライアントから呼び出せるようにするハブのメソッドを公開するには、次の例に示すように、パブリック メソッドを宣言します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

戻り値の型と複合型アレイなど、任意の c# のメソッドの場合と、パラメーターを指定することができます。 パラメーターで受信するか、呼び出し元に返すデータは、JSON を使用して、クライアントとサーバー間でやり取りされ、SignalR は複雑なオブジェクトのバインドとオブジェクトの配列を自動的に処理します。

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>JavaScript クライアントでメソッド名の camel 形式の大文字と小文字

既定では、JavaScript クライアントは、メソッド名の camel 形式のバージョンを使用してハブ メソッドを参照します。 SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

クライアントを使用して、追加する別の名前を指定するかどうか、`HubMethodName`属性。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>非同期的に実行するタイミング

メソッドがある実行時間の長いまたは作業を実行するにする場合はデータベースの参照など、web サービス呼び出しの待機を伴うを返すことによってのハブ メソッドを非同期に作成、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(の代わりに`void`を返す) または[タスク&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)オブジェクト (の代わりに`T`型を返す)。 戻るとき、`Task`メソッド、SignalR からオブジェクトを待って、`Task`完了するにはし、そのラップ解除された結果、クライアントに戻すため、クライアントのメソッド呼び出しをコーディングする方法の違いはありません。

ハブ メソッド非同期回避 WebSocket トランスポートを使用する場合は、接続をブロックします。 ハブ メソッドが同期的に実行すると、トランスポートは、WebSocket、ハブのメソッドが完了するまで、同じクライアントからハブのメソッドの後続の呼び出しはブロックされます。

例を次に、同じメソッドが同期的に実行するようにコーディングまたはいずれかのバージョンを呼び出すための動作をクライアント コードを JavaScript の後に、非同期的にします。

**同期**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**Asynchronous - ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

ASP.NET 4.5 での非同期メソッドを使用する方法の詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)です。

<a id="overloads"></a>

### <a name="defining-overloads"></a>オーバー ロードを定義します。

メソッドのオーバー ロードを定義する場合は、各オーバー ロードのパラメーターの数が異なる場合があります。 別のパラメーターの型を指定するだけでオーバー ロードを区別する場合は、ハブ クラスがコンパイルされるが、SignalR サービス呼び出しのオーバー ロードのいずれかをクライアントのとき、実行時に例外がスローされます。

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>クライアントに、ハブ クラスからのメソッドを呼び出す方法

サーバーからクライアント メソッドを呼び出すには、使用、`Clients`ハブ クラスにメソッドのプロパティです。 次の例では、サーバー コードを呼び出す`addNewMessageToPage`接続されているすべてのクライアント、および JavaScript クライアントで、メソッドを定義するクライアント コードにします。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

クライアント メソッドから戻り値を取得することはできません。などの構文`int x = Clients.All.add(1,1)`は機能しません。

複合型およびパラメーターの配列を指定することができます。 次の例では、複合型をメソッド パラメーターに、クライアントに渡します。

**複雑なオブジェクトを使用してクライアントのメソッドを呼び出すサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>どのクライアントを選択すると、RPC が表示されます。

クライアントのプロパティから返される、 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) RPC を受信するクライアントを指定するためのいくつかのオプションを提供するオブジェクト。

- 接続されているすべてのクライアントです。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- 呼び出し元のクライアントのみです。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- 呼び出し元のクライアントを除くすべてのクライアントです。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- 接続 ID で識別される特定のクライアント

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    この例では`addContosoChatMessageToPage`呼び出し元のクライアントで使用すると同じ効果があり`Clients.Caller`です。
- 接続 ID で識別される、指定されたクライアントを除くすべての接続されたクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- 指定したグループ内のすべての接続されたクライアント。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- 接続 ID で識別される、指定されたクライアントを除く、指定したグループ内のすべての接続されたクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- 指定されたグループ内のすべての接続されているクライアントは、呼び出し元のクライアントを除きます。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>メソッド名のないコンパイル時の検証

指定したメソッド名は、動的なオブジェクトは、IntelliSense またはのコンパイル時の検証がないことを意味として解釈されます。 式は、実行時に評価されます。 メソッドの呼び出しを実行するときは、メソッドが呼び出される SignalR メソッド名とパラメーターの値をクライアントに送信し、名前に一致する場合は、クライアントは、メソッド、およびパラメーターの値がそれに渡されます。 クライアントで一致するメソッドが見つからない場合、エラーは発生しません。 SignalR はクライアント メソッドを呼び出すと、シーンの背後にあるクライアントに送信するデータの形式については、次を参照してください。 [SignalR の概要](index.md)です。

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>大文字と小文字のメソッドの名前の一致

メソッド名に一致する小文字が区別されません。 たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addcontosochatmessagetopage`、または`addContosoChatMessageToPage`クライアントにします。

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>非同期の実行

呼び出すメソッドは、非同期的に実行されます。 SignalR の後続の行のコードをすることを指定しない限り、クライアントにデータの送信を完了するを待たずクライアントへのメソッド呼び出しはすぐに実行した後に付属しているすべてのコードは、メソッドの完了を待つ必要があります。 次のコード例を次の 2 つのクライアント メソッドを順番に実行する方法を示してを使用してコードの .NET 4.5 で動作する、1 つを使用して .NET 4 での動作をコードします。

**.NET 4.5 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**.NET 4 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

使用する場合`await`または`ContinueWith`クライアント メソッドは、次のコード行が実行される前に完了するまで待機する、必ずしもクライアントが実際にメッセージが表示される次のコード行が実行される前にします。 「完了」のクライアント メソッド呼び出しのでは、SignalR がすべてのメッセージを送信するために必要な終了、のみを意味します。 クライアントがメッセージを受信することの確認を必要がある場合は、自分でこのメカニズムをプログラミングする必要があります。 コードでしたなど、`MessageReceived`メソッドと、ハブで、`addContosoChatMessageToPage`メソッドを呼び出すことも、クライアントを`MessageReceived`操作を行った後のクライアントで行う作業する必要です。 `MessageReceived`ハブでどのような作業は、実際のクライアントの受信と、元のメソッド呼び出しの処理によって異なります。 を行うことができます。

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>メソッドの名前として文字列変数を使用する方法

キャスト メソッドの名前として文字列変数を使用してクライアント メソッドを呼び出す場合`Clients.All`(または`Clients.Others`、`Clients.Caller`など) に`IClientProxy`し[Invoke (methodName、args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>ハブ クラスからグループのメンバーシップを管理する方法

SignalR でグループは、接続しているクライアントの指定されたサブセットにメッセージをブロードキャストする方法を提供します。 グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。

グループ メンバーシップを管理するを使用して、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)と[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)によって提供されるメソッド、`Groups`ハブ クラスのプロパティです。 次の例は、`Groups.Add`と`Groups.Remove`これらを呼び出す JavaScript クライアント コードでクライアント コードによって呼び出されるハブ メソッドで使用されるメソッドの後にします。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**生成されたプロキシを使用して、JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

グループを明示的に作成する必要はありません。 有効で、グループが初めてへの呼び出しでその名前を指定する自動的に作成`Groups.Add`、内のメンバーシップから最後の接続を削除するときに削除されます。

グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。 SignalR クライアントおよびグループを基にメッセージを送信する、[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、し、サーバーがグループまたはグループ メンバーシップの一覧を管理しません。 これにより、スケーラビリティを最大化 SignalR を保持するいずれかの状態は、新しいノードに反映する必要が web ファームにノードを追加するたびにできます。

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Add および Remove メソッドの非同期実行

`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。 クライアントをグループに追加し、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、ことを確認する必要が、`Groups.Add`メソッドが最初に終了します。 次のコード例は、表示を行うには、.NET 4.5、および .NET 4 で動作するコードを使用していずれかで動作するコードを使用して 1 つの方法

**.NET 4.5 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**.NET 4 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>グループ メンバーシップの永続化

SignalR 接続を追跡する、接続のように、同じグループにユーザーを確立するたびにユーザーが必要なユーザーではなく、その場合を呼び出す必要がある`Groups.Add`たびに、ユーザーが新しい接続を確立します。

接続の一時的な障害は後、場合もあります SignalR ことができます、接続を自動的に復元します。 その場合は、SignalR は、同じ接続を復元は、新しい接続を確立できませんし、そのため、クライアントのグループ メンバーシップは自動的に回復します。 これには、一時的な中断が、サーバーを再起動または障害の結果が場合でも、グループ メンバーシップを含む、各クライアントの接続状態は、ラウンドト リップをクライアントにあるため。 場合は、サーバーがダウンしては、接続がタイムアウトする前に、新しいサーバーで置き換えられます、クライアントは、新しいサーバーに自動的に再接続し、グループのメンバーであることに再登録します。

接続は、接続が失われた後に自動的に復元できませんまたは接続がタイムアウトしたときにしたりすると、クライアントには、(たとえば、ときに、ブラウザーは、新しいページに移動) が切断されると、グループ メンバーシップは失われます。 次回ユーザーが接続を新しい接続となります。 同じユーザーが新しい接続を確立するときに、グループ メンバーシップを維持アプリケーションに、ユーザーとグループ間の関連付けを追跡し、ユーザーが新しい接続を確立するたびにグループ メンバーシップを復元します。

接続と再接続に関する詳細については、次を参照してください。[ハブ クラスでの接続の有効期間のイベントを処理する方法](#connectionlifetime)このトピックで後述します。

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>シングル ユーザー グループ

通常 SignalR を使用するアプリケーションは、ユーザーはメッセージを送信し、どのユーザーにメッセージを受信する必要がありますを知るためには、ユーザーとの接続間の関連付けの追跡に必要です。 グループは、これを行うため、2 つの一般的に使用されるパターンのいずれかで使用されます。

- シングル ユーザー グループ。

    グループ名とユーザー名を指定し、ユーザーの接続または再接続するたびに、現在の接続 ID をグループに追加できます。 グループに送信するユーザーにメッセージを送信します。 この方法の欠点は、グループしないかどうか、ユーザーはオンラインまたはオフラインを検索する方法に提供です。
- ユーザー名と接続 Id の間の関連付けを追跡します。

    各ユーザー名と 1 つまたは複数の接続 Id の関連付けを辞書またはデータベースに格納し、ユーザーの接続または切断するたびに、格納されたデータを更新できます。 ユーザーにメッセージを送信するには、接続 Id を指定します。 この方法の欠点より多くのメモリを受け取ることです。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>ハブ クラスでの接続の有効期間のイベントを処理する方法

接続の有効期間イベントを処理するための一般的な原因として、または、ユーザーが接続されているかどうかを追跡するユーザー名と接続 Id 間の関連付けを追跡するためです。 クライアントが接続または切断ときは、独自のコードを実行するには、上書き、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`仮想メソッド、ハブのクラスに、次の例に示すようにします。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>OnConnected、OnDisconnected、および OnReconnected が呼び出されるタイミング

ブラウザーが新しいページに移動するたびに、新しい接続が確立され、SignalR を実行することを意味、`OnDisconnected`メソッドに続けて、`OnConnected`メソッドです。 SignalR は、新しい接続が確立されたときに常に新しい接続 ID を作成します。

`OnReconnected`メソッドが呼び出されてがあった一時中断 SignalR がときに、ケーブルが一時的に切断され、接続がタイムアウトする前に再接続などから、回復できるように自動的に接続します。`OnDisconnected`メソッドは、クライアントが切断され、SignalR 自動的に再接続できません、ブラウザーが新しいページに移動する場合などです。 したがって、可能な一連の特定のクライアントのイベントは`OnConnected`、 `OnReconnected`、 `OnDisconnected`; または`OnConnected`、`OnDisconnected`です。 シーケンスは表示されません`OnConnected`、 `OnDisconnected`、`OnReconnected`特定の接続。

`OnDisconnected`サーバーがダウンした場合など、一部のシナリオで呼び出されないメソッドまたはアプリケーション ドメインがリサイクルされるを取得します。 別のサーバーがオンラインまたはアプリケーション ドメインには、そのリサイクルが完了すると、一部のクライアントできる場合がありますに再接続して、起動、`OnReconnected`イベント。

詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](index.md)です。

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>呼び出し元の状態値を返さない

いずれかの状態に格納されていることを意味すると、サーバーからの接続の有効期間のイベント ハンドラー メソッドが呼び出される、`state`で、クライアント上のオブジェクトは設定されません、`Caller`サーバーのプロパティです。 については、`state`オブジェクトおよび`Caller`プロパティを参照してください[クライアントと、ハブ クラス間で状態を渡す方法](#passstate)このトピックで後述します。

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>コンテキスト プロパティからのクライアントに関する情報を取得する方法

クライアントに関する情報を取得する、`Context`ハブ クラスのプロパティです。 `Context`プロパティから返される、 [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)次の情報へのアクセスを提供するオブジェクト。

- 呼び出し元のクライアントの接続 ID。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    接続 ID は、SignalR (値は、独自のコードでは指定できません) によって割り当てられた GUID です。 アプリケーションに複数のハブがある場合、ID がすべてのハブで使用されて、同じ接続し、各接続の 1 つの接続 ID があります。
- HTTP ヘッダーのデータ。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    HTTP ヘッダーを取得することもできます。`Context.Headers`です。 多重参照を同じことには、その理由は`Context.Headers`が最初に、作成された、`Context.Request`プロパティが、後で追加されたと`Context.Headers`旧バージョンとの互換性のために保持されました。
- 文字列データのクエリを実行します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    クエリの文字列データを取得することも`Context.QueryString`します。

    このプロパティで取得するクエリ文字列は、SignalR 接続が確立される HTTP 要求に使用されたものです。 クライアントでのクエリ文字列パラメーターを追加するには、クライアントからサーバーにクライアントに関するデータを渡すための便利な方法であると、接続を構成します。 次の例では、生成されたプロキシを使用すると、JavaScript クライアントで、クエリ文字列を追加する 1 つの方法を示します。

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    クエリ文字列パラメーターの設定に関する詳細については、の API のマニュアルを参照して、 [JavaScript](index.md)と[.NET](index.md)クライアント。

    SignalR によって内部的に使用されるその他のいくつかの値と共に、クエリ文字列のデータの接続に使用するトランスポート メソッドがあります。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    値`transportMethod`"Websocket"、"serverSentEvents"、"foreverFrame"または"longPolling"になります。 この値を確認する場合、`OnConnected`イベント ハンドラー メソッドでは、一部のシナリオで、接続の最終的なネゴシエートされたトランスポート メソッドではないトランスポート値を取得する可能性があります最初にします。 その場合はこのメソッドは例外をスローし、最終的なトランスポート メソッドが確立されたときに後でもう一度呼び出されます。
- Cookie。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Cookie を取得することもできます。`Context.RequestCookies`です。
- ユーザーの情報。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- 要求の HttpContext オブジェクト:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    取得する代わりにこのメソッドを使用して`HttpContext.Current`を取得する、 `HttpContext` SignalR 接続のオブジェクト。

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>クライアントと、ハブ クラス間で状態を渡す方法

クライアント プロキシは、提供、`state`オブジェクトの各メソッド呼び出しを使用してサーバーに送信するデータを格納できます。 サーバー上でこのデータにアクセスすることができます、`Clients.Caller`クライアントによって呼び出されるハブ メソッドでのプロパティです。 `Clients.Caller`接続の有効期間のイベント ハンドラー メソッドのプロパティは設定されません`OnConnected`、 `OnDisconnected`、および`OnReconnected`です。

作成または内のデータを更新する、`state`オブジェクトおよび`Clients.Caller`プロパティが双方向で動作します。 サーバーで値を更新することができ、それらのクライアントに渡されます。

次の例では、すべてのメソッド呼び出しを使用してサーバーに送信する状態を格納する JavaScript クライアント コードを示します。

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

次の例では、.NET クライアントで同等のコードを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

を、ハブ クラス内でこのデータにアクセスすることができます、`Clients.Caller`プロパティです。 次の例では、前の例で参照される状態を取得するコードを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> このメカニズムの状態を保持するものではありません、データの量が多い場合以降にするすべてのもの、`state`または`Clients.Caller`プロパティは、ラウンドト リップ メソッド呼び出しのたびにします。 ユーザー名またはカウンターなどの小さい項目に対して便利です。


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>ハブ クラスのエラーを処理する方法

ハブ クラスのメソッドで発生したエラーを処理するには、次のメソッドの一方または両方を使用します。

- Try catch ブロックでは、メソッドのコードをラップし、例外オブジェクトを記録します。 デバッグの目的では、クライアントに例外を送信できますが、セキュリティ上の理由から実稼働環境でクライアントに詳細な情報を送信はお勧めしません。
- 処理するハブ パイプライン モジュールを作成、 [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)メソッドです。 次の例では、モジュールでは、ハブ パイプラインに挿入 Global.asax でコードを続けて、エラー ログに記録するパイプライン モジュールを示します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

ハブ パイプライン モジュールに関する詳細については、次を参照してください。[をハブ パイプラインをカスタマイズする方法](#hubpipeline)このトピックで後述します。

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>トレースを有効にする方法

サーバー側トレースを有効にするには、この例のように、Web.config ファイルに system.diagnostics 要素を追加します。

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

内のログを表示するには Visual Studio で、アプリケーションを実行すると、**出力**ウィンドウです。

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>クライアントのメソッドを呼び出すし、ハブ クラスの外部からグループを管理する方法

ハブ クラスよりも別のクラスからのクライアント メソッドを呼び出す、ハブの SignalR コンテキスト オブジェクトへの参照を取得を使用してクライアントのメソッドを呼び出すか、グループを管理します。、

次のサンプル`StockTicker`クラス コンテキスト オブジェクトを取得、クラスのインスタンスに格納、静的なプロパティで、クラスのインスタンスを格納およびシングルトン クラスのインスタンスからコンテキストを使用して呼び出す、`updateStockPrice`なっているクライアントでメソッドという名前のハブに接続されている`StockTickerHub`です。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

有効期間が長いオブジェクト内でコンテキストの複数回使用する必要がある場合は、1 回の参照を取得し、ことはなく、毎回取得を保存します。 コンテキストを 1 回取得とは、SignalR が、同じシーケンスをハブ メソッドの機能によってクライアント メソッド呼び出し内のクライアントにメッセージを送信することを確認します。 コンテキストを使用して、SignalR ハブの方法を示すチュートリアルでは、次を参照してください。[サーバーは、ASP.NET SignalR でブロードキャスト](index.md)です。

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>クライアントのメソッドを呼び出す

RPC を受信するクライアントを指定することができますが、ハブ クラスから呼び出す場合よりも少ないオプションです。 その理由は、コンテキストがある、クライアントから特定の呼び出しに関連付けられた任意のメソッドを必要とする現在の接続 ID のサポートなど、 `Clients.Others`、または`Clients.Caller`、または`Clients.OthersInGroup`、ご利用いただけません。 次のオプションを使用できます。

- 接続されているすべてのクライアントです。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- 接続 ID で識別される特定のクライアント

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- 接続 ID で識別される、指定されたクライアントを除くすべての接続されたクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- 指定したグループ内のすべての接続されたクライアント。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- 接続 ID で識別される、指定されたクライアントを除く、指定したグループ内のすべての接続されたクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

呼び出す場合、非ハブ クラスにメソッドからハブ クラスに、現在の接続 id を渡すし、でそれを使用`Clients.Client`、 `Clients.AllExcept`、または`Clients.Group`をシミュレートする`Clients.Caller`、 `Clients.Others`、または`Clients.OthersInGroup`です。 次の例で、`MoveShapeHub`クラスに合格する接続 ID、`Broadcaster`クラスできるように、`Broadcaster`クラスをシミュレートできます`Clients.Others`です。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>グループ メンバーシップを管理します。

グループを管理するがある同じオプション ハブ クラスの場合とします。

- クライアントをグループに追加します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- クライアントをグループから削除します。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>ハブ パイプラインをカスタマイズする方法

SignalR では、独自のコードをハブ パイプラインに挿入することができます。 次の例では、クライアントとクライアントで呼び出された送信メソッド呼び出しから受信した各受信メソッド呼び出しをログに記録するカスタム ハブ パイプライン モジュールを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

次のコードで、 *Global.asax*ファイルをハブ パイプラインで実行するモジュールを登録します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

オーバーライドできるさまざまな方法があります。 完全な一覧についてを参照してください。 [HubPipelineModule メソッド](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)です。
