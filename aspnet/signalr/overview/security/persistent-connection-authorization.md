---
uid: signalr/overview/security/persistent-connection-authorization
title: "SignalR 固定接続の認証と承認 |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、永続的な接続の承認を適用する方法について説明します。 概要については、SignalR アプリケーションへのセキュリティの統合しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>SignalR 固定接続の認証と承認
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、永続的な接続の承認を適用する方法について説明します。 SignalR アプリケーションへのセキュリティの統合の詳細については、次を参照してください。[セキュリティの概要](introduction-to-security.md)です。 
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


## <a name="enforce-authorization"></a>承認を適用します。

使用する場合は、承認規則を適用する、 [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)オーバーライドする必要があります、`AuthorizeRequest`メソッドです。 使用することはできません、`Authorize`永続的な接続の属性です。 `AuthorizeRequest`メソッドが要求された操作を実行するユーザーが許可されていることを確認するすべての要求の前に、SignalR フレームワークによって呼び出されます。 `AuthorizeRequest`メソッドは、クライアントからは呼び出されません以外の場合は、アプリケーションの標準の認証メカニズムにより、ユーザーを認証する代わりに、します。

次の例では、認証されたユーザーに要求を制限する方法を示します。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

AuthorizeRequest メソッドの任意のカスタマイズされた承認ロジックを追加することができます。など、ユーザーが特定のロールに属しているかどうかを確認しています。
