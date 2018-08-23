---
uid: signalr/overview/security/persistent-connection-authorization
title: SignalR 永続的な接続の認証と承認 |Microsoft Docs
author: pfletcher
description: このトピックでは、永続的な接続の承認を適用する方法を説明します。 概要については、SignalR アプリケーションでは、セキュリティと統合しています.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: e7ae160cbe4c5f6cdb393768758f5bdec4203dbf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831966"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>SignalR 永続的な接続の認証と承認
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、永続的な接続の承認を適用する方法を説明します。 SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](introduction-to-security.md)します。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 2 のバージョン
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[以前のバージョンの SignalR](../older-versions/index.md)します。
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


## <a name="enforce-authorization"></a>承認を適用します。

使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッド。 使用することはできません、`Authorize`永続的な接続を持つ属性です。 `AuthorizeRequest`メソッドは要求ごとに、要求された操作を実行するユーザーが許可されていることを確認する前に SignalR フレームワークによって呼び出されます。 `AuthorizeRequest`メソッドは、クライアントからは呼び出されません。 代わりに、アプリケーションの標準的な認証メカニズムを通じてユーザーを認証します。

次の例では、認証されたユーザーへの要求に制限する方法を示します。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

AuthorizeRequest メソッドで、カスタマイズされた承認ロジックを追加することができます。次のように、ユーザーが特定のロールに属しているかどうかを確認しています。
