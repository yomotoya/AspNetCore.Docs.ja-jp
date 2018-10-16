---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: ASP.NET Web API 2 のセキュリティ ガイダンス OData |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325719"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>ASP.NET Web API 2 のセキュリティ ガイダンス OData
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

このトピックでは、OData でのデータセットを公開するときに考慮すべきセキュリティの問題について説明します。

## <a name="edm-security"></a>EDM のセキュリティ

クエリのセマンティクスは、entity data model (EDM) でないモデルの基になる型に基づきます。 プロパティを除外するには、EDM からして、クエリには表示されません。 たとえば、給与プロパティを持つ従業員の種類が、モデルに含まれています。 クライアントから非表示にする、EDM からこのプロパティを除外する場合があります。

除外する 2 つの方法、EDM からプロパティ。 設定することができます、 **[IgnoreDataMember]** モデル クラスのプロパティの属性。

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

EDM からプログラムで、プロパティも削除できます。

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>クエリのセキュリティ

悪意のあるまたは単純なクライアントを実行する非常に長い時間がかかるクエリを作成することがあります。 最悪の場合に、サービスへのアクセスをこれが中断されることができます。

**[Queryable]** 属性は、解析、検証、およびクエリを適用するアクション フィルター。 フィルターは、クエリ オプションを LINQ 式に変換します。 OData コント ローラーを返す場合、 **IQueryable**の種類、 **IQueryable** LINQ プロバイダーは、クエリに LINQ 式を変換します。 そのため、パフォーマンスは、LINQ プロバイダーを使用して、データセットやデータベース スキーマの特定の特性を依存します。

ASP.NET Web API での OData クエリ オプションの使用に関する詳細については、次を参照してください。 [OData クエリ オプションをサポートしている](supporting-odata-query-options.md)します。

(たとえば、エンタープライズ環境) で、すべてのクライアントが信頼されていることがわかっている場合、または、データセットが小さい場合は、クエリのパフォーマンスが問題になりません。 それ以外の場合、次の推奨事項を考慮する必要があります。

- さまざまなクエリを使用してサービスをテストし、DB をプロファイルします。
- 1 つのクエリで大規模なデータ セットを返すことを回避するために、サーバー ドリブン ページングを有効にします。 詳細については、次を参照してください。 [Server-Driven ページング](supporting-odata-query-options.md#server-paging)します。 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- $Filter、$orderby が必要ですか。 一部のアプリケーションは、ページング、$top、$skip を使用してクライアントを許可するが、その他のクエリ オプションを無効にする可能性があります。 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- クラスター化インデックスのプロパティに $orderby を制限することを検討してください。 クラスター化インデックスがなくても大量のデータの並べ替えは、低速です。 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- 最大ノード数: **MaxNodeCount**プロパティ **[Queryable]** $filter 構文ツリー内で許可されているノードの最大数を設定します。 既定値は 100 が、コンパイルに時間がかかることができるノードの数が多いため、低い値を設定する可能性があります。 これは LINQ to Objects (つまり、中間の LINQ プロバイダーを使用せずに、メモリ内コレクションに対して LINQ クエリ) を使用している場合に特に当てはまります。 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Any() および all() の関数で無効にすると遅くなることが検討します。 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- 任意の文字列プロパティには、大きな文字列が含まれている場合&#8212;製品の説明、ブログ エントリなど、&#8212;文字列関数を無効にしてみます。 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- ナビゲーション プロパティのフィルター処理を禁止することを検討してください。 ナビゲーション プロパティのフィルター処理、結合、遅く、データベース スキーマに応じて、可能性のある可能性があります。 次のコードでは、ナビゲーション プロパティのフィルター処理しないようにクエリ検証コントロールを示します。 クエリ検証コントロールの詳細については、次を参照してください。[クエリ検証](supporting-odata-query-options.md#query-validation)です。 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- データベース用にカスタマイズされている検証コントロールを記述することで $filter のクエリを制限することを検討してください。 たとえば、これら 2 つのクエリがあるとします。 

  - すべての映画"A"で始まる最後の名前を持つ actors の使用。
  - 1994 年にリリースされたすべての映画します。

    映画がアクターによってインデックスが作成しない限り、最初のクエリは、ムービーのリスト全体をスキャンする DB エンジンを必要があります。 2 番目のクエリもかまいませんが、リリース年ごとと仮定して映画のインデックスが作成されます。

    次のコードでは、"ReleaseYear"と"Title"プロパティがないその他のプロパティのフィルター処理できるようにする検証コントロールを示します。

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- 一般に、必要などの $filter 機能を検討してください。 クライアントは $filter の完全な表現力を必要としない場合は、関数を使用できますを制限できます。
