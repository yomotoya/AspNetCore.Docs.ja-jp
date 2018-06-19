---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: 単体テストの ASP.NET web API 2 コント ローラー |Microsoft ドキュメント
author: MikeWasson
description: このトピックでは、単体テストの Web API 2 コント ローラーの特定の方法について説明します。 このトピックを読む前に単位のチュートリアルを読みたい場合があります.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/11/2014
ms.topic: article
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: bda5148a4c1553d70f3173de66371fbb8576e83f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28039545"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>単体テストの ASP.NET web API 2 コント ローラー
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> このトピックでは、単体テストの Web API 2 コント ローラーの特定の方法について説明します。 このトピックを読む前にすることができます、チュートリアルを読み取る[ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)、単体テスト プロジェクトをソリューションに追加する方法が示されます。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Moq を使用しましたが、考え方は同じが任意のモック フレームワークに適用されます。 Moq 4.5.30 (以降) Visual Studio 2017、Roslyn および .NET 4.5 以降のバージョンをサポートします。

単体テストでの一般的なパターンは&quot;整列 act アサート&quot;:

- 配置: を実行するテストのすべての前提条件を設定します。
- Act: は、テストを実行します。
- : をアサートすると、テストが成功したことを確認します。

配置手順を多くの場合、モックを使用してまたはスタブに対応するオブジェクト。 依存関係の数は、テストが 1 つのテストの重点を置いてようにを最小化します。

次に、Web API コント ローラーで単体テストを必要があるいくつかの点を示します。

- アクションでは、適切な種類の応答を返します。
- 無効なパラメーターでは、適切なエラー応答を返します。
- アクションは、リポジトリまたはサービス層で正しいメソッドを呼び出します。
- 応答には、ドメイン モデルが含まれている場合は、モデルの種類を確認します。

いくつの一般的な機能をテストするには、これらがコント ローラーの実装によって異なります。 具体的には、これにより、大きな違いコント ローラーのアクションを返すかどうか**HttpResponseMessage**または**IHttpActionResult**です。 結果型の詳細については、次を参照してください。 [Web Api 2 のアクション結果](../getting-started-with-aspnet-web-api/action-results.md)です。

## <a name="testing-actions-that-return-httpresponsemessage"></a>HttpResponseMessage を返しますのテストの操作

次の例は、コント ローラーのアクションの戻り値**HttpResponseMessage**です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

通知は、コント ローラーでは、依存関係の挿入を使用して、挿入、`IProductRepository`です。 検証を実現コント ローラーより、モック リポジトリを挿入するためです。 次の単体テストであることを確認、`Get`メソッドの書き込み、`Product`応答の本体。 あると想定`repository`はモック`IProductRepository`です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

設定することが重要**要求**と**構成**コント ローラーにします。 テストは失敗するそれ以外の場合、 **ArgumentNullException**または**InvalidOperationException**です。

## <a name="testing-link-generation"></a>リンクのテストの生成

`Post`メソッド呼び出し**UrlHelper.Link**応答でリンクを作成します。 これには、単体テストでは少し複雑なセットアップが必要です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper**クラスは、これらの値を設定するため、テスト要求の URL とルート データを必要があります。 別のオプションは、モックまたはスタブ**UrlHelper**です。 この方法での既定値を置換する[ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx)モックまたはスタブ バージョン固定値を返しますを使用します。

使用してテストを記述し直しましょう、 [Moq](https://github.com/Moq)フレームワークです。 インストール、`Moq`テスト プロジェクトでの NuGet パッケージです。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

このバージョンで必要はありません、ルート データを設定するため、モック**UrlHelper**定数文字列を返します。


## <a name="testing-actions-that-return-ihttpactionresult"></a>IHttpActionResult を返すためのテストの操作

Web API 2 コント ローラーのアクションを返すことができます**IHttpActionResult**、これに似ています**ActionResult** ASP.NET MVC でします。 **IHttpActionResult**インターフェイスは、HTTP 応答を作成するコマンドのパターンを定義します。 応答を直接作成するには、代わりに、コント ローラーを返します、 **IHttpActionResult**です。 後で、パイプラインを呼び出す、 **IHttpActionResult**応答を作成します。 このアプローチやすく単体テストを記述するために必要なセットアップの多くを省略できます**HttpResponseMessage**です。

例のコント ローラーのアクションの戻り値を次に示します**IHttpActionResult**です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

この例を使用して、一般的なパターンを示しています**IHttpActionResult**です。 見てどの単位にそれらをテストします。

### <a name="action-returns-200-ok-with-a-response-body"></a>アクションは、応答本体を持つ 200 (OK) を返します

`Get`メソッド呼び出し`Ok(product)`場合は、製品が見つかりました。 単体テストでは、戻り値の型を確認してください**OkNegotiatedContentResult**し、返された製品が、右側の id です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

単体テストがアクションの結果を実行しないことに注意してください。 アクションの結果は、HTTP 応答を正しく作成を想定することができます。 (理由は、Web API フレームワークは、独自の単体テストを含む!)

### <a name="action-returns-404-not-found"></a>アクションには、404 (Not Found) が返されます

`Get`メソッド呼び出し`NotFound()`製品が見つからない場合。 この場合、単体テストの単なるチェック戻り値の型がある場合**NotFoundResult**です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>応答本文のない 200 (OK) を返すアクション

`Delete`メソッド呼び出し`Ok()`を空の HTTP 200 の応答を返します。 前の例のように、単体テストをチェック戻り値の型では、ここでは**OkResult**です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>場所ヘッダーを持つ 201 (Created) を返すアクション

`Post`メソッド呼び出し`CreatedAtRoute`Location ヘッダーの URI を持つ、HTTP 201 の応答を返すにします。 単体テストでは、アクションが正しいルーティング値を設定することを確認します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>アクションは、応答本体を持つ別の 2 xx を返します

`Put`メソッド呼び出し`Content`応答本文と HTTP 202 (Accepted) 応答が返さにします。 この場合は 200 (OK) を返すことに似ていますが、単体テストは、ステータス コードを確認する必要がありますもします。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>その他のリソース

- [Entity Framework をモック作成時に単体テストの ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [ASP.NET Web API サービスのテストを記述](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)(Youssef Moussaoui でブログの投稿)。
- [ASP.NET Web API のルートのデバッガーのデバッグ](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
