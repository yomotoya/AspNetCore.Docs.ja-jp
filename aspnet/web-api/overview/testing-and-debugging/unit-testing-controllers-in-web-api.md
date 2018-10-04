---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: 単体テストでは、ASP.NET Web API 2 コント ローラー |Microsoft Docs
author: MikeWasson
description: このトピックでは、単体テストで Web API 2 コント ローラーの具体的な技法について説明します。 このトピックを読む前に単位のチュートリアルを読みたい場合があります.
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e1bb1aa120ced95db7674eae1831f2a2c7356fc0
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794825"
---
<a name="unit-testing-controllers-in-aspnet-web-api-2"></a>単体テストでは、ASP.NET Web API 2 コント ローラー
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> このトピックでは、単体テストで Web API 2 コント ローラーの具体的な技法について説明します。 このトピックを読む前にチュートリアルを参照する可能性があります[ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)、単体テスト プロジェクトをソリューションに追加する方法を示します。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2
> - [Moq](https://github.com/Moq) 4.5.30

> [!NOTE]
> Moq を使用しましたが、同じ考え方は、任意のモック作成フレームワークに適用されます。 Moq 4.5.30 (以降) Visual Studio 2017、Roslyn と .NET 4.5 以降のバージョンをサポートします。

単体テストでの一般的なパターンは&quot;配置 act アサート&quot;:

- 配置: は、テストを実行する場合、前提条件を設定します。
- Act: は、テストを実行します。
- : をアサートすると、テストが成功したことを確認します。

配置手順では、多くの場合、モックを使用して、または、オブジェクトのスタブは。 依存関係の数を最小化するため、テストが 1 つのテストに重点を置いています。

Web API コント ローラーで単体テストする点を次に示します。

- アクションは、適切な種類の応答を返します。
- 無効なパラメーターには、適切なエラー応答が返されます。
- アクションは、リポジトリまたはサービス層で適切なメソッドを呼び出します。
- 応答には、ドメイン モデルが含まれている場合は、モデルの種類を確認します。

これらの一般的なことをテストするが、コント ローラーの実装によって異なります。 大きな違いは、その具体的には、コント ローラーのアクションを返すかどうか**HttpResponseMessage**または**IHttpActionResult**します。 これらの結果の種類の詳細については、次を参照してください。 [Web Api 2 でアクションの結果](../getting-started-with-aspnet-web-api/action-results.md)します。

## <a name="testing-actions-that-return-httpresponsemessage"></a>テスト HttpResponseMessage を返すアクション

あるアクションの戻り値、コント ローラーの例を示します**HttpResponseMessage**します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

通知コント ローラーを挿入する依存関係の挿入を使用して、`IProductRepository`します。 により、コント ローラーよりテスト可能なモック リポジトリを挿入するためです。 次の単体テストを検証する、`Get`メソッドの書き込みを`Product`応答本文にします。 ある`repository`はモック`IProductRepository`します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

設定することが重要**要求**と**構成**コント ローラーにします。 それ以外の場合、テストは失敗で、 **ArgumentNullException**または**InvalidOperationException**します。

## <a name="testing-link-generation"></a>テストのリンクの生成

`Post`メソッド呼び出し**UrlHelper.Link**応答でリンクを作成します。 これには、単体テストで少し多くの設定が必要です。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

**UrlHelper**クラスは、これらの値を設定するため、テスト要求 URL とルート データを必要があります。 もう 1 つのオプションは、モックまたはスタブ**UrlHelper**します。 この方法での既定値を交換する[ApiController.Url](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx)モックまたはスタブ バージョンの固定値を返します。

使用してテストを書き直します、 [Moq](https://github.com/Moq)フレームワーク。 インストール、`Moq`テスト プロジェクトで NuGet パッケージ。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

このバージョンでは、する必要はありません、ルート データを設定するため、モック**UrlHelper**定数文字列を返します。


## <a name="testing-actions-that-return-ihttpactionresult"></a>IHttpActionResult を返すアクションのテスト

Web API 2 では、コント ローラーのアクションを返すことができます**IHttpActionResult**に似ていますが、 **ActionResult** ASP.NET MVC でします。 **IHttpActionResult**インターフェイスは、HTTP 応答を作成するため、コマンドのパターンを定義します。 応答を直接作成する代わりに、コント ローラーを返します、 **IHttpActionResult**します。 その後、パイプラインを呼び出す、 **IHttpActionResult**応答を作成します。 このアプローチにより、単体テストを記述しやすくために必要なセットアップの多くを省略できます**HttpResponseMessage**します。

ここでは例のコント ローラーを持つアクションの戻り値**IHttpActionResult**します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

この例を使用して、一般的なパターンを示しています。 **IHttpActionResult**します。 見てどの単位にそれらをテストします。

### <a name="action-returns-200-ok-with-a-response-body"></a>アクションは 200 (OK) 応答の本文を返します

`Get`メソッド呼び出し`Ok(product)`製品が見つかった場合します。 単体テストでは、戻り値の型を確認します**OkNegotiatedContentResult**返された製品であり、適切な id。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

単体テストがアクションの結果を実行しないことに注意してください。 アクションの結果、HTTP 応答を正しく作成すると想定することができます。 (その理由は、Web API フレームワークは、独自の単体テストです)。

### <a name="action-returns-404-not-found"></a>アクションは 404 (Not Found) を返します

`Get`メソッド呼び出し`NotFound()`製品が存在しない場合。 戻り値の型がある場合、この場合の単位がチェックだけをテスト**NotFoundResult**します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>アクションは、応答本文なしで 200 (OK) を返します

`Delete`メソッド呼び出し`Ok()`を空の HTTP 200 応答を返します。 前の例のように、単体テストを確認します戻り値の型では、ここで**OkResult**します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>アクションは、Location ヘッダーで 201 (Created) を返します

`Post`メソッド呼び出し`CreatedAtRoute`を Location ヘッダーの URI を持つ HTTP 201 応答を返します。 単体テストでは、アクションが正しいルーティング値を設定することを確認します。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>アクションは別の 2 xx 応答の本文を返します

`Put`メソッド呼び出し`Content`応答本文を含む HTTP 202 (Accepted) 応答を返すにします。 この場合は 200 (OK) を返すことに似ていますが、単体テストは、ステータス コードを確認する必要がありますもします。

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>その他のリソース

- [Entity Framework のモックを作成するときに単体テストの ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [ASP.NET Web API サービスのテストの記述](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx)(Youssef Moussaoui でブログの投稿)。
- [ルートのデバッガーを使用した ASP.NET Web API のデバッグ](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
