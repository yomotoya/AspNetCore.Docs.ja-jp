---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: "Web API 2.2 を使用して OData v4 でコンテインメント |Microsoft ドキュメント"
author: rick-anderson
description: "従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。 OData v4 には、単一予測クエリおよび Con 2 つの追加オプションを提供しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Web API 2.2 を使用して OData v4 の包含
====================
Jinfu tan をクリックして

> 従来、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセス可能性があります。 OData v4 がシングルトンや含有関係、WebAPI 2.2 をどちらもサポートしている 2 つの追加オプションを提供します。


このトピックでは、WebApi 2.2 で OData エンドポイントでコンテインメントを定義する方法を示します。 コンテインメントの詳細については、次を参照してください。 [OData v4 に今後予定されているコンテインメント](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)です。 Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用する ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)です。

最初は、モデルを作成コンテインメント ドメイン OData サービスでこのデータ モデルを使用します。

![データ モデル](odata-containment-in-web-api-22/_static/image1.png)

アカウントには、多く PaymentInstruments (PI) が含まれていますが、エンティティ PI のセットを定義していません。 代わりに、Pi は、アカウントを介してのみアクセスできることができます。

このトピックで使用されるソリューションをダウンロードする[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)です。

## <a name="defining-the-data-model"></a>データ モデルを定義します。

1. CLR 型を定義します。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained`コンテインメント ナビゲーション プロパティの属性を使用します。
2. CLR 型に基づき、EDM モデルを生成します。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` EDM モデルを構築する場合を処理する、`Contained`属性が、対応するナビゲーション プロパティに追加します。 プロパティがコレクション型の場合、`GetCount(string NameContains)`関数も作成されます。

    生成されたメタデータは、次のようになります。

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget`属性の場合、ナビゲーション プロパティが、包含であることを示します。

## <a name="define-the-containing-entity-set-controller"></a>含むエンティティ セットのコント ローラーを定義します。

含まれているエンティティは、独自のコント ローラー; がありません。アクションが含まれるエンティティ セットのコント ローラーで定義されます。 このサンプルでは、AccountsController がない PaymentInstrumentsController です。

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

OData パスが 4 つ以上のセグメントである場合は、専用の属性がルーティングのしくみなど`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上のコント ローラーにします。 それ以外の場合、属性と従来のルーティングの両方の動作: たとえば、`GetPayInPIs(int key)`と一致する`GET ~/Accounts(1)/PayinPIs`です。

*この記事の元のコンテンツ Leo %hu に感謝します。*
