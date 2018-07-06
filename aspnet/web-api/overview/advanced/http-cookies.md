---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API の HTTP Cookie |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 21ba186c11f39bbeedd1c320b98476ba13af27e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819161"
---
<a name="http-cookies-in-aspnet-web-api"></a>ASP.NET Web API の HTTP Cookie
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、Web API で HTTP クッキーを送受信する方法について説明します。

## <a name="background-on-http-cookies"></a>HTTP クッキーの背景

ここでは、HTTP レベルで cookie を実装する方法について概説します。 詳細についてを参照してください[RFC 6265](http://tools.ietf.org/html/rfc6265)します。

Cookie とは、サーバーが HTTP 応答で送信するデータの一部です。 (省略可能)、クライアントはクッキーを格納し、subsequet 要求で返します。 これにより、クライアントとサーバー状態を共有できます。 サーバーには cookie を設定するには、応答 Set-cookie ヘッダーが含まれています。 Cookie の形式は、省略可能な属性を持つ、名前と値のペアです。 例えば:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

属性を持つ例を次に示します。

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

クライアントにはサーバーに返す cookie は、以降の要求で Cookie ヘッダーが含まれています。

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP 応答には、複数 Set-cookie ヘッダーを含めることができます。

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

クッキー ヘッダーを 1 つを使用して複数の cookie をクライアントに返します。

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Set-cookie ヘッダーで次の属性では、スコープと cookie の期間が制御されます。

- **ドメイン**: どのドメインに cookie を受け取るクライアントに通知します。 たとえば、ドメインが"example.com"の場合は、クライアントは、example.com のすべてのサブドメインに cookie を返します。 指定しない場合、ドメイン、配信元サーバーです。
- **パス**: cookie をドメイン内で指定されたパスに制限します。 指定されていない場合は、要求 URI のパスが使用されます。
- **有効期限が切れる**: cookie の有効期限を設定します。 クライアントは、有効期限が切れた cookie を削除します。
- **Max Age**: cookie の最大有効期間を設定します。 クライアントは、最大有効期間に達すると、クッキーを削除します。

両方`Expires`と`Max-Age`が設定されている`Max-Age`が優先されます。 どちらも設定されている場合、クライアントは、現在のセッションの終了時に cookie を削除します。 (「セッション」の厳密な意味は、ユーザー エージェントによって決まります。)

ただし、クライアントがクッキーを無視することこともあります。 たとえば、ユーザーには、プライバシー保護のための cookie が無効にする可能性があります。 クライアントは、有効期限、または格納されているクッキーの数を制限する前に、cookie を削除できます。 プライバシー上の理由から、クライアントは多くの場合、ドメインが配信元サーバーと一致しません、「サードパーティ」の cookie を拒否します。 簡単に言えば、これを設定する cookie を取得するのには、サーバーが依存する必要があります。

## <a name="cookies-in-web-api"></a>Web API の cookie

クッキーを HTTP 応答に追加するには、作成、 **CookieHeaderValue**クッキーを表すインスタンス。 呼び出して、 **AddCookies**で定義されている拡張メソッド、 **System.Net.Http します。HttpResponseHeadersExtensions**クラスに、cookie を追加します。

たとえば、次のコードでは、コント ローラー アクション内のクッキーが追加されます。

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

注意**AddCookies**の配列を受け取る**CookieHeaderValue**インスタンス。

クライアント要求から cookie を抽出する呼び出し、 **GetCookies**メソッド。

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue**のコレクションを含む**CookieState**インスタンス。 各**CookieState** cookie の 1 つを表します。 取得するインデクサー メソッドを使用して、 **CookieState**名前で示すようにします。

## <a name="structured-cookie-data"></a>構造化された Cookie のデータ

多くのブラウザーの制限数の cookie が格納されます&#8212;合計の数と、ドメインごとの数。 したがって、複数の cookie を設定する代わりに、1 つの cookie に構造化データを格納するのに便利ですができます。

> [!NOTE]
> RFC 6265 は、cookie のデータの構造を定義しません。


使用して、 **CookieHeaderValue**クラス、cookie のデータの名前と値のペアの一覧を渡すことができます。 これらの名前と値のペアは、Set-cookie ヘッダーの URL でエンコードされたフォーム データとしてエンコードされます。

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

前のコードでは、次の Set-cookie ヘッダーが生成されます。

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState**クラス インデクサー要求メッセージ内の cookie からサブの値を読み取るメソッドを提供します。

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>例: 設定し、メッセージ ハンドラーで Cookie を取得します。

前の例では、Web API コント ローラー内から cookie を使用する方法を示しました。 別のオプションは、使用する[メッセージ ハンドラー](http-message-handlers.md)します。 コント ローラーとパイプラインの初期には、メッセージのハンドラーが呼び出されます。 メッセージ ハンドラーは、コント ローラーに到達する前に、cookie を要求から読み取りまたはコント ローラーが応答を生成した後、応答に cookie を追加できます。

![](http-cookies/_static/image2.png)

次のコードでは、セッション Id を作成するためのメッセージ ハンドラーを示します。 セッション ID は、cookie に格納されます。 ハンドラーは、セッション cookie の要求を確認します。 ハンドラーが新しいセッション ID を生成する要求にクッキーが含まれていない場合 どちらの場合、ハンドラーが内のセッション ID を格納、 **HttpRequestMessage.Properties**プロパティ バッグ。 また、セッション クッキーを HTTP 応答に追加します。

この実装では、クライアントからのセッション ID がサーバーによって実際に発行されたことを検証しません。 認証の形式として使用しないでください。 この例のポイントでは、HTTP クッキー管理を説明します。

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

コント ローラーからのセッション ID を取得できます、 **HttpRequestMessage.Properties**プロパティ バッグ。

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
