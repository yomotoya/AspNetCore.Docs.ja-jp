---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: このドキュメントでは、コード サンプル demonstratin と version 1.1 SignalR for ASP.NET SignalR ハブの API のサーバー側のプログラミングを紹介しています.
ms.author: riande
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: a51a2077e0b6cde80bc679e3a310c0c804d19d68
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288028"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a>ASP.NET SignalR ハブ API ガイド - サーバー (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このドキュメントでは、一般的なオプションを示すコード サンプルを使用したバージョン 1.1 では、SignalR のサーバー側の ASP.NET SignalR ハブの API をプログラミングの概要を示します。
> 
> SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 SignalR は、のすべてのクライアントとサーバーが処理されます。
> 
> SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。 概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](index.md)します。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [SignalR のルートを登録し、SignalR のオプションを構成する方法](#route)

    - [/Signalr URL](#signalrurl)
    - [SignalR のオプションを構成します。](#options)
- [作成し、ハブ クラスを使用する方法](#hubclass)

    - [ハブ オブジェクトの有効期間](#transience)
    - [JavaScript クライアントではハブの名前のキャメル](#hubnames)
    - [複数のハブ](#multiplehubs)
- [クライアントが呼び出すことができる、ハブ クラスにメソッドを定義する方法](#hubmethods)

    - [JavaScript クライアント内のメソッド名の大文字小文字のキャメル形式](#methodnames)
    - [非同期的に実行するタイミング](#asyncmethods)
    - [オーバー ロードを定義します。](#overloads)
- [Hub クラスからのクライアント メソッドを呼び出す方法](#callfromhub)

    - [クライアントを選択すると、RPC が表示されます。](#selectingclients)
    - [メソッド名のコンパイル時検証なし](#dynamicmethodnames)
    - [大文字のメソッド名に一致します。](#caseinsensitive)
    - [非同期実行](#asyncclient)
- [Hub クラスからグループ メンバーシップを管理する方法](#groupsfromhub)

    - [Add および Remove メソッドの非同期実行](#asyncgroupmethods)
    - [グループ メンバーシップの永続化](#grouppersistence)
    - [シングル ユーザー グループ](#singleusergroups)
- [ハブ クラスでの接続の有効期間イベントを処理する方法](#connectionlifetime)

    - [OnConnected、OnDisconnected、OnReconnected を呼び出すときに](#onreconnected)
    - [設定されていない呼び出し元の状態](#nocallerstate)
- [コンテキスト プロパティからのクライアントに関する情報を取得する方法](#contextproperty)
- [クライアントとハブ クラス間で状態を渡す方法](#passstate)
- [ハブ クラスのエラーを処理する方法](#handleErrors)
- [クライアントにメソッドを呼び出し、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)

    - [クライアントのメソッドを呼び出す](#callingclientsoutsidehub)
    - [グループ メンバーシップを管理します。](#managinggroupsoutsidehub)
- [トレースを有効にする方法](#tracing)
- [ハブのパイプラインをカスタマイズする方法](#hubpipeline)

クライアントのプログラム方法については、次のリソースを参照してください。

- [SignalR ハブ API ガイド - JavaScript クライアント](index.md)
- [SignalR ハブ API ガイド - .NET クライアント](index.md)

.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。 .NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a>SignalR のルートを登録し、SignalR のオプションを構成する方法

クライアントがハブへの接続に使用するルートを定義するには、呼び出し、 [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx)メソッド、アプリケーションの起動時にします。 `MapHubs` [拡張メソッド](https://msdn.microsoft.com/library/vstudio/bb383977.aspx)の`System.Web.Routing.RouteCollection`クラス。 次の例は、SignalR ハブ ルートを定義する方法を示します、 *Global.asax*ファイル。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

ASP.NET MVC アプリケーションに SignalR の機能を追加する場合は、SignalR のルートが、他のルートの前に追加されたことを確認します。 詳細については、次を参照してください。[チュートリアル。SignalR と MVC 4 の概要](index.md)します。

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>/Signalr URL

既定では、クライアントがハブへの接続に使用するルートの URL は"/signalr"。 (この URL は自動的に生成された JavaScript ファイルの「/signalr ハブ」の url を混同しないでください。 生成されたプロキシの詳細については、次を参照してください[SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとは何が](index.md)。)。

SignalR; に対しては使用できませんのベース URL を構成した特殊な状況である可能性があります。たとえば、という名前のプロジェクトのフォルダーがある*signalr*名前を変更するとします。 次の例に示すように、ベースの URL を変更する場合、(置換"/signalr"で、目的の URL を使用してサンプル コード)。

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

オーバー ロード、`MapHubs`メソッドを使用すると、カスタムの URL、カスタム依存関係競合回避モジュール、および、次のオプションを指定します。

- ブラウザー クライアントからのクロス ドメイン呼び出しを有効にします。

    通常、ブラウザーがからページを読み込む場合`http://contoso.com`、SignalR 接続が同じドメイン内では`http://contoso.com/signalr`します。 場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。 セキュリティ上の理由から、ドメイン間の接続が既定で無効になります。 詳細については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - JavaScript クライアントのドメイン間の接続を確立する方法](index.md)します。
- 詳細なエラー メッセージを有効にします。

    エラーが発生すると、SignalR の既定の動作が発生した事象に関する詳細なしの通知メッセージをクライアントに送信するは。 詳細なエラー情報をクライアントに送信することは推奨されません、運用環境で悪意のあるユーザーは、アプリケーションに対する攻撃の情報を使用できない可能性があるため。 トラブルシューティングについては、一時的に詳しいエラー報告を有効にするのにこのオプションを使用できます。
- 自動的に生成された JavaScript プロキシ ファイルを無効にします。

    既定では、URL「/signalr ハブ」への応答で、ハブ クラスのプロキシを持つ JavaScript ファイルが生成されます。 JavaScript プロキシを使用する場合、または手動でこのファイルを生成し、クライアントの物理ファイルを参照する場合は、プロキシの生成を無効にするのにこのオプションを使用できます。 詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアント - SignalR の物理ファイルを作成する方法は、プロキシを生成](index.md)します。

次の例への呼び出しで SignalR 接続の URL とこれらのオプションを指定する方法を示しています、`MapHubs`メソッド。 カスタム URL を指定するには、置き換える"/signalr"を使用する URL の例です。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>作成し、ハブ クラスを使用する方法

ハブを作成するから派生したクラスを作成[Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx)します。 次の例では、チャット アプリケーションの単純なハブ クラスを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

この例では、接続されているクライアントが呼び出すことができます、`NewContosoChatMessage`メソッドでは、接続されているすべてのクライアントに受信したデータをブロードキャストするときと。

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>ハブ オブジェクトの有効期間

ハブ クラスをインスタンス化したり、サーバー上のコードからメソッドを呼び出すしません。これらすべてはの SignalR ハブ パイプラインによって行われます。 SignalR は、ときに、クライアントが接続切断されると、またはサーバー メソッドの呼び出しを行ったなど、ハブの操作を処理する必要があるたびに、ハブ クラスの新しいインスタンスを作成します。

ハブ クラスのインスタンスは、一時的なであるため、次の 1 つのメソッド呼び出しからの状態を維持するために使用できません。 たびに、サーバー メッセージを受信メソッド呼び出しをクライアントでは、ハブ クラスのプロセスの新しいインスタンス。 を介して複数の接続とメソッドの呼び出しの状態を維持するために、ハブ クラスまたは別のクラスから派生していないデータベース、または静的変数などの他のメソッドを使用して、`Hub`します。 メモリ内のデータを永続化する場合、ハブ クラスに静的変数などのメソッドを使用して、データは失われますアプリ ドメインがリサイクルされる場合。

ハブ クラス外部で実行されるコードからクライアントにメッセージを送信する場合、ハブ クラスのインスタンスをインスタンス化して行うことはできませんが、ハブ クラスの SignalR コンテキスト オブジェクトへの参照を取得することによって行うことができます。 詳細については、次を参照してください。[クライアント メソッドを呼び出すと、ハブ クラスの外部からグループを管理する方法](#callfromoutsidehub)このトピックで後述します。

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>JavaScript クライアントではハブの名前のキャメル

既定では、JavaScript クライアントがハブにクラス名の camel 形式のバージョンを使用して参照します。 SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。 前の例と呼ばれる`contosoChatHub`JavaScript コードにします。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

クライアントを使用して、追加するための別の名前を指定するかどうか、`HubName`属性。 使用すると、`HubName`属性は、JavaScript クライアントにキャメル ケースに名前の変更はありません。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>複数のハブ

アプリケーションでは、複数のハブ クラスを定義できます。 そうと、接続は共有されますが、グループは別。

- すべてのクライアントは、サービスと SignalR の接続を確立するために、同じ URL を使用 ("/signalr"またはいずれかを指定した場合、カスタムの URL)、すべてのハブの接続を使用して、サービスで定義されています。

    比較する 1 つのクラスでハブのすべての機能を定義すると、複数のハブのパフォーマンスの違いはありません。
- すべてのハブでは、同じ HTTP 要求情報を取得します。

    すべてのハブが同じ接続を共有するため、サーバーを取得する HTTP 要求情報は、SignalR の接続を確立する元の HTTP 要求で何が。 クエリ文字列を指定することで、クライアントからサーバーに情報を渡すために、接続要求を使用する場合は、別のハブに別のクエリ文字列を提供できません。 すべてのハブと同じ情報が表示されます。
- JavaScript プロキシの生成されたファイルは、1 つのファイルですべてのハブ プロキシが含まれます。

    JavaScript プロキシの詳細については、次を参照してください。 [SignalR ハブ API ガイド - JavaScript クライアントで生成されたプロキシとは何が](index.md)します。
- グループは、ハブ内で定義されます。

    SignalR を定義することで接続されているクライアントのサブセットにブロードキャストするグループの名前。 グループは、各ハブとは別に保持されます。 たとえば、"Administrators"という名前のグループはクライアントの 1 つのセットを追加、`ContosoChatHub`クラスと同じグループ名は参照されている場合にクライアントを別のセットに、`StockTickerHub`クラス。

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>クライアントが呼び出すことができる、ハブ クラスにメソッドを定義する方法

クライアントから呼び出せるようにするハブのメソッドを公開するには、次の例に示すように、パブリック メソッドを宣言します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。 パラメーターが表示されるか、呼び出し元に返すデータは、JSON を使用して、クライアントとサーバー間通信し、SignalR は複雑なオブジェクトのバインドとオブジェクトの配列を自動的に処理します。

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>JavaScript クライアント内のメソッド名の大文字小文字のキャメル形式

既定では、メソッド名の camel 形式のバージョンを使用して JavaScript クライアントがハブ メソッドを参照します。 SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

クライアントを使用して、追加するための別の名前を指定するかどうか、`HubMethodName`属性。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>非同期的に実行するタイミング

データベース検索など、web サービス呼び出しの待機を含むのハブ メソッドを返すことによって非同期にする場合がある実行時間の長いメソッドまたは作業を実行する必要がありますは、[タスク](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)(の代わりに`void`を返す) または[タスク&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx)オブジェクト (の代わりに`T`型を返す)。 戻ると、`Task`オブジェクト、メソッドでは、SignalR を待ちます、`Task`を完了するし、そのラップ解除された結果、クライアントに戻るため、クライアントでメソッドの呼び出しをコーディングする方法に違いはありません。

ハブ メソッドを行う非同期回避 WebSocket トランスポートを使用する場合は、接続をブロックしています。 ハブ メソッドを同期的に実行すると、トランスポートが WebSocket のハブ メソッドが完了するまでに、同じクライアントからハブのメソッドの後続の呼び出しがブロックされます。

次の例と同じメソッドが同期的に実行するコード化されたまたはいずれかのバージョンを呼び出すために動作する JavaScript クライアント コードの後に、非同期的を示します。

**同期**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

**非同期の ASP.NET 4.5**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

ASP.NET 4.5 で非同期メソッドを使用する方法の詳細については、次を参照してください。 [ASP.NET MVC 4 での非同期のメソッドを使用して](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)します。

<a id="overloads"></a>

### <a name="defining-overloads"></a>オーバー ロードを定義します。

メソッドのオーバー ロードを定義する場合は、各オーバー ロードのパラメーターの数が異なる場合があります。 さまざまなパラメーターの型を指定するだけでオーバー ロードを区別する場合は、ハブ クラスはコンパイルされますが、SignalR サービスはオーバー ロードのいずれかの呼び出しにクライアントがしようとするいると、実行時に例外をスローします。

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Hub クラスからのクライアント メソッドを呼び出す方法

サーバーからクライアント メソッドを呼び出すには、使用、`Clients`ハブ クラス内のメソッドのプロパティ。 次の例では、サーバー コードを呼び出す`addNewMessageToPage`接続されているすべてのクライアント、および JavaScript クライアント内でメソッドを定義するクライアント コードにします。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

クライアントのメソッドから戻り値を取得できません。などの構文`int x = Clients.All.add(1,1)`は機能しません。

複合型とパラメーターの配列を指定することができます。 次の例では、複合型をメソッドのパラメーターに、クライアントに渡します。

**サーバー コードを複雑なオブジェクトを使用してクライアント メソッドを呼び出す**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

**複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>クライアントを選択すると、RPC が表示されます。

クライアントのプロパティを返します、 [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) RPC 受信するクライアントを指定するためのいくつかのオプションを提供するオブジェクト。

- 接続されているすべてのクライアント。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- 呼び出し元のクライアントのみです。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- 呼び出し元のクライアントを除くすべてのクライアント。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- 接続 ID で識別される特定のクライアント

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    この例では`addContosoChatMessageToPage`呼び出し元のクライアントで使用すると同じ効果があり`Clients.Caller`します。
- 接続されているすべてのクライアント接続 ID で識別される、指定されたクライアントを除く

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- すべてには、指定したグループ内のクライアントが接続されています。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- 接続 ID で識別される、指定されたクライアントを除く、指定したグループに接続されているすべてのクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- 指定したグループ内のすべての接続されているクライアントは、呼び出し元のクライアントを除きます。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>メソッド名のコンパイル時検証なし

指定したメソッド名は、IntelliSense またはコンパイル時検証がないことを意味の動的オブジェクトとして解釈されます。 式は、実行時に評価されます。 メソッドの呼び出しを実行するときにそのメソッドが呼び出される SignalR メソッド名とパラメーターの値をクライアントに送信し、名前に一致する場合は、クライアントがある、メソッド、およびパラメーターの値は渡されます。 クライアントに一致するメソッドが見つからない場合、エラーは発生しません。 SignalR は、クライアント メソッドを呼び出すと、バック グラウンドでクライアントに送信するデータの形式の詳細については、次を参照してください。 [SignalR 入門](index.md)します。

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>大文字のメソッド名に一致します。

大文字と小文字が一致するメソッドの名前です。 たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addcontosochatmessagetopage`、または`addContosoChatMessageToPage`クライアント。

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>非同期実行

メソッドを呼び出すことは非同期的に実行します。 SignalR の後続行のコードをすることを指定しない限り、クライアントにデータの送信を完了するを待たず、クライアントへのメソッド呼び出しはすぐに実行後に付属している任意のコードでは、メソッドの完了を待つ必要があります。 クライアントの 2 つの方法を順番に実行する方法を表示する次のコード例を使用して .NET 4.5 で動作するコードを使用して .NET 4 で動作するコードです。

**.NET 4.5 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

**.NET 4 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

使用する場合`await`または`ContinueWith`クライアント メソッドは、次のコード行が実行される前に完了するまで待機するわけクライアントが実際にメッセージが表示される前に、次のコード行を実行します。 のみのクライアント メソッドの呼び出しには、「完了」は、SignalR がすべてのメッセージを送信するために必要な終了することを意味します。 クライアントがメッセージを受信することの確認が必要な場合、そのメカニズムを自分でプログラムがあります。 たとえば、コーディングする可能性があります、`MessageReceived`メソッドと、ハブで、`addContosoChatMessageToPage`メソッドを呼び出すことも、クライアントを`MessageReceived`その後はすべて使用するクライアントで実行する必要があります。 `MessageReceived`ハブでどのような作業は、実際のクライアントの受信と、元のメソッド呼び出しの処理によって異なります。 を実行できます。

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>メソッドの名前として文字列変数を使用する方法

キャスト、メソッド名と文字列変数を使用してクライアント メソッドを呼び出す場合`Clients.All`(または`Clients.Others`、`Clients.Caller`など) に`IClientProxy`を呼び出して[Invoke (methodName, args…)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Hub クラスからグループ メンバーシップを管理する方法

SignalR でグループは、接続されているクライアントのサブセットを指定するメッセージをブロードキャストする方法を提供します。 グループは、クライアントの任意の数を持つことができ、クライアントは任意の数のグループのメンバーであることができます。

グループのメンバーシップを管理するを使用して、[追加](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx)と[削除](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx)によって提供されるメソッド、`Groups`ハブ クラスのプロパティ。 次の例は、`Groups.Add`と`Groups.Remove`呼び出し元となる JavaScript クライアント コードの後に、クライアント コードによって呼び出されるハブ メソッドで使用されるメソッド。

**サーバー**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

**生成されたプロキシを使用して JavaScript クライアント**

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

グループを明示的に作成する必要はありません。 有効なグループが初めての呼び出しでその名前を指定する自動的に作成`Groups.Add`、そのメンバーシップから最後の接続を削除すると削除されます。

グループ メンバーシップの一覧またはグループの一覧を取得するための API はありません。 SignalR では、クライアントとグループを基にメッセージを送信する[パブリッシュ/サブスクライブ モデル](http://en.wikipedia.org/wiki/Publish/subscribe)、し、サーバーがグループまたはグループ メンバーシップの一覧を保持しません。 こうため、SignalR を保持する任意の状態が新しいノードに適用するのには web ファームにノードを追加するたびに、スケーラビリティを最大化します。

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Add および Remove メソッドの非同期実行

`Groups.Add`と`Groups.Remove`メソッドが非同期的に実行します。 クライアント グループを追加して、すぐに、グループを使用して、クライアントにメッセージを送信する場合は、ことを確認する必要がある、`Groups.Add`メソッドが最初に終了しました。 次のコード例は、その .NET 4.5、および .NET 4 で動作するコードを使用していずれかで動作するコードを使用して 1 つの方法を表示します。

**.NET 4.5 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

**.NET 4 の例**

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>グループ メンバーシップの永続化

SignalR 接続の追跡、ユーザーが同じグループにユーザーを確立するたびに接続するユーザーではなく、その場合を呼び出す必要が`Groups.Add`たびに、ユーザーが新しい接続を確立します。

接続の一時的な喪失、後に場合があります SignalR ことができます、接続を自動的に復元します。 その場合は、SignalR は、同じ接続を復元は、新しい接続を確立できませんし、そのため、クライアントのグループのメンバーシップが自動的に復元します。 これは可能なサーバーの再起動または失敗の結果が場合でも、一時的な中断がグループのメンバーシップを含む、各クライアントの接続状態をクライアントにラウンドト リップします。 サーバーが停止して、接続がタイムアウトする前に、新しいサーバーに置換される場合、クライアントは、新しいサーバーに自動的に再接続し、再登録するグループのメンバーであります。

接続は、接続の喪失後に自動的に復元できない場合にまたは接続がタイムアウトしたときに、(たとえば、ブラウザーは、新しいページに移動します) 場合、クライアントが切断されたときは、グループ メンバーシップは失われます。 次に、ユーザーが接続したときは、新しい接続になります。 を、同じユーザーが新しい接続が確立されるときのグループ メンバーシップを維持するには、アプリケーションは、ユーザーとグループの間の関連付けを追跡し、ユーザーが新しい接続を確立するたびにグループ メンバーシップを復元するがします。

接続と再接続に関する詳細については、次を参照してください。[ハブ クラスでの接続の有効期間イベントを処理する方法](#connectionlifetime)このトピックで後述します。

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>シングル ユーザー グループ

どのユーザーがメッセージを送信し、どのユーザーにメッセージを受信する必要がありますを知るためには、ユーザーと接続間の関連付けを追跡する必要は通常、SignalR を使用するアプリケーション。 グループは、これを行うための 2 つの一般的に使用されるパターンのいずれかで使用されます。

- シングル ユーザーのグループ。

    グループ名としてユーザー名を指定でき、ユーザーが接続または再接続するたびに、現在の接続 ID をグループに追加することができます。 ユーザー、グループに送信するには、メッセージを送信します。 この方法の欠点は、グループを調べるかどうか、ユーザーはオンラインまたはオフラインにするに提供します。
- ユーザー名と接続 Id の間の関連付けを追跡します。

    ディクショナリまたはデータベース内の各ユーザー名と 1 つまたは複数の接続 Id 間のアソシエーションを格納し、ユーザーが接続または切断するたびに、格納されたデータを更新できます。 ユーザーにメッセージを送信するには、接続 Id を指定します。 この方法の欠点より多くのメモリがかかることです。

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>ハブ クラスでの接続の有効期間イベントを処理する方法

接続の有効期間イベントを処理するための一般的な原因としてとユーザー名と接続 Id 間の関連付けを追跡するか、ユーザーが接続されているかどうかを追跡するのには。 クライアントが接続または切断ときは、独自のコードを実行するには、オーバーライド、 `OnConnected`、 `OnDisconnected`、および`OnReconnected`ハブの仮想メソッドをクラスの次の例で示す。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>OnConnected、OnDisconnected、OnReconnected を呼び出すときに

ブラウザーが新しいページに移動するたびに新しい接続でが確立されているため、SignalR は実行、`OnDisconnected`メソッドに続けて、`OnConnected`メソッド。 SignalR は、新しい接続が確立されたときに常に新しい接続 ID を作成します。

`OnReconnected`メソッドが呼び出されますが、一時中断 SignalR がときに、ケーブルが一時的に切断され、再接続、接続がタイムアウトする前になどから、回復できるように自動的に接続します。`OnDisconnected`クライアントを切断して SignalR 自動的に再接続できないなど、ブラウザーが新しいページに移動するときにメソッドが呼び出されます。 そのため、特定のクライアントのイベントの可能なシーケンスは`OnConnected`、 `OnReconnected`、 `OnDisconnected`; または`OnConnected`、`OnDisconnected`します。 シーケンスは表示されません`OnConnected`、 `OnDisconnected`、`OnReconnected`特定の接続。

`OnDisconnected`メソッドなど、サーバーがダウンしたときに、一部のシナリオで呼び出されませんまたはアプリケーション ドメインが再利用されます。 別のサーバーがオンラインにするか、アプリ ドメインのリサイクルが完了すると、一部のクライアントが再接続して起動できるようになることがあります、`OnReconnected`イベント。

詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](index.md)します。

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>設定されていない呼び出し元の状態

任意の状態に格納されていることを意味するサーバーからの接続の有効期間イベント ハンドラー メソッドが呼び出される、`state`で、クライアント上のオブジェクトは設定されません、`Caller`サーバーのプロパティ。 については、`state`オブジェクトと`Caller`プロパティを参照してください[クライアントとハブ クラス間で状態を渡す方法](#passstate)このトピックで後述します。

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>コンテキスト プロパティからのクライアントに関する情報を取得する方法

クライアントに関する情報を取得する、`Context`ハブ クラスのプロパティ。 `Context`プロパティが返す、 [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx)次の情報へのアクセスを提供するオブジェクト。

- 呼び出し元のクライアントの接続 ID。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    接続 ID は、SignalR (値は、独自のコードでは指定できません) によって割り当てられた GUID です。 同じ接続 ID は、アプリケーションで複数のハブがある場合、すべてのハブで使用し、各接続の 1 つの接続 ID があります。
- HTTP ヘッダーのデータ。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    HTTP ヘッダーを取得することもできます。`Context.Headers`します。 多重参照を同じことには、その理由は`Context.Headers`が最初に、作成された、`Context.Request`プロパティが追加された後と`Context.Headers`旧バージョンと互換性が保持されます。
- 文字列データのクエリを実行します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    クエリの文字列データを取得することも`Context.QueryString`します。

    このプロパティで取得するクエリ文字列確立された SignalR 接続 HTTP 要求で使用されていたです。 クライアントでは、クライアントからサーバーにクライアントに関するデータを渡すに便利ですが、接続を構成することでクエリ文字列パラメーターを追加できます。 次の例では、生成されたプロキシを使用すると、JavaScript クライアント内でクエリ文字列を追加する 1 つの方法を示します。

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    クエリ文字列パラメーターを設定する方法についての詳細については、API ガイドを参照してください、 [JavaScript](index.md)と[.NET](index.md)クライアント。

    SignalR によって内部的に使用されるその他のいくつかの値と共に、クエリ文字列のデータの接続に使用されるトランスポート メソッドがあります。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    値`transportMethod`"Websocket"、"serverSentEvents"、"foreverFrame"または"longPolling"になります。 この値を確認する場合、`OnConnected`イベント ハンドラー メソッドでは、一部のシナリオで、接続のネゴシエートされたトランスポートの最終的なメソッドではないトランスポートの値を取得する可能性があります最初にします。 その場合は、メソッドは、例外がスローされ、最終的なトランスポート メソッドが確立されたときに後でもう一度呼び出されます。
- クッキー。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Cookie を取得することもできます。`Context.RequestCookies`します。
- ユーザー情報。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- 要求の HttpContext オブジェクト:

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    このメソッドを使用して取得する代わりに`HttpContext.Current`を取得する、 `HttpContext` SignalR 接続のオブジェクト。

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>クライアントとハブ クラス間で状態を渡す方法

クライアント プロキシを提供する`state`オブジェクトの各メソッドの呼び出しで、サーバーに送信するデータを保存できます。 サーバー上でこのデータにアクセスすることができます、`Clients.Caller`クライアントによって呼び出されるハブ メソッドでプロパティ。 `Clients.Caller`プロパティは設定されませんが、接続の有効期間イベントのハンドラー メソッド`OnConnected`、 `OnDisconnected`、および`OnReconnected`します。

作成または更新でのデータ、`state`オブジェクトと`Clients.Caller`プロパティが双方向で機能します。 サーバーで値を更新して、クライアントに渡されます。

次の例では、すべてのメソッド呼び出しで、サーバーに送信する状態を格納する JavaScript クライアント コードを示します。

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

次の例では、.NET クライアントでの同等のコードを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

ハブ クラス内でこのデータにアクセスすることができます、`Clients.Caller`プロパティ。 次の例では、前の例で参照される状態を取得するコードを示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> 状態を保持するためには、このメカニズムはありません大量のデータをすべてを配置した後、`state`または`Clients.Caller`プロパティは、メソッド呼び出しのたびにラウンド トリップします。 ユーザー名やカウンターなどの小さい項目と便利です。


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>ハブ クラスのエラーを処理する方法

ハブ クラスのメソッドで発生するエラーを処理するには、次のメソッドの一方または両方を使用します。

- Try catch ブロックでは、メソッドのコードをラップし、例外オブジェクトを記録します。 デバッグの目的では、クライアントに例外を送信できますが、セキュリティ上の理由から運用環境でクライアントに詳細な情報を送信することはお勧めしません。
- 処理するハブ パイプライン モジュールを作成、 [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx)メソッド。 次の例では、モジュールをハブ パイプラインに挿入する Global.asax 内のコードの後に、エラー ログに記録するパイプラインのモジュールを示します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

ハブ パイプライン モジュールの詳細については、次を参照してください。[ハブ パイプラインをカスタマイズする方法について](#hubpipeline)このトピックで後述します。

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>トレースを有効にする方法

サーバー側トレースを有効にするには、この例で示すように、Web.config ファイルに system.diagnostics 要素を追加します。

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

ログを表示するには Visual Studio でアプリケーションを実行すると、**出力**ウィンドウ。

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>クライアントにメソッドを呼び出し、ハブ クラスの外部からグループを管理する方法

ハブ クラスよりも別のクラスからクライアント メソッドを呼び出すにハブの SignalR コンテキスト オブジェクトへの参照を取得およびクライアントのメソッドを呼び出すか、グループを管理するに使用します。

次のサンプル`StockTicker`クラス コンテキスト オブジェクトを取得、クラスのインスタンスに格納、静的なプロパティで、クラスのインスタンスを格納およびシングルトン クラスのインスタンスからコンテキストを使用して呼び出す、`updateStockPrice`クライアント上のメソッドという名前のハブに接続されている`StockTickerHub`します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

有効期間が長いオブジェクト コンテキストの複数回を使用する必要がある場合は、1 回の参照を取得しよりも、毎回の取得を保存します。 コンテキストを 1 回取得すると、SignalR がメッセージをハブのメソッド、クライアントのメソッドの呼び出しと同じシーケンス内のクライアントに送信するようにします。 ハブの SignalR コンテキストを使用する方法を示すチュートリアルについては、次を参照してください。 [ASP.NET SignalR によるサーバー ブロードキャスト](index.md)します。

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>クライアントのメソッドを呼び出す

RPC を受信するクライアントを指定できますが、ハブ クラスから呼び出す場合よりも少ないオプションがあります。 この理由は、すべてのメソッドを必要とする現在の接続 ID のサポート技術情報など、コンテキストがクライアントから、特定の呼び出しに関連付けられていない`Clients.Others`、または`Clients.Caller`、または`Clients.OthersInGroup`は使用できません。 次のオプションを使用できます。

- 接続されているすべてのクライアント。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- 接続 ID で識別される特定のクライアント

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- 接続されているすべてのクライアント接続 ID で識別される、指定されたクライアントを除く

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- すべてには、指定したグループ内のクライアントが接続されています。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- 接続 ID で識別される、指定されたクライアントを除く、指定したグループに接続されているすべてのクライアント

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

呼び出す場合、非ハブ クラスにメソッドから、ハブ クラスで、現在の接続 ID を渡すし、を使用して`Clients.Client`、 `Clients.AllExcept`、または`Clients.Group`をシミュレートする`Clients.Caller`、 `Clients.Others`、または`Clients.OthersInGroup`します。 次の例では、`MoveShapeHub`クラスに合格する接続 ID、`Broadcaster`クラスように、`Broadcaster`クラスをシミュレートできます`Clients.Others`します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>グループ メンバーシップを管理します。

グループの管理のハブ クラスの場合と同じオプションがあります。

- クライアントをグループに追加します。

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- グループからのクライアントを削除します。

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>ハブのパイプラインをカスタマイズする方法

SignalR では、独自のコードをハブ パイプラインに挿入することができます。 次の例は、クライアントとクライアントで呼び出されるメソッド呼び出しの送信から受信した受信した各メソッド呼び出しのログは、カスタム ハブ パイプライン モジュールを示しています。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

次のコードで、 *Global.asax*ファイルをハブ パイプラインで実行するモジュールを登録します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

上書き可能なさまざまな方法があります。 完全な一覧についてを参照してください。 [HubPipelineModule メソッド](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx)します。
