---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'パート 2: ドメイン モデルの作成 |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379726"
---
<a name="part-2-creating-the-domain-models"></a>パート 2: ドメイン モデルの作成
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>モデルを追加します。

Entity Framework のアプローチに 3 つの方法はあります。

- データベース ファースト: データベースで開始して、Entity Framework には、コードが生成されます。
- モデル ファースト: ビジュアルのモデルから開始して、Entity Framework には、データベースとコードの両方が生成されます。
- Code first: コードを開始して、Entity Framework には、データベースが生成されます。

Poco (plain-old CLR object) としてのドメイン オブジェクトを定義することで開始 code first アプローチを使用しています。 Code first アプローチでは、ドメイン オブジェクトは、トランザクションや永続化など、データベース層をサポートするために余分なコードを必要はありません。 (具体的には、それらから継承する必要はありません、 [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx)クラスです)。Entity Framework がデータベース スキーマを作成する方法を制御するのに、データ注釈を引き続き使用できます。

Poco を記述する追加のプロパティを搬送しないため、[データベース状態](https://msdn.microsoft.com/library/system.data.entitystate.aspx)、JSON または XML にシリアルできます化簡単にします。 ただし、わけには、クライアントでは、直接、Entity Framework モデルを常に公開する必要があります、チュートリアルの後半で表示されるようにします。

次の Poco を作成します。

- 製品
- 順番
- OrderDetail

各クラスを作成するには、ソリューション エクスプ ローラーで Models フォルダーを右クリックします。 コンテキスト メニューでは、次のように選択します。**追加**選び**クラス。**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

追加、`Product`次の実装クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

慣例により、Entity Framework を使用して、`Id`プロパティを主キーとして、データベース テーブルに id 列にマップします。 新規に作成するときに`Product`インスタンスの値を設定しません`Id`データベースの値を生成するためです。

**ScaffoldColumn**属性に指示をスキップする ASP.NET MVC、`Id`プロパティ エディターのフォームを生成するときにします。 **必要**属性は、モデルの検証に使用します。 指定します、`Name`プロパティは空でない文字列である必要があります。

追加、`Order`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

追加、`OrderDetail`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>外部キーの関係

注文は多くの注文の詳細を含み、各注文の詳細は、1 つの製品を参照します。 これらのリレーションを表す、`OrderDetail`クラスという名前のプロパティを定義する`OrderId`と`ProductId`します。 これらのプロパティが外部キーを表すし、データベースに foreign key 制約を追加、entity Framework は推論します。

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order`と`OrderDetail`クラスでは、関連オブジェクトへの参照を含む「ナビゲーション」プロパティも含まれます。 順序を指定すると、ナビゲーション プロパティを次の順序で製品に移動できます。

これで、プロジェクトをコンパイルします。 Entity Framework では、リフレクションを使用して、コンパイル済みのアセンブリ、データベース スキーマを作成する必要がありますので、モデルのプロパティを検出します。

## <a name="configure-the-media-type-formatters"></a>メディア タイプ フォーマッタを構成します。

A[メディア タイプ フォーマッタ](../../formats-and-model-binding/media-formatters.md)は Web API は HTTP 応答の本文を書き込むときに、データをシリアル化するオブジェクトです。 組み込みのフォーマッタは、JSON および XML の出力をサポートします。 既定で、すべてのオブジェクトを値別これらフォーマッタのどちらもシリアル化します。

値によるシリアル化は、オブジェクト グラフに循環参照が含まれている場合、問題を作成します。 これは、場合で正確には、`Order`と`OrderDetail`クラスをそれぞれに、もう一方への参照が格納されているためです。 フォーマッタは以下の参照、値によって各オブジェクトを記述し、円で移動します。 そのため、既定の動作を変更する必要があります。

ソリューション エクスプ ローラーでアプリケーションを拡張して\_フォルダーを起動し、WebApiConfig.cs という名前のファイルを開きます。 `WebApiConfig` クラスに次のコードを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

このコードでは、オブジェクトの参照を保持するために、JSON フォーマッタを設定し、パイプラインから完全 XML フォーマッタを削除します。 (オブジェクトの参照を保持するために、XML フォーマッタを構成することができますが、もう少しの作業が、このアプリケーションの JSON のみ必要があります。 詳細については、次を参照してください[循環オブジェクト参照の処理](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)。)。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-1.md)
> [次へ](using-web-api-with-entity-framework-part-3.md)
