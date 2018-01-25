---
uid: signalr/overview/older-versions/hub-authorization
title: "SignalR ハブの認証と承認 (SignalR 1.x) |Microsoft ドキュメント"
author: pfletcher
description: "このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: d73ab6c9091556a62e5d9475baf67a18e305585f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>SignalR ハブの認証と承認 (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

> このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。


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

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。 この属性にある、`Microsoft.AspNet.SignalR`名前空間。 適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。 適用すると、`Authorize`ハブ クラス、指定した承認要件に属性はすべてのハブ メソッドに適用します。 適用可能な承認要件の種類は次のとおりです。 なし、`Authorize`属性、ハブのすべてのパブリック メソッドがハブに接続されているクライアントで使用します。

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

アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド アプリケーションの起動時にします。 複数のハブがあり、それらのすべての認証要件を強制する場合は、このメソッドを使用する場合があります。 このメソッドを使用して役割、ユーザー、または送信の承認を指定することはできません。 のみ、ハブ メソッドへのアクセスが認証されたユーザーに制限されていることを指定することができます。 ただし、Authorize 属性には、ハブやその他の要件を指定する方法にも適用できます。 属性で指定したすべての要件は、認証の基本的な要件に加えて適用されます。

次の例では、すべてのハブ メソッドを認証されたユーザーに制限が Global.asax ファイルを示します。

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

呼び出す場合は、 `RequireAuthentication()` SignalR 要求が処理された後、SignalR をスロー、`InvalidOperationException`例外。 パイプラインを呼び出した後に、モジュールを HubPipeline に追加することはできませんので、この例外がスローされます。 前の例では、通話、`RequireAuthentication`メソッドで、`Application_Start`最初の要求を処理する前に 1 回実行されるメソッド。

<a id="custom"></a>

## <a name="customized-authorization"></a>カスタマイズした承認

派生するクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッドです。 このメソッドは、ユーザーに承認要求を完了するかどうかを判断するには、各要求に対して呼び出されます。 オーバーライドされたメソッドでは、承認のシナリオに必要なロジックを提供します。 次の例では、クレーム ベース id を介して承認を適用する方法を示します。

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

.NET クライアントは、ASP.NET フォーム認証を使用するハブと対話するとき、は、接続の認証クッキーを手動で設定する必要があります。 Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。 次の例では、web ページから、認証 cookie を取得し、接続にその cookie を追加するコンソール アプリを示します。 URL `https://www.contoso.com/RemoteLogin` web ページを作成する必要がありますをする例をポイントします。 ページは、ポストされたユーザー名とパスワードを取得し、資格情報を持つユーザーにログインしようとしています。

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

コンソール アプリは、次の分離コード ファイルを含む空のページを参照する www.contoso.com/RemoteLogin する資格情報を送信します。

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows 認証

Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)プロパティです。 される DefaultCredentials の値には、接続の資格情報を設定します。

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>接続ヘッダー

アプリケーションが cookie を使用していない場合は、接続ヘッダーのユーザー情報を渡すことができます。 たとえば、接続のヘッダーにトークンを渡すことができます。

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

次に、ハブでは、ユーザーのトークンを確認します。

<a id="certificate"></a>

### <a name="certificate"></a>証明書

ユーザーを確認するクライアント証明書を渡すことができます。 接続を作成するときに、証明書を追加します。 次の例では、接続にクライアント証明書を追加する方法のみコンソールへのフル アプリケーションは表示されません。 使用して、 [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)証明書を作成するいくつかの方法を提供するクラス。

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
