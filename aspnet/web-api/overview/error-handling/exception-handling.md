---
uid: web-api/overview/error-handling/exception-handling
title: ASP.NET Web API での例外処理 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 62e6187cd82252e7d30f21e03cc4d08418fa39ee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831905"
---
<a name="exception-handling-in-aspnet-web-api"></a>ASP.NET Web API での例外処理
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

この記事では、エラーと ASP.NET Web API での例外処理について説明します。

- [HttpResponseException](#httpresponserexception)
- [例外フィルター](#exception_filters)
- [例外フィルターを登録します。](#registering_exception_filters)
- [Http エラー](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Web API コント ローラーがキャッチされない例外をスローするとどうなりますか。 既定では、ほとんどの例外は、ステータス コード 500 内部サーバー エラーの HTTP 応答に変換されます。

**HttpResponseException**型が特殊なケースです。 この例外は、例外のコンス トラクターで指定した任意の HTTP ステータス コードを返します。 次のメソッドに見つからない、404 が返されます場合など、 *id*パラメーターが無効です。

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

応答をより細かく制御することができますも全体の応答メッセージの構築とで、 **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>例外フィルター

Web API が記述することで例外を処理する方法をカスタマイズすることができます、*例外フィルター*します。 コント ローラー メソッドがある未処理の例外をスローした場合、例外フィルターが実行される*いない*、 **HttpResponseException**例外。 **HttpResponseException** HTTP 応答を返すためには、具体的には設計されているので、型が特殊なケースです。

例外フィルターの実装、 **System.Web.Http.Filters.IExceptionFilter**インターフェイス。 例外フィルターを記述する最も簡単な方法がから派生するには、 **System.Web.Http.Filters.ExceptionFilterAttribute**クラスし、オーバーライド、 **OnException**メソッド。

> [!NOTE]
> ASP.NET Web API での例外フィルターは、ASP.NET mvc と似ています。 ただし、個別に、別の名前空間と関数で宣言されています。 具体的には、 **HandleErrorAttribute** MVC で使用されるクラスでは、Web API コント ローラーによってスローされた例外は処理されません。


ここでは、変換を行うフィルター **NotImplementedException**例外に HTTP 状態コード、501 Not Implemented:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**応答**のプロパティ、 **HttpActionExecutedContext**オブジェクトには、クライアントに送信される HTTP 応答メッセージが含まれています。

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>例外フィルターを登録します。

Web API 例外フィルターを登録するいくつかの方法はあります。

- アクションによって
- コント ローラーによって
- グローバルに

特定のアクションにフィルターを適用するには、アクションに属性として、フィルターを追加します。

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

すべてのコント ローラーのアクションをフィルターを適用するには、コント ローラー クラスに属性としてフィルターを追加します。

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

すべての Web API コント ローラーにグローバルにフィルターを適用するフィルターのインスタンスを追加、 **GlobalConfiguration.Configuration.Filters**コレクション。 このコレクション内の例外フィルターは、任意の Web API コント ローラー アクションに適用されます。

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

配置内で Web API の構成コードの「ASP.NET MVC 4 Web アプリケーション」プロジェクト テンプレートをプロジェクトの作成に使用する場合、`WebApiConfig`クラスは、アプリである\_開始フォルダー。

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Http エラー

**HttpError**オブジェクトは、応答本文にエラー情報を返す一貫した方法を提供します。 次の例では、HTTP 状態コード 404 (Not Found) を返す方法を示しますで、 **HttpError**応答の本文。

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse**拡張メソッドで定義されている、 **System.Net.Http.HttpRequestMessageExtensions**クラス。 内部的には、 **CreateErrorResponse**作成、 **HttpError**をインスタンス化し、作成、 **HttpResponseMessage**を格納している、 **httpエラー**.

この例でメソッドが成功した場合、製品、HTTP 応答で返します。 HTTP 応答に含まれる、要求された製品が存在しない場合は、 **HttpError**要求本文にします。 応答は、次のようになります。

[!code-console[Main](exception-handling/samples/sample9.cmd)]

注意、 **HttpError**がこの例では JSON にシリアル化します。 使用する利点の 1 つ**HttpError**を同一になったことが[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)およびシリアル化は、他の厳密に型指定されたモデルを処理します。

### <a name="httperror-and-model-validation"></a>Http エラーとモデルの検証

モデルの検証のためには、モデルの状態を渡すことができます**CreateErrorResponse**応答に検証エラーを含める。

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

この例では、次の応答を返す可能性があります。

[!code-console[Main](exception-handling/samples/sample11.cmd)]

モデルの検証の詳細については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)します。

### <a name="using-httperror-with-httpresponseexception"></a>HttpResponseException での http エラーの使用

前の例では、返されます、 **HttpResponseMessage** 、コント ローラー アクションからのメッセージを使用することも**HttpResponseException**を返す、 **HttpError**します。 これにより、まだを返すときに通常の成功の場合、厳密に型指定されたモデルを返すできます**HttpError**エラーがある場合。

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
