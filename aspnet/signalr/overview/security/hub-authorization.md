---
uid: signalr/overview/security/hub-authorization
title: "SignalR ハブの認証と承認 |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。 ソフトウェアのバージョンは、このトピックで Visual Studio 2013 .NET 4.5 SignalR ve を使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/05/2015
ms.topic: article
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: f1538c933ff9e8e680d70ce1e63d24b189be47e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-hubs"></a>SignalR ハブの認証と承認
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。 
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

このトピックは、次のセクションで構成されています。

- [属性を承認します。](#authorizeattribute)
- [すべてのハブに対して認証を要求します。](#requireauth)
- [カスタマイズした承認](#custom)
- [クライアントに認証情報を渡す](#passauth)
- [.NET クライアントの認証オプション](#authoptions)

    - [フォーム認証 cookie](#cookie)
    - [Windows 認証](#windows)
    - [接続ヘッダー](#header)
    - [証明書](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>属性を承認します。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。 この属性にある、`Microsoft.AspNet.SignalR`名前空間。 適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。 適用すると、`Authorize`ハブ クラス、指定した承認要件に属性はすべてのハブ メソッドに適用します。 このトピックでは、適用可能な承認要件のさまざまな種類の例を示します。 なし、`Authorize`属性、接続されたクライアントがハブのすべてのパブリック メソッドにアクセスできます。

Web アプリケーションで"Admin"をという名前のロールを定義している場合は、次のコード ハブをそのロールのユーザーのみがアクセスできることを指定できます。

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

または、すべてのユーザーに提供される 1 つのメソッドと 2 番目のメソッドだけに提供される認証されたユーザーは、次に示すようにハブが含まれているを指定することができます。

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

次の例は、さまざまな承認シナリオに対応します。

- `[Authorize]`– 認証されたユーザーのみ
- `[Authorize(Roles = "Admin,Manager")]`– 指定したロールのユーザーを認証されたのみ
- `[Authorize(Users = "user1,user2")]`– 指定されたユーザー名を持つユーザーを認証されたのみ
- `[Authorize(RequireOutgoing=false)]`– だけで認証されたユーザーは、ハブを呼び出すことができますが、サーバーからクライアントに返送の呼び出しによる制限はありません、承認など、特定のユーザーだけがメッセージを送信できますが、他のすべてのユーザー メッセージが表示されることができます。 RequireOutgoing プロパティは、ハブ内の個人用メソッドではなく、hub 全体にのみ適用できます。 RequireOutgoing が false に設定されていない、サーバーから承認要件を満たすユーザーのみが呼び出されます。

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>すべてのハブに対して認証を要求します。

アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド アプリケーションの起動時にします。 複数のハブがあり、それらのすべての認証要件を強制する場合は、このメソッドを使用する場合があります。 このメソッドを使用して役割、ユーザー、または送信の承認の要件を指定できません。 のみ、ハブ メソッドへのアクセスが認証されたユーザーに制限されていることを指定することができます。 ただし、Authorize 属性には、ハブやその他の要件を指定する方法にも適用できます。 属性で指定したすべての要件は、認証の基本的な要件に追加されます。

次の例では、すべてのハブ メソッドを認証されたユーザーに制限するスタートアップ ファイルを示します。

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

呼び出す場合は、 `RequireAuthentication()` SignalR 要求が処理された後、SignalR をスロー、`InvalidOperationException`例外。 SignalR は、パイプラインを呼び出した後に、モジュールを HubPipeline に追加することはできませんので、この例外をスローします。 前の例では、通話、`RequireAuthentication`メソッドで、`Configuration`最初の要求を処理する前に 1 回実行されるメソッド。

<a id="custom"></a>

## <a name="customized-authorization"></a>カスタマイズした承認

派生するクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッドです。 要求ごとには、SignalR は、ユーザーに承認要求を完了するかどうかを決定するには、このメソッドを呼び出します。 オーバーライドされたメソッドでは、承認のシナリオに必要なロジックを提供します。 次の例では、クレーム ベース id を介して承認を適用する方法を示します。

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>クライアントに認証情報を渡す

クライアントで実行されるコードでの認証情報を使用する必要があります。 クライアントで、メソッドを呼び出すときに、必要な情報を渡します。 たとえば、チャット アプリケーション メソッドでしたをパラメーターとして渡す、メッセージの送信を行う人物のユーザー名次のようにします。

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

または、次に示すように、認証情報を表し、そのオブジェクトをパラメーターとして渡しオブジェクトを作成することができます。

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

悪意のあるユーザーを使用すると、そのクライアントからの要求を模倣するために使用できなかったと他のクライアントに 1 つのクライアント接続 id を渡してください。

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET クライアントの認証オプション

認証されたユーザーに限られているハブをやり取りするコンソール アプリなどの .NET クライアントがある場合は、cookie、接続のヘッダー、または証明書で認証資格情報を渡すことができます。 このセクションの例では、これらのさまざまなメソッドを使用してユーザーを認証する方法を示します。 SignalR アプリケーションを完全に機能はありません。 SignalR の .NET クライアントの詳細については、次を参照してください。 [Hubs API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)です。

<a id="cookie"></a>

### <a name="cookie"></a>クッキー

.NET クライアントは、ASP.NET フォーム認証を使用するハブと対話するとき、は、接続の認証クッキーを手動で設定する必要があります。 Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。 次の例では、web ページから、認証 cookie を取得し、接続にその cookie を追加するコンソール アプリを示します。

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

コンソール アプリに資格情報の記事**www.contoso.com/RemoteLogin**を次の分離コード ファイルを含む空のページを参照する可能性があります。

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows 認証

Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/en-us/library/system.net.credentialcache.defaultcredentials.aspx)プロパティです。 される DefaultCredentials の値には、接続の資格情報を設定します。

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>接続ヘッダー

アプリケーションが cookie を使用していない場合は、接続ヘッダーのユーザー情報を渡すことができます。 たとえば、接続のヘッダーにトークンを渡すことができます。

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

次に、ハブでは、ユーザーのトークンを確認します。

<a id="certificate"></a>

### <a name="certificate"></a>証明書

ユーザーを確認するクライアント証明書を渡すことができます。 接続を作成するときに、証明書を追加します。 次の例では、接続にクライアント証明書を追加する方法のみコンソールへのフル アプリケーションは表示されません。 使用して、 [X509Certificate](https://msdn.microsoft.com/en-us/library/system.security.cryptography.x509certificates.x509certificate.aspx)証明書を作成するいくつかの方法を提供するクラス。

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
