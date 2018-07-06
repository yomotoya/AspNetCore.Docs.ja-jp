---
uid: web-api/overview/advanced/http-message-handlers
title: ASP.NET Web API の HTTP メッセージ ハンドラー |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 5694845d6f7f9d868c7b3f83785fddda863756d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821798"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API の HTTP メッセージ ハンドラー
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

A*メッセージ ハンドラー*は HTTP 要求を受信し、HTTP 応答を返すクラスです。 メッセージ ハンドラーは、抽象型から派生**HttpMessageHandler**クラス。

通常、一連のメッセージ ハンドラーが連結されます。 最初のハンドラー HTTP 要求を受信するには、いくつかの処理およびの次のハンドラーへの要求を提供します。 いくつかの時点では、応答が作成され、チェーンの上位に戻ります。 このパターンと呼ばれます、*委任*ハンドラー。

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>サーバー側のメッセージ ハンドラー

サーバー側では、Web API パイプラインは、いくつか組み込まれているメッセージのハンドラーを使用します。

- **HttpServer**ホストから要求を取得します。
- **HttpRoutingDispatcher**ルートに基づいて、要求をディスパッチします。
- **HttpControllerDispatcher** Web API コント ローラーに要求を送信します。

パイプラインにカスタム ハンドラーを追加できます。 メッセージ ハンドラーは HTTP メッセージ (なくコント ローラー アクション) のレベルで操作するには、横断的関心事に適しています。 たとえば、メッセージ ハンドラーでは次の場合があります。

- 読み取りまたは要求ヘッダーを変更します。
- 応答ヘッダーを応答に追加します。
- コント ローラーに到達する前に、要求を検証します。

この図は、パイプラインに挿入された 2 つのカスタム ハンドラーを示しています。

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> HttpClient は、クライアント側で、メッセージのハンドラーも使用します。 詳細については、次を参照してください。 [HttpClient メッセージ ハンドラー](httpclient-message-handlers.md)します。


## <a name="custom-message-handlers"></a>カスタム メッセージ ハンドラー

派生するカスタム メッセージ ハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッド。 このメソッドのシグネチャは次のとおりです。

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

メソッドには、 **HttpRequestMessage**として入力し、非同期的に返します、 **HttpResponseMessage**します。 一般的な実装は、次を行います。

1. 要求メッセージを処理します。
2. 呼び出す`base.SendAsync`内部ハンドラーに要求を送信します。
3. 内部ハンドラーは、応答メッセージを返します。 (この手順は、非同期です)。
4. 応答を処理し、呼び出し元に戻すこと。

簡単な例を次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> 呼び出し`base.SendAsync`は非同期です。 存在する場合、ハンドラーはこの呼び出しの後の作業を使用して、 **await**キーワードを示すようにします。


デリゲート ハンドラーは、内部ハンドラーをスキップすることもおよび応答を直接作成できます。

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

ハンドラーが呼び出さずに応答を作成する場合は、委任`base.SendAsync`、要求パイプラインの残りの部分をスキップします。 (エラー応答の作成) 要求を検証するハンドラーの便利なことができます。

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>パイプラインにハンドラーを追加します。

サーバー側でメッセージのハンドラーを追加するハンドラーを追加、 **HttpConfiguration.MessageHandlers**コレクション。 プロジェクトを作成する、「ASP.NET MVC 4 Web アプリケーション」テンプレートを使用した場合は、この内部を行うことができます、 **WebApiConfig**クラス。

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

メッセージ ハンドラーは内で出現する順序で呼び出されます**MessageHandlers**コレクション。 ネストされているため、応答メッセージは他の方向に移動します。 これは最後のハンドラーでは、応答メッセージを取得する 1 つ目があります。

内部のハンドラーを設定する必要があることに注意してください。Web API フレームワークは、メッセージ ハンドラーを自動的に接続します。

場合[自己ホスト](../older-versions/self-host-a-web-api.md)のインスタンスを作成、 **HttpSelfHostConfiguration**クラスし、ハンドラーを追加、 **MessageHandlers**コレクション。

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

