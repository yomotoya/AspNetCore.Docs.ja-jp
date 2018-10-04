---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: アクションと ASP.NET Web API 2.2 を使用して OData v4 の関数 |Microsoft Docs
author: MikeWasson
description: Odata では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルでは方法.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 45b84ec4ee76e83ece99bf6841c28e13c3ab7728
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795268"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>アクションと ASP.NET Web API 2.2 を使用して OData v4 の関数
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> Odata では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルでは、アクションと関数を Web API 2.2 を使用して、OData v4 エンドポイントを追加する方法を示します。 チュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
> - Web API 2.2
> - OData v4
> - Visual Studio 2013 (Visual Studio 2017 ダウンロード[ここ](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
>
> OData バージョン 3 では、次を参照してください。[の ASP.NET Web API 2 OData アクション](../odata-v3/odata-actions.md)します。

間の差*アクション*と*関数*アクションは、副作用を持つことができます、関数がないことです。 アクションと関数の両方では、データを返すことができます。 アクションの使用は次のとおりです。

- 複雑なトランザクション。
- 一度に複数のエンティティを操作します。
- エンティティの特定のプロパティにのみ更新できるようにします。
- エンティティではないデータを送信します。

関数は、エンティティまたはコレクションに直接対応しない情報を返す場合に便利です。

操作 (または関数) は、1 つのエンティティまたはコレクションを対象ことができます。 これは、OData の用語では、*バインド*します。 こともできます&quot;バインドされていない&quot;アクション/関数、サービスで静的な操作として呼び出されます。

## <a name="example-adding-an-action"></a>例: アクションの追加

製品を評価するアクションを定義してみましょう。

> [!NOTE]
> このチュートリアルでは、チュートリアルを[OData v4 エンドポイントを使用して ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)


最初に、追加、`ProductRating`モデルを評価を表します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

追加も、 **DbSet**を`ProductsContext`クラス、EF はデータベースの評価テーブルを作成できるようにします。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>EDM にアクションを追加します。

WebApiConfig.cs で、次のコードを追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**メソッドは、entity data model (EDM) にアクションを追加します。 **パラメーター**メソッドは、アクションの型指定されたパラメーターを指定します。

また、このコードは、EDM の名前空間を設定します。 名前空間には、アクションの URI には、アクションの完全修飾名が含まれているためにが重要です。

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 一般的な IIS 構成でこの URL にドットがエラー 404 を返す IIS が発生します。 これは、Web.Config ファイルに次のセクションを追加することで解決できます。

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>アクションのコント ローラー メソッドを追加します。

有効にする、&quot;レート&quot;アクションでは、次のメソッドを追加`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

メソッド名が、アクションの名前と一致することに注意してください。 **[HttpPost]** 属性は、メソッドが HTTP POST メソッドを指定します。

アクションを呼び出すクライアントは、次のような HTTP POST 要求を送信します。

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;レート&quot;アクションの URI は、エンティティの URI に追加されたアクションの完全修飾名、製品のインスタンスへのアクションがバインドされます。 (に EDM 名前空間を設定することを思い出してください&quot;ProductService&quot;でアクションの完全修飾名があるため、 &quot;ProductService.Rate&quot;)。

要求の本文には、JSON ペイロードとしてのアクション パラメーターが含まれています。 Web API に JSON ペイロードを自動的に変換する、 **ODataActionParameters**オブジェクトで、パラメーター値のディクショナリのみです。 コント ローラー メソッドでパラメーターにアクセスするのにには、このディクショナリを使用します。

クライアントが正しくないアクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は false です。 コント ローラー メソッドでは、このフラグをチェックし、場合は、エラーを返します**IsValid**は false です。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>例: 関数の追加

これで、最も高価な製品が返す OData 関数を追加してみましょう。 ようにする前に、最初の手順は、EDM に関数を追加します。 WebApiConfig.cs では、次のコードを追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

この場合、関数は、個々 の製品のインスタンスではなく、製品のコレクションにバインドされます。 クライアントは、GET 要求を送信することによって、関数を呼び出します。

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

この関数のコント ローラー メソッドを次に示します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

メソッド名に関数名と一致することに注意してください。 **[HttpGet]** 属性は、メソッドが HTTP GET メソッドを指定します。

HTTP 応答を次に示します。

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>例: バインドされていない、関数を追加します。

前の例では、コレクションにバインドされている関数をしました。 この次の例で作成します、*バインドされていない*関数。 バインドされていない関数は、サービスで静的な操作として呼び出されます。 この例では、関数では、特定の郵便番号の税抜きを返します。

WebApiConfig ファイルでは、EDM に関数を追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

呼び出すことに注意してください**関数**上で直接、**に**エンティティ型またはコレクションの代わりにします。 これにより、関数がバインドであるモデル ビルダー。

関数を実装するコント ローラー メソッドを次に示します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Web API コント ローラーでこのメソッドを配置する必要はありません。 配置する`ProductsController`、または別のコント ローラーを定義します。 **[ODataRoute]** 属性は、関数の URI テンプレートを定義します。

クライアント要求の例を次に示します。

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 応答:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
