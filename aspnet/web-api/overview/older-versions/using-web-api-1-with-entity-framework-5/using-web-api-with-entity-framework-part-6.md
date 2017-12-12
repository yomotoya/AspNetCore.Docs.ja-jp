---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "パート 6: 製品と順序コント ローラーの作成 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a>パート 6: 製品の作成と順序コント ローラー
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>製品のコント ローラーを追加します。

管理コント ローラーは、管理者特権を持っているユーザーに対してです。 お客様は、その一方で、できます製品の表示ことはできませんを作成、更新、またはそれらを削除します。

Get メソッドを開いたまま、Post、Put、および削除のメソッドへのアクセスを制限おできます簡単にします。 しかし、製品の返されるデータを見てください。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost`プロパティを顧客に表示することはできません。 ソリューションは、定義する、*データ転送オブジェクト*(DTO) が含まれている顧客に表示する必要のあるプロパティのサブセットにはです。 LINQ を使用してプロジェクト`Product`にインスタンス`ProductDTO`インスタンス。

という名前のクラスを追加`ProductDTO`Models フォルダーにします。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

コント ローラーを追加します。 ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**選択してから、**コント ローラー**です。 **コント ローラーの追加**ダイアログ ボックスで、名前、コント ローラー &quot;ProductsController&quot;です。 **テンプレート****空 API コント ローラー**です。

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

次のコードでは、ソース ファイル内のすべてを置き換えます。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

コント ローラーが現在も使用して、`OrdersContext`データベースを照会します。 返すのではなく`Product`インスタンスを直接呼んで`MapProducts`上にそれらをプロジェクトに`ProductDTO`インスタンス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts`メソッドを返します、 **IQueryable**ので、他のクエリ パラメーターと結果を作成します。 これで確認できます、`GetProduct`メソッドで、追加、**場所**クエリ句。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>注文のコント ローラーを追加します。

次に、ユーザーを作成し、注文を表示できるようにしているコント ローラーを追加します。

別の DTO をまず確認します。 ソリューション エクスプ ローラーで、Models フォルダーを右クリックし、という名前のクラスを追加`OrderDTO`次の実装を使用します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

コント ローラーを追加します。 ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**選択してから、**コント ローラー**です。 **コント ローラーの追加**ダイアログ ボックスで、次のオプションを設定します。

- **コント ローラー名**、"OrdersController"を入力します。
- **テンプレート**「Entity Framework を使用して、読み取り/書き込みアクションがある API コント ローラー」です。
- **モデル クラス**&quot;順序 (ProductStore.Models)&quot;です。
- **データ コンテキスト クラス** &quot;OrdersContext (ProductStore.Models)&quot;です。

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

**[追加]**をクリックします。 OrdersController.cs をという名前のファイルが追加されます。 次に、コント ローラーの既定の実装を変更する必要があります。

最初に、削除、`PutOrder`と`DeleteOrder`メソッドです。 このサンプルでは、お客様は変更または既存の注文を削除できません。 実際のアプリケーションでは、このような場合を処理するためのバックエンド ロジックの多くする必要があります。 (たとえばが、既に発送しますか?)

変更、`GetOrders`をユーザーに属する注文だけを返すメソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

変更、`GetOrder`メソッドを次のようにします。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

メソッドに加え変更を次に示します。

- 戻り値は、 `OrderDTO` 、インスタンスの代わりに、`Order`です。
- 使用して、注文のデータベースのクエリを実行おとき、 [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395)関連をフェッチするメソッド`OrderDetail`と`Product`エンティティです。
- 結果をフラット化私たちには、投影を使用します。

HTTP 応答の数量が製品の配列が含まれます。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

この形式は、クライアントが元のオブジェクトのグラフは、入れ子になったエンティティ (注文、詳細、および製品) を含むより使用できるは簡単です。

これを考慮する最後のメソッド`PostOrder`です。 現在、このメソッドは、`Order`インスタンス。 動作を検討してください。 クライアントは、次のように、要求本文を送信する場合。

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

これは、適切に構成された順序であり、Entity Framework では、データベースに挿入問題なく。 以前存在しなかった製品エンティティが含まれています。 データベースに新しい製品を作成したクライアントです。 クマの注文を確認したときに、注文 fullfilment 部門に驚くことになります。 教訓、本当に POST または PUT 要求でそのまま使用するデータに関する注意してください。

この問題を避けるためには、変更、`PostOrder`メソッドを`OrderDTO`インスタンス。 使用して、`OrderDTO`を作成する、`Order`です。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

使用に注意してください、`ProductID`と`Quantity`プロパティ、およびおは、製品名または価格のいずれかのクライアントが送信したすべての値を無視します。 製品 ID が有効でない場合、データベースの外部キー制約に違反することし、は、挿入は失敗します。

ここでは、完全な`PostOrder`メソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

最後に、追加、 **Authorize**コント ローラーに属性します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

これで登録されたユーザーのみを作成したり注文を表示します。

>[!div class="step-by-step"]
[前へ](using-web-api-with-entity-framework-part-5.md)
[次へ](using-web-api-with-entity-framework-part-7.md)
