---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: ASP.NET Web API 2 OData クエリ オプションをサポートする |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508021"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET web API 2 OData クエリ オプションをサポートします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

OData は、OData のクエリを変更するために使用するパラメーターを定義します。 クライアントは、要求 URI のクエリ文字列内のこれらのパラメーターを送信します。 たとえば、クライアントに結果を並べ替えるには、$orderby パラメーターを使用してください。

`http://localhost/Products?$orderby=Name`

OData の仕様は、これらのパラメーターを呼び出す*クエリ オプション*です。 プロジェクト & #8212; で任意の Web API コント ローラーの OData クエリ オプションを有効にすることができます。コント ローラーは、OData エンドポイントである必要はありません。 これにより、フィルター処理および並べ替えを任意の Web API アプリケーションなどの機能を追加する便利な方法です。

トピックを参照してください。 クエリのオプションを有効にする前に[OData セキュリティ ガイダンス](odata-security-guidance.md)です。

- [OData クエリ オプションを有効にします。](#enable)
- [クエリの例](#examples)
- [サーバー ドリブン ページング](#server-paging)
- [クエリ オプションを制限します。](#limiting_query_options)
- [クエリ オプションの直接呼び出し](#ODataQueryOptions)
- [クエリの検証](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>OData クエリ オプションを有効にします。

Web API には、次の OData クエリ オプションがサポートされています。

| オプション | 説明 |
| --- | --- |
| $ を展開します。 | 関連エンティティのインライン展開されます。 |
| $filter | ブール条件に基づいて、結果をフィルター処理します。 |
| $inlinecount | 応答に対応するエンティティの合計数を追加するには、サーバーに指示します。 (サーバー側のページングに役立ちます。) |
| $orderby | 結果を並べ替えます。 |
| $select | 応答に含めるプロパティを選択します。 |
| $skip | 最初の n 個の結果をスキップします。 |
| $top | 最初の n のみ結果を返します。 |

OData クエリ オプションを使用するには、必要があります有効にするに明示的にします。 全体のアプリケーションに対してグローバルに有効にしたり、特定のコント ローラーまたは特定のアクションを有効にすることできます。

OData クエリ オプションをグローバルに有効にするを呼び出す**EnableQuerySupport**上、 **HttpConfiguration**起動時クラス。

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport**メソッドを返すコント ローラーのアクションのグローバル クエリ オプションを有効にする**IQueryable**型です。 クエリ オプションをアプリケーション全体に対して有効にしない場合を有効にすることの特定のコント ローラー アクションの追加、 **[Queryable]** 属性をアクション メソッドにします。

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>クエリの例

このセクションでは、OData クエリ オプションを使用してクエリの種類を示します。 特定の詳細については、クエリ オプションでは、ドキュメントを参照して、OData に[www.odata.org](http://www.odata.org/)です。

$ について順に展開し、$select を参照してください[$ $select を使用すると、展開しとで ASP.NET Web API OData $value](using-select-expand-and-value.md)です。

**クライアント主導のページング**

大きなエンティティ セットに対して、クライアントで結果の数を制限する必要が場合があります。 など、クライアントでは、結果の次のページを取得する [次へ] のリンクと共に、一度に 10 個のエントリを表示する可能性があります。 これを行うには、クライアントは、$top、$skip のオプションを使用します。

`http://localhost/Products?$top=10&$skip=20`

$Top オプションが返される項目の最大数を提供し、$skip オプションは、スキップするエントリの数を提供します。 前の例では、21 30 からのエントリをフェッチします。

**フィルター処理**

$Filter オプションは、ブール式を適用することにより、結果をフィルター処理クライアントを使用します。 フィルター式では、非常に強力です。論理演算子および算術演算子、文字列関数、および日付関数が含まれます。

| すべての製品カテゴリの"Toys"に等しいを返します。 | `http://localhost/Products?$filter=Category`eq 'Toys' |
| --- | --- |
| すべての製品の価格が 10 よりも小さいかを返します。 | `http://localhost/Products?$filter=Price`lt 10 |
| 論理演算子: すべての製品を返す価格、> = 5 と価格 < 15 を = です。 | `http://localhost/Products?$filter=Price`ge 5 と価格 le 15 |
| 文字列関数: 名に"zz"のすべての製品を返します。 | `http://localhost/Products?$filter=substringof('zz',Name)` |
| 日付関数: 2005年より後 ReleaseDate のすべての製品を返します。 | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**並べ替え**

結果を並べ替えるには、$orderby フィルターを使用します。

| 価格で並べ替えます。 | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| 価格 (最高ランク) の順序を降順で並べ替えます。 | `http://localhost/Products?$orderby=Price desc` |
| カテゴリ別に並べ替えるしてから、価格のカテゴリで降順に並べ替えます。 | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>サーバー ドリブン ページング

送信しない場合は、データベースには、数百万レコードにはが含まれています、すべて 1 つのペイロードにします。 これを防ぐためには、サーバーは、1 つの応答で送信されるエントリの数を制限できます。 サーバー ページングを有効にするには設定、 **PageSize**プロパティに、 **Queryable**属性。 値は、返される項目の最大数です。

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

コント ローラーには、OData 形式が返された場合、応答本文には次のデータ ページへのリンクが含まれます。

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

クライアントは、次のページをフェッチするのにこのリンクを使用できます。 結果セット内のエントリの合計数については、クライアント設定できます、値を持つ $inlinecount クエリ オプション。"allpages"。

`http://localhost/Products?$inlinecount=allpages`

値は、"allpages"は、応答の合計数を追加するには、サーバーを示します。

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> 次のページへのリンクとインライン カウント OData 形式が必要です。 その理由は、OData が、リンクとカウントを保持するために、応答本文で特別なフィールドを定義します。


非 OData 形式は、することでクエリ結果をラップすることによって、次のページのリンクとインライン カウントをサポートすること、 **PageResult&lt;T&gt;** オブジェクト。 ただし、もう少しコードが必要です。 次に例を示します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

JSON 応答の例を次に示します。

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>クエリ オプションを制限します。

クエリ オプションは、クライアントに多数のサーバーで実行されるクエリに制御を提供します。 場合によっては、セキュリティやパフォーマンス上の理由から使用可能なオプションを制限することができます。 **[Queryable]** 属性がいくつかビルド プロパティでこのです。 以下に例をいくつか示します。

ページングとそれ以外のものをサポートするために $skip と $top、のみを許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

データベースでインデックス化されていないプロパティでの並べ替えを防ぐために、特定のプロパティだけが順序付けを許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

"Eq"論理関数ですがないその他の論理関数を許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

すべての算術演算子をことはできません。

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

作成することによりグローバルにオプションを制限することができます、 **QueryableAttribute**インスタンスに渡すと、 **EnableQuerySupport**関数。

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>クエリ オプションの直接呼び出し

使用する代わりに、 **[Queryable]** 属性に、コント ローラーで直接クエリ オプションを呼び出すことができます。 これを行うには、追加、 **ODataQueryOptions**コント ローラー メソッドへのパラメーターです。 この場合、する必要はありません、 **[Queryable]** 属性。

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API には追加、 **ODataQueryOptions** URI からクエリ文字列。 適用するには、クエリを渡す、 **IQueryable**を**ApplyTo**メソッドです。 メソッドを返します別**IQueryable**です。

高度なシナリオではない場合、 **IQueryable**クエリ プロバイダーを調べることができます、 **ODataQueryOptions**し、別のフォームへのクエリ オプションを変換します。 (たとえば、RaghuRam Nadiminti のブログの投稿を参照してください[翻訳する OData クエリの HQL へ](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)も含まれていますが、[サンプル](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt))。

<a id="query-validation"></a>
## <a name="query-validation"></a>クエリの検証

**[Queryable]** 属性は、実行する前に、クエリを検証します。 検証のステップは、実行、 **QueryableAttribute.ValidateQuery**メソッドです。 検証プロセスをカスタマイズすることもできます。

参照してください[OData セキュリティ ガイダンス](odata-security-guidance.md)です。

最初に、オーバーライドされている検証コントロールのいずれかのクラスが定義されている、 **Web.Http.OData.Query.Validators**名前空間。 たとえば、次の検証コントロール クラスには、$orderby オプションの 'desc' オプションが無効にします。

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

サブクラス、 **[Queryable]** をオーバーライドする属性、 **ValidateQuery**メソッドです。

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

カスタム属性を設定するか、グローバルに、コント ローラーごと。

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

使用している場合**ODataQueryOptions**直接、オプションで、検証コントロールを設定します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
