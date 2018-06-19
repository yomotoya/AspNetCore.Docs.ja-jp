---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 固定接続の認証と承認 (SignalR 1.x) |Microsoft ドキュメント
author: pfletcher
description: このトピックでは、永続的な接続の承認を適用する方法について説明します。 概要については、SignalR アプリケーションへのセキュリティの統合しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 2e97dfd03c61b110325c41a992b4af490fcd17de
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036103"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>SignalR 固定接続の認証と承認 (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、永続的な接続の承認を適用する方法について説明します。 SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](index.md)です。


## <a name="enforce-authorization"></a>承認を適用します。

使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッドです。 使用することはできません、`Authorize`永続的な接続の属性です。 `AuthorizeRequest`メソッドが要求された操作を実行するユーザーが許可されていることを確認するすべての要求の前に、SignalR フレームワークによって呼び出されます。 `AuthorizeRequest`メソッドは、クライアントからは呼び出されません以外の場合は、アプリケーションの標準の認証メカニズムにより、ユーザーを認証する代わりに、します。

次の例では、認証されたユーザーに要求を制限する方法を示します。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

AuthorizeRequest メソッドの任意のカスタマイズされた承認ロジックを追加することができます。など、ユーザーが特定のロールに属しているかどうかを確認しています。
