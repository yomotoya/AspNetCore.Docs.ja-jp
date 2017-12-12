---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "ASP.NET Web API で HttpClient メッセージ ハンドラー |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API で HttpClient メッセージ ハンドラー
====================
によって[Mike Wasson](https://github.com/MikeWasson)

A*メッセージ ハンドラー* HTTP 要求を受信して、HTTP 応答を返すクラスです。

通常は、一連のメッセージのハンドラーを連結します。 最初のハンドラーが HTTP 要求を受信するには、いくつかの処理を行うおよび要求は、次のハンドラーにします。 ある時点で、応答が作成され、チェーンの上に戻ります。 このパターンが呼び出された、*委任*ハンドラー。

![](httpclient-message-handlers/_static/image1.png)

クライアント側で、 **HttpClient**クラスは、要求を処理するメッセージのハンドラーを使用します。 既定のハンドラーは**HttpClientHandler**、ネットワーク経由で要求を送信し、サーバーから応答を取得します。 カスタム メッセージのハンドラーは、クライアントのパイプラインに挿入できます。

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> また、ASP.NET Web API は、サーバー側のメッセージ ハンドラーを使用します。 詳細については、次を参照してください。 [HTTP メッセージ ハンドラー](http-message-handlers.md)です。


## <a name="custom-message-handlers"></a>カスタム メッセージ ハンドラー

派生するカスタム メッセージのハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッドです。 メソッドのシグネチャを次に示します。

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

メソッドは、 **HttpRequestMessage**として入力非同期に返す、 **HttpResponseMessage**です。 一般的な実装は次のとおり

1. 要求メッセージを処理します。
2. 呼び出す`base.SendAsync`内部ハンドラーに、要求を送信します。
3. 内部ハンドラーでは、応答メッセージを返します。 (この手順は、非同期です)。
4. 応答を処理し、呼び出し元に返します。

次の例では、送信要求にカスタム ヘッダーを追加するメッセージ ハンドラーを示します。

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

呼び出し`base.SendAsync`は非同期です。 場合は、ハンドラーは、この呼び出しの後の作業を使用して、 **await**メソッドの完了後に実行を再開するキーワードです。 次の例は、エラー コードをログに記録するハンドラーを示しています。 自体のログが非常に興味深いではありませんが、例では、応答ハンドラー内では、取得する方法を示しています。

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>クライアントのパイプラインにメッセージのハンドラーを追加します。

カスタム ハンドラーを追加する**HttpClient**を使用して、 **HttpClientFactory.Create**メソッド。

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

メッセージ ハンドラーに渡したりする順序で呼び出されます、**作成**メソッドです。 ハンドラーが入れ子になっているため、応答メッセージは逆方向に移動します。 その最後のハンドラーは、最初に応答メッセージを取得します。
