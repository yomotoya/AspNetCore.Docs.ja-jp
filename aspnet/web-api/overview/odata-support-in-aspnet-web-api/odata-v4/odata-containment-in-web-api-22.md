---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Web API 2.2 を使用して OData v4 のコンテインメイト |Microsoft Docs
author: rick-anderson
description: これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。 OData v4 でシングルトンおよび Con 2 つの追加オプションを提供しています.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: aca263a04df25ca241bc0b9798b3a0b588d4cae8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826343"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Web API 2.2 を使用して OData v4 のコンテインメイト
====================
によって Jinfu Tan

> これまでは、エンティティは、エンティティ セット内にカプセル化された場合にのみアクセスできます。 OData v4、シングルトンおよびコンテインメント、web Api 2.2 をサポートする 2 つの追加オプションを提供します。


このトピックでは、web Api 2.2 で OData エンドポイントでコンテインメントを定義する方法を示します。 コンテインメントの詳細については、次を参照してください。 [OData v4 で予定されている包含](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx)します。 Web api OData V4 エンドポイントを作成するを参照してください。[作成 OData v4 エンドポイントを使用して ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md)します。

最初に、作成しますコンテインメントのドメイン モデル、OData サービスでこのデータ モデルを使用します。

![データ モデル](odata-containment-in-web-api-22/_static/image1.png)

アカウントには多く PaymentInstruments (PI) が含まれていますが、エンティティ、PI のセットを定義していません。 代わりに、Pi は、アカウントを介してのみアクセスできます。

このトピックで使用するソリューションをダウンロードする[CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)します。

## <a name="defining-the-data-model"></a>データ モデルを定義します。

1. CLR 型を定義します。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    `Contained`コンテインメント ナビゲーション プロパティに属性を使用します。
2. CLR 型に基づく EDM モデルを生成します。

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    `ODataConventionModelBuilder` EDM モデルを構築する場合を処理する、`Contained`属性は、対応するナビゲーション プロパティに追加されます。 プロパティがコレクション型の場合、`GetCount(string NameContains)`関数も作成されます。

    生成されるメタデータは、次のようになります。

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    `ContainsTarget`属性は、ナビゲーション プロパティは、包含であることを示します。

## <a name="define-the-containing-entity-set-controller"></a>親エンティティ セットのコント ローラーを定義します。

含まれているエンティティは、独自のコント ローラーは必要はありません。アクションは、親エンティティ セットのコント ローラーで定義されます。 このサンプルでは、AccountsController が PaymentInstrumentsController しません。

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

OData パスが 4 つ以上のセグメントである場合は、専用の属性がルーティングのしくみなど`[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]`上記のコント ローラー。 それ以外の場合、属性と従来のルーティングの動作: たとえば、`GetPayInPIs(int key)`と一致する`GET ~/Accounts(1)/PayinPIs`します。

*Leo Hu に改訂された記事については、元のコンテンツいただきありがとうございます。*
