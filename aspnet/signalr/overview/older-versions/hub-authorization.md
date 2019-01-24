---
uid: signalr/overview/older-versions/hub-authorization
title: SignalR ハブの認証と承認 (SignalR 1.x) |Microsoft Docs
author: bradygaster
description: このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7f4a76109111f19dc4381ad01e642afdabade336
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836900"
---
<a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a>SignalR ハブの認証と承認 (SignalR 1.x)
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このトピックでは、どのユーザーまたはロールがハブ メソッドにアクセスできる制限する方法について説明します。


## <a name="overview"></a>概要

このトピックは、次のセクションで構成されています。

- [属性を承認します。](#authorizeattribute)
- [すべてのハブの認証が必要です。](#requireauth)
- [カスタマイズされた承認](#custom)
- [クライアントに認証情報を渡す](#passauth)
- [.NET クライアントの認証オプション](#authoptions)

    - [フォーム認証 cookie](#cookie)
    - [Windows 認証](#windows)
    - [接続ヘッダー](#header)
    - [証明書](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>属性を承認します。

SignalR の提供、 [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx)ハブまたはメソッドへのアクセスを持つユーザーまたはロールを指定する属性。 この属性にある、`Microsoft.AspNet.SignalR`名前空間。 適用する、`Authorize`属性をハブまたはハブ内の特定のメソッドのいずれか。 適用すると、 `Authorize` hub クラスでは、指定された承認要件を属性のすべてのハブ メソッドに適用されます。 適用できる承認要件のさまざまな種類は、以下に示します。 なし、`Authorize`属性、ハブのすべてのパブリック メソッドは、ハブに接続されているクライアントで使用します。

Web アプリケーションで"Admin"をという名前のロールを定義している場合は、次のコードでのハブをそのロール内のユーザーのみがアクセスできることを指定できます。

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

または、すべてのユーザーに提供される 1 つのメソッドと 2 番目のメソッドのみが利用できる認証されたユーザーは、次に示すようにハブが含まれているを指定することができます。

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

次の例は、さまざまな承認シナリオに対処します。

- `[Authorize]` -認証されたユーザーのみ
- `[Authorize(Roles = "Admin,Manager")]` – 指定したロールのユーザーを認証されたのみ
- `[Authorize(Users = "user1,user2")]` – 指定されたユーザー名を持つユーザーを認証されたのみ
- `[Authorize(RequireOutgoing=false)]` – だけで認証されたユーザーは、ハブで呼び出すことができますが、サーバーからクライアントに返送の呼び出しによる制限はありません、承認など、特定のユーザーだけがメッセージを送信できますが、他のすべてのユーザー メッセージが表示されることができます。 RequireOutgoing プロパティは、ハブ内の個人メソッドではなく、全体のハブにのみ適用できます。 RequireOutgoing が false に設定されていない場合は、サーバーから承認要件を満たすユーザーのみが呼び出されます。

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>すべてのハブの認証が必要です。

アプリケーションで呼び出すことによってすべてのハブおよびハブ メソッドの認証を要求できます、 [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx)メソッド、アプリケーションの起動時にします。 複数のハブおよびすべての認証の要件を適用するした場合、このメソッドを使用する場合があります。 この方法では、ロール、ユーザー、または送信の承認を指定できません。 ハブ メソッドへのアクセスが認証されたユーザーに制限のみ指定することができます。 ただし、ハブまたはその他の要件を指定するメソッドに、Authorize 属性を適用できます。 属性で指定する必要は、認証の基本的な要件に加えて適用されます。

次の例では、すべてのハブ メソッドを認証されたユーザーに制限する Global.asax ファイルを示します。

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

呼び出す場合、`RequireAuthentication()`メソッド SignalR 要求の処理が完了したら、SignalR がスローされます、`InvalidOperationException`例外。 パイプラインが呼び出された後、モジュールを HubPipeline に追加することはできませんので、この例外がスローされます。 前の例では、呼び出しを示しています、`RequireAuthentication`メソッドで、`Application_Start`メソッドの最初の要求を処理する前に 1 回実行されます。

<a id="custom"></a>

## <a name="customized-authorization"></a>カスタマイズされた承認

派生したクラスを作成するには承認を決定する方法をカスタマイズする必要がある場合`AuthorizeAttribute`をオーバーライドし、 [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx)メソッド。 このメソッドは、ユーザーに承認要求を完了するかどうかを判断するには、各要求に対して呼び出されます。 オーバーライドされたメソッドで、承認のシナリオに必要なロジックを提供します。 次の例では、クレーム ベース id 経由で認証を実行する方法を示します。

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>クライアントに認証情報を渡す

クライアントで実行されるコードで認証情報を使用する必要があります。 クライアントでメソッドの呼び出し時に、必要な情報を渡します。 たとえば、チャット アプリケーションの方法でしたをパラメーターとして渡す、メッセージを投稿する人のユーザー名次に示すよう。

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

または、次に示すように認証情報を表し、そのオブジェクトをパラメーターとして渡すオブジェクトを作成することができます。

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

悪意のあるユーザーがそのクライアントからの要求を模倣するために使用できますよう他のクライアントに 1 つのクライアント接続 id を渡してください。

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>.NET クライアントの認証オプション

認証されたユーザーを制限するハブとの対話、により、コンソール アプリなどの .NET クライアントがある場合は、cookie、connection ヘッダー。 または、証明書で認証資格情報を渡すことができます。 このセクションの例では、ユーザーを認証するため、これらのさまざまなメソッドを使用する方法を示します。 完全に機能する SignalR アプリケーションではありません。 SignalR を使って .NET クライアントの詳細については、次を参照してください。[ハブ API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)します。

<a id="cookie"></a>

### <a name="cookie"></a>クッキー

ASP.NET フォーム認証を使用するハブを操作すると、.NET クライアント、接続の認証クッキーを手動で設定する必要があります。 Cookie を追加する、`CookieContainer`プロパティを[HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx)オブジェクト。 次の例では、web ページから、認証 cookie を取得し、接続をそのクッキーを追加します。 コンソール アプリを示します。 URL`https://www.contoso.com/RemoteLogin`で例を作成する必要のある web ページ。 ページは、ポストされたユーザー名とパスワードを取得し、資格情報でユーザーにログインしようとしています。

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

コンソール アプリはでした次のコード ビハインド ファイルを含む空のページを参照している www.contoso.com/RemoteLogin に資格情報を送信します。

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Windows 認証

Windows 認証を使用する場合を使用して現在のユーザーの資格情報を渡すことができます、[される DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx)プロパティ。 される DefaultCredentials の値には、接続の資格情報を設定します。

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>接続ヘッダー

アプリケーションが cookie を使用していない場合は、ユーザー情報を接続ヘッダーで渡すことができます。 たとえば、接続ヘッダーでトークンを渡すことができます。

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

次に、ハブでは、ユーザーのトークンを確認します。

<a id="certificate"></a>

### <a name="certificate"></a>証明書

ユーザーを確認するクライアント証明書を渡すことができます。 接続を作成するときに、証明書を追加します。 次の例では、接続にクライアント証明書を追加する方法のみを示しています。完全なコンソール アプリは表示されません。 使用して、 [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx)クラス、証明書を作成するいくつかの方法を提供します。

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
