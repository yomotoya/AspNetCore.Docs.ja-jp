---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: "ASP.NET Web API 2 のセキュリティ ガイダンス OData |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 799e2a0c742b545acf3b5cd27531d734aa7def80
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>ASP.NET Web API 2 のセキュリティ ガイダンス OData
====================
によって[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、OData を介してデータセットを公開するときに考慮すべきセキュリティの問題について説明します。

## <a name="edm-security"></a>EDM のセキュリティ

クエリのセマンティクスは、基になるモデル型ではなく entity data model (EDM) に基づきます。 EDM からプロパティを除外することし、クエリに表示されません。 たとえば、モデルには、給与プロパティを持つ従業員タイプが含まれるとします。 クライアントから非表示にする、EDM からこのプロパティを除外することがあります。

除外する 2 つの方法 EDM からプロパティ。 設定することができます、 **[IgnoreDataMember]**モデル クラスのプロパティの属性。

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

削除することも、プロパティ、EDM からプログラムで。

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>クエリのセキュリティ

悪意のある、または単純なクライアントを実行する非常に長い時間がかかるクエリを作成することがあります。 最悪の場合に、サービスへのアクセスをこのが中断されることができます。

**[Queryable]**属性は、アクション フィルターを解析して、検証し、クエリを適用します。 フィルターは、クエリ オプションを LINQ 式に変換します。 OData コント ローラーを返す場合、 **IQueryable**の種類、 **IQueryable** LINQ プロバイダーは、クエリを LINQ 式に変換します。 そのため、パフォーマンスは、使用されている LINQ プロバイダーとも、データセットやデータベース スキーマの特定の特性によって異なります。

詳細については、ASP.NET Web API での OData クエリ オプションを使用して、次を参照してください。 [OData クエリ オプションをサポートする](supporting-odata-query-options.md)です。

(たとえば、エンタープライズ環境で)、信頼されるすべてのクライアントであることがわかっている場合、またはデータセットが小さい場合は、クエリのパフォーマンスが問題をできないがあります。 それ以外の場合、次の推奨事項を検討する必要があります。

- さまざまなクエリで、サービスをテストし、DB をプロファイルします。
- 大きなデータ セットを 1 つのクエリで返されないようにする、サーバー ドリブン ページングを有効にします。 詳細については、次を参照してください。 [Server-Driven ページング](supporting-odata-query-options.md#server-paging)です。 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- $Filter、$orderby が必要ですか。 一部のアプリケーションは、ページング、$top、$skip を使用してクライアントを許可するが、他のクエリ オプションを無効にする可能性があります。 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- $Orderby をクラスター化インデックスのプロパティに制限することを検討してください。 クラスター化インデックスを大規模なデータの並べ替えは低速です。 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- ノードの最大数: **MaxNodeCount**プロパティ**[Queryable]** $filter 構文ツリー内で許可されている最大の数値ノードを設定します。 既定値は 100、ですが、多数のノードをコンパイルに時間がかかることができるのでを下限の値を設定することがあります。 これは LINQ to Objects (中級者向けの LINQ プロバイダーを使用せず、メモリ内コレクションに対して LINQ クエリなど) を使用している場合に特に当てはまります。 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- 遅くなることが、any() と all() と関数の無効化を検討します。 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- 場合は、文字列プロパティには、大きな文字列 & #8212for 例、製品の説明またはブログ エントリ & #8212consider 文字列関数の無効化が含まれます。 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- ナビゲーション プロパティのフィルター処理を禁止することを検討してください。 ナビゲーション プロパティのフィルター処理結果、結合は、データベース スキーマに応じて、遅くなる可能性があります。 次のコードでは、ナビゲーション プロパティのフィルター処理しないようにクエリ検証コントロールを示します。 クエリ検証コントロールの詳細については、次を参照してください。[クエリ検証](supporting-odata-query-options.md#query-validation)です。 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- データベース用にカスタマイズされている検証コントロールを記述して $filter のクエリを制限することを検討してください。 たとえば、これら 2 つのクエリについて考えてみます。 

    - すべてのムービーで最後の名前 'A' で始まるアクターとします。
    - 1994 年にリリースされたすべてのムービーします。

    ムービーがアクターによってインデックスが作成しない限り、最初のクエリは、ムービーの一覧全体をスキャンする DB エンジンを必要があります。 許容可能な 2 番目のクエリでありと仮定してムービーはリリース年によってインデックスが作成されます。

    次のコードでは、"ReleaseYear"と"Title"プロパティがないその他のプロパティでフィルター処理するバリデーターを示しています。

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- 一般に、必要などの $filter 機能を検討してください。 クライアントで必要としない $filter の完全表現力場合は、使用可能な関数を制限できます。