これでカスタム メッセージ ハンドラーの例をいくつか見てみましょう。

## <a name="example-x-http-method-override"></a>例: X-HTTP のメソッドのオーバーライド

X HTTP メソッド オーバーライドは、非標準の HTTP ヘッダーです。 PUT や DELETE など、特定種類の HTTP 要求を送信できないクライアントに設計されています。 代わりに、クライアントは POST 要求を送信し、必要なメソッドに X HTTP メソッド オーバーライドのヘッダーを設定します。 例えば:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

X HTTP メソッド オーバーライドのサポートを追加するメッセージ ハンドラーを次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

**SendAsync**メソッドかどうか、POST 要求は、要求メッセージと X HTTP メソッド オーバーライド ヘッダーが含まれているかどうか、ハンドラーを確認します。 そうである場合、ヘッダーの値を検証し、要求メソッドを次に変更します。 最後に、ハンドラーが呼び出す`base.SendAsync`にメッセージを次のハンドラーに渡します。

要求が達したとき、 **HttpControllerDispatcher**クラス、 **HttpControllerDispatcher**は更新された要求のメソッドに基づく要求をルーティングします。

## <a name="example-adding-a-custom-response-header"></a>例: カスタムの応答ヘッダーを追加します。

すべての応答メッセージにカスタム ヘッダーを追加するメッセージ ハンドラーを次に示します。

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

最初に、ハンドラーが呼び出す`base.SendAsync`内部メッセージ ハンドラーに要求を渡す。 内部ハンドラーが応答メッセージを返しますを使用して非同期には、**タスク&lt;T&gt;** オブジェクト。 応答メッセージがまでご利用いただけません`base.SendAsync`が非同期的に完了するとします。

この例では、 **await**キーワードを作業を実行後に非同期的に`SendAsync`が完了するとします。 .NET Framework 4.0 を対象とする場合は、使用、**タスク**&lt;T&gt;**します。ContinueWith**メソッド。

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>例: は、API キーの確認

一部の web サービスでは、クライアントの要求に API キーを含める必要があります。 次の例では、メッセージ ハンドラーが有効な API キーの要求を確認する方法を示します。

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

このハンドラーは、URI クエリ文字列内の API キーを検索します。 (この例では、想定キーが静的な文字列であります。 実際の実装はおそらく使用より複雑な検証します。)クエリ文字列にキーが含まれている場合、ハンドラーは、内部ハンドラーに要求を渡します。

ハンドラーが状態 403、応答メッセージを作成する要求が有効なキーを持たない場合は許可されていません。 この場合、ハンドラーは呼び出されません`base.SendAsync`、内部ハンドラーが、要求を受け取るようにも、コント ローラーには。 そのため、コント ローラーでは、すべての着信要求が有効な API キーを持っていることを想定できます。

> [!NOTE]
> API キーは、特定のコント ローラー アクションにのみ適用する場合は、メッセージ ハンドラーの代わりにアクション フィルターの使用を検討してください。 アクション フィルターは、URI のルーティングが実行された後に実行します。


## <a name="per-route-message-handlers"></a>ルート メッセージ ハンドラー

ハンドラーで、 **HttpConfiguration.MessageHandlers**コレクションはグローバルに適用されます。

または、ルートを定義するときに、特定のルートにメッセージ ハンドラーを追加できます。

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

この例では、要求 URI が"Route2"と一致する場合、要求にディスパッチされます`MessageHandler2`します。 次の図は、これら 2 つのルートのパイプラインを示しています。

![](http-message-handlers/_static/image4.png)

注意`MessageHandler2`、既定値の代わり**HttpControllerDispatcher**します。 この例で`MessageHandler2`応答を作成し、コント ローラーで作業しない"Route2"一致する要求。 これにより、全体の Web API コント ローラーのメカニズムを独自のカスタム エンドポイントに置き換えます。

または、ルート別のメッセージ ハンドラーに委任できます**HttpControllerDispatcher**、コント ローラーをそこにディスパッチします。

![](http-message-handlers/_static/image5.png)

次のコードでは、このルートを構成する方法を示します。

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
