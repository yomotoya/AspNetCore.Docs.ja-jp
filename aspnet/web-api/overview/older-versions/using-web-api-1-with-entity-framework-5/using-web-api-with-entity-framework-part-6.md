---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'パート 6: 製品と Order コント ローラーを作成する |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831314"
---
<a name="part-6-creating-product-and-order-controllers"></a>パート 6: 製品の作成と Order コント ローラー
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>製品のコント ローラーを追加します。

管理者コント ローラーは管理者特権を持つユーザーです。 お客様は、その一方で、ことができます製品の表示ことはできませんを作成、更新、またはそれらを削除します。

Get メソッドを開いたまま、Post、Put、および Delete メソッドを簡単にアクセスが制限できます。 製品に対して返されるデータになります。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost`プロパティを顧客に表示することはできません。 ソリューションは、定義する、*データ転送オブジェクト*(DTO) を含む顧客に表示されるプロパティのサブセットです。 LINQ を使用してプロジェクト`Product`インスタンス`ProductDTO`インスタンス。

という名前のクラスを追加`ProductDTO`Models フォルダーにします。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

コント ローラーを追加します。 ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**を選択し、**コント ローラー**します。 **コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;します。 **テンプレート**、**空の API コント ローラー**します。

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

次のコードでは、ソース ファイル内のすべてを置き換えます。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

コント ローラーを引き続き使用、`OrdersContext`データベース クエリを実行します。 返すのではなく`Product`インスタンスを直接呼び出して`MapProducts`をプロジェクトに`ProductDTO`インスタンス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts`メソッドが返す、 **IQueryable**ので、他のクエリ パラメーターと結果を作成します。 これで確認できます、`GetProduct`メソッドで、追加、**場所**句をクエリ。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>注文のコント ローラーを追加します。

次に、ユーザーの作成し、注文を表示できるようにするコント ローラーを追加します。

別の DTO を始めます。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前のクラスを追加`OrderDTO`次の実装を使用します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

コント ローラーを追加します。 ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**を選択し、**コント ローラー**します。 **コント ローラーの追加**ダイアログ ボックスで、次のオプションを設定します。

- **コント ローラー名**、"OrdersController"を入力します。
- **テンプレート**選択「を Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー」。
- **モデル クラス**、&quot;順序 (ProductStore.Models)&quot;します。
- **データ コンテキスト クラス**、 &quot;OrdersContext (ProductStore.Models)&quot;します。

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

**[追加]** をクリックします。 これには、OrdersController.cs という名前のファイルが追加されます。 次に、コント ローラーの既定の実装を変更する必要があります。

最初に、削除、`PutOrder`と`DeleteOrder`メソッド。 このサンプルでは、顧客は変更または既存の注文を削除できません。 実際のアプリケーションでは、このような場合を処理するためにバックエンド ロジックの多くする必要があります。 (たとえば、注文既に出荷でしょうか。)

変更、`GetOrders`をユーザーに属する注文だけを返すメソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

変更、`GetOrder`メソッドとして、次のとおりです。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

変更するためのメソッドを次に示します。

- 戻り値は、`OrderDTO`インスタンスの代わりに、`Order`します。
- 使用して、注文データベース クエリを実行、ときに、 [DbQuery.Include](https://msdn.microsoft.com/library/gg696395)関連をフェッチするメソッド`OrderDetail`と`Product`エンティティ。
- 射影を使用して結果をフラット化しました。

HTTP 応答の数量と製品の配列が含まれます。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

この形式は、クライアントが元のオブジェクトのグラフは、入れ子になったエンティティ (注文、詳細、および製品) が含まれるよりも使用できるは簡単です。

最後のメソッドが考慮すべき`PostOrder`します。 現在のところ、このメソッドは、`Order`インスタンス。 何が起きるが、クライアントは、このような要求の本文を送信した場合。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

これは、適切に構成された順序では、および Entity Framework では、データベースに挿入さいわいです。 以前は存在しなかった Product エンティティが含まれています。 クライアントは、新しい製品を作成したデータベースに! クマの注文を表示するときは、順序、フルフィルメント業務部門に驚くことになります。 使用は、POST または PUT 要求でそのまま使用するデータについて本当に注意してください。

この問題を回避するには、変更、`PostOrder`メソッドを`OrderDTO`インスタンス。 使用して、`OrderDTO`を作成する、`Order`します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

使用に注意してください、`ProductID`と`Quantity`プロパティ、および製品名または価格のいずれかのクライアントが送信されるすべての値を無視します。 製品 ID が有効でない場合は、データベースの外部キー制約に違反することとは、挿入は失敗します。

ここでは、完全な`PostOrder`メソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

最後に、追加、 **Authorize**属性をコント ローラー。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

今すぐ登録されたユーザーのみは作成または注文を表示できます。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-5.md)
> [次へ](using-web-api-with-entity-framework-part-7.md)
