---
uid: web-api/overview/advanced/httpclient-message-handlers
title: ASP.NET Web API の HttpClient メッセージ ハンドラー |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 1712f190c5a313c79b7c91b671214dd8972cb3c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402942"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>ASP.NET Web API の HttpClient メッセージ ハンドラー
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

A*メッセージ ハンドラー*は HTTP 要求を受信し、HTTP 応答を返すクラスです。

通常、一連のメッセージ ハンドラーが連結されます。 最初のハンドラー HTTP 要求を受信するには、いくつかの処理およびの次のハンドラーへの要求を提供します。 いくつかの時点では、応答が作成され、チェーンの上位に戻ります。 このパターンと呼ばれます、*委任*ハンドラー。

![](httpclient-message-handlers/_static/image1.png)

クライアント側で、 **HttpClient**クラスはメッセージ ハンドラーを使用して要求を処理します。 既定のハンドラーは**HttpClientHandler**、ネットワーク経由で要求を送信して、サーバーからの応答を取得します。 クライアント パイプラインにカスタム メッセージ ハンドラーを挿入することができます。

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> また、ASP.NET Web API は、サーバー側でメッセージのハンドラーを使用します。 詳細については、次を参照してください。 [HTTP メッセージ ハンドラー](http-message-handlers.md)します。


## <a name="custom-message-handlers"></a>カスタム メッセージ ハンドラー

派生するカスタム メッセージ ハンドラーを書き込む**System.Net.Http.DelegatingHandler**をオーバーライドし、 **SendAsync**メソッド。 メソッド シグネチャを次に示します。

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

メソッドには、 **HttpRequestMessage**として入力し、非同期的に返します、 **HttpResponseMessage**します。 一般的な実装は、次を行います。

1. 要求メッセージを処理します。
2. 呼び出す`base.SendAsync`内部ハンドラーに要求を送信します。
3. 内部ハンドラーは、応答メッセージを返します。 (この手順は、非同期です)。
4. 応答を処理し、呼び出し元に戻すこと。

次の例では、送信要求にカスタム ヘッダーを追加するメッセージ ハンドラーを示します。

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

呼び出し`base.SendAsync`は非同期です。 存在する場合、ハンドラーはこの呼び出しの後の作業を使用して、 **await**キーワードをメソッドの完了後に実行を再開します。 次の例では、コードのエラー ログに記録するハンドラーを示します。 ログ自体が非常に興味深いではありませんが、ハンドラー内で応答を取得する方法の例に示します。

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>クライアントのパイプラインへのメッセージ ハンドラーの追加

カスタム ハンドラーを追加する**HttpClient**を使用して、 **HttpClientFactory.Create**メソッド。

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

メッセージ ハンドラーは順番に渡す、**作成**メソッド。 ハンドラーは入れ子になったため、応答メッセージは、他の方向に移動します。 これは最後のハンドラーでは、応答メッセージを取得する 1 つ目があります。
