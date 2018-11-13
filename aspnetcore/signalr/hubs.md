---
title: ASP.NET Core SignalR のハブを使用します。
author: tdykstra
description: ASP.NET Core SignalR のハブを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569920"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>ASP.NET core SignalR のハブの使用

によって[Rachel Appel](https://twitter.com/rachelappel)と[Kevin Griffin](https://twitter.com/1kevgriff)

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>SignalR ハブとは

SignalR ハブの API を使用すると、サーバーから接続しているクライアントのメソッドを呼び出すことができます。 サーバー コードでは、クライアントによって呼び出されるメソッドを定義します。 クライアント コードでは、サーバーから呼び出されるメソッドを定義します。 SignalR は、バック グラウンドでクライアントとサーバーとサーバーからクライアントのリアルタイム通信を可能にするすべての処理されます。

## <a name="configure-signalr-hubs"></a>SignalR ハブを構成します。

SignalR のミドルウェアがサービスによっては、呼び出すことによって構成されている必要があります`services.AddSignalR`します。

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

SignalR の機能を ASP.NET Core アプリを追加する場合は、SignalR のルートをセットアップを呼び出して`app.UseSignalR`で、`Startup.Configure`メソッド。

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>作成し、ハブの使用

継承するクラスを宣言することで、ハブの作成`Hub`、しをパブリック メソッドを追加します。 クライアントとして定義されているメソッドを呼び出すことができます`public`します。

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

戻り値の型と複合型と配列を含む c# メソッドの場合と、パラメーターを指定できます。 SignalR では、複雑なオブジェクトと、パラメーターと戻り値の配列の逆シリアル化とシリアル化を処理します。

> [!NOTE]
> ハブは、一時的なものです。
> * ハブ クラスのプロパティの状態を保存しないでください。 すべてのハブ メソッド呼び出しは、新しいハブ インスタンスで実行されます。  
> * 使用`await`ままになります、ハブに依存する非同期のメソッドを呼び出すとき。 などのメソッドではたとえば、`Clients.All.SendAsync(...)`が失敗することがなく呼び出された場合`await`ハブ メソッドの完了と`SendAsync`が完了するとします。

## <a name="the-context-object"></a>コンテキスト オブジェクト

`Hub`クラスには、`Context`接続に関する情報を次のプロパティを含むプロパティ。

| プロパティ | 説明 |
| ------ | ----------- |
| `ConnectionId` | SignalR によって割り当てられている接続の一意の ID を取得します。 接続ごとに 1 つの接続 ID があります。|
| `UserIdentifier` | 取得、[ユーザー識別子](xref:signalr/groups)します。 既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。 |
| `User` | 取得、`ClaimsPrincipal`現在のユーザーに関連付けられています。 |
| `Items` | この接続のスコープ内のデータを共有するために使用できるキー/値コレクションを取得します。 このコレクションに格納できるデータおよび接続の別のハブ メソッド呼び出し間で保持されます。 |
| `Features` | 接続で使用できる機能のコレクションを取得します。 ここでは、このコレクションは必要ありません、ほとんどのシナリオでまだ詳しく記載されていないためです。 |
| `ConnectionAborted` | 取得、`CancellationToken`接続が中止されたときに通知します。 |

`Hub.Context` また、次のメソッドが含まれます。

| メソッド | 説明 |
| ------ | ----------- |
| `GetHttpContext` | 返します、`HttpContext`接続、または`null`接続が HTTP 要求に関連付けられていない場合。 HTTP 接続で HTTP ヘッダーやクエリ文字列などの情報を取得するのにこのメソッドを使用することができます。 |
| `Abort` | 接続を中止します。 |

## <a name="the-clients-object"></a>クライアント オブジェクト

`Hub`クラスには、`Clients`サーバーとクライアント間の通信は、次のプロパティを含むプロパティ。

| プロパティ | 説明 |
| ------ | ----------- |
| `All` | 接続されているすべてのクライアントのメソッドを呼び出す |
| `Caller` | ハブ メソッドを呼び出したクライアントのメソッドを呼び出す |
| `Others` | メソッドを呼び出したクライアントを除くすべての接続されているクライアントのメソッドを呼び出す |


`Hub.Clients` また、次のメソッドが含まれます。

| メソッド | 説明 |
| ------ | ----------- |
| `AllExcept` | 指定された接続を除くすべての接続されているクライアントのメソッドを呼び出す |
| `Client` | 特定の接続されているクライアントのメソッドを呼び出す |
| `Clients` | 接続されているクライアントが特定のメソッドを呼び出す |
| `Group` | 指定したグループ内のすべての接続のメソッドを呼び出す  |
| `GroupExcept` | 指定された接続を除く、指定したグループ内のすべての接続のメソッドを呼び出す |
| `Groups` | 接続のグループを複数のメソッドを呼び出す  |
| `OthersInGroup` | ハブ メソッドを呼び出したクライアントを除く、接続のグループのメソッドを呼び出す  |
| `User` | 特定のユーザーに関連付けられているすべての接続のメソッドを呼び出す |
| `Users` | 指定されたユーザーに関連付けられているすべての接続のメソッドを呼び出す |

プロパティまたはメソッドでは、上記のテーブルごとにオブジェクトを返します、`SendAsync`メソッド。 `SendAsync`メソッドを呼び出すクライアント メソッドのパラメーターと名前を指定することができます。

## <a name="send-messages-to-clients"></a>クライアントにメッセージを送信します。

特定のクライアントへの呼び出しをするためには、プロパティを使用して、`Clients`オブジェクト。 次の例では、次の 3 つのハブ メソッドがあります。

* `SendMessage` 使用して、接続されているすべてのクライアントにメッセージを送信`Clients.All`します。
* `SendMessageToCaller` 使用して、呼び出し元にメッセージを送信`Clients.Caller`します。
* `SendMessageToGroups` すべてのクライアントにメッセージを送信、`SignalR Users`グループ。

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a>厳密に型指定されたハブ

使用する欠点`SendAsync`が呼び出されるクライアント メソッドを指定する文字列に依存していること。 これにより、メソッド名のスペルが正しい場合、ランタイム エラー コードを開くまたはクライアントから不足しています。

使用する代わりに`SendAsync`を厳密に型が、`Hub`で<xref:Microsoft.AspNetCore.SignalR.Hub`1>します。 次の例では、`ChatHub`クライアント メソッドがアウトというインターフェイスに抽出された`IChatClient`します。  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

リファクタリングの前に、このインターフェイスを使用する`ChatHub`例。

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

使用して`Hub<IChatClient>`クライアント メソッドのコンパイル時にチェックできるようにします。 これにより、問題のため、マジック文字列を使用することで`Hub<T>`のみ、インターフェイスで定義されたメソッドへのアクセスを提供できます。

厳密に型指定を使用して`Hub<T>`を使用する機能を無効にします。`SendAsync`します。

## <a name="change-the-name-of-a-hub-method"></a>ハブ メソッドの名前を変更します。

既定では、サーバー ハブのメソッド名は、.NET メソッドの名前です。 ただし、使用することができます、 [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute)この既定の設定を変更し、手動で、メソッドの名前を指定する属性。 クライアントは、メソッドを呼び出すときに、.NET のメソッド名の代わりにこの名前を使用する必要があります。

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a>接続のイベントの処理

SignalR ハブ API は、提供、`OnConnectedAsync`と`OnDisconnectedAsync`接続を管理するための仮想メソッド。 上書き、`OnConnectedAsync`クライアントがグループに追加するなど、ハブに接続するときにアクションを実行する仮想メソッド。

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

上書き、`OnDisconnectedAsync`クライアントが切断されたときにアクションを実行する仮想メソッド。 クライアントが意図的に切断した場合 (呼び出して`connection.stop()`など)、`exception`パラメーターになります`null`します。 ただし、エラー (ネットワーク障害の場合) などのため、クライアントが切断されている場合は、`exception`パラメーターには、例外、エラーを説明します。

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a>エラー処理

ハブ メソッドでスローされた例外は、メソッドを呼び出したクライアントに送信されます。 JavaScript クライアントで、`invoke`メソッドを返します。 を[JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)します。 クライアントが、ハンドラーでエラーを受信すると promise を使用して、接続されている`catch`、呼び出されるは、JavaScript として渡される`Error`オブジェクト。

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

既定では場合、ハブ、例外がスロー SignalR を返し、一般的なエラー メッセージをクライアントにします。 例えば:

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

多くの場合、予期しない例外には、データベース接続が失敗したときにトリガーされます例外内のデータベース サーバーの名前などの機密情報が含まれます。 SignalR は、セキュリティ対策として、既定でこれらの詳細なエラー メッセージを公開しません。 参照してください、[セキュリティに関する考慮事項に関する記事](xref:signalr/security#exceptions)理由の詳細については、例外の詳細が表示されません。

ある場合、例外的な条件を*は*クライアントに反映されるまでは、使用することができます、`HubException`クラス。 スローする場合、 `HubException` 、SignalR hub メソッドから**は**メッセージ全体を未変更の状態、クライアントに送信します。

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> SignalR のみを送信、`Message`クライアントに例外のプロパティ。 スタック トレースと例外の他のプロパティは、クライアントに使用できません。

## <a name="related-resources"></a>関連資料

* [ASP.NET Core SignalR の概要](xref:signalr/introduction)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
