---
uid: web-api/overview/advanced/http-cookies
title: "ASP.NET Web API で HTTP Cookie |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: e17c51946a268aa13ec035d18dc516928c9f4419
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="http-cookies-in-aspnet-web-api"></a>ASP.NET Web API で HTTP Cookie
====================
によって[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、Web API で HTTP クッキーを送受信する方法について説明します。

## <a name="background-on-http-cookies"></a>HTTP クッキーの背景

このセクションでは、HTTP レベルでは cookie を実装する方法の概要を説明します。 詳細についてを参照してください[RFC 6265](http://tools.ietf.org/html/rfc6265)です。

Cookie とは、サーバーは、HTTP 応答で送信されるデータの一部です。 (省略可能) クライアントは、cookie を格納し、subsequet 要求に返します。 これにより、クライアントとサーバー状態を共有できます。 Cookie を設定するには、サーバーは、応答に Set-cookie ヘッダーを含めます。 Cookie の形式は、省略可能な属性の名前と値ペアです。 例:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

属性の使用例を次に示します。

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

以降の要求で、クッキーをクッキー ヘッダーに、サーバー、クライアント掲載されていますに戻ります。

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP 応答には、複数の Set-cookie ヘッダーを含めることができます。

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

クライアントでは、Cookie のヘッダーを 1 つを使用して複数の cookie を返します。

[!code-console[Main](http-cookies/samples/sample5.cmd)]

Set-cookie ヘッダーで次の属性によって、スコープと cookie の期間が制御されます。

- **ドメイン**: のどのドメインは、cookie を受信する必要がありますがクライアントに通知します。 たとえば、ドメインが"example.com"の場合は、クライアントは、example.com のすべてのサブドメインに cookie を返します。指定しない場合、ドメインは、元のサーバーです。
- **パス**: ドメイン内で指定されたパスに cookie を制限します。 指定されていない場合、要求 URI のパスが使用されます。
- **有効期限が切れる**: cookie の有効期限を設定します。 クライアントは、有効期限が切れるときに、cookie を削除します。
- **Max-age**: cookie の最大有効期間を設定します。 クライアントは、最大有効期間に達したときに、cookie を削除します。

両方`Expires`と`Max-Age`設定されている`Max-Age`が優先されます。 どちらも設定されている場合、クライアントは、現在のセッションの終了時に、cookie を削除します。 (「セッション」の意味は、ユーザー エージェントによって決まります。)

ただし、クライアントがクッキーを無視することがある注意してください。 たとえば、ユーザーには、プライバシー保護のための cookie が無効にする可能性があります。 クライアントは、有効期限、または保存されている cookie の数を制限する前に、cookie を削除できます。 プライバシー上の理由から、クライアントは多くの場合、ドメインが、元のサーバーと一致しません、「サードパーティ」の cookie を拒否します。 つまり、サーバーでは、上に設定する cookie を取得する保証はありません。

## <a name="cookies-in-web-api"></a>Web API の cookie

HTTP 応答に cookie を追加するには、作成、 **CookieHeaderValue** cookie を表すインスタンス。 まず、 **AddCookies**で定義されている拡張メソッド、 **System.Net.Http です。HttpResponseHeadersExtensions**クラスに、cookie を追加します。

たとえば、次のコードは、コント ローラーのアクション内の cookie を追加します。

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

注意して**AddCookies**の配列を受け取る**CookieHeaderValue**インスタンス。

クライアントの要求から cookie を抽出する呼び出し、 **GetCookies**メソッド。

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue**のコレクションを格納**CookieState**インスタンス。 各**CookieState** 1 つの cookie を表します。 取得するインデクサー メソッドを使用して、 **CookieState**名前で示すようにします。

## <a name="structured-cookie-data"></a>構造化された Cookie のデータ

ブラウザーの多くは、数の cookie が格納されます &#8212; 両方総数、およびドメインあたりの数を制限します。 したがって、複数の cookie を設定する代わりに、1 つの cookie に構造化データを格納すると便利ですができます。

> [!NOTE]
> RFC 6265 は、cookie のデータの構造を定義しません。


使用して、 **CookieHeaderValue**クラス、cookie のデータの名前と値のペアの一覧を渡すことができます。 これらの名前と値のペアが Set-cookie ヘッダーの URL でエンコードされたフォーム データとしてエンコードされます。

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

前のコードは、次の Set-cookie ヘッダーを生成します。

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState**クラス値を読み取る、サブ要求メッセージ内の cookie からインデクサー メソッドを提供します。

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>例: 設定し、メッセージのハンドラーで Cookie を取得

前の例では、Web API コント ローラー内から cookie を使用する方法を示しました。 別のオプションは、使用する[メッセージ ハンドラー](http-message-handlers.md)です。 コント ローラーよりも、パイプラインの初期には、メッセージのハンドラーが呼び出されます。 メッセージ ハンドラーでは、コント ローラーに到達する前に、要求の cookie を読み取るしたり、コント ローラーが応答を生成した後、応答に cookie を追加することができます。

![](http-cookies/_static/image2.png)

次のコードでは、セッション Id を作成するためのメッセージ ハンドラーを示します。 セッション ID は、cookie に格納されます。 ハンドラーでは、セッションの cookie の要求を確認します。 ハンドラーが新しいセッション ID を生成する要求にクッキーが含まれていない場合 どちらの場合、ハンドラーが内のセッション ID を格納、 **HttpRequestMessage.Properties**プロパティ バッグ。 また、セッション クッキーを HTTP 応答に追加します。

この実装では、クライアントからのセッション ID がサーバーによって実際に発行されたことを検証しません。 認証の形式として使用しません。 例のポイントでは、HTTP クッキー管理を説明します。

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

コント ローラーからセッション ID を取得できます、 **HttpRequestMessage.Properties**プロパティ バッグ。

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
