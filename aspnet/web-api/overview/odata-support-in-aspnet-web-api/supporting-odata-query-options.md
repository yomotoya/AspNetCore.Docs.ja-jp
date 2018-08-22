---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: ASP.NET Web API 2 OData クエリ オプションをサポートしている |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825951"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET web API 2 OData クエリ オプションをサポート
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

OData では、OData のクエリを変更するために使用するパラメーターを定義します。 クライアントは、要求 URI のクエリ文字列で、これらのパラメーターを送信します。 たとえば、クライアントに結果を並べ替えるには、$orderby パラメーターを使用してください。

`http://localhost/Products?$orderby=Name`

OData の仕様は、これらのパラメーターを呼び出す*クエリ オプション*します。 プロジェクトで任意の Web API コント ローラーの OData クエリ オプションを有効にできます&#8212;コント ローラーは、OData エンドポイントである必要はありません。 これにより、フィルター処理およびすべての Web API アプリケーションに並べ替えなどの機能を追加する便利な方法です。

トピックを参照してください、クエリ オプションを有効にする前に[OData セキュリティ ガイダンス](odata-security-guidance.md)します。

- [OData クエリ オプションを有効にします。](#enable)
- [クエリの例](#examples)
- [サーバー ドリブン ページング](#server-paging)
- [クエリ オプションを制限します。](#limiting_query_options)
- [クエリ オプションを直接呼び出す](#ODataQueryOptions)
- [クエリの検証](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>OData クエリ オプションを有効にします。

Web API には、次の OData クエリ オプションがサポートされています。

| オプション | 説明 |
| --- | --- |
| $ を展開します。 | インラインの関連エンティティを展開します。 |
| $filter | ブール条件に基づいて、結果をフィルター処理します。 |
| $inlinecount | 一致するエンティティの合計数を応答に含めるようにサーバーに指示します。 (サーバー側のページングに役立ちます。) |
| $orderby | 結果を並べ替えます。 |
| $select | 応答に含めるプロパティを選択します。 |
| $skip | 最初の n 個の結果をスキップします。 |
| $top | 最初の n のみに結果を返します。 |

OData クエリ オプションを使用するには、必要があります有効に明示的にします。 特定のコント ローラーまたは特定のアクションのために有効または、アプリケーション全体でグローバルに有効にできます。

OData クエリ オプションをグローバルに有効にするを呼び出す**EnableQuerySupport**上、 **HttpConfiguration**起動時クラス。

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport**メソッドを返すコント ローラーのアクションに対してグローバルにクエリ オプションを使用できます、 **IQueryable**型。 クエリ オプションをアプリケーション全体に対して有効にしない場合は、ことができますの特定のコント ローラーのアクションを追加して、 **[Queryable]** 属性をアクション メソッド。

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>クエリの例

このセクションでは、OData クエリ オプションを使用してクエリの種類が表示されます。 特定の詳細については、クエリ オプションでは、OData ドキュメントを参照してください[www.odata.org](http://www.odata.org/)します。

$ について順に展開し、$select を参照してください[$ の $select を使用して、展開と ASP.NET Web API odata $value](using-select-expand-and-value.md)。

**クライアント ドリブン ページング**

大きなエンティティ セットの場合は、クライアントは、結果の数を制限する可能性があります。 たとえば、クライアントは、結果の次のページを取得する [次へ] のリンク時に 10 個のエントリを表示する可能性があります。 これを行うには、クライアントは、$top、$skip オプションを使用します。

`http://localhost/Products?$top=10&$skip=20`

$Top オプションは、返される項目の最大数と、$skip オプションは、スキップするエントリの数を示します。 前の例では、21 ~ 30 のエントリをフェッチします。

**フィルター処理**

$Filter オプションは、ブール式を適用することで、結果をフィルター処理クライアントを使用できます。 フィルター式は非常に強力です。論理演算子および算術演算子、文字列関数、および日付関数が含まれます。

| "Toys"に等しいすべての製品のカテゴリを返します。 | `http://localhost/Products?$filter=Category` eq 'Toys' |
| --- | --- |
| すべての製品の価格が 10 よりも小さいかを返します。 | `http://localhost/Products?$filter=Price` lt 10 |
| 論理演算子: すべての製品を返す価格、> = 5 と価格 < 15 を = です。 | `http://localhost/Products?$filter=Price` ge 5 と価格 le 15 |
| 文字列関数: 名前ですべての製品の"zz"を返します。 | `http://localhost/Products?$filter=substringof('zz',Name)` |
| 日付関数: 2005年より後のすべての製品 ReleaseDate を返します。 | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**並べ替え**

結果を並べ替えるには、$orderby フィルターを使用します。

| 価格で並べ替えます。 | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| 降順 (最低に最高) の価格で並べ替えます。 | `http://localhost/Products?$orderby=Price desc` |
| カテゴリ別に並べ替えるし、価格のカテゴリ内の降順で並べ替えます。 | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>サーバー ドリブン ページング

送信しない場合は、データベースには、何百万もレコードにはが含まれています、すべて 1 つのペイロードにします。 これを防ぐためには、サーバーは、1 つの応答で送信するエントリの数を制限できます。 サーバーのページングを有効にするには設定、 **PageSize**プロパティ、 **Queryable**属性。 値は、返される項目の最大数です。

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

コント ローラーには、OData 形式が返された場合、応答本文にはデータの次のページへのリンクが含まれます。

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

クライアントは、次のページをフェッチするのにこのリンクを使用できます。 結果セット内のエントリの合計数については、クライアント設定できます $inlinecount クエリ オプション値を持つ"allpages"。

`http://localhost/Products?$inlinecount=allpages`

値"allpages"応答の合計数を追加するには、サーバーに指示します。

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> 次のページへのリンクとインライン カウントの OData 形式が必要です。 理由は、OData のリンクとカウントを保持するために、応答本文で特別なフィールドを定義します。


クエリの結果をラップすることで、次のページのリンクとインライン カウントをサポートするためにできますが、非 OData 形式の場合、 **PageResult&lt;T&gt;** オブジェクト。 ただし、少しコードが必要です。 次に例を示します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

JSON 応答例を次に示します。

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>クエリ オプションを制限します。

クエリ オプションは、クライアントに多数のサーバーで実行されるクエリに制御を提供します。 場合によっては、セキュリティやパフォーマンス上の理由から使用可能なオプションを制限する可能性があります。 **[Queryable]** 属性が一部の組み込みプロパティでこのします。 以下に例をいくつか示します。

ページングと何をサポートするために、$skip、$top、のみを許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

特定のプロパティによってのみ、データベースでインデックス化されていないプロパティで並べ替えを防ぐために順序付けを許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

"Eq"論理関数ですがないその他の論理関数を許可します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

すべての算術演算子をことはできません。

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

オプションをグローバルに制限するには作成することにより、 **QueryableAttribute**インスタンスとに渡して、 **EnableQuerySupport**関数。

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>クエリ オプションを直接呼び出す

使用する代わりに、 **[Queryable]** 属性をコント ローラーで直接クエリ オプションを呼び出すことができます。 これを行うには、追加、 **ODataQueryOptions**コント ローラー メソッドのパラメーター。 この場合、必要はありません、 **[Queryable]** 属性。

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API の作成、 **ODataQueryOptions** URI からクエリ文字列。 クエリを適用するには、渡す、 **IQueryable**を**ApplyTo**メソッド。 メソッドを返します別**IQueryable**します。

高度なシナリオではない場合、 **IQueryable**クエリ プロバイダーを調べることができます、 **ODataQueryOptions**し、別のフォームへのクエリ オプションを変換します。 (たとえば、RaghuRam Nadiminti のブログの投稿を参照してください[変換の OData クエリの HQL へ](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)も含まれていますが、[サンプル](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt))。

<a id="query-validation"></a>
## <a name="query-validation"></a>クエリの検証

**[Queryable]** 属性は、実行する前に、クエリを検証します。 検証手順がで実行される、 **QueryableAttribute.ValidateQuery**メソッド。 検証プロセスをカスタマイズすることもできます。

参照してください[OData セキュリティ ガイダンス](odata-security-guidance.md)します。

最初に、オーバーライドされている検証コントロールのいずれかのクラスが定義されている、 **Web.Http.OData.Query.Validators**名前空間。 たとえば、次の検証コントロール クラスには、$orderby オプションの 'desc' オプションが無効にします。

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

サブクラス、 **[Queryable]** をオーバーライドする属性、 **ValidateQuery**メソッド。

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

カスタム属性を設定するかグローバルにまたはコント ローラーごと。

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

使用する場合**ODataQueryOptions**を直接検証コントロールをオプションに設定します。

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
