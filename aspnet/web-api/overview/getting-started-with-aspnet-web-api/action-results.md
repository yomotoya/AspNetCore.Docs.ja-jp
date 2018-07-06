---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Web API 2 の操作の結果 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 74a837cb9f606d78fb516968343d498d3c37c4ed
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808928"
---
<a name="action-results-in-web-api-2"></a>Web API 2 のアクションの結果
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、ASP.NET Web API に HTTP 応答メッセージにコント ローラー アクションからの戻り値を変換する方法について説明します。

Web API コント ローラーのアクションは、次のいずれかを返すことができます。

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. その他の種類

これらのいずれかによって返される、Web API は、別のメカニズムを使用して HTTP 応答を作成します。

| 戻り値の型 | Web API が応答を作成する方法 |
| --- | --- |
| void | 空の 204 (No Content) を返す |
| **HttpResponseMessage** | HTTP 応答メッセージに直接変換します。 |
| **IHttpActionResult** | 呼び出す**ExecuteAsync**を作成する、 **HttpResponseMessage**、HTTP 応答メッセージに変換します。 |
| その他の種類 | 応答の本体にシリアル化された戻り値を書き込む200 (OK) を返します。 |

このトピックの残りの部分では、各オプションの詳細について説明します。

## <a name="void"></a>void

戻り値の型がある場合`void`、Web API は、単に、空の HTTP 応答状態コード 204 (No Content) を返します。

例のコント ローラー:

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP 応答:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

アクションが返された場合、 [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)、Web API からのプロパティを使用して、HTTP 応答メッセージに直接戻り値が変換、 **HttpResponseMessage**を設定するオブジェクト、応答です。

このオプションでは、多数の応答メッセージを制御できます。 たとえば、次のコント ローラー アクションは、Cache-control ヘッダーを設定します。

[!code-csharp[Main](action-results/samples/sample3.cs)]

応答 :

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

ドメイン モデルを渡す場合、 **CreateResponse**メソッドは、Web API では、[メディア フォーマッタ](../formats-and-model-binding/media-formatters.md)応答本文にシリアル化されたモデルを記述します。

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web API では、要求の Accept ヘッダーを使用して、フォーマッタを選択します。 詳細については、次を参照してください。[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)します。

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult**インターフェイスが Web API 2 で導入されました。 基本的には、定義、 **HttpResponseMessage**ファクトリ。 使用しての利点は次のとおり、 **IHttpActionResult**インターフェイス。

- 簡略化[単体テスト](../testing-and-debugging/unit-testing-controllers-in-web-api.md)コント ローラー。
- 別のクラスの HTTP 応答を作成するための一般的なロジックを移動します。
- わかりやすく、コント ローラー アクションの目的は、応答を構築する低レベルの詳細を非表示にしています。

**IHttpActionResult**含まれる 1 つのメソッドは**ExecuteAsync**、非同期的に作成し、 **HttpResponseMessage**インスタンス。

[!code-csharp[Main](action-results/samples/sample6.cs)]

コント ローラーのアクションが返された場合、 **IHttpActionResult**、Web API 呼び出し、 **ExecuteAsync**を作成する方法、 **HttpResponseMessage**します。 変換し、 **HttpResponseMessage**を HTTP 応答メッセージにします。

単純な実装を次に示します**IHttpActionResult**プレーン テキストの応答を作成します。

[!code-csharp[Main](action-results/samples/sample7.cs)]

コント ローラー アクションの例:

[!code-csharp[Main](action-results/samples/sample8.cs)]

応答 :

[!code-console[Main](action-results/samples/sample9.cmd)]

多くの場合、使用する、 **IHttpActionResult**実装で定義されている、 **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** 名前空間。 **ApiController**クラスは、これらの組み込みのアクション結果を返すヘルパー メソッドを定義します。

次の例では、要求が既存の製品 ID と一致しない場合、コント ローラーを呼び出す[ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) 404 (Not Found) 応答を作成します。 それ以外の場合、コント ローラーを呼び出す[ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx)、200 (OK) 応答を作成する、製品が含まれています。

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>その他の戻り値の型

その他のすべての戻り値型の Web API を使用して、[メディア フォーマッタ](../formats-and-model-binding/media-formatters.md)戻り値をシリアル化します。 Web API は、応答本文にシリアル化された値を書き込みます。 応答ステータス コードは、200 (OK) です。

[!code-csharp[Main](action-results/samples/sample11.cs)]

この方法の欠点は、404 など、エラー コードを直接返すことはできません。 ただし、スロー、 **HttpResponseException**エラー コード。 詳細については、次を参照してください。 [ASP.NET Web API での例外処理](../error-handling/exception-handling.md)します。

Web API では、要求の Accept ヘッダーを使用して、フォーマッタを選択します。 詳細については、次を参照してください。[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)します。

要求の例

[!code-console[Main](action-results/samples/sample12.cmd)]

応答の例:

[!code-console[Main](action-results/samples/sample13.cmd)]
