---
uid: web-api/overview/error-handling/exception-handling
title: "ASP.NET Web API での例外処理 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>ASP.NET Web API での例外処理
====================
によって[Mike Wasson](https://github.com/MikeWasson)

この記事では、エラー、ASP.NET Web API での例外処理について説明します。

- [HttpResponseException](#httpresponserexception)
- [例外フィルター](#exception_filters)
- [例外フィルターを登録します。](#registering_exception_filters)
- [Http エラー](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Web API コント ローラーがキャッチされない例外をスローするとどうなりますか。 既定では、ほとんどの例外は、ステータス コード 500 内部サーバー エラーの HTTP 応答に変換されます。

**HttpResponseException**型は、特殊なケースです。 この例外は、例外のコンス トラクターで指定したすべての HTTP 状態コードを返します。 次のメソッドに見つかりません、404 が返されます場合など、 *id*パラメーターが無効です。

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

応答をより細かく制御することができますも全体の応答メッセージの構築とと共に、 **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>例外フィルター

記述して、Web API が例外を処理する方法をカスタマイズすることができます、*例外フィルター*です。 コント ローラー メソッドによってはハンドルされない例外がスローされる例外フィルターが実行される*いない*、 **HttpResponseException**例外。 **HttpResponseException**型では特殊なケースでは、HTTP 応答を返す向けに設計されています。

例外フィルターを実装して、 **System.Web.Http.Filters.IExceptionFilter**インターフェイスです。 例外フィルターを作成する最も簡単な方法から派生する、 **System.Web.Http.Filters.ExceptionFilterAttribute**クラスし、オーバーライド、 **OnException**メソッドです。

> [!NOTE]
> ASP.NET Web API で例外フィルターは、ASP.NET MVC のと似ています。 ただし、個別に、別の名前空間と関数で宣言します。 具体的には、 **HandleErrorAttribute** MVC で使用されるクラスは、Web API コント ローラーによってスローされる例外を処理しません。


ここでは、変換を行うフィルター **NotImplementedException** HTTP ステータスに例外コード 501 Not Implemented:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**応答**のプロパティ、 **HttpActionExecutedContext**オブジェクトには、クライアントに送信される HTTP 応答メッセージが含まれています。

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>例外フィルターを登録します。

これには Web API の例外フィルターを登録するいくつかの方法があります。

- アクションによって
- コント ローラーによって
- グローバル

特定の操作にフィルターを適用するには、アクションの属性としてフィルターを追加します。

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

あるすべてのコント ローラーのアクション フィルターを適用するには、コント ローラー クラスに属性としてフィルターを追加します。

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

すべての Web API コント ローラーにグローバルにフィルターを適用するフィルターのインスタンスを追加、 **GlobalConfiguration.Configuration.Filters**コレクション。 このコレクション内の例外フィルターは、どの Web API コント ローラーのアクションに適用されます。

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

「ASP.NET MVC 4 Web アプリケーション」のプロジェクト テンプレートを使用して、プロジェクトを作成する場合は、コードを記述して Web API 構成中、`WebApiConfig`クラスは、アプリにある\_開始フォルダー。

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>Http エラー

**HttpError**オブジェクトは、応答本文にエラー情報を返す一貫した方法を提供します。 次の例では、HTTP ステータス コード 404 (Not Found) を返す方法を示しますで、 **HttpError**応答本文にします。

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse**拡張メソッドで定義されている、 **System.Net.Http.HttpRequestMessageExtensions**クラスです。 内部的には、 **CreateErrorResponse**を作成、 **HttpError**をインスタンス化し、作成、 **HttpResponseMessage**を格納している、 **HttpError**.

この例では、メソッドが成功すると場合、HTTP 応答で製品返します。 要求された製品が見つからない場合、HTTP 応答が含まれますが、 **httperror など**要求本文にします。 応答は、次のようになります。

[!code-console[Main](exception-handling/samples/sample9.cmd)]

注意して、 **HttpError**この例では JSON にシリアル化されました。 使用する利点の 1 つ**HttpError**はあるが、同じ[コンテンツ ネゴシエーション](../formats-and-model-binding/content-negotiation.md)およびシリアル化は、他の厳密に型指定されたモデルを処理します。

### <a name="httperror-and-model-validation"></a>Http エラーとモデルの検証

モデル検証のためには、モデルの状態を渡すことができます**CreateErrorResponse**応答に検証エラーを含める。

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

この例では、次の応答を返す場合があります。

[!code-console[Main](exception-handling/samples/sample11.cmd)]

モデルの検証の詳細については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)です。

### <a name="using-httperror-with-httpresponseexception"></a>HttpResponseException で httperror などの使用

前の例では、返されます、 **HttpResponseMessage**コント ローラーのアクションからのメッセージでも使用できます**HttpResponseException**を返す、 **HttpError**です。 これにより、まだを返すときに通常の成功の場合、厳密に型指定されたモデルを返すできます**HttpError**エラーがある場合。

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
