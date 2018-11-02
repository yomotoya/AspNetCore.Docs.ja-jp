---
title: SignalR のユーザーとグループを管理します。
author: tdykstra
description: ASP.NET Core SignalR のユーザーとグループの管理の概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 02db46f090c487a03171de244ff7ad0d5e9de0fa
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758168"
---
# <a name="manage-users-and-groups-in-signalr"></a>SignalR のユーザーとグループを管理します。

によって[真紀 Brennan](https://github.com/BrennanConroy)

SignalR では、特定のユーザーに関連付けられているすべての接続に送信するだけでなく接続の名前付きグループにメッセージを許可します。

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR でのユーザー

SignalR では、特定のユーザーに関連付けられているすべての接続にメッセージを送信できます。 既定では、SignalR を使用して、`ClaimTypes.NameIdentifier`から、`ClaimsPrincipal`に関連付けられたユーザー識別子として接続します。 1 人のユーザーには、SignalR のアプリに複数の接続を持つことができます。 たとえば、ユーザーが自分のデスクトップと携帯電話接続でした。 各デバイスには、別々 の SignalR 接続がすべて、同じユーザーに関連付けられています。 メッセージは、ユーザーに送信される、すべてのユーザーに関連付けられている接続と、メッセージが表示されます。 接続のユーザー id によってアクセスできる、`Context.UserIdentifier`ハブ内のプロパティ。

ユーザー id を渡すことによって、特定のユーザーにメッセージを送信、`User`次の例で示すように、ハブ メソッドで機能します。

> [!NOTE]
> ユーザー識別子は、大文字小文字を区別します。

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

ユーザー識別子を作成してカスタマイズできる、 `IUserIdProvider`、登録することで、`ConfigureServices`します。

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> カスタム SignalR サービスを登録する前に、AddSignalR を呼び出す必要があります。

## <a name="groups-in-signalr"></a>SignalR でグループ

グループは、名前に関連付けられている接続のコレクションです。 グループ内のすべての接続にメッセージを送信できます。 グループは、グループは、アプリケーションによって管理されるため、接続または複数の接続に送信することをお勧めの方法です。 接続は、複数のグループのメンバーであることができます。 これによりグループ理想的なチャット アプリケーションのようなものは、各部屋をグループとして表現できます。 接続を追加またはを使用してグループから削除することができます、`AddToGroupAsync`と`RemoveFromGroupAsync`メソッド。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

接続が再接続されると、グループ メンバーシップは保持されません。 接続が再確立されているときに、グループに再度参加する必要があります。 この情報は、アプリケーションが複数のサーバーにスケーリングされる場合に使用ではないため、グループのメンバーをカウントすることはできません。

リソースへのアクセスを保護するグループを使用しているときに、使用[認証と承認](xref:signalr/authn-and-authz)ASP.NET Core で機能します。 追加した場合のみユーザーをグループに、資格情報がそのグループの有効な場合、そのグループに送信されるメッセージは承認されたユーザーにのみ移動します。 ただし、グループは、セキュリティ機能ではありません。 認証要求には、有効期限と取り消しなど、グループにはない機能があります。 グループにアクセスするユーザーのアクセス許可が取り消された場合は、手動で検出されると、グループから削除する必要があります。

> [!NOTE]
> グループの名前が大文字小文字を区別します。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:tutorials/signalr)
* [ハブ](xref:signalr/hubs)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
