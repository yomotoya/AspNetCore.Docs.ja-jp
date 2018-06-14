---
title: SignalR のユーザーとグループを管理します。
author: rachelappel
description: ASP.NET Core SignalR のユーザーとグループ管理の概要です。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358440"
---
# <a name="manage-users-and-groups-in-signalr"></a>SignalR のユーザーとグループを管理します。

によって[真紀 Brennan](https://github.com/BrennanConroy)

SignalR には、特定のユーザーに関連付けられているすべての接続に送信するだけでなく名前付き接続のグループへのメッセージが可能です。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR のユーザー

SignalR では、特定のユーザーに関連付けられているすべての接続にメッセージを送信することができます。 SignalR を使用して既定では、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`ユーザー識別子としての接続に関連付けられています。 1 人のユーザーには、SignalR アプリケーションを複数の接続を持つことができます。 たとえば、ユーザーは自分のデスクトップだけでなく、携帯電話で接続でした。 各デバイスには、個々 の SignalR 接続がすべて同じユーザーに関連付けられています。 メッセージは、ユーザーに送信される、すべてのユーザーに関連付けられている接続と、メッセージが表示されます。

ユーザー id を渡すことによって、特定のユーザーにメッセージを送信、`User`次の例で示すようにハブ メソッドで機能します。

> [!NOTE]
> ユーザー識別子は、大文字小文字を区別します。

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

ユーザー識別子を作成することでカスタマイズできる、 `IUserIdProvider`、登録することで`ConfigureServices`です。

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR は、カスタムの SignalR サービスを登録する前に呼び出す必要があります。

## <a name="groups-in-signalr"></a>SignalR でグループ

グループは、名前に関連付けられている接続のコレクションです。 グループ内のすべての接続にメッセージを送信できます。 グループは、グループは、アプリケーションによって管理されるため、接続または複数の接続に送信することをお勧めします。 接続は、複数のグループのメンバーであることができます。 これにより、グループ理想的なチャット アプリケーションのような出力を各部屋をグループとして表すことができます。 接続を追加またはを使用してグループから削除することができます、`AddToGroupAsync`と`RemoveFromGroupAsync`メソッドです。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

接続が再接続されると、グループのメンバーシップは保持されません。 接続が再確立される、グループに再度参加する必要があります。 この情報は、アプリケーションは複数のサーバーにスケーリング場合ではないため、グループのメンバーをカウントすることはできません。

> [!NOTE]
> グループ名では大文字小文字を区別します。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:signalr/get-started)
* [ハブ](xref:signalr/hubs)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
