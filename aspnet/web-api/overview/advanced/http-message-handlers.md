---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET web API HTTP メッセージ ハンドラー |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506951"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>ASP.NET web API HTTP メッセージ ハンドラー
====================
によって[Mike Wasson](https://github.com/MikeWasson)

A*メッセージ ハンドラー* HTTP 要求を受信して、HTTP 応答を返すクラスです。 メッセージ ハンドラーは、抽象型から派生**HttpMessageHandler**クラスです。

通常は、一連のメッセージのハンドラーを連結します。 最初のハンドラーが HTTP 要求を受信するには、いくつかの処理を行うおよび要求は、次のハンドラーにします。 ある時点で、応答が作成され、チェーンの上に戻ります。 このパターンが呼び出された、*委任*ハンドラー。

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>サーバー側のメッセージ ハンドラー

サーバー側では、Web API パイプラインは、いくつか組み込まれているメッセージのハンドラーを使用します。

- **HttpServer**ホストから要求を取得します。
- **HttpRoutingDispatcher**ルートに基づいて要求をディスパッチします。
- **HttpControllerDispatcher** Web API コント ローラーに要求を送信します。

パイプラインにカスタム ハンドラーを追加することができます。 メッセージ ハンドラーは、HTTP メッセージ (なくコント ローラーのアクション) のレベルで動作する横断的関心事に適してします。 たとえば、メッセージのハンドラーがあります。

- 読み取りまたは要求ヘッダーを変更します。
- 応答ヘッダーを応答に追加します。
- コント ローラーに到達する前に、要求を検証します。

この図は、パイプラインに挿入された 2 つのカスタム ハンドラーを示しています。

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> クライアント側で HttpClient もメッセージ ハンドラーを使用します。 詳細については、次を参照してください。 [HttpClient メッセージ ハンドラー](httpclient-message-handlers.md)です。


## <a name="custom-message-handlers"></a>カスタム メッセージ ハンドラー

派生するカスタム メッセージのハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッドです。 このメソッドのシグネチャは次のとおりです。

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

メソッドは、 **HttpRequestMessage**として入力非同期に返す、 **HttpResponseMessage**です。 一般的な実装は次のとおり

1. 要求メッセージを処理します。
2. 呼び出す`base.SendAsync`内部ハンドラーに、要求を送信します。
3. 内部ハンドラーでは、応答メッセージを返します。 (この手順は、非同期です)。
4. 応答を処理し、呼び出し元に返します。

簡単な例を次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> 呼び出し`base.SendAsync`は非同期です。 場合は、ハンドラーは、この呼び出しの後の作業を使用して、 **await**キーワードで示すようにします。


委任のハンドラーも内部ハンドラーをスキップし、直接応答を作成します。

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

ハンドラーが呼び出さずに応答を作成する場合は、委任`base.SendAsync`、要求パイプラインの残りの部分をスキップします。 要求を検証します (エラー応答の作成) のハンドラーに役立つことができます。

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>パイプラインにハンドラーを追加します。

サーバー側でメッセージのハンドラーを追加するハンドラーを追加、 **HttpConfiguration.MessageHandlers**コレクション。 「ASP.NET MVC 4 Web アプリケーション」テンプレートを使用して、プロジェクトを作成した場合は、この内側を行うことができます、 **WebApiConfig**クラス。

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

メッセージ ハンドラーは記載されているのと同じ順序で呼び出されます**MessageHandlers**コレクション。 ネストされているため、応答メッセージは逆方向に移動します。 その最後のハンドラーは、最初に応答メッセージを取得します。

内部ハンドラー; を設定する必要がないことに注意してください。Web API フレームワークでは、メッセージのハンドラーに自動的に接続します。

場合[自己ホスト型](../older-versions/self-host-a-web-api.md)のインスタンスを作成、 **HttpSelfHostConfiguration**クラスし、へハンドラーを追加、 **MessageHandlers**コレクション。

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

これでカスタム メッセージ ハンドラーの例をいくつか見てみましょう。

## <a name="example-x-http-method-override"></a>例: X-HTTP のメソッドのオーバーライド

X HTTP メソッド オーバーライドは、標準 HTTP ヘッダーです。 PUT や DELETE など、特定の HTTP 要求種類を送信できないクライアントに設計されています。 代わりに、クライアントは POST 要求を送信し、目的のメソッドに X HTTP メソッド オーバーライドのヘッダーを設定します。 例:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

X HTTP メソッド オーバーライドのサポートを追加するメッセージ ハンドラーを次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

**SendAsync**メソッド、ハンドラーは、セルの内容かどうか、要求メッセージが POST 要求では、X HTTP メソッド オーバーライド ヘッダーが含まれているかどうか。 場合は、ヘッダー値を検証し、要求メソッドを変更します。 最後に、ハンドラーが呼び出され`base.SendAsync`にメッセージを次のハンドラーに渡します。

要求に達したとき、 **HttpControllerDispatcher**クラス、 **HttpControllerDispatcher**更新要求のメソッドに基づく要求をルーティングします。

## <a name="example-adding-a-custom-response-header"></a>例: カスタム応答ヘッダーを追加します。

すべての応答メッセージにカスタム ヘッダーを追加するメッセージ ハンドラーを次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

最初に、ハンドラーを呼び出す`base.SendAsync`内部メッセージ ハンドラーに要求を渡す。 内部ハンドラーが、応答メッセージを返しますが、これは、非同期に実行を使用して、**タスク&lt;T&gt;** オブジェクト。 応答メッセージがまで利用できない`base.SendAsync`非同期的に完了します。

この例では、 **await**作業を実行するキーワード後に非同期的に`SendAsync`が完了します。 .NET Framework 4.0 を対象とする場合を使用して、**タスク**&lt;T&gt;**です。ContinueWith**メソッド。

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>例: の API キーの確認

一部の web サービスでは、クライアントがその要求に API キーを含める必要があります。 次の例では、メッセージのハンドラーが有効な API キーの要求を確認する方法を示します。

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

このハンドラーは、URI クエリ文字列内の API キーを検索します。 (この例では、ものとキーは、静的な文字列であります。 実際の実装はおそらく使用より複雑な検証します。)クエリ文字列にキーが含まれている場合、ハンドラーは要求を内部ハンドラーに渡します。

ハンドラーの応答メッセージをステータス 403、作成要求が有効なキーを持たない場合は許可されていません。 この場合、ハンドラーを呼び出しません`base.SendAsync`、内部ハンドラーが決して要求を受け取りようにまたはコント ローラー。 そのため、コント ローラーでは、すべての着信要求が有効な API キーを持っていることを想定できます。

> [!NOTE]
> API キーは、特定のコント ローラー アクションにのみ適用する場合は、メッセージ ハンドラーではなくアクション フィルターの使用を検討します。 アクション フィルターは、URI のルーティングが実行された後に実行されます。


## <a name="per-route-message-handlers"></a>ルートごとのメッセージ ハンドラー

ハンドラー、 **HttpConfiguration.MessageHandlers**コレクションをグローバルに適用します。

または、ルートを定義するときは、特定のルートにメッセージ ハンドラーを追加できます。

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

この例では、要求の URI"Route2"に一致する場合、要求にディスパッチされる`MessageHandler2`です。 次の図は、これら 2 つのルートのパイプラインを示しています。

![](http-message-handlers/_static/image4.png)

注意して`MessageHandler2`既定値を置き換えます**HttpControllerDispatcher**です。 この例では`MessageHandler2`応答を作成し、コント ローラーには決して"Route2"一致する要求。 これにより、独自のカスタム エンドポイントを持つ Web API コント ローラーのメカニズムを全体を置換できます。

または、ルート別のメッセージ ハンドラーに委任できます**HttpControllerDispatcher**、コント ローラーを次にディスパッチします。

![](http-message-handlers/_static/image5.png)

次のコードは、このルートを構成する方法を示しています。

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
