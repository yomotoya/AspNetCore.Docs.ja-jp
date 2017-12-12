---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: "アクションと ASP.NET Web API 2.2 を使用して OData v4 に関数 |Microsoft ドキュメント"
author: MikeWasson
description: "OData では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルで説明する方法."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>アクションと ASP.NET Web API 2.2 を使用して OData v4 に関数
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> OData では、アクションと関数は、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 このチュートリアルでは、Web API 2.2 を使用して、OData v4 エンドポイントをアクションと関数を追加する方法を示します。 このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>チュートリアルのバージョン
> 
> OData バージョン 3 では、次を参照してください。 [ASP.NET Web API 2 OData アクション](../odata-v3/odata-actions.md)です。


違い*アクション*と*関数*は関数は、アクションは、副作用があることができます。 両方のアクションと関数は、データを返すことができます。 いくつかの操作の使用は、次のとおりです。

- 複雑なトランザクションです。
- 一度に複数のエンティティを操作します。
- エンティティの特定のプロパティにのみ更新を許可します。
- エンティティではないデータを送信しています。

関数は、エンティティまたはコレクションに直接対応していない情報を返すに役立ちます。

操作 (または関数) は、1 つのエンティティまたはコレクションを対象ことができます。 これは、OData の用語で、*バインディング*です。 こともできます&quot;バインドされていない&quot;アクション/関数、サービス上の静的な操作としてと呼ばれます。

## <a name="example-adding-an-action"></a>例: アクションの追加

製品を評価するアクションを定義してみましょう。

> [!NOTE]
> このチュートリアルは、チュートリアルに基づいて[OData v4 エンドポイントを使用する ASP.NET Web API 2 の作成](create-an-odata-v4-endpoint.md)


最初に、追加、`ProductRating`モデルを評価を表します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

追加も、 **DbSet**を`ProductsContext`クラス、EF はデータベースに評価テーブルを作成できるようにします。

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>EDM にアクションを追加します。

WebApiConfig.cs では、次のコードを追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action**メソッドは、entity data model (EDM) にアクションを追加します。 **パラメーター**メソッドは、アクションの型指定されたパラメーターを指定します。

また、このコードは、EDM の名前空間を設定します。 名前空間が重要なは、アクションの URI には、アクションの完全修飾名が含まれているため。

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> 一般的な IIS の構成では、この URL にドットと、IIS はエラー 404 が返されます。 これは、Web.Config ファイルに次のセクションを追加することで解決できます。

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>アクションのコント ローラーのメソッドを追加します。

有効にする、&quot;レート&quot;アクションには、次のメソッドを追加`ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

メソッド名が、アクションの名前と一致することに注意してください。 **[HttpPost]**属性は、メソッドが HTTP POST メソッドを指定します。

アクションを呼び出すには、クライアントは、次のように HTTP POST 要求を送信します。

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;レート&quot;のため、操作の URI は、エンティティの URI に追加操作の完全修飾名、製品のインスタンスにアクションがバインドされます。 (には、EDM 名前空間を設定おことに注意してください&quot;ProductService&quot;、アクションの完全修飾名が、 &quot;ProductService.Rate&quot;)。

要求の本文には、JSON ペイロードとしてのアクション パラメーターが含まれています。 Web API では、JSON ペイロードを自動的に変換、 **ODataActionParameters**オブジェクトで、パラメーター値のディクショナリだけです。 コント ローラー メソッドでパラメーターにアクセスするのにには、このディクショナリを使用します。

クライアントで間違ったアクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は false。 コント ローラー メソッドでは、このフラグをチェックし、エラーを返します**IsValid**は false。

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>例: 関数の追加

これで最も高い製品を返す OData 関数を追加してみましょう。 ようにする前に、最初の手順は、EDM に関数を追加します。 WebApiConfig.cs では、次のコードを追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

この場合、関数は、個々 の製品のインスタンスではなく製品コレクションにバインドされています。 クライアントは、GET 要求を送信することによって、関数を呼び出します。

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

この関数のコント ローラーのメソッドを次に示します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

メソッド名が、関数名と一致することに注意してください。 **[HttpGet]**属性は、メソッドが HTTP GET メソッドを指定します。

HTTP 応答を次に示します。

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>例: バインドされていない関数を追加します。

前の例では、コレクションにバインドされた関数はでした。 次の例を作成して、*バインドされていない*関数。 バインドされていない関数は、サービス上の静的な操作として呼び出されます。 この例では、関数では、特定の郵便番号の売上税を返します。

WebApiConfig ファイルでは、EDM に関数を追加します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

呼び出していることを確認**関数**上で直接、**ため**エンティティ型またはコレクションの代わりにします。 これは、ビルダーに指示、モデル、関数はバインドされていることはありません。

関数を実装するコント ローラーのメソッドを次に示します。

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

どの Web API コント ローラーでこのメソッドを配置する必要はありません。 内に配置できなかった`ProductsController`、または別のコント ローラーを定義します。 **[ODataRoute]**属性は、関数の URI テンプレートを定義します。

クライアント要求の例を次に示します。

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP 応答:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
