---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: ASP.NET Web API を使用した OData v4 の複合型の継承 |Microsoft Docs
author: microsoft
description: OData v4 仕様では、複合型は、別の複合型から継承できます。 (複合型は、キーのない構造化型です)。Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 84a887b445959c4aa6d1ee372f067f93cd725d77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372058"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API を使用した OData v4 の複合型の継承
====================
によって[Microsoft](https://github.com/microsoft)

> OData v4 に従って[仕様](http://www.odata.org/documentation/odata-version-4-0/)、複合型は、別の複合型から継承できます。 (A*複雑な*型はキーのない構造化された型です)。Web API OData 5.3 は、複合型の継承をサポートします。
> 
> このトピックでは、複雑な継承の種類で entity data model (EDM) を構築する方法を示します。 完全なソース コードでは、次を参照してください。 [OData の複合型継承サンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt)します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>モデル階層

複合型の継承を示すためには、次のクラス階層を使用します。

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` 抽象複合型です。 `Rectangle`、 `Triangle`、および`Circle`複合型から派生`Shape`、および`RoundRectangle`から派生した`Rectangle`します。 `Window` エンティティ型は、含む、`Shape`インスタンス。

これらの型を定義する CLR クラスを次に示します。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>EDM モデルを構築します。

使用することができます、EDM を作成する**ODataConventionModelBuilder**CLR 型からの継承関係を推論します。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

ビルドすることも、EDM に明示的を使用して**に**します。 これにより、多くのコードを受け取るが、EDM より詳細に制御できます。

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

これら 2 つの例では、同じ EDM スキーマを作成します。

## <a name="metadata-document"></a>メタデータ ドキュメント

OData の複合型の継承を示すメタデータ ドキュメントを次に示します。

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

メタデータ ドキュメントをことを確認できます。

- `Shape`複雑な型が抽象型。
- `Rectangle`、 `Triangle`、および`Circle`複合型が基本型である`Shape`します。
- `RoundRectangle`型が基本型`Rectangle`します。

## <a name="casting-complex-types"></a>複合型のキャスト

複合型のキャストはサポートされています。 たとえば、次のクエリのキャスト、`Shape`を`Rectangle`します。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

応答ペイロードを次に示します。

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
