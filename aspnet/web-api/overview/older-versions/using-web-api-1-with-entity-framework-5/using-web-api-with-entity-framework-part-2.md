---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'パート 2: ドメイン モデルの作成 |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878582"
---
<a name="part-2-creating-the-domain-models"></a>パート 2: ドメイン モデルの作成
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>モデルを追加します。

これには Entity Framework のアプローチを次の 3 つの方法があります。

- データベース優先: データベースを起動して、Entity Framework には、コードが生成されます。
- モデル優先: ビジュアル モデルから起動して、Entity Framework では、データベースとコードの両方が生成されます。
- コード優先: コードでは、起動して、Entity Framework には、データベースが生成されます。

コード優先のアプローチは使用した poco から (従来の CLR オブジェクト) として、ドメイン オブジェクトを定義することで開始するためです。 コード優先のアプローチでは、ドメイン オブジェクトはトランザクション処理や永続化など、データベース層をサポートするために余分なコードを必要ありません。 (具体的には、それらから継承する必要はありません、 [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)クラスです)。データ注釈を使用して、Entity Framework が、データベース スキーマを作成する方法を制御することができますも。

した poco からに説明する追加のプロパティを使用することはありませんので[データベース状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)、JSON または XML に簡単にシリアル化します。 ただしをないわけでは常に、クライアントに直接、Entity Framework モデルを公開する必要があります、チュートリアルで後ほどおわかりのようです。

次のした poco からを作成されます。

- 製品
- 順番
- OrderDetail

各クラスを作成するには、ソリューション エクスプ ローラーで、[モデル] フォルダーを右クリックします。 コンテキスト メニューから選択**追加**し、**クラスです。**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

追加、`Product`次の実装を持つクラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

慣例により、Entity Framework を使用して、`Id`主キーとしてのプロパティであり、データベース テーブルに id 列にマップします。 新規に作成するときに`Product`、インスタンスの値を設定しない`Id`データベースには、値がによって生成されるため、します。

**ScaffoldColumn**属性に指示をスキップする ASP.NET MVC、`Id`プロパティ エディター フォームを生成するときにします。 **必要**属性は、モデルの検証に使用します。 指定する、`Name`プロパティが空でない文字列にする必要があります。

追加、`Order`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

追加、`OrderDetail`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>外部キー関係

注文には、多くの注文の詳細が含まれています。 し、各注文の詳細は 1 つの製品を参照します。 これらのリレーションを表す、`OrderDetail`クラスという名前のプロパティを定義する`OrderId`と`ProductId`です。 これらのプロパティが外部キーを表すし、データベースに foreign key 制約を追加、entity Framework は推論されます。

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order`と`OrderDetail`クラスも含まれて「ナビゲーション」のプロパティは、関連するオブジェクトへの参照が含まれています。 順序を指定するに移動できます順序では、製品によって次のナビゲーション プロパティ。

これで、プロジェクトをコンパイルします。 Entity Framework では、リフレクションを使用して、コンパイルされたアセンブリをデータベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。

## <a name="configure-the-media-type-formatters"></a>メディア タイプ フォーマッタを構成します。

A[メディア タイプ フォーマッタ](../../formats-and-model-binding/media-formatters.md)Web API は、HTTP 応答の本文を書き込むときに、データをシリアル化するオブジェクトです。 組み込みのフォーマッタは、JSON と XML の出力をサポートします。 既定では、これらフォーマッタの両方をシリアル化のすべてのオブジェクト値。

値によるシリアル化は、オブジェクト グラフに循環参照が含まれている場合、問題を作成します。 場合と正確には、`Order`と`OrderDetail`クラス、それぞれに他方への参照が格納されているためです。 フォーマッタでは、次の参照、値によって、各オブジェクトの作成し、堂々巡りに移動します。 したがって、既定の動作を変更する必要があります。

ソリューション エクスプ ローラーで、アプリを展開して\_フォルダーを起動し、WebApiConfig.cs をという名前のファイルを開きます。 `WebApiConfig` クラスに次のコードを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

このコードでは、オブジェクト参照を保持するために JSON フォーマッタを設定し、パイプラインから完全 XML フォーマッタを削除します。 (オブジェクト参照を保持する XML フォーマッタを構成することができますが、少しの作業は、このアプリケーションの JSON のみ必要があります。 詳細については、次を参照してください[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-1.md)
> [次へ](using-web-api-with-entity-framework-part-3.md)
